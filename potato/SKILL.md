---
name: Potato
description: Create and update projects in the potato-projects docs repo (big-potato/small-potato). Use when the user wants to start a project, add docs to one, or update its status.
---

# Potato

Repo root: `/home/aldrsze/Projects/potato-projects`

```
README.md              # index table
_template/             # PRD.md, plan.md, roadmap.md
big-potato/<project>/
small-potato/<project>/
```

## Rules

- Never invent content. Only write what the user or scanned docs gave you.
- Never write before the user confirms the plan.
- Never push. Commit locally and stop. The user pushes.
- Never run `git push`, `git push --force`, `gh pr create`, or `gh repo edit`. No exceptions, even if I say "ship it".
- One commit per task. Show hash + `git show --stat`, then stop.
- Ask permission before every commit. Show the exact message, wait for my yes.

## Workflow

### 1. Scan

- Repo: read `README.md`, list folders in `big-potato/` and `small-potato/`.
- Current directory (wherever invoked): read `PRD.md`, `plan.md`, `roadmap.md`, or any markdown that reads like one.

### 2. Ask

One numbered message. Ask only what the scan did not answer:

- Project name (kebab-case)
- Big or small potato
- What — one sentence
- Why — one sentence
- Done when — checklist
- Success — how you know it worked
- Out of scope
- Stack
- Plan steps
- Risks / unknowns
- Roadmap milestones

If the user says "you decide", still ask for the missing facts.

### 3. Confirm

Show target path, files to create/edit, and which facts came from docs vs. user. Wait for explicit yes.

### 4. Write

- New: `cp -r _template <size>/<project>`, then replace placeholders.
- Existing: edit only the docs the user named.
- Keep each doc short. No extra sections.

### 5. Index

Add or update one row in `README.md`:

| Project | Size | Status |

Link the folder. Status: `planning`, `active`, `done`, `paused`.

### 6. Commit

Ask first: show `git status --short` and the exact commit message. Wait for yes.
Then `git add -A && git commit -m "<action>: <project>"`.

Show the hash and `git show --stat`. Then stop.

## Update flow

User names a project → read its docs → ask what changed → edit → index → commit → stop.

## Roadmap rules

- Each milestone = one outcome you can demo, not a theme.
- No "improve", "polish", "misc". If you can't check it, rewrite it.
- Every row: outcome, done-when checklist, depends-on, risk.
- Milestone 1 = smallest thing that works end to end.
- Split any milestone over ~1 week of student work.
