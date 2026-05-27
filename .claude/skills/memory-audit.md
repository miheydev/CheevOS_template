---
description: "Mechanical hygiene scan of auto-memory: orphans, broken wikilinks, duplicate slugs, empty/missing-fm entries, stale projects. Read-only by default — execution only after explicit ack. No LLM, ~1s, ~0 tokens."
language: en
---

# Skill: /memory-audit

> Periodic hygiene check over `~/.claude/projects/-Users-igor-Documents-CheevOS/memory/`. Mechanical only — no semantic/LLM pass in V1. Inspired by r/ClaudeAI thread «6 months of .md memory, conflicting facts are the hard part» (see [reference_arctic_shift_reddit.md](../../memory/reference_arctic_shift_reddit.md)).

## Triggers

- `/memory-audit`
- `/audit-memory`
- "проверь память"
- "разбери память"
- "почисти память"

## Scope

In: `~/.claude/projects/-Users-igor-Documents-CheevOS/memory/*.md` (excl. `MEMORY.md`)
Out: anything else (CLAUDE.md, .claude/skills/, project memories elsewhere)

## Phase 1 — Run the scanner (read-only)

Run this Python one-shot. It prints a structured report. Costs ~1s, 0 tokens.

```bash
cd "/Users/igor/.claude/projects/-Users-igor-Documents-CheevOS/memory" && python3 <<'PY'
import os, re, time, glob
from collections import defaultdict
SKILLS = '/Users/igor/Documents/CheevOS/.claude/skills'

mem_files = sorted([f for f in glob.glob('*.md') if f != 'MEMORY.md'])
skill_files = {os.path.basename(p) for p in glob.glob(f'{SKILLS}/*.md')} if os.path.isdir(SKILLS) else set()
mem_set = set(mem_files)
memory_md = open('MEMORY.md').read()

def parse_fm(text):
    if not text.startswith('---'): return {}, text
    m = re.match(r'^---\n(.*?)\n---\n?(.*)$', text, re.DOTALL)
    if not m: return {}, text
    fm = {}
    for line in m.group(1).splitlines():
        if ':' in line:
            k, v = line.split(':', 1); fm[k.strip()] = v.strip()
    return fm, m.group(2)

entries = {}
for f in mem_files:
    text = open(f).read()
    fm, body = parse_fm(text)
    entries[f] = {'name': fm.get('name',''), 'type': fm.get('type',''),
                  'body': body, 'body_len': len(body.strip()),
                  'mtime': os.path.getmtime(f), 'text': text}

def resolve_wikilink(slug):
    """Smart match: try direct, with type prefix, dash/underscore variants, then skills."""
    variants = {slug, slug.replace('-','_'), slug.replace('_','-')}
    hits = []
    for v in variants:
        for ext in ['', 'feedback_', 'reference_', 'project_', 'user_']:
            if f'{ext}{v}.md' in mem_set: hits.append(('memory', f'{ext}{v}.md'))
        if f'{v}.md' in skill_files: hits.append(('skill', f'{v}.md'))
    # dedupe
    return list(dict.fromkeys(hits))

# 1. Orphans
orphans = [f for f in mem_files if f not in memory_md]
# 2. Index-only
referenced = set(re.findall(r'\(([a-z0-9_\-]+\.md)\)', memory_md))
index_only = sorted(referenced - mem_set - {'MEMORY.md'})
# 3. Wikilinks
broken_wl, ambig_wl = [], []
for f in mem_files:
    for slug in set(re.findall(r'\[\[([a-z0-9_\-]+)\]\]', entries[f]['text'])):
        hits = resolve_wikilink(slug)
        if not hits: broken_wl.append((f, slug))
        elif len(hits) > 1: ambig_wl.append((f, slug, hits))
# 4. Duplicate slugs
slug_map = defaultdict(list)
for f, e in entries.items():
    if e['name']: slug_map[e['name']].append(f)
dupes = {k: v for k, v in slug_map.items() if len(v) > 1}
# 5. Empty
empty = [(f, e['body_len']) for f, e in entries.items() if e['body_len'] < 50]
# 6. Missing frontmatter
no_fm = [f for f, e in entries.items() if not (e['name'] and e['type'])]
# 7. Stale projects (>60d + active words)
active = re.compile(r'\b(in progress|active|planning|активн|в процессе|планир)\b', re.I)
now = time.time()
stale = [(f, int((now - entries[f]['mtime'])/86400)) for f in entries
         if f.startswith('project_') and (now - entries[f]['mtime']) > 60*86400
         and active.search(entries[f]['body'])]
# 8. Invalid type
valid = {'user','feedback','project','reference'}
bad_type = [(f, e['type']) for f, e in entries.items() if e['type'] and e['type'] not in valid]

def section(title, items, fmt=lambda x: f'  • {x}'):
    print(f'--- {title} ({len(items)}) ---')
    for x in items: print(fmt(x))
    print()

print(f'\n🧠 Memory audit — {time.strftime("%Y-%m-%d")} — {len(mem_files)} files\n')
section('1. ORPHANS (on disk, not in MEMORY.md)', orphans)
section('2. INDEX-ONLY (in MEMORY.md, not on disk)', index_only)
section('3. BROKEN WIKILINKS (no target after smart match)', broken_wl,
        lambda x: f'  • {x[0]} → [[{x[1]}]]')
section('4. AMBIGUOUS WIKILINKS (>1 target)', ambig_wl,
        lambda x: f'  • {x[0]} → [[{x[1]}]] matches: {x[2]}')
section('5. DUPLICATE SLUGS (same frontmatter name:)', list(dupes.items()),
        lambda x: f'  • name="{x[0]}":\n      ' + '\n      '.join(f'- {f}' for f in x[1]))
section('6. EMPTY (body < 50 chars)', empty, lambda x: f'  • {x[0]} (body={x[1]} chars)')
section('7. MISSING FRONTMATTER (no name: or no type:)', no_fm)
section('8. STALE PROJECTS (>60d + active-state words)', stale,
        lambda x: f'  • {x[0]} (age={x[1]}d)')
section('9. INVALID TYPE TOKEN', bad_type, lambda x: f'  • {x[0]} (type={x[1]!r})')

total = sum(map(len, [orphans, index_only, broken_wl, ambig_wl, dupes, empty, no_fm, stale, bad_type]))
print(f'=== TOTAL FINDINGS: {total} ===')
PY
```

## Phase 2 — Report + gate

Take the scanner output. Summarize to the user in this format:

```
🧠 Memory audit — <today> — scanned <N> files

🟢 Hygiene (auto-fixable, no behavioral risk):
  • <N> orphans     → can add to MEMORY.md
  • <N> empty       → can delete
  • <N> bad-type    → can fix frontmatter

🟡 Need decision:
  • <N> ambiguous wikilinks  → pick target
  • <N> duplicate slugs      → pick which to keep
  • <N> stale projects       → archive vs update

🔴 Skip without input:
  • <N> truly broken wikilinks (need new target or strip)
```

Then ask **one** gate question:

> Готово к чистке. Подтверди: всё / только 🟢 hygiene / выборочно (укажи номера) / только показать.

## Phase 3 — Execute (only after explicit ack)

Per approved item:
- **Orphan** → ask the user for one-line description (or propose from file's frontmatter `description`), then add bullet to `MEMORY.md` in an appropriate section
- **Index-only** → remove line from `MEMORY.md` (unless it's a cross-domain link like `<your-knowledge-base>/tasks.md` — those are intentional)
- **Empty** → delete file + remove from `MEMORY.md` if indexed
- **Duplicate slug** → ask which to keep; delete the other; update `MEMORY.md` and any backlinks
- **Stale project** → ask: archive (move to `memory/archive/<name>.md`) or update body
- **Ambiguous wikilink** → ask which target; rewrite `[[slug]]` to full filename slug
- **Truly broken wikilink** → ask: new target or strip `[[…]]` brackets

If memory folder is git-tracked, commit afterward; if not, tell the user "changes on disk only".

## Cadence

- Manual trigger only — no cron ([[no_premature_automation]] applies).
- Suggested: monthly, or when memory crosses +20 new files since last audit.
- Skill is cheap enough to run any time (1s, 0 tokens).

## Out of scope (deferred to V2 if needed)

- **Semantic conflict detection** between feedback entries (e.g. two feedbacks giving opposing guidance for same scope). V1 dry-run on the user's memory found 0 such cases — defer until signal appears.
- **Reference verification** (does the file/function/path named in a `reference_*.md` still exist?). Bound problem but adds I/O; defer.
- **Date-anchor drift** (relative dates without absolute anchor). High false-positive rate when the user's quotes contain «сегодня» verbatim. Defer.
- **Rewriting tone/style.** Audit ≠ editing.

## Notes on the wikilink convention

CLAUDE.md memory rules say link by frontmatter `name:` slug — but in practice the user's `name:` are sentences and `[[wikilinks]]` use **filename slug without type prefix** (e.g. `[[email_prompt_injection]]` → `feedback_email_prompt_injection.md`). The scanner uses smart resolution to handle this. If the convention is ever unified, simplify `resolve_wikilink()`.
