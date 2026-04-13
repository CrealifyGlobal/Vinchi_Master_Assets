# Antigravity Agent Planning Playbook (Comprehensive Spec + Plan Framework)
Version: 1.0
Purpose: Provide a repeatable, high-signal practice for creating “good specs” that reliably guide AI agents to plan and execute work in Antigravity with minimal drift, rework, and risk.

---

## 0) Core Principles (Non-Negotiables)
1. **Clarity over volume**
   - Write what matters most: goals, constraints, success criteria, and boundaries.
   - Avoid narrative. Prefer bullets, tables, and explicit rules.

2. **Plan-first, execute-second**
   - The agent must produce and validate a plan before generating code or changing systems.
   - If the plan is unclear, the agent must revise the plan—not “wing it.”

3. **Modular context**
   - Break work into small, independently verifiable chunks.
   - Provide only the context needed for the current chunk.

4. **Guardrails + approvals**
   - Define “always-do” rules, “never-do” rules, and “ask-first” items.
   - Make risk visible and enforceable (tests, checks, gates).

5. **Spec is the source of truth**
   - The spec is a living document that is updated when reality changes.
   - Outputs must trace back to the spec.

---

## 1) Antigravity Spec-to-Work Lifecycle (Operational Loop)
This is the end-to-end routine you repeat for every project.

### Phase A — Brief (Human or Lead Agent)
**Outcome:** A short brief that captures intent and boundaries (1–2 pages max).
- Define:
  - Problem statement
  - Target users and jobs-to-be-done
  - Primary outcomes
  - Key constraints
  - Known risks and unknowns

**Rule:** If the brief can’t be written simply, the project is not ready for agents.

### Phase B — Draft Spec (Agent, Plan Mode)
**Outcome:** A structured spec the agent can follow without guessing.
- Convert brief → full spec using the **Spec Template** (Section 3).
- Identify ambiguities, list decisions needed, propose defaults.

### Phase C — Spec Review + Tightening (Human-in-the-loop)
**Outcome:** Spec is “plan-ready.”
- Validate:
  - Success criteria are measurable
  - Constraints are explicit
  - Non-goals are included
  - Dependencies and risks are listed
- Resolve ask-first items or explicitly defer them with decision dates.

### Phase D — Execution Plan (Agent, Plan Mode)
**Outcome:** A step-by-step plan mapped to spec sections and quality gates.
- Break into milestones, tasks, subtasks.
- For each task: inputs, outputs, checks, rollback.
- Confirm: “Definition of Done” for each milestone.

### Phase E — Work Packets (Agent execution, chunked)
**Outcome:** Implementation delivered in small batches.
- Each work packet:
  - Uses only relevant spec module(s)
  - Produces a testable artifact
  - Includes evidence (tests, logs, screenshots, diffs)

### Phase F — Verify + Iterate (Agent + Human)
**Outcome:** Confirm it works; update spec if needed.
- Run verification checklist.
- If spec mismatch:
  - Update spec
  - Update plan
  - Continue

---

## 2) Roles and Responsibilities (Lightweight RACI)
- **Human Sponsor (You)**
  - Owns: goals, priorities, final approvals
  - Decides: ask-first items
- **Lead Agent (Planner)**
  - Owns: spec drafting, planning, decomposition, risk surfacing
- **Worker Agent(s)**
  - Owns: implementing scoped work packets, tests, documentation
- **Reviewer Agent (Optional)**
  - Owns: adversarial review against spec (drift detection, missing checks)

---

## 3) The “Good Spec” Template (Agent-Readable)
> Use this template as a single document. Keep it updated. Link all outputs back to it.

### 3.1 Document Control
- Title:
- Owner:
- Date:
- Status: Draft / Reviewed / Approved / Superseded
- Change log:
  - YYYY-MM-DD: summary of change

### 3.2 One-Sentence Goal
- **Goal:** (single sentence)

### 3.3 Problem Statement (Plain Language)
- What is broken / missing today?
- Why does it matter?
- Who is impacted?

### 3.4 Users and Use Cases
- Primary users:
- Top 3 use cases (each with a short scenario):
  1.
  2.
  3.

### 3.5 Scope
**In scope**
- …

**Out of scope (Non-goals)**
- …
- (Explicitly list things the agent must not add “because it seemed useful.”)

### 3.6 Success Criteria (Measurable)
Define “done” in observable terms.
- Functional:
  - e.g., “User can X within Y steps”
- Quality:
  - performance thresholds, latency, memory, cost ceilings
- Reliability:
  - error rates, retry behavior, uptime assumptions
- Security & privacy:
  - access rules, data retention, PII constraints
- Operability:
  - monitoring, logs, runbooks, alerts

### 3.7 Constraints and Boundaries (Hard Rules)
- Tech constraints:
- Architectural constraints:
- Tooling constraints:
- Time/budget constraints:
- “No-go” rules:
  - e.g., “No new dependencies without approval”
  - “Do not store secrets in repo”
  - “Do not change production without a rollback plan”

### 3.8 Assumptions and Unknowns
- Assumptions (agent can proceed with):
- Unknowns (must clarify or mark as ask-first):
- Decision log:
  - Decision, date, rationale

### 3.9 System Context (Only What’s Necessary)
- High-level architecture diagram (optional)
- Key components involved:
- Data flows:
- External dependencies/APIs:
- Environments:
  - dev/staging/prod (or equivalents)
- Access and permissions model:

### 3.10 Functional Requirements (Numbered)
Use MUST/SHOULD/MAY language.
- FR-1 MUST …
- FR-2 MUST …
- FR-3 SHOULD …
- FR-4 MAY …

### 3.11 Non-Functional Requirements (Numbered)
- NFR-1 MUST …
- NFR-2 SHOULD …

### 3.12 UX / Interaction Requirements (If Relevant)
- Screens, flows, commands, API endpoints
- Copy rules (tone, brevity, error messages)
- Accessibility requirements (if applicable)

### 3.13 Data Requirements
- Data entities:
- Schema (if known):
- Validation rules:
- Retention:
- PII classification:
- Audit trails:

### 3.14 Error Handling & Edge Cases
- Expected failure modes:
- Retry policy:
- Timeouts:
- Idempotency rules:
- Partial failure behavior:

### 3.15 Testing Strategy (Definition of Evidence)
- Unit tests:
- Integration tests:
- End-to-end tests:
- Contract tests (if APIs):
- Performance tests:
- Security checks:
- Acceptance tests mapped to use cases:

### 3.16 Observability & Operations
- Logs:
- Metrics:
- Traces:
- Alerting thresholds:
- Runbook links/steps:
- On-call assumptions (if any):

### 3.17 Delivery & Deployment
- Release strategy:
- Feature flags (if used):
- Migration steps:
- Rollback plan:
- Backwards compatibility requirements:

### 3.18 Project Conventions (Agent Instructions)
- Repo structure expectations:
- Coding style:
- Naming conventions:
- Commands to run:
  - Build:
  - Test:
  - Lint:
- Documentation expectations:
- Git workflow:
  - branch naming, commit message rules, PR checklist

### 3.19 Guardrails (Explicit Agent Policies)
**Always-do**
- (e.g., update tests, keep changes small, cite spec sections)

**Never-do**
- (e.g., never commit secrets, never deploy without tests)

**Ask-first**
- (e.g., adding dependencies, altering data models, changing public APIs)

### 3.20 Open Questions (Blockers)
- Q1:
- Q2:
- Owner:
- Due date:

---

## 4) Execution Planning Template (Agent Plan Mode Output)
> The plan must be a structured artifact. It must be traceable to the spec.

### 4.1 Milestone Map
For each milestone:
- Milestone ID:
- Purpose:
- Spec coverage (list relevant sections / requirement IDs):
- Deliverables (artifacts):
- Verification (how we prove it’s done):
- Risk level:
- Rollback / recovery strategy:

### 4.2 Task Decomposition (Per Milestone)
For each task:
- Task ID:
- Goal:
- Inputs:
- Steps:
- Outputs:
- Checks:
- Failure modes:
- Rollback:

### 4.3 Work Packet Sizing Rules (Keep Agents Focused)
A work packet should:
- Touch a small number of files/components
- Produce one testable increment
- Include tests and evidence
- Take the shortest path consistent with the spec

If a task becomes large:
- Split by interface boundary, module boundary, or test boundary.

### 4.4 Plan Quality Checklist (Agent Self-Check)
Before execution:
- [ ] Every task maps to at least one spec requirement
- [ ] Every milestone has verification steps
- [ ] Unknowns are either resolved or marked ask-first
- [ ] Risks have mitigations
- [ ] Rollback exists for changes with blast radius
- [ ] “Never-do” rules acknowledged

---

## 5) Context Modularity: How to Feed Agents Without Overload
### 5.1 Spec Modules (Recommended Breakdown)
Split the spec into modules that can be supplied selectively:
- Module A: Goal, scope, success criteria, constraints
- Module B: Functional requirements
- Module C: Data + integrations
- Module D: Testing + operations
- Module E: Delivery + conventions + guardrails

### 5.2 Context Delivery Protocol
When assigning work:
1. Provide:
   - the relevant spec module(s)
   - the current milestone + tasks
   - relevant repo/system snippets only
2. Do NOT provide:
   - entire project history
   - unrelated modules
3. Require:
   - agent to restate their task + success criteria in 3–5 bullets
   - agent to list what they will not do

### 5.3 “Spec Index” (So Agents Can Navigate)
Maintain an index at top of the spec:
- Requirements list (FR-#, NFR-#)
- Where decisions are logged
- Where testing strategy lives
- Where guardrails live

---

## 6) Guardrail System (Make Safety and Quality Enforceable)
### 6.1 Three-Tier Policy Model
**Tier 1: Hard bans**
- Must never happen (security, data loss, irreversible actions).

**Tier 2: Approval gates**
- Allowed only after explicit approval.

**Tier 3: Autonomous**
- Agent can do freely within scope.

### 6.2 Typical “Ask-First” Items (Good Defaults)
- Adding or upgrading dependencies
- Changing data schemas
- Changing public API behavior
- Expanding scope beyond spec
- Touching production resources
- Handling of PII or secrets
- Performance tradeoffs that increase cost

### 6.3 Drift Detection
Require the agent to include in every output:
- “Spec alignment” section:
  - Which FR/NFR did this satisfy?
- “Out-of-scope check” section:
  - Confirm nothing outside scope was added.

---

## 7) Quality Gates (Definition of Done That Agents Can Execute)
### Gate 1 — Spec Ready
- [ ] Goals and success criteria are measurable
- [ ] Scope and non-goals are explicit
- [ ] Constraints and guardrails are explicit
- [ ] Unknowns captured; ask-first list exists

### Gate 2 — Plan Ready
- [ ] Milestones mapped to spec requirements
- [ ] Verification steps exist per milestone
- [ ] Rollback exists for risky changes
- [ ] Work packets are sized appropriately

### Gate 3 — Build Ready
- [ ] Code compiles/builds
- [ ] Linting passes (if applicable)
- [ ] Tests added/updated

### Gate 4 — Acceptance Ready
- [ ] Acceptance tests mapped to use cases pass
- [ ] Observability hooks exist (logs/metrics)
- [ ] Documentation updated

### Gate 5 — Release Ready
- [ ] Deployment plan exists
- [ ] Rollback plan validated
- [ ] Risk review completed

---

## 8) Agent Prompt Packs (Reusable Instruction Blocks)
> Use these “packs” inside Antigravity to standardize agent behavior.

### 8.1 Planner Agent Pack (Plan Mode)
- You are the planning agent.
- Do not produce code.
- Produce:
  1) spec improvements (if needed),
  2) a milestone plan,
  3) task breakdown with verification steps,
  4) list of ask-first items.
- If anything is ambiguous, propose 1–3 options with tradeoffs and default recommendation.

### 8.2 Worker Agent Pack (Execution Mode)
- You are executing one work packet only.
- You must:
  - restate the task in your own words
  - list relevant spec requirement IDs
  - list tests/evidence you will provide
  - implement minimal changes
  - produce verification output
- You must not:
  - expand scope
  - introduce new dependencies without approval
  - skip tests unless explicitly allowed

### 8.3 Reviewer Agent Pack (Adversarial Review)
- You are reviewing against the spec.
- Find:
  - scope creep
  - missing tests
  - unhandled edge cases
  - violations of guardrails
  - unclear acceptance criteria
- Output:
  - a punch-list prioritized by risk

---

## 9) Evidence and Reporting (What “Good Output” Looks Like)
Every agent deliverable should include:
- Summary (what changed)
- Spec alignment (FR/NFR references)
- Evidence:
  - test results, logs, screenshots, benchmarks
- Risks introduced (if any)
- Rollback notes (if applicable)
- Follow-ups / TODOs (explicitly listed)

---

## 10) “Living Spec” Maintenance Rules
- If execution reveals missing requirements:
  - Update spec first, then update plan.
- Keep a decision log.
- Keep a change log.
- Mark deprecated sections as “Superseded” rather than silently editing history.

---

## 11) Practical Default: Minimal Spec Starter (When You’re in a Hurry)
If you only have 15 minutes, write:
1. Goal (one sentence)
2. Scope / Non-goals (bullets)
3. Success criteria (3–7 measurable bullets)
4. Constraints + guardrails (bullets)
5. Top 3 use cases (short scenarios)
6. Ask-first list (bullets)
Then have the planner agent expand into the full spec template.

---

## 12) Quick Reference: Agent Readiness Checklist
Before you deploy agents:
- [ ] Spec exists and is structured
- [ ] Success criteria are measurable
- [ ] Guardrails are explicit (always/never/ask-first)
- [ ] Plan exists with milestones and verification
- [ ] Work is decomposed into small packets
- [ ] Evidence expectations are defined
- [ ] Spec modules are prepared for selective context delivery

---

## 13) Optional Enhancements (If Antigravity Supports It)
- Automated checks:
  - CI that enforces tests + lint + secret scanning
- Spec linting:
  - “No unmeasurable success criteria”
  - “Non-goals must exist”
- Traceability:
  - Require commit/PR references to FR/NFR IDs

---

## 14) Example Output Format (For Agents)
When responding, agents should use:
1. Restatement
2. Requirements mapped
3. Plan / steps
4. Implementation (if allowed)
5. Verification and evidence
6. Risks and rollbacks
7. Drift check (“what I did NOT do”)

---

End of Playbook
```0