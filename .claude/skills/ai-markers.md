---
description: "Checks text for AI-generation markers (negative parallelism, bureaucratic language, filler openers, etc.) and cleans them up. Based on reference_ai_text_markers.md."
language: en
---

# /ai-markers — AI text marker cleanup

Final filter before publishing. Finds patterns in the text that reveal machine generation and removes them while preserving the author's voice.

**When to invoke:** before sending a post / email / proposal / landing page / any text written on a human's behalf. Especially if part of the draft was written by Claude.

**Do NOT invoke** for: internal notes, code, raw transcripts, logs — these patterns are not a problem there.

---

## Source of truth

Full checklist with examples and explanations — [reference_ai_text_markers.md](../../../../.claude/projects/-Users-igor-Documents-CheevOS/memory/reference_ai_text_markers.md) (in auto-memory).

If it's not available — use the built-in list below.

---

## Marker categories

1. **Negative parallelism** — «не X, а Y», «это не просто X, это Y» (Claude's primary tell)
2. **Rhetorical self-answers** — «Вопрос? Ответ.», «Результат? Разрушительный.»
3. **Filler openers** — «стоит отметить», «важно понимать», «в современном мире»
4. **Mentoring tone** — «давайте разберём», «let's unpack», «вот в чём дело»
5. **Despite-adversity-optimism formula** — «несмотря на вызовы, результат превзошёл»
6. **Bureaucratic language (канцелярит)** — «осуществление», «в рамках», «выступает в роли»
7. **Amplifiers without substance** — ключевой, фундаментальный, системный, комплексный; delve, leverage, robust
8. **Marker adverbs** — тихо, глубоко, принципиально, примечательно
9. **Structural patterns** — symmetric triples, em-dash mid-sentence, unicode arrows →, synonym carousel
10. **Content-level** — one idea repeated 10 ways, only positives, empty references ("experts say")
11. **Closing stamps** — «таким образом», «подводя итог», «the future looks bright»

---

## PROCESS

### Step 1: Read the text

1. `$ARGUMENTS` — file path or the text itself
2. Identify the type: post / email / proposal / landing page
3. If a ToV profile for the author exists — apply it when making edits

### Step 2: Diagnosis

Go through the text and list **every** hit by categories 1–11. Format:

```
## Category N: [name]

- Quote: «...»
  Marker: [what exactly]
  Fix: «...»
```

If there are no hits for a category — skip it, do not bloat the report.

### Step 3: Prioritization

- **Critical** — negative parallelism, rhetorical self-answers, closing stamps (instantly reveal AI)
- **Important** — bureaucratic language, amplifiers without substance, filler openers
- **Cosmetic** — em-dash, adverbs, structural symmetries

### Step 4: Show the plan

Give the user the list of edits with quotes **before** making any changes. They may reject some (certain phrases are intentionally theirs — alive voice).

### Step 5: Apply fixes via Edit

- Work with the existing file, do not rewrite from scratch
- One fix — one `Edit`, so the diff is readable
- **Preserve the author's conversational phrases** (e.g. «короче», «прям», «ну и» in Russian) — these are NOT fillers
- If in doubt between author's voice and cleanliness — keep the author's voice

### Step 6: Final check

- [ ] Went through all 11 categories
- [ ] No remaining «не X, а Y» left in the text
- [ ] No closing stamps
- [ ] Author's tone and rhythm preserved

---

## WHAT TO LEAVE UNTOUCHED

- Author's conversational phrases
- Repeated ideas from different angles (if it's a deliberate device, not an AI loop)
- Specifics: numbers, names, dates
- Unfinished sentences (when appropriate)
- Real first-person stories
- Emotion and opinion

---

## Integration

- Built into `/mega-polish` as a mandatory step before the final check
- Invoked separately when only marker cleanup is needed without a full rewrite

---

Text to clean:

$ARGUMENTS
