---
description: Independent quality evaluation of a result via a critic agent with a clean context (quality gate). Invoked as /evaluate
language: en
---

# Evaluate — Quality Gate

The agent that did the work CANNOT objectively evaluate it — confirmation bias. A fresh agent with zero context about the process sees only "what was requested" and "what was produced".

---

## Triggers

- `/evaluate`
- `/evaluate path/to/file.md`
- "evaluate the result", "quality gate", "check what came out"

---

## Process

### Step 1. Collect inputs

From `$ARGUMENTS` and the conversation context, extract two things:

1. **ORIGINAL_REQUEST** — the original user request. Exactly what was asked.
2. **RESULT_CONTENT** — the work output. Read all files that were created/changed.

If information is missing — ask the user:
- "What was the original request?"
- "Which files should be evaluated?"

### Step 2. Launch the critic agent

Launch **1 subagent** with parameters:
- `subagent_type: "general-purpose"`
- `model: "sonnet"`

Critical: the subagent receives ONLY the request + result. No reasoning history, no intermediate steps, no context about HOW the work was done.

Subagent prompt (in English — better evaluation quality):

```
You are an independent quality reviewer performing a blind review. You have NEVER seen this work before. You know NOTHING about how it was created, what tools were used, or what challenges were encountered.

Your job: evaluate the RESULT against the ORIGINAL REQUEST. Nothing else.

## ORIGINAL REQUEST

{ORIGINAL_REQUEST}

## RESULT TO EVALUATE

{RESULT_CONTENT}

---

## Instructions

Evaluate this result against 5 criteria. For each criterion:
- Score (1-10)
- Evidence: specific quotes/examples from the result that justify your score
- Gap: what is missing or wrong (if score < 8)

### CRITERIA

**1. COMPLETENESS (1-10)**
Does the result address ALL parts of the original request? List each requirement from the request and check if it's covered. Flag anything skipped or partially done.

**2. CORRECTNESS (1-10)**
Is the result technically/factually accurate? For code: bugs, logical errors, security issues? For text: claims supported, no contradictions? For analysis: methodology sound?

**3. QUALITY (1-10)**
Is the execution high quality? For code: clean, idiomatic, well-structured? For text: clear, well-organized, no fluff? For design: thoughtful, consistent?

**4. ALIGNMENT (1-10)**
Does the result match the INTENT behind the request, not just the literal words? Would the person who made this request be satisfied? Does it solve their actual problem?

**5. USABILITY (1-10)**
Can the requester immediately use this? Is it complete and self-contained? Or does it need significant additional work, context, or explanation?

---

## OUTPUT FORMAT (follow strictly)

### Scores

| Criterion | Score | One-line verdict |
|-----------|-------|-----------------|
| Completeness | X/10 | ... |
| Correctness | X/10 | ... |
| Quality | X/10 | ... |
| Alignment | X/10 | ... |
| Usability | X/10 | ... |
| **OVERALL** | **X/10** | **[verdict]** |

Overall = average of 5 scores, rounded to nearest integer.

### Detailed Analysis

#### Completeness (X/10)
[evidence and gaps — with specific quotes from the result]

#### Correctness (X/10)
[evidence and gaps]

#### Quality (X/10)
[evidence and gaps]

#### Alignment (X/10)
[evidence and gaps]

#### Usability (X/10)
[evidence and gaps]

### What's Good
- [specific strength 1 with evidence]
- [specific strength 2 with evidence]

### What's Wrong
- [specific issue 1]: [what's wrong] → [how to fix]
- [specific issue 2]: [what's wrong] → [how to fix]

### Top-3 Actions (priority order)
1. [most critical action] — WHY and HOW specifically
2. [second action] — WHY and HOW specifically
3. [third action] — WHY and HOW specifically

Be brutally honest. Do not soften criticism. The purpose is to catch what the original creator missed. If the result is genuinely excellent, say so — but still find at least one thing that could be better.
```

### Step 3. Present the result

Receive the subagent's response and format it for the user.

**Traffic light by average score:**
- **GREEN** (8-10) — ready, high quality
- **YELLOW** (5-7) — needs refinement, but the foundation is solid
- **RED** (1-4) — significant rework required

**Chat output format:**

```
## Quality Gate: [object name]

**Verdict: [GREEN/YELLOW/RED] [X/10]**

[1-2 sentences — the gist]

| Criterion | Score | Verdict |
|-----------|-------|---------|
| Completeness | X/10 | ... |
| Correctness | X/10 | ... |
| Quality | X/10 | ... |
| Alignment | X/10 | ... |
| Usability | X/10 | ... |

### What to fix (top-3)
1. ...
2. ...
3. ...

Full report → [path to file]
```

### Step 4. Save the full report

Save the subagent's full report (translated to Russian) to a file:
- Name: `Quality-Gate-[Topic]-YYYY-MM-DD.md` (topic short, in Latin characters)
- Location: current working directory, or next to the evaluated files

### Step 5. If RED — create a fix plan

If average score < 5:
- Extract all "how to fix" items into a concrete action plan
- Suggest: "Want me to fix it? I'll start with item 1."

---

## Rules

1. **One agent, clean context.** The critic receives ONLY the request + result. No history.
2. **Prompt in English, report in Russian.** The subagent works in English; the final presentation is in Russian.
3. **Specificity.** Every score must include quotes from the result. No generic "could be better".
4. **Write-first.** Full report always goes to a file.
5. **Do not defend your own work.** If you (the orchestrator) did the evaluated work — DO NOT argue with the critic. Present the result as-is.
6. **Sonnet for the critic.** Use `model: "sonnet"` — faster, cheaper, outsider perspective does not require the strongest model.

---

$ARGUMENTS
