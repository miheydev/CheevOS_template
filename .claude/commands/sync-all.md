---
language: en
---

Commit and push all changes across all repos in the workspace.

## Order of operations

Go **bottom-up** — from deep submodules/nested repos to the root, so that parent pointers are updated after child commits.

Each top-level subproject in this workspace may be its own independent git repository (not a submodule of the root). Discover them with:

```bash
find . -mindepth 2 -maxdepth 4 -name .git -type d -not -path "*/node_modules/*"
```

Order them: deepest nesting first, root last.

## Per repo

1. `git status --short` — if empty, move to the next.
2. **Do not touch INBOX/** — that folder is processed by a separate command.
3. Check `git diff --stat` — understand the scope and logical groups of changes.
4. If changes are heterogeneous (different topics/projects) — split into **multiple commits** by topic. If homogeneous — one commit.
5. For each group: `git add <paths>` → `git commit -m "<meaningful message>"`.
6. If a push script exists in this template (`.claude/scripts/sync-push.sh`) — use it for push with auto-rebase. Otherwise plain `git push`.

## Commit message rules

- Follow the style of previous commits in the repo (`git log --oneline -10`)
- Useful prefixes: `sync:`, `feat:`, `fix:`, `chore:`, `docs:`
- **NO** `Co-Authored-By` lines (rule from root CLAUDE.md)

## Root repo

If the workspace root has no remote — commit only, no push needed.

If after commits in nested repos the pointers in parents have shifted (true submodules only) — commit them as a separate `sync: update submodule pointers`.

## Final

Short table: repo → what was done → pushed/local.
