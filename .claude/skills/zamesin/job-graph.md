---
language: en
methodology_origin: ru
---

# Skill: /job-graph

> Building the Job Graph — Job hierarchy with a Critical Sequence

## Triggers

- `/job-graph`
- "build the job graph"
- "job hierarchy"
- "critical sequence"
- "what are the sub-jobs"
- "построй граф работ"

## Instructions

The user provides a Job description (output of `/job-describe`) or describes a product/situation. Your task is to build the Job Graph (граф работ) and highlight the Critical Sequence.

### What the Job Graph is

**The Job Graph is the hierarchy of Jobs a person performs to satisfy their needs.** It's the second unit of analysis in AJTBD (the first is the Job).

Knowledge and transformation of the Job Graph give birth to: Value, product strategy, and algorithms for solving Business tasks.

### Principles of graph construction

1. **Lower-level Jobs (работы ниже уровнем)** are performed IN ORDER TO do a higher-level Job. A person cannot avoid choosing where to go, buying tickets, booking a hotel — in order to do "have a vacation".

2. **"Many to many" relationships.** The Job "hire an executive" can serve several higher Jobs: "retain employees", "grow sales", "take load off myself".

3. **The real graph is shaped by two factors:**
   - The graph structure baked in by the Solution's creators
   - The person's choice to do the Job with a specific Solution

4. **Different Solutions = different graphs.** "Have a beach vacation" on your own = one set of sub-jobs. Through a tour operator = a different set.

5. **You don't need to know ALL the Jobs.** To solve a Business task it's enough to know a small part of the graph — the relevant part.

### Critical Sequence

**The Critical Sequence is the set of Jobs a person CANNOT skip in order for the higher-level Job to get done.**

- Fixing problems in the Critical Sequence is always priority #1
- Failing a Job in the Critical Sequence → customer drop-off
- A CJM, in the limit, should match the Critical Sequence

### Additional Jobs

Jobs outside the Critical Sequence are additional. Their presence increases Value, but their absence does not cause drop-off.

### Tax Jobs

**Tax Jobs (налоговые работы)** are additional Jobs a person is FORCED to do because of problems with the Solution. Always a negative experience.

### Job properties in the graph

- **Frequency** — how often performed (matters for retention models)
- **Sequence** — order of execution (matters for conversion)
- **Virality** — performing the Job can yield cheap leads
- **Level** — the higher the level, the more strategically important

### Algorithm

1. **Take the top-level Job** (Big Job)
2. **Unpack the lower-level Jobs** — what must be done to perform the Big Job
3. **For each lower Job** unpack one more level (if needed)
4. **Mark the Critical Sequence** — which Jobs the Big Job cannot be done without
5. **Mark additional Jobs** — increase Value, but not critical
6. **For each Job note the current Solution and satisfaction**

---

## Response format

### Job Graph: [Big Job]

**Segment:** [for whom]
**Current Solution for the Big Job:** [which one]

#### Job tree

```
[Big Job] ★
├── [Core Job 1] ★ CRITICAL
│   ├── [Small Job 1.1] ★ CRITICAL
│   ├── [Small Job 1.2] ★ CRITICAL
│   └── [Small Job 1.3] — additional
├── [Core Job 2] ★ CRITICAL
│   ├── [Small Job 2.1] ★ CRITICAL
│   └── [Small Job 2.2] — additional
├── [Core Job 3] — additional
│   └── [Small Job 3.1]
└── [Core Job 4] — additional (viral)
```

★ = Critical Sequence

#### Critical Sequence

| Order | Job | Current Solution | Problems | Satisfaction |
|-------|-----|------------------|----------|--------------|
| 1 | ... | ... | ... | low/medium/high |
| 2 | ... | ... | ... | ... |

#### Additional Jobs

| Job | Current Solution | Value potential |
|-----|------------------|-----------------|
| ... | ... | low/medium/high |

#### Tax Jobs (if any)

| Job | Caused by which problem | Annoyance |
|-----|-------------------------|-----------|
| ... | ... | low/medium/high |

#### Opportunities from the graph

1. **Problems in the Critical Sequence:** [fix first]
2. **Jobs that can be killed:** [simplification]
3. **Jobs that can be combined:** [more Jobs with one Solution]
4. **Move up to a higher Job:** [strategic Mechanic]

**Next step:** To generate Value Mechanics over the graph — `/value-mechanics`. For segmentation — `/segment-find`.

---

**Jobs / product to build the graph for:**

$ARGUMENTS
