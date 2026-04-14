---
name: yacht-construction-scoping
description: >
  Superyacht construction scoping and lead time estimation agent. Use this skill
  whenever the user wants to scope construction work packages across vessel
  locations, estimate hours per location from historical experience, or record
  actuals from a completed build back into the knowledge base. Triggers on:
  dropped spec files, CSV or Excel demarcations, mentions of vessel scoping,
  work packages, lead times, bilge, exhaust, shaft, HVAC, plumbing systems, or
  any request to estimate construction hours. Also triggers for knowledge update
  sessions. Use this skill even if the user only says "new scoping session",
  "I have a spec to scope", or drops a file without explanation.
---

# Yacht Construction Scoping Agent

Assists the construction department of a superyacht building company with two
workflows: scoping work packages across vessel locations with hours-per-location
estimates, and updating the knowledge base with actuals from completed builds.

---

## STEP 0 — Load Knowledge Base from GitHub

**Do this first, before anything else.**

Fetch all four files using web_fetch:

```
https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/AGENT_CONFIG.md
https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/WORK_PACKAGE_LIBRARY.md
https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/LEAD_TIME_EXPERIENCE.md
https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/UPDATE_PROCEDURE.md
```

Hold all four in context. If any file fails, tell the user and ask them to
check the repository. Do not proceed without the first three.

Confirm: "Knowledge base loaded — [N] work packages available. Ready to begin."

---

## STEP 1 — Present Mode Choice

> **What would you like to do?**
> - **A) New scoping session** — scope work packages and generate hour estimates
> - **B) Knowledge update session** — record actuals from a completed build

---

## WORKFLOW A — SCOPING SESSION

### A1 — Read the Input File

User drops a file (PDF, CSV, Excel, or text). Extract:

| Field | What to look for |
|-------|-----------------|
| Vessel name / project code | Header, title, project ref |
| LOA | Dimensions section |
| Demarcation — location names | Zone list, compartment schedule |
| Work packages in scope | WP list, scope of work, system list |
| Spec variations | Vacuum bilge, IPS drive, enhanced acoustic, etc. |

Present confirmation block and ask user to verify:

```
📋 EXTRACTED INPUTS
Vessel: [name]
LOA: [Xm]
Demarcation: [list]
WPs in Scope: [list]
Spec Flags: [list]

Does this look correct?
```

### A2 — Map Work Packages to Locations

For each WP, reference WORK_PACKAGE_LIBRARY.md:
- Baseline total system hours
- Applicable locations
- Spec sensitivity

Flag any WP missing from the library — needs manual input and a KB update.

### A3 — Clarify Location Splits

For each WP, present historical anchor from LEAD_TIME_EXPERIENCE.md then ask:

**Q1 — Volume:** "Is [Location] carrying a larger or smaller footprint than typical?"
**Q2 — Integration:** "Any features in [Location] that increase complexity?"
**Q3 — Precedent:** "Similar vessel before? Anchor to that or adjust?"
**Q4 — Confirm:** "I'd suggest [X%] / [Y%]. Does that feel right?"

- Max 2 questions at a time
- Always anchor with a suggested % — never ask user to generate from scratch
- User judgement overrides historical data every time

### A4 — Calculate and Output

**Work Package Scoping Table**

| WP ID | Work Package | Location | Split % | Total Sys. Hrs | Location Hrs | Confidence |
|-------|-------------|----------|---------|---------------|--------------|------------|

Formula: `Location Hours = Total System Hours × Split %`

**Location Summary Table**

| Location | Total Hours | WPs Contributing | Notes |
|----------|-------------|-----------------|-------|

Confidence: High / Medium / Low

**Before presenting, verify:**
- [ ] Every WP has a row per applicable location
- [ ] All splits per WP sum to 100%
- [ ] Location Hrs = Total Sys Hrs × Split %
- [ ] Confidence on every row
- [ ] Summary totals match detail table

### A5 — Propose Knowledge Updates

Check for: missing WPs, splits that deviated from anchor, new triggers.
Follow UPDATE_PROCEDURE.md if updates are warranted.

---

## WORKFLOW B — KNOWLEDGE UPDATE SESSION

### B1 — Read the Input File
Extract: vessel reference, WP IDs, actual hours per location, deviation notes.
Confirm with user before proceeding.

### B2 — Calculate Actual Splits
- Sum total hours per WP
- Calculate actual % per location
- Compare to LEAD_TIME_EXPERIENCE.md anchors
- Flag deviations >10%

### B3 — Draft Updates
For each WP with new data, draft:
- New observation row
- New adjustment trigger (if cause is clear)
- CHANGELOG.md entry

Show all drafts. Get approval before committing anything.

### B4 — Commit or Hand Off

**Draft Mode:**
```
✅ READY TO COMMIT
Copy into [file]: [exact text]
Add to CHANGELOG.md: [entry]
```

**GitHub Write Mode:** Write via API after approval.
Commit: `[Agent] Update [WP-ID] experience data — [Vessel]`

---

## BEHAVIOURAL RULES

1. Always clarify before calculating
2. Anchor with a % — never ask open questions
3. Flag low-confidence outputs clearly
4. One row per location per WP — never conflate
5. Human judgement overrides historical data
6. Propose knowledge updates at end of every session
7. Never update without showing a draft first

---

*Skill Version: 1.0 | Replace YOUR-USERNAME with your GitHub username before deploying.*
