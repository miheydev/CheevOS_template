---
language: en
methodology_origin: ru
---

# Skill: /segment-find

> Finding and ranking customer Segments by Jobs (AJTBD + ABCDX)

## Triggers

- `/segment-find`
- "find the Segments"
- "who is our customer"
- "segmentation"
- "which Segment to focus on"
- "найди сегменты"

## Instructions

The user describes a product, market, or situation. Your task is to find and rank Segments (сегменты) by Jobs, picking the economically most attractive one.

### Segmentation by Jobs — the main segmentation for a product

**A Segment is a group of people/businesses united by similar Job+Solution pairs.**

Typical segmentations (by income, age, role) give a false sense of clarity but contain no cause-and-effect. A 20-year-old can buy a premium apartment, and a wealthy entrepreneur can buy five economy ones.

**Segmentation by Jobs lets you:**
- Understand WHY people buy
- Pick the economically most attractive Segment
- Create Value with precision
- Acquire customers more efficiently
- Communicate the product through Jobs

### Case: LiFT (personal brand agency)

Before AJTBD: focus on experts who want clients from social media, no budget — a tough, low-margin Segment.

After AJTBD research: found a Segment of top managers with budgets and high motivation → average check x3, revenue +60% in 3 months.

### Segment discovery process

1. **Hypothesize Segments from expertise** — Hypotheses about Jobs of different groups
2. **For each Segment describe:**
   - Jobs (Big + Core)
   - Current Solutions
   - Problems with current Solutions
   - Satisfaction
3. **Rank by economic attractiveness:**
   - Segment size
   - Ability to pay and budget for the Job
   - Customer margin
   - Satisfaction with the current Solution (the lower — the bigger the opportunity)
   - Reachability (can you actually get to them)
4. **Apply ABCDX segmentation** (for existing products)

### ABCDX segmentation

| Segment | Description | What to do |
|---------|-------------|------------|
| **A** | High margin, high satisfaction | Scale acquisition |
| **B** | High margin, medium satisfaction | Grow Value |
| **C** | Low margin, satisfied | Automate service |
| **D** | Low margin, unsatisfied | Don't spend resources |
| **X** | Potentially huge, but not customers yet | Research and experiment |

### B2B specifics

- A B2B deal involves: Business Buyer, decision-maker (ЛПР), influencer (ЛВР), budget holder, recommenders, saboteurs, end users
- **90% of purchase motivation comes from PERSONAL Jobs**, not business Jobs
- Personal Jobs = personal motivation to execute the assigned business goals
- It's important to know the Jobs of ALL roles in the deal

### Warnings

- Expert Hypotheses about Segments = risky. The market may contain more attractive Segments you don't know about
- Validation requires: AJTBD interviews (qualitative) → quantitative validation
- Minimize risk: don't make large decisions on Hypotheses without validation

---

## Response format

### Segments: [product/market]

#### Segment map

| # | Segment | Job | Current Solution | Budget | Satisfaction | ABCDX |
|---|---------|-----|------------------|--------|--------------|-------|
| 1 | ... | ... | ... | ... | ... | A/B/C/D/X |
| 2 | ... | ... | ... | ... | ... | ... |

#### Ranking by attractiveness

| # | Segment | Size | Margin | Opportunity | Reachability | Total |
|---|---------|------|--------|-------------|--------------|-------|
| 1 | ... | .../5 | .../5 | .../5 | .../5 | .../20 |

#### Recommendation

**Focus Segment:** [which one and why]
**Why not others:** [brief reasoning]
**Hypothesis risks:** [what could be wrong]

#### B2B roles (if applicable)

| Role | Business Job | Personal Job |
|------|--------------|--------------|
| Decision-maker (ЛПР) | ... | ... |
| Business Buyer | ... | ... |
| End user | ... | ... |

**Next step:** To describe the focus Segment's Jobs — `/job-describe`. To build the graph — `/job-graph`.

---

**Product/market for segmentation:**

$ARGUMENTS
