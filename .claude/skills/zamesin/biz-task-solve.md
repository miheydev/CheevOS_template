---
language: en
methodology_origin: ru
---

# Skill: /biz-task-solve

> 5-step algorithm for solving Business tasks using AJTBD

## Triggers

- `/biz-task-solve`
- "how to solve a business task"
- "how to grow revenue"
- "how to reduce churn"
- "how to increase conversion"
- "how to scale the product"
- "как решить бизнес-задачу"

## Instructions

The user describes a Business task (бизнес-задача). Your job is to walk them through the 5-step AJTBD algorithm, plugging in other skills as needed.

### Principle

**Business tasks are solved by an algorithm of applying Mechanics over Job Graphs (графы работ) of different Segments.** Innovation is nothing more than applying value-creation Mechanics (механики) and product strategy.

**Product strategy** = the decision about which parts of which Segments' graphs we compete for.

### Typical Business tasks AJTBD works for

- Find a Segment for a new product
- Create product Value
- Grow signup / payment conversion
- Reduce churn, improve retention, increase return rate
- Scale an existing product
- Acquire customers more efficiently
- Escape direct competition
- Build disruptive products
- Grow average check
- Communicate the product

### The 5-step algorithm

#### Step 1. Identify the Business task with maximum ROI

Find the growth point. Where to look:
- Unit economics (which metric, if improved, yields the biggest growth?)
- Knowledge about an opportunity to create Value in the market
- Business strategic priorities

**Typical mistakes:**
- Generating Solution Hypotheses (гипотезы) without research
- Starting research without a clear logic of "who to talk to and about what"

#### Step 2. Hypothesize Segments and their Job Graphs (expert)

→ Use `/segment-find` to find Segments
→ Use `/job-describe` to describe Jobs
→ Use `/job-graph` to build Job Graphs

Hypothesize:
- What Segments exist in the market (by Jobs)
- What their Job Graphs look like
- What current Solutions they hire

#### Step 3. Hypothesize applicable Mechanics

→ Use `/value-mechanics` to generate Mechanics

From the expert list of Segments and their graphs, hypothesize:
- Which value-creation Mechanics have potential
- Which business-task-solving Mechanics apply
- For which parts of the graph

#### Step 4. Run research

Decide what needs to be validated:
- **Qualitative research** (AJTBD interviews) — uncover real Jobs, graphs, problems
- **Quantitative validation** — Segment size, ability to pay, satisfaction
- **Expert interviews** — confirm Hypotheses
- **Competitor analysis** → `/consideration-set`

#### Step 5. Generate Mechanic Hypotheses

Based on research data:
- Which value-creation Mechanics to apply
- Which business-task-solving Mechanics to use
- Prioritize by: ROI, complexity, speed of execution

### 50 Mechanics for solving Business tasks (categories)

**Value growth Mechanics:**
- Fix problems in the Critical Sequence (критическая последовательность)
- Kill Jobs (simplify)
- Do more Jobs with one Solution
- Move up to a higher-level Job
- Take the Job off the human

**Revenue growth Mechanics:**
- Shift to an adjacent Segment (more solvent)
- Activate into additional Jobs (cross-sell)
- Move to the previous / next Job
- Split the benefit and deliver it earlier

**Acquisition Mechanics:**
- Communicate via Jobs (not features)
- Build a channel through the previous Job
- Perform viral Jobs

**Retention Mechanics:**
- Fix problems in the Critical Sequence
- Activate into new Jobs
- Grow usage frequency

**Escape-from-competition Mechanics:**
- Compete for a different part of the graph
- Move to a higher-level Job
- Combine Jobs that competitors do separately

---

## Response format

### Solving the Business task: [task statement]

#### Step 1: Growth point

- **Business task:** [concrete statement]
- **Why this is the growth point:** [reasoning — unit economics, strategy, market opportunity]
- **Success metric:** [what we measure]

#### Step 2: Segments and graphs (expert Hypotheses)

| Segment | Big Job | Current Solution | Size | Margin |
|---------|---------|------------------|------|--------|
| ... | ... | ... | ... | ... |

**Focus Segment:** [which one and why]

#### Step 3: Applicable Mechanics

| # | Mechanic | For which Job | Hypothesis | ROI potential |
|---|----------|---------------|------------|---------------|
| 1 | ... | ... | ... | high/medium |

#### Step 4: Research plan

| What to validate | Method | With whom | Quantity |
|------------------|--------|-----------|----------|
| ... | AJTBD interview / survey / analysis | ... | ... |

#### Step 5: Solution Hypotheses (post-research or expert)

**Hypothesis 1:** [concrete Solution]
- Mechanic: [which one]
- Segment: [for whom]
- Expected effect: [which metric and by how much]
- How to validate: [MVP / experiment]

**Hypothesis 2:** ...

#### Next steps

1. [concrete first step]
2. [second step]
3. [third step]

---

**Business task:**

$ARGUMENTS
