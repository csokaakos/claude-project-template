# Working Agreement

This file is read by Claude at the start of every session.
It defines how we work together on this project.

---

## Repositories

Two repositories are involved in every project:

- **`claude-project-template`** — the master template repository. Contains the latest version of `PROCESS.md` and all starter files. Updated at the end of every project with lessons learned.
- **This repository** — the actual project. Has its own `PROCESS.md` copied from the template at project start. Evolves independently during the project.

## Git

- Claude handles all commits and pushes — the user does not commit manually
- Project work happens on a branch and reaches `master` only through a pull request the user approved (see `PROCESS.md` → Branches and Pull Requests)
- Commit at the end of each completed and approved step
- Always announce before pushing to remote
- Each commit = one completed milestone
- Secrets never go to git — kept in `local.env` which is gitignored

## Communication

- [FILL IN: user's technical level and communication preferences — update after first session]
- User explicitly approves before Claude moves to the next step — never advance without approval
- Do not write code outside the Construction phase

## Session Start

Do these in order, every session:

1. **Verify local file access** — run a simple command (e.g. `whoami`) to confirm commands execute on the user's machine. If this fails, immediately warn: "WARNING: I may not have local file/command access. This session may behave unexpectedly — consider switching to the VS Code / Claude Code extension." Do not proceed silently.
2. **Read `CURRENT_STATE.md`** — understand where we are and what is next
3. **Read `NEXT_EPIC.md`** — be aware of upcoming requirements (do not implement, just be aware)
4. **Read `docs/decisions.md`** — before proposing any design or approach, check if it was already decided. If a proposal conflicts with a logged decision, say: "we settled this before — [decision] because [reason]. Still proceed?"
5. **Append to `docs/audit.md`** — log session start: timestamp, verbatim user opening message, current phase from `CURRENT_STATE.md`. Create the file if it does not exist yet.
6. **Check `docs/plan.md`** — if it exists and has unchecked items, resume from the first unchecked item. Do not restart the stage from the beginning. Also check for open pull requests and unmerged branches (`gh pr list`) — they are unfinished work.
7. **Resume** — after reading `CURRENT_STATE.md` and `docs/plan.md`, present a one-line status summary before starting work:
   - Current phase and step
   - Last completed action
   - What comes next

   Then proceed — unless the summary reveals ambiguity, in which case ask first.

## Workflow

- The process guide for this project is `PROCESS.md` — read it and follow it
- Follow the phase/step structure defined there and tracked in `CURRENT_STATE.md`
- Document decisions before implementing them
- Record all major decisions in `docs/decisions.md` as they are made
- Update `PROCESS.md` during the project whenever a lesson is learned — do not wait until the end
- For any multi-step Construction stage, write a plan to `docs/plan.md` and get user approval before starting. Mark each step `[x]` immediately on completion — same interaction, never batched at the end.
- When the user corrects Claude in a way that would apply again, ask: "Keep this as a rule? (a) this project only, (b) also for the template, (c) no." (a) → this `CLAUDE.md`; (b) → this `CLAUDE.md` + `docs/template-candidates.md`; (c) → nothing. Ask once per correction.

## Project Start

When starting a new project using this template:
- `PROCESS.md` in this project is a copy of the template repo's `PROCESS.md`
- It is now this project's own — modify it freely as the project evolves

## Definition of Done

Before saying a task is complete, Claude must check each of these:

- [ ] Code works, tests pass (where the Light Testing Rule applies), CI is green
- [ ] Merged to `master` through a pull request the user approved
- [ ] `docs/decisions.md` updated — if any architectural decision was made
- [ ] `PROCESS.md` updated — if any lesson was learned
- [ ] `CURRENT_STATE.md` updated — if the project state changed
- [ ] `docs/audit.md` updated — session-end entry appended (what was completed, any approvals received)
- [ ] Any corrections from this task offered as rules (this project / also template / no)
- [ ] At end of an epic or the project: review `docs/template-candidates.md` with the user (sync to template / project-only / drop)

This checklist is mandatory, not optional. Do not say "done" without running through it.

## Project End

Before closing out the project:
1. Review this project's `PROCESS.md` against the template repo's `PROCESS.md`
2. Identify improvements learned during this project
3. Propose updates to the template repo so the next project starts from a better baseline
