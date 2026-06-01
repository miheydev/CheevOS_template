---
language: en
---

# Template Skills — scope & sync notes

These skills are the **reusable starter set** shipped with CheevOS-Template for spinning up a new workspace. They are deliberately a *subset* of the live CheevOS workspace, not a full mirror.

## Conventions

- Every skill/command file carries a `language:` marker (`en` / `ru` / `partial`) per the workspace language policy. Markers here are mirrored from the live CheevOS copies (synced 2026-06-01).
- Voice-/content-sensitive skills stay `ru` (write-post, voice, tov-write, create-proposal, …); instruction-style skills are `en`.

## Intentionally NOT included (personal / workspace-bound)

These live only in the real CheevOS workspace because they encode Igor-specific systems, paths, or accounts — they would be noise (or leak personal structure) in a fresh template:

- `mail.md` (+ `mail-references/`) — Apple Mail + AAA CRM auto-detect, account-specific maps
- `plan-day.md`, `close-day.md` — personal daily ritual + Notion Mindcraft integration
- `beads-and-tasks.md`, `tasks-overview.md` — personal task system
- `crimson-ingest.md` — a specific newsletter→Finance-wiki pipeline
- `switch-project.md`, `workshop-slides.md`, `zoom-routing.md`, `zoom-to-kinescope.md`, `zamesin/`

## Reusable, worth adding when generalized

Present in the live workspace and broadly reusable — port them here once de-personalized:

- `context-audit.md`, `workspace-status.md`, `cross-search.md`, `quick-task.md`

Keep this file in sync when the skill set changes.
