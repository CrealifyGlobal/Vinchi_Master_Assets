# Superyacht Construction Scope & Lead Time Agent

A self-contained AI agent for the construction department. Drop into any LLM — Claude, ChatGPT, Gemini — to scope work packages and estimate hours per location based on historical experience.

---

## File Structure

```
yacht-scope-agent/
├── README.md                     ← This file
├── AGENT_CONFIG.md               ← Agent brain — use as system prompt
├── WORK_PACKAGE_LIBRARY.md       ← WP definitions and baseline hours
├── LEAD_TIME_EXPERIENCE.md       ← Historical split patterns
├── UPDATE_PROCEDURE.md           ← How to draft and commit knowledge updates
├── CHANGELOG.md                  ← Record of all changes
└── cowork-skill/
    ├── SKILL.md                  ← Self-contained Claude skill (single file)
    └── skill-config.json         ← Skill metadata
```

---

## How to Deploy

### Option A — Claude Project
1. Create a new Claude Project
2. Paste `AGENT_CONFIG.md` into Project Instructions
3. Upload `WORK_PACKAGE_LIBRARY.md`, `LEAD_TIME_EXPERIENCE.md`, `UPDATE_PROCEDURE.md` as knowledge files
4. Use Invocation Prompt 1 below

### Option B — Claude Skill (Single File)
1. Go to Claude Project → Skills → Add Skill
2. Upload `cowork-skill/SKILL.md`
3. Replace `YOUR-USERNAME` in the file with your GitHub username first
4. The skill reads knowledge files live from GitHub every session

### Option C — GitHub + Any LLM
1. Push all files to a public GitHub repo
2. Use Invocation Prompt 2 below in any AI tool

---

## Invocation Prompts

### Prompt 1 — Standard (files already uploaded)

```
You are the Superyacht Construction Scope & Lead Time Agent as defined in
AGENT_CONFIG.md. Your knowledge base is in WORK_PACKAGE_LIBRARY.md,
LEAD_TIME_EXPERIENCE.md, and UPDATE_PROCEDURE.md.

Begin by confirming you have access to these files, then ask me for:
- Vessel name and project code
- Demarcation list (location names)
- Work packages in scope
- Any known spec variations
```

### Prompt 2 — GitHub (files hosted in repo)

```
You are a Construction Scoping Agent for a superyacht building company.

Start by reading these files in order:
1. https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/AGENT_CONFIG.md
2. https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/WORK_PACKAGE_LIBRARY.md
3. https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/LEAD_TIME_EXPERIENCE.md
4. https://raw.githubusercontent.com/YOUR-USERNAME/yacht-scope-agent/main/UPDATE_PROCEDURE.md

Once loaded, confirm what you have read, then follow the workflow in AGENT_CONFIG.md.
Ask me for vessel name, demarcation, and work packages in scope.
```

> Replace `YOUR-USERNAME` with your actual GitHub username.

### Prompt 3 — Quick Session (experienced users, Claude Project)

```
New scoping session.
Vessel: [name]
Demarcation: [list locations]
WPs in scope: [list WP IDs]
```

### Prompt 4 — Knowledge Update Session

```
Knowledge update session — not a scoping session.
I have actuals from a completed build to record.
Load LEAD_TIME_EXPERIENCE.md and UPDATE_PROCEDURE.md,
then ask me which work packages have new actuals.
```

---

## Using the Cowork Skill

The `cowork-skill/SKILL.md` is a single self-contained file that works as a
Claude Project skill. It reads all knowledge files live from GitHub at the
start of every session — no uploads needed.

**Before installing:** Open `cowork-skill/SKILL.md` and replace `YOUR-USERNAME`
with your actual GitHub username in the `REPO_BASE` line.

**To install:** Claude Project → Skills → Add Skill → select `SKILL.md`

---

## Maintaining the Knowledge Base

After every completed build, run a Knowledge Update Session (Prompt 4).
The agent drafts updates → you approve → paste into GitHub (Draft Mode)
or the agent commits directly (GitHub Write Mode — see UPDATE_PROCEDURE.md).

**Minimum data rule:** Never change a baseline or split anchor without
at least 3 completed builds supporting the change.

---

## What the Agent Does NOT Do
- Schedule work or assign dates
- Allocate resources or headcount
- Override your team's domain judgement

---

*Version: 1.0 | Released: 2026-04-08 | Unit: Hours per Location*
