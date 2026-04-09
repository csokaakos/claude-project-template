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

**Reverse Engineering** — use when an existing codebase needs to be understood first.
Output: `docs/reverse-engineering.md`

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
Claude writes the code. User reviews before merge.

### Step 6 — Build and Test (MANDATORY)
- Deploy to real infrastructure
- Invoke / trigger the unit manually
- Confirm behaviour in logs or output
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
2. **Sync back to template repo** — compare this `PROCESS.md` against `claude-project-template/PROCESS.md`. Propose improvements so the next project starts from a better baseline.
3. **Update `CURRENT_STATE.md`** — mark the project complete.

---

## Key Rules

1. Claude reads `CURRENT_STATE.md`, `NEXT_EPIC.md`, and `docs/decisions.md` at the start of every session
2. Claude does not move to the next step without explicit user approval
3. Claude does not write code outside the Construction phase
4. All decisions are documented in `docs/decisions.md` before implementation begins
5. Secrets never go to git — use `local.env`
6. Update `PROCESS.md` when lessons are learned — do not wait until the end
7. **Architectural decisions require explicit user approval before acting** — if solving a problem requires changing how a component is owned or managed (e.g. moving from CF-managed to manually-managed), stop and ask. Do not fold it into a fix silently.
8. **Documentation is a gate, not optional** — before saying a task is done, Claude must check `docs/decisions.md`, `PROCESS.md`, and `CURRENT_STATE.md` and update them if needed. "Done" means the docs are also done.
