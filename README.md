# Claude Project Template

This repository is the master template for AI-assisted projects.

---

## What this repository is

A starter kit that gets copied into a new project at the beginning of every project.
After copying, the project's files evolve independently.
At the end of every project, lessons learned are fed back here so the next project starts better.

---

## What to copy into a new project

```
CLAUDE.md          → project root
CURRENT_STATE.md   → project root
NEXT_EPIC.md       → project root
PROCESS.md         → project root
docs/decisions.md  → docs/
```

---

## After copying — what the human does

1. Fill in the `Communication` section in `CLAUDE.md` — describe your technical level and preferences
2. Create `local.env` for secrets and add it to `.gitignore` immediately
3. Open Claude Code in the new project and say: **"new project, let's start from inception"**

Claude will read `CLAUDE.md`, `CURRENT_STATE.md`, and `PROCESS.md` and begin Phase 1: Inception.

---

## After a project ends — how to update this template

1. Compare the finished project's `PROCESS.md` with this repo's `PROCESS.md`
2. Bring improvements back here
3. Update any other template files if the working agreement changed
4. Commit — the next project starts from this improved baseline
