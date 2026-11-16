# Process Guide

This is the working process guide for this project.
Update it during the project when lessons are learned — do not wait until the end.

> Based on: publicpages-sync project (first use). Refined version lives in `claude-project-template`.

---

## Two-Repository Model

Every AI-assisted project involves two repositories:

| Repository | Purpose |
|---|---|
| `claude-project-template` | Master template. Always up to date. Copied into new projects. Updated at project end. |
| This repository | The actual project. Has its own `PROCESS.md` that evolves independently. |

At the end of this project, review what changed in this file and feed improvements back to the template repo.

---

## Phase 1: Inception

### Step 1 — Workspace Detection (MANDATORY)
**Goal:** Confirm Claude has everything it needs to work.

Checklist:
- [ ] Working directory accessible (read + write)
- [ ] Git initialised and connected to remote
- [ ] Credentials / secrets in place (never committed to git)
- [ ] `.gitignore` covers all secret files
- [ ] Claude can commit and push
- [ ] GitHub CLI (`gh`) installed and logged in — needed to open pull requests

Output: Confirmed workspace. `CURRENT_STATE.md` created.

---

### Step 2 — Requirements Analysis (MANDATORY)
**Goal:** Understand what we are building and why.

Claude asks questions across these areas:
- What exists already vs. what are we building?
- Who are the users and what is the threat model?
- What are the functional requirements?
- What are the non-functional requirements (cost, performance, security)?
- What integrations are needed?
- What are the alerting / notification needs?
- What operational modes exist?

Output: `docs/requirements.md` — full requirements reference.
Also create `docs/decisions.md` at this point — empty log, ready to populate.

---

### Step 3 — Workflow Planning (MANDATORY)
**Goal:** Agree on tools, services, and overall approach BEFORE designing anything.

Topics:
- Infrastructure-as-code tool (Terraform / CloudFormation / CDK / scripts)
- Compute approach (serverless / containers / VMs)
- Storage approach
- Deployment approach
- Cost estimate
- Domain / DNS setup
- Testing approach — which parts have real logic worth automated tests (see Light Testing Rule)
- CI — what the GitHub Actions check runs on every pull request (tests, or a basic build check if there are no tests)

Output: `docs/workflow-planning.md` — confirmed tooling and approach.

---

### Step 4 — Application Design (CONDITIONAL)
Use when: building a new application from scratch.
Skip when: extending or modifying an existing system.

**Goal:** Define the application structure before writing code.

Topics:
- Component breakdown
- Data models
- API design (if applicable)
- State management

**If the project includes a dashboard or any user-facing UI, design it here — before units are planned.**
The UI determines what data must be collected and stored. Units that produce data must be designed
to serve the UI's needs. Designing the UI after data-producing units are built causes rework.

UI design process:
1. Agree on layout using ASCII wireframes — useful for structure, not sufficient when colour or visual hierarchy matters
2. If colour or cell-level status is important, go directly to a static HTML prototype
3. Iterate on the prototype (open in browser, give feedback) until approved
4. Only then design the data model and units

Output: `docs/application-design.md` + approved UI prototype in `docs/`

---

### Step 5 — Units Generation (CONDITIONAL)
Use when: the application is large enough to split into independent deliverable units.

**Goal:** Break the work into buildable chunks with clear interfaces.

Output: `docs/units.md` — list of units with input/output definitions.

---

### Step 6 — NEXT_EPIC.md (RECOMMENDED)
**Goal:** Capture requirements for the next iteration before starting construction.
Prevents scope creep while ensuring upcoming needs are considered in current design decisions.

`NEXT_EPIC.md` is read at every session start. Claude uses it silently to avoid design decisions
that would cause rework later — or flags it explicitly if a current decision conflicts.

Contents:
- Functional features planned for the next epic
- Non-functional requirements anticipated
- Any workflow or collaboration changes to trial

This file is NOT acted on during the current epic — reference only.

---

### Supporting Steps (CONDITIONAL)

### Reverse Engineering (CONDITIONAL)
Use when: brownfield project and no existing reverse engineering artifacts.
Skip when: greenfield, or artifacts already exist and are current.

**Goal:** Build a semantically rich picture of the existing system before touching anything.

Produce `docs/reverse-engineering/` with:
- `business-overview.md` — what the system does, key business transactions end-to-end
- `architecture.md` — system components, data flows, deployment topology
- `code-structure.md` — package layout, design patterns, notable conventions
- `api.md` — REST/internal APIs, data models (skip if not applicable)
- `tech-stack.md` — languages, frameworks, key libraries, build tools
- `dependencies.md` — internal and external dependency map
- `code-quality.md` — test coverage, known debt, linting state

**Wait for explicit approval** before proceeding to Requirements Analysis.

**User Stories** — use when user-facing behaviour needs to be explicitly defined.
Output: `docs/user-stories.md`

---

## Phase 2: Construction

Repeat for each unit.

Steps 1–4 are CONDITIONAL — skip any that are not needed. If the scope of a unit is already clear
from Inception, go directly to Code Generation. Do not add process for its own sake.

### Step 1 — Functional Design (CONDITIONAL)
Use when: the unit's behaviour is not yet fully defined.
Define exactly what the unit does — inputs, outputs, logic, edge cases.

### Step 2 — NFR Requirements (CONDITIONAL)
Use when: the unit has specific non-functional concerns not covered in Inception.
Topics: performance, security, cost, resilience.

### Step 3 — NFR Design (CONDITIONAL)
Use when: NFRs require a technical design decision.
How the NFRs are implemented technically.

### Step 4 — Infrastructure Design (CONDITIONAL)
Use when: the unit introduces new cloud resources not already designed.

### Step 5 — Code Generation (MANDATORY)
1. **Plan first** — write the plan to `docs/plan.md` and present it. **Wait for explicit approval before writing any code.** (Industry term: "plan mode".)
2. **Branch** — create a branch for the unit (see Branches and Pull Requests).
3. **Write the code** — tick each plan step `[x]` immediately on completion.
4. **Tests** — add automated tests where the Light Testing Rule says so.
5. **AI review** — run a code review (`/code-review`) on the branch before the user sees it. Fix real findings; list anything left open in the pull request.
6. **Pull request** — push the branch and open a PR. The user reviews it in Step 6.

### Step 6 — Build and Test (MANDATORY)
- CI check on the pull request is green (automated tests pass)
- Deploy to real infrastructure
- Invoke / trigger the unit manually
- Confirm behaviour in logs or output
- User reviews the pull request and approves → merge to `master`
- User approves before marking unit complete

---

## Phase 3: Operations

- Monitoring in place
- Runbook documented (`docs/runbook.md`)
- Rollback procedure defined
- Handover complete

### Wrap-up (MANDATORY)
Before closing the project:

1. **Update this file** — review what was followed, what was skipped, what was added. Update `PROCESS.md` to reflect what was actually useful.
2. **Review `docs/template-candidates.md` with the user** — go through each open entry and ask: sync to the template / keep project-only / drop. Mark each entry with the decision.
3. **Sync back to template repo** — apply the approved candidates, then compare this `PROCESS.md` against `claude-project-template/PROCESS.md`. Propose any further improvements so the next project starts from a better baseline.
4. **Update `CURRENT_STATE.md`** — mark the project complete.

The same template-candidates review also runs at the end of each epic (before work on `NEXT_EPIC.md` starts), so candidates do not pile up across long projects.

---

## Branches and Pull Requests

Every change reaches `master` through a pull request (PR) the user has approved. This is how professional teams work, AI-assisted or not.

**Rules:**
- One branch per unit or per self-contained change: `unit/<name>` or `fix/<short-description>`
- Claude commits to the branch, pushes it, and opens the PR with `gh pr create`
- PR description contains: what changed and why, how it was tested, AI review findings still open (if any)
- The user reviews the PR on GitHub (summary + "Files changed" tab) and approves in chat or on GitHub
- Merge only after user approval and a green CI check. Claude merges (`gh pr merge`) unless the user prefers to click Merge themselves
- After merge: delete the branch, pull `master`
- On session start: check for open PRs and unmerged branches (`gh pr list`) — they are unfinished work

---

## CI (Continuous Integration)

A GitHub Actions workflow (`.github/workflows/ci.yml`) runs automatically on every pull request.

- Set up together with the first unit that has code
- Runs the automated tests; if the project has no tests, runs a basic build / syntax check instead
- A red check blocks the merge — fix it before asking for approval

---

## Light Testing Rule

Automated tests are written where they pay off — not for coverage numbers.

| Write tests for | Skip tests for |
|---|---|
| Real logic: parsing, calculations, comparisons, decisions, data transformation | Pure infrastructure glue (wiring cloud services together) — the real-deploy check in Build and Test covers it better |
| Anything likely to be changed again later (guards against regressions — old behaviour breaking silently) | Visual / UI look and layout |

- Tests run with one command, documented in the project README
- The real-deploy check in Build and Test stays mandatory regardless of tests

---

## Corrections Become Rules

When the user corrects Claude in a way that would apply again (a convention, a preference, a way of working — not a one-off fix like a typo), Claude asks:

> **Keep this as a rule?**
> **(a)** This project only
> **(b)** Also for the template
> **(c)** No

| Answer | What Claude does |
|---|---|
| (a) | Adds the rule to this project's `CLAUDE.md` |
| (b) | Adds the rule to this project's `CLAUDE.md` **and** appends it to `docs/template-candidates.md` |
| (c) | Nothing |

Each correction is asked about once. The question and answer are logged in `docs/audit.md`.
Template candidates are reviewed with the user at the end of each epic and at project wrap-up (see Wrap-up).

---

## Plan Tracking

File: `docs/plan.md`

**Purpose**: When a stage has multiple sub-steps, write the full plan to a file before starting any work. Check off each step immediately on completion. If the session drops mid-stage, the plan file is the recovery point — resume from the first unchecked item rather than restarting.

**When to use**: Any Construction stage with more than two sub-steps. Always for Code Generation.

**Rules:**
- Write `docs/plan.md` before starting work — not after
- Present the plan and get explicit user approval before starting work
- Mark each step `[x]` in the same interaction it is completed — no exceptions, no batching
- On session start: if `docs/plan.md` exists with unchecked items, continue from there
- Replace `docs/plan.md` when a new stage begins

**Format:**

```
# Plan: [Stage Name] — [Unit if applicable]
**Stage**: [Phase] / [Stage]
**Started**: YYYY-MM-DDTHH:MM:SSZ

## Steps
- [ ] Step one
- [ ] Step two
- [ ] Step three
```

---

## Audit Log

File: `docs/audit.md`

**Purpose**: Permanent, append-only record of sessions — what was asked, what was decided, what was approved. Enables any future session to reconstruct context without relying on memory.

**Rules:**
- Always **append**. Never overwrite. Use Edit to add entries, never Write on the whole file.
- Capture user input **verbatim** — never summarise or paraphrase.
- Use ISO 8601 timestamps (`YYYY-MM-DDTHH:MM:SSZ`).

**Log these events:**

| Event | When |
|---|---|
| Session start | After reading state — include verbatim opening message and current phase |
| Stage gate | Every user approval or rejection — verbatim response |
| Major decision | When anything is added to `docs/decisions.md` |
| Session end | When wrapping up — what was completed |

**Entry format:**

```
### [Event type] — [short label]
**Timestamp**: YYYY-MM-DDTHH:MM:SSZ
**User input**: "[complete raw input]"
**Action**: [what Claude did or decided]
**Context**: [phase and stage]
```

---

## Key Rules

1. Claude reads `CURRENT_STATE.md`, `NEXT_EPIC.md`, and `docs/decisions.md` at the start of every session
2. Claude does not move to the next step without explicit user approval
3. Claude does not write code outside the Construction phase
4. All decisions are documented in `docs/decisions.md` before implementation begins
5. Secrets never go to git — use `local.env`
6. Update `PROCESS.md` when lessons are learned — do not wait until the end
7. **Architectural decisions require explicit user approval before acting** — if solving a problem requires changing how a component is owned or managed (e.g. moving from CF-managed to manually-managed), stop and ask. Do not fold it into a fix silently.
8. **Documentation is a gate, not optional** — before saying a task is done, Claude must check `docs/decisions.md`, `PROCESS.md`, `CURRENT_STATE.md`, and `docs/audit.md` and update them if needed. "Done" means the docs are also done.
9. **Plan before code** — no code is written until the user has approved the plan in `docs/plan.md`.
10. **Everything goes through a pull request** — no direct commits to `master` for project work. Merge only after user approval and a green CI check.
11. **Corrections become rules** — when corrected, ask "keep as a rule: this project / also template / no" (see Corrections Become Rules).
