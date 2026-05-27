---
language: en
methodology_origin: ru
---

# Skill: /job-describe

> Describing customer Jobs using the Advanced Jobs To Be Done formula

## Triggers

- `/job-describe`
- "describe the Jobs"
- "what Jobs do customers have"
- "what do customers want"
- "formulate the Jobs"
- "опиши работы"

## Instructions

The user describes a situation, product, customer, or market. Your task is to describe customer Jobs (работы) using the AJTBD formula.

### Key AJTBD principles

**The Job is the main unit of analysis.** A Job = achieving a goal/task, moving from Point A (job not done) to Point B (job done, result received).

**The Job is primary, Solutions are replaceable.** The product, its benefit, functionality, interfaces, copy — all derivative of the Job. People don't need our products — they want to accomplish their goals.

**A Solution (решение)** is any product, service, or action a person uses to do a Job. The act of choosing a Solution = "hiring a Solution for a Job".

### The Job-description formula

Full formula (from maximum to minimum detail):

1. **When** [context/situation]
2. **I'm in the context of** [life situation, role]
3. **I have past experience and/or knowledge** [relevant experience]
4. **I have psychological traits** [fears, beliefs]
5. **I'm feeling negative emotions** [what they feel at Point A]
6. **A Trigger happened** [event that started the action]
7. **I want to get the expected result** [Point B]
8. **With criteria** [exactly how the result should be achieved]
9. **In order to** [higher-level Job / why this matters]
10. **And to feel** [emotional state at Point B]

**Minimum description:** "I want + verb" (e.g.: "I want to close the cookie popup").

### Job levels

- **Big Job (Большая Работа)** — high-level goal (e.g.: "grow the business", "provide for the family")
- **Core Job (Ядровая работа)** — the main Job the product focuses on (e.g.: "acquire leads at target metrics")
- **Small Job** — micro-Jobs inside the Core Job (e.g.: "configure targeting in the ad cabinet")

### Algorithm

1. **Set the context** — who these people are, in what situation, what product/market
2. **Identify Big Jobs** — 1-3 high-level Jobs
3. **Unpack Core Jobs** — 3-7 main Jobs performed for the Big Job
4. **Detail Small Jobs** — 2-5 micro-Jobs per Core Job (if needed)
5. **For each Job** describe via the formula (depth depends on user's task)

### Important

- A Job is ALWAYS a verb in infinitive form: find, fire, eat, confirm
- Don't confuse Jobs with problems: "automate marketing" — Job, "marketing is not automated" — problem
- You can write Jobs from expertise (Hypotheses), but warn about risks: the market may contain other Segments with other Jobs
- Every word in a Job description is gold — changing a single word can radically change the product

---

## Response format

### Customer Jobs: [product/situation name]

**Context:** [brief situation summary]

#### Big Jobs (high-level)
| # | Job | Trigger | Expected result |
|---|-----|---------|-----------------|
| 1 | ... | ... | ... |

#### Core Jobs (main)
| # | Job | For which Big Job | Current Solution | Satisfaction |
|---|-----|-------------------|------------------|--------------|
| 1 | ... | ... | ... | ?/low/medium/high |

#### Small Jobs (detailed) — if needed
| # | Job | For which Core Job | Formula |
|---|-----|--------------------|---------|
| 1 | ... | ... | When [context], I want [result], in order to [Big Job] |

**Hypothesis risks:** [warning if the Jobs are written from expertise without research]

**Next step:** To build the Job Graph use `/job-graph`. To find Segments — `/segment-find`.

---

**User input:**

$ARGUMENTS
