# Update Procedure Protocol
## Companion File — Superyacht Construction Scope Agent

---

## WHEN TO TRIGGER AN UPDATE

| Trigger | Target File |
|--------|-------------|
| New WP used that doesn't exist in library | WORK_PACKAGE_LIBRARY.md |
| Baseline hours revised from new actuals | WORK_PACKAGE_LIBRARY.md |
| New location identified for existing WP | WORK_PACKAGE_LIBRARY.md |
| Completed build provides actual split data | LEAD_TIME_EXPERIENCE.md |
| New split pattern observed vs. anchor | LEAD_TIME_EXPERIENCE.md |
| New adjustment trigger identified | LEAD_TIME_EXPERIENCE.md |
| Agent workflow needs refinement | AGENT_CONFIG.md |

**Never update silently. Always show a draft and get approval first.**

---

## UPDATE MODES

**MODE 1 — DRAFT MODE (Active by default)**
Agent generates exact update text + changelog entry. User copies into GitHub manually.

**MODE 2 — GITHUB WRITE MODE (Activate when ready)**
Agent writes directly to repo via GitHub API after user approval.

---

## PROCEDURE

### STEP 1 — Identify
State which file, what is changing, and why.

### STEP 2 — Draft the File Update
Produce the exact text to add/change, formatted to match the existing file structure.

```
📝 DRAFT UPDATE — LEAD_TIME_EXPERIENCE.md
Section: WP-01 | Bilge System
Action: Add new historical observation row

ADD to table:
| Vacuum system, 3-zone | 28% | 48% | 14% | 10% | 1 |

ADD to Adjustment Triggers:
- 3-zone vacuum with crew bilge → 28/48/14/10 split
```

### STEP 3 — Draft the Changelog Entry

```
📝 DRAFT UPDATE — CHANGELOG.md

## [Date] — [Vessel / Session Reference]
### Changed
- LEAD_TIME_EXPERIENCE.md: Added WP-01 observation from [Project]
### Reason
- Actuals differed from anchor; pattern recorded for future use
```

### STEP 4 — Request Approval
> "I've drafted the above update. Commit, adjust, or discard?"

### STEP 5 — Write or Hand Off

**Draft Mode:**
```
✅ READY TO COMMIT

Copy into [filename]:
[exact text]

Add to CHANGELOG.md:
[changelog entry]
```

**GitHub Write Mode:** Fetch file SHA → apply update → write back → write changelog entry.
Commit message: `[Agent] Update WP-01 experience data — [Vessel Name]`

---

## WHAT MAY AND MAY NOT BE UPDATED

**✅ With approval:** New WP entries, new split observations, new triggers, changelog.

**⚠️ With explicit instruction only:** Existing baselines (needs 3+ actuals), existing anchors, agent workflow.

**❌ Never:** Update silently. Change minimum data rules. Modify this file without a deliberate session.

---

## GITHUB WRITE MODE — CONFIGURATION

```
GITHUB_WRITE_MODE: false
GITHUB_REPO: your-username/yacht-scope-agent
GITHUB_BRANCH: main
GITHUB_TOKEN: [paste token here — keep private]
```

**To generate a token:**
1. GitHub → Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens
2. Scope to this repo only
3. Permission: Contents — Read and Write
4. Expiry: 90 days recommended

---

## CHANGELOG FORMAT

```markdown
## YYYY-MM-DD — [Project / "General"]
### Added
- [File]: [what and why]
### Changed
- [File]: [old → new]
### Reason
- [one sentence]
```

---

*Update Procedure Version: 1.0 | Mode: Draft (active) / GitHub Write (inactive)*
