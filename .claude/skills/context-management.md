---
description: "Rules for context management and overflow prevention. Load during long sessions or when context grows large."
language: en
---

# Context Management (Overflow Prevention)

**Problem:** working on multiple tasks in one session overflows context → "Prompt is too long" → loss of progress. On the Max plan the limit is a rolling 5-hour window.

## Context Management Commands

| Command | When to use |
|---------|-------------|
| `/context` | Check how much context is currently used |
| `/compact` | Compress history into a summary (task not finished, but context has grown large) |
| `/clear` | Full reset when switching to an unrelated task |

## Proactive Cleanup Between Tasks

**After completing a major task** (commit made, files written, summary given) — before starting the next request, recommend (but let the user decide):
- `/compact` — if the next task is related to the current one
- `/clear` — if the next task is from a different project/context

**Signs of a "major task":** 50+ tool calls, work across 5+ files, wiki ingest, transcript processing, artifact generation.

**Do NOT recommend** during a chain of small questions — only when a block of work is complete.

## Token budget: when to /compact

Autocompact fires automatically at ~70% context (≈140K tokens for 200K window, ≈700K for 1M window). But long agentic sessions
(400+ turns) drain the **5-hour rolling token budget** long before hitting the context limit.

| Turns in session | Action |
|-----------------|--------|
| ~60 turns | recommend `/compact` after completing a task block — the user decides |
| ~100 turns | recommend `/compact` even mid-task — the user decides |
| Switch topic | recommend `/clear` — the user decides |

**Why it matters:** a 400-turn session costs ~2–3M tokens total. A 60-turn session after
`/compact` costs ~200K. Same work, 10× cheaper.

## Rules

1. **Maximum 2–3 tasks per session.** If there are more tasks — warn the user and suggest splitting into multiple sessions.
2. **Delegate to subagents.** File exploration, codebase search, reading large files — offload to subagents. This keeps the main context clean.
3. **Read files economically:**
   - Do not read the entire file if you only need specific lines — use `offset` + `limit`
   - Do not re-read a file you already read in this session
   - For large files (>200 lines) first locate the relevant section via `Grep`, then read only that fragment
4. **Write plans to a file, not into context.** If a work plan is needed — write it to a file rather than keeping it in memory.
5. **After completing each task** — commit the result (commit or write to file). If the session is interrupted, progress will not be lost.
6. **If there are >3 tasks — create a plan file** with a task list and statuses. Work through it, marking items done.
