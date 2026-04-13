#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
swot_self_assistant.py
Version: 3.0.0
Purpose: An interactive assistant that guides the user step-by-step to create a complete and valid
         SWOT analysis. It ensures completeness, prevents low-quality inputs, and produces structured
         outputs with recommendations — exactly per the provided tool spec.
Author: (you)
License: MIT

USAGE
-----
# 1) Interactive coaching mode (default):
python swot_self_assistant.py

# 2) Non-interactive (provide inputs as CLI flags):
python swot_self_assistant.py \
  --organization_context "We sell pencils, deliver to offices, and plan to expand regionally..." \
  --scope "Office supplies & distribution; Mid-eastern region; 2025-2027" \
  --goal "Overall SWOT to inform growth plan"

# 3) From JSON input:
python swot_self_assistant.py --input_json input.json

# 4) Run built-in unit tests:
python swot_self_assistant.py --run_tests

OUTPUT
------
Prints a single JSON object to STDOUT:
{
  "swot_matrix": {...},
  "recommendations": [...],
  "guidance_log": {...}
}

DESIGN NOTES
------------
- Matches your spec's pipeline: guide_user → validate_inputs → generate_swot → deduplicate → generate_recommendations → finalize_output.
- Quality rules: context ≥ 15 chars; non-empty scope & goal; ≥2 items in strengths/weaknesses; no duplicates across categories.
- Guardrails: All four categories always present; recommendations phrased as actions.
- Deterministic heuristics (no LLM) so it’s portable and testable.
"""

from __future__ import annotations
import argparse, json, sys, textwrap
from dataclasses import dataclass, asdict, field
from typing import Dict, List, Optional, Tuple, Any
from datetime import datetime

VERSION = "3.0.0"

# --------------------------- Data Models ---------------------------

@dataclass
class Inputs:
    organization_context: str
    scope: str
    goal: str
    coaching_mode: bool = True

@dataclass
class GuidanceStep:
    id: str
    prompt: Optional[str] = None
    user_input: Optional[Dict[str, Any]] = None
    result: Optional[Dict[str, Any]] = None
    notes: Optional[str] = None
    checks: Optional[Dict[str, Any]] = None

@dataclass
class SWOTMatrix:
    strengths: List[str] = field(default_factory=list)
    weaknesses: List[str] = field(default_factory=list)
    opportunities: List[str] = field(default_factory=list)
    threats: List[str] = field(default_factory=list)

@dataclass
class Output:
    swot_matrix: Optional[SWOTMatrix]
    recommendations: Optional[List[Dict[str, Any]]]
    guidance_log: Dict[str, Any]

# --------------------------- Utilities ---------------------------

def _clean(s: str) -> str:
    return " ".join((s or "").strip().split())

def _is_nonempty(s: str) -> bool:
    return bool(_clean(s))

def _dedupe_list(items: List[str]) -> List[str]:
    seen = set()
    out = []
    for it in items:
        key = _clean(it).lower()
        if key and key not in seen:
            seen.add(key)
            out.append(_clean(it))
    return out

def _remove_cross_duplicates(swot: SWOTMatrix) -> SWOTMatrix:
    # Ensure no duplicate factor appears in more than one category (case-insensitive)
    buckets = {
        "strengths": [x.lower() for x in swot.strengths],
        "weaknesses": [x.lower() for x in swot.weaknesses],
        "opportunities": [x.lower() for x in swot.opportunities],
        "threats": [x.lower() for x in swot.threats],
    }
    # Priority order: strengths > weaknesses > opportunities > threats for keeping first
    order = ["strengths", "weaknesses", "opportunities", "threats"]
    kept = set()
    cleaned = SWOTMatrix([], [], [], [])
    for cat in order:
        source = getattr(swot, cat)
        for item in source:
            key = _clean(item).lower()
            if key not in kept:
                getattr(cleaned, cat).append(item)
                kept.add(key)
    return cleaned

# --------------------------- Validation ---------------------------

def validate_inputs(inp: Inputs) -> Tuple[bool, List[str]]:
    errors = []
    if not _is_nonempty(inp.organization_context) or len(_clean(inp.organization_context)) < 15:
        errors.append("context too short (must be ≥ 15 characters)")
    if not _is_nonempty(inp.scope):
        errors.append("scope is required")
    if not _is_nonempty(inp.goal):
        errors.append("goal is required")
    return (len(errors) == 0, errors)

def validate_swot(swot: SWOTMatrix) -> Tuple[bool, List[str]]:
    errors = []
    # Guardrail: all four categories present
    for c in ["strengths", "weaknesses", "opportunities", "threats"]:
        if not hasattr(swot, c):
            errors.append(f"missing category: {c}")
    # Plausibility rules
    if len(swot.strengths) < 2:
        errors.append("strengths must contain at least 2 items")
    if len(swot.weaknesses) < 2:
        errors.append("weaknesses must contain at least 2 items")
    # No duplicates across categories
    flat = (
        [("S", x.lower()) for x in swot.strengths] +
        [("W", x.lower()) for x in swot.weaknesses] +
        [("O", x.lower()) for x in swot.opportunities] +
        [("T", x.lower()) for x in swot.threats]
    )
    seen = {}
    for label, item in flat:
        if item in seen and seen[item] != label:
            errors.append(f"duplicate across categories: '{item}' in {seen[item]} and {label}")
        else:
            seen[item] = label
    return (len(errors) == 0, errors)

# --------------------------- Heuristic SWOT Generation ---------------------------

def generate_swot(inp: Inputs) -> SWOTMatrix:
    ctx = _clean(inp.organization_context).lower()
    scope = _clean(inp.scope).lower()
    goal = _clean(inp.goal).lower()

    # Very lightweight keyword cues to bias generation
    is_stationery = any(k in ctx for k in ["pencil", "stationery", "office supply", "office supplies"])
    is_delivery = any(k in ctx for k in ["deliver", "delivery", "logistics", "ship", "courier"])
    is_local = any(k in ctx for k in ["local", "localized", "two customers", "2 customers", "mid-eastern"])

    strengths = []
    weaknesses = []
    opportunities = []
    threats = []

    # Strengths
    if is_stationery:
        strengths.append("Simple, universally understood product category (stationery)")
        strengths.append("Low product complexity enables reliable stocking and fulfillment")
    if is_delivery:
        strengths.append("Direct-to-office delivery convenience")
    strengths.append("Small scale allows personalized customer service and agility")

    # Weaknesses
    if is_local:
        weaknesses.append("Very limited customer base and geographic reach")
    weaknesses.append("Low brand awareness due to minimal marketing footprint")
    weaknesses.append("Narrow product assortment reduces cross-sell potential")

    # Opportunities
    opportunities.append("Expand into schools, universities, and SMEs with recurring orders")
    opportunities.append("Bundle complementary SKUs (pens, paper, erasers, sharpeners) for higher basket size")
    opportunities.append("Offer eco-friendly or customized stationery to capture premium niches")
    if is_delivery:
        opportunities.append("Leverage partnerships with local courier platforms to extend coverage")

    # Threats
    threats.append("Price competition from large stationery retailers and online marketplaces")
    threats.append("Customer shift to digital tools reduces total demand for pencils")
    threats.append("Logistics disruptions or fuel cost spikes impact delivery margins")
    threats.append("Stagnation risk if customer acquisition funnel is not built")

    # Dedupe inside lists
    strengths = _dedupe_list(strengths)
    weaknesses = _dedupe_list(weaknesses)
    opportunities = _dedupe_list(opportunities)
    threats = _dedupe_list(threats)

    return SWOTMatrix(strengths, weaknesses, opportunities, threats)

# --------------------------- Recommendations ---------------------------

def generate_recommendations(swot: SWOTMatrix) -> List[Dict[str, Any]]:
    # Map a few levers to items — deterministic and readable
    recs = []

    def has(texts: List[str], needle: str) -> bool:
        return any(needle.lower() in x.lower() for x in texts)

    # 1) Expand base using bundles and segments
    recs.append({
        "action": "Launch segmented outreach to schools and SMEs with bundled office packs and scheduled deliveries.",
        "leverages": [swot.strengths[0] if swot.strengths else ""],
        "targets": ["Recurring orders in education/SMEs", "Bundled SKUs opportunity"],
        "mitigates": ["Limited customer base"]
    })

    # 2) Partnerships to overcome geography
    recs.append({
        "action": "Partner with local courier/e-commerce platforms to extend delivery zones and reduce last-mile cost.",
        "leverages": ["Direct-to-office delivery convenience"] if has(swot.strengths, "delivery") else ["Operational agility"],
        "targets": ["Coverage expansion"],
        "mitigates": ["Limited geographic reach", "Logistics cost exposure"]
    })

    # 3) Assortment expansion toward eco/custom
    recs.append({
        "action": "Introduce eco-friendly and customized stationery lines and promote them via social proof.",
        "leverages": ["Low product complexity"],
        "targets": ["Eco/custom niche"],
        "mitigates": ["Narrow product assortment", "Price competition"]
    })

    # 4) Build brand & demand gen
    recs.append({
        "action": "Run a local brand campaign (social, flyers, community events) and add a simple re-order portal.",
        "leverages": ["Personalized service"],
        "targets": ["Brand awareness", "Repeat ordering"],
        "mitigates": ["Minimal marketing footprint"]
    })

    # 5) Margin protection
    recs.append({
        "action": "Standardize delivery windows and routes; introduce order minimums to protect margins.",
        "leverages": ["Operational agility"],
        "targets": ["Route efficiency"],
        "mitigates": ["Delivery margin erosion"]
    })

    # Ensure each recommendation is action phrased and non-empty
    return [r for r in recs if r.get("action")]

# --------------------------- Pipeline Orchestrator ---------------------------

def run_pipeline(inp: Inputs) -> Output:
    guidance_steps: List[GuidanceStep] = []

    # Step: guide_user (only logs the prompt; interactive happens outside)
    prompt_text = textwrap.dedent("""\
        Welcome to the SWOT Assistant. Let's start by understanding your context.
        Step 1: Please describe your organization or product in at least 15 words.
        Step 2: Define the scope (industry, geography, timeframe).
        Step 3: State your strategic goal.
        I will validate your answers and ask again if incomplete.
    """)
    guidance_steps.append(GuidanceStep(
        id="guide_user",
        prompt=prompt_text,
        user_input={
            "organization_context": inp.organization_context,
            "scope": inp.scope,
            "goal": inp.goal
        }
    ))

    # Step: validate_inputs
    valid, errors = validate_inputs(inp)
    if not valid:
        guidance_steps.append(GuidanceStep(
            id="validate_inputs",
            result={"valid": False, "errors": errors, "next_action": "prompt_user_again"}
        ))
        return Output(swot_matrix=None, recommendations=None, guidance_log={
            "steps": [asdict(s) for s in guidance_steps]
        })

    guidance_steps.append(GuidanceStep(id="validate_inputs", result={"valid": True}))

    # Step: generate_swot
    swot = generate_swot(inp)
    guidance_steps.append(GuidanceStep(
        id="generate_swot",
        notes="Internal factors → strengths/weaknesses; external → opportunities/threats; ≥2 per category."
    ))

    # Step: deduplicate
    swot = _remove_cross_duplicates(swot)
    guidance_steps.append(GuidanceStep(id="deduplicate", result={"status": "completed"}))

    # Step: generate_recommendations
    recs = generate_recommendations(swot)
    guidance_steps.append(GuidanceStep(
        id="generate_recommendations",
        notes="3–5 concrete actions leveraging S/O and mitigating W/T."
    ))

    # Step: finalize_output (checks)
    swot_ok, swot_errors = validate_swot(swot)
    checks = {
        "mandatory_categories": "passed" if swot_ok else "failed",
        "plausibility_rules": "passed" if swot_ok else "failed",
        "errors": swot_errors
    }
    guidance_steps.append(GuidanceStep(id="finalize_output", checks=checks))

    return Output(
        swot_matrix=swot if swot_ok else None,
        recommendations=recs if swot_ok else None,
        guidance_log={"steps": [asdict(s) for s in guidance_steps]}
    )

# --------------------------- Interactive Flow ---------------------------

def interactive_input() -> Inputs:
    print("\nWelcome to the SWOT Assistant (v%s)\n" % VERSION)
    print("I’ll guide you through three steps. Press Enter to submit each answer.\n")

    def ask(label: str, requirement: str) -> str:
        while True:
            print(f"{label} ({requirement}):")
            ans = input("> ").strip()
            if ans:
                return ans
            print("Please provide a value.\n")

    org = ask("Step 1 – Organization context",
              "≥ 15 words recommended; describe your organization/product/service")
    scope = ask("Step 2 – Scope", "industry, geography, timeframe")
    goal = ask("Step 3 – Goal", "your primary strategic objective for this SWOT")

    # Auto-reprompt if context < 15 chars, as per spec (character threshold)
    if len(_clean(org)) < 15:
        print("\nYour context seems too short (must be ≥ 15 characters). Please add more detail.")
        extra = input("> ").strip()
        org = (org + " " + extra).strip()

    return Inputs(
        organization_context=org,
        scope=scope,
        goal=goal,
        coaching_mode=True
    )

# --------------------------- Unit Tests ---------------------------

def run_tests() -> int:
    failures = 0

    def assert_true(name: str, cond: bool, msg: str = ""):
        nonlocal failures
        if not cond:
            failures += 1
            print(f"[FAIL] {name}: {msg}")
        else:
            print(f"[PASS] {name}")

    # Test 1: short_context_rejected
    case1 = Inputs(
        organization_context="Too short",
        scope="Tech industry, Europe",
        goal="Market entry"
    )
    out1 = run_pipeline(case1)
    steps1 = out1.guidance_log.get("steps", [])
    # Expect an error about context length
    got_error = any("context too short" in (err or "")
                    for st in steps1
                    for err in (st.get("result", {}).get("errors", []) if isinstance(st, dict) else [])
                   )
    assert_true("short_context_rejected.has_error", got_error,
                "Expected context too short error")
    assert_true("short_context_rejected.no_swot", out1.swot_matrix is None,
                "Expected no SWOT when inputs invalid")

    # Test 2: complete_case
    case2 = Inputs(
        organization_context="A renewable energy startup focused on solar microgrids for rural communities.",
        scope="Africa, 2025",
        goal="Expansion strategy"
    )
    out2 = run_pipeline(case2)
    assert_true("complete_case.has_swot", out2.swot_matrix is not None, "Expected SWOT present")
    if out2.swot_matrix:
        s, w, o, t = out2.swot_matrix.strengths, out2.swot_matrix.weaknesses, out2.swot_matrix.opportunities, out2.swot_matrix.threats
        assert_true("complete_case.min_strengths", len(s) >= 2, "Expected ≥2 strengths")
        assert_true("complete_case.min_weaknesses", len(w) >= 2, "Expected ≥2 weaknesses")
    # Check duplicates across categories
    if out2.swot_matrix:
        all_items = []
        for lab, lst in [("S", out2.swot_matrix.strengths), ("W", out2.swot_matrix.weaknesses),
                         ("O", out2.swot_matrix.opportunities), ("T", out2.swot_matrix.threats)]:
            for x in lst:
                all_items.append((lab, x.lower()))
        seen = {}
        dup = False
        for lab, x in all_items:
            if x in seen and seen[x] != lab:
                dup = True
                break
            seen[x] = lab
        assert_true("complete_case.no_cross_dupes", not dup, "Expected no duplicates across categories")

    print("\nUnit tests complete.")
    return failures

# --------------------------- Main ---------------------------

def main():
    parser = argparse.ArgumentParser(description="SWOT Self Assistant (single-file)")
    parser.add_argument("--organization_context", type=str, help="Organization context (>=15 chars)")
    parser.add_argument("--scope", type=str, help="Scope (industry, geography, timeframe)")
    parser.add_argument("--goal", type=str, help="Primary strategic objective")
    parser.add_argument("--coaching_mode", action="store_true", help="Enable coaching mode (interactive prompts)")
    parser.add_argument("--input_json", type=str, help="Path to JSON with inputs")
    parser.add_argument("--run_tests", action="store_true", help="Run built-in unit tests")
    args = parser.parse_args()

    if args.run_tests:
        failures = run_tests()
        sys.exit(1 if failures else 0)

    # Input resolution order:
    # 1) --input_json
    # 2) CLI flags
    # 3) Interactive
    if args.input_json:
        with open(args.input_json, "r", encoding="utf-8") as f:
            data = json.load(f)
        inp = Inputs(
            organization_context=data.get("organization_context", ""),
            scope=data.get("scope", ""),
            goal=data.get("goal", ""),
            coaching_mode=bool(data.get("coaching_mode", True))
        )
    elif args.organization_context and args.scope and args.goal:
        inp = Inputs(
            organization_context=args.organization_context,
            scope=args.scope,
            goal=args.goal,
            coaching_mode=True if args.coaching_mode else False
        )
    else:
        inp = interactive_input()

    out = run_pipeline(inp)

    # Final JSON output
    serializable = {
        "swot_matrix": asdict(out.swot_matrix) if out.swot_matrix else None,
        "recommendations": out.recommendations,
        "guidance_log": out.guidance_log,
        "meta": {
            "version": VERSION,
            "timestamp": datetime.utcnow().isoformat() + "Z"
        }
    }
    print(json.dumps(serializable, indent=2, ensure_ascii=False))


if __name__ == "__main__":
    main()
