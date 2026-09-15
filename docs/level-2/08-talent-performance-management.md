---
description: "Talent & Performance Management — This module introduces Workday's Talent and Performance functionality — the modules governing performance reviews…"
---

# 08 · Talent & Performance Management

This module introduces Workday's Talent and Performance functionality —
the modules governing performance reviews, goals, talent calibration, and
succession planning, building on the staffing and organization concepts
from earlier modules.

## The performance review cycle

A **Performance Review** in Workday runs as its own business process
(same engine as Hire or Job Change from Module 1), typically structured
around a recurring **review period**:

| Stage | What happens |
|---|---|
| **Goal setting** | Worker (often with manager input) sets goals for the period, stored against the worker's record |
| **Self-evaluation** | Worker rates themselves against goals and competencies |
| **Manager evaluation** | Manager rates the worker, often after seeing the self-evaluation |
| **Calibration** (optional) | A cross-team review adjusting ratings for consistency before finalization |
| **Share/acknowledge** | Final review shared with the worker, who acknowledges receipt |

Because this runs through the standard BP engine, the same condition-rule
mechanism from Module 1 can route reviews differently by worker
population — for example, requiring calibration only for management-level
reviews, or skipping self-evaluation for workers hired within the last 30
days of the period.

## Goals and competencies as reusable objects

**Goals** can be worker-authored (individual, specific) or **cascaded**
from an organizational goal down through the supervisory hierarchy — a
company-wide goal ("reduce customer wait time by 15%") cascaded to
relevant store-level goals that roll up to it. **Competencies** are a
separate, reusable library of skill/behavior definitions (e.g.,
"Customer Focus," "Team Leadership") that review templates reference,
letting the same competency be rated consistently across every review
template that includes it, tenant-wide.

## Talent calibration and 9-box grids

Beyond individual reviews, Workday supports **talent calibration** —
comparing workers against each other, commonly visualized on a **9-box
grid** plotting performance against potential:

| Axis | Represents |
|---|---|
| **Performance (horizontal)** | How well the worker has performed against goals/expectations |
| **Potential (vertical)** | Assessed capacity for growth into larger roles |

The 9-box isn't a stored field on the worker — it's a calculated placement
derived from the worker's performance rating and a separately captured
potential assessment, letting talent reviewers visually identify
succession candidates (high-potential, high-performance) versus workers
needing development support.

## Succession planning

**Succession plans** attach candidate pools to specific positions or job
profiles considered critical — "who could step into the Regional Director
role if it opened tomorrow." Candidates are tagged with readiness levels
(ready now, ready in 1-2 years, ready in 3+ years), and succession data
draws directly on the same talent/performance data (9-box placement, goal
history) rather than being entered as an entirely separate assessment.

## Worked example: Meridian's annual review cycle

Meridian runs an annual performance cycle for all corporate and management
workers:

1. **Goal setting opens** in Q1, cascading Meridian's company-wide goal
   ("expand into 5 new markets") down to relevant regional and store
   management goals.
2. **Mid-year check-in** — an optional interim review step, configured via
   condition rule to apply only to management-level job profiles, not
   hourly store associates.
3. **Year-end self and manager evaluation** — Jordan Ellis (now Assistant
   Manager, Level 1/Module 5's worked example) completes a self-evaluation
   against their store-level goal, then their manager evaluates.
4. **Calibration** — district managers meet to calibrate ratings across
   all Assistant Managers in the district before ratings are finalized,
   ensuring one manager's "exceeds expectations" means roughly the same
   thing as another's.
5. **9-box placement** — Jordan's finalized performance rating plus a
   separately captured potential assessment places them on the district's
   9-box grid, feeding into the succession plan for the District Manager
   role.

## How It Actually Works

Performance and talent data are built from the same effective-dated,
relationship-based data model as staffing and organization data — a
review, a goal, and a 9-box placement are each their own dated records
tied to the `Worker` object, not fields overwritten in place.

**A performance review is a business process instance, and its multi-step
structure (self-eval, manager-eval, calibration) is the same state
machine from Module 1 applied to talent data instead of staffing data.**
Skipping self-evaluation for a recent hire works exactly like skipping the
Works Council step for a non-German termination in Module 1's example —
a condition rule (worker's hire date vs. review period start) evaluated at
that step boundary determines whether the step activates for that specific
instance. This is why the same review template can silently behave
differently for a recently hired worker without a separate, duplicated
template being configured for that population.

**A worker's 9-box position is calculated at the moment it's viewed, from
the two underlying rating records, not stored as a coordinate.** Because
performance rating and potential assessment are captured as separate,
independently timestamped records (potential is often assessed less
frequently than performance), the 9-box grid is a live cross-reference of
"most recent finalized performance rating" × "most recent potential
assessment" for each worker in the population being viewed — which is
also why a grid viewed today can shift for a worker whose potential was
just reassessed, without any change to their last performance rating.

**Cascaded goals maintain a parent-child relationship between goal
records, so progress can roll up without each level's goal being a copy of
the one above it.** The company-wide "expand into 5 new markets" goal and
a store-level cascaded goal are two distinct goal records linked by a
parent reference, similar in spirit to the organization-hierarchy parent
relationship from Module 2. Rolling up progress means walking that
parent-child goal tree and aggregating child goal status — not literally
copying the parent goal's text down to every store, which would make
updating the top-level goal require touching every cascaded copy.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Cascaded goal | An org-level goal propagated down the hierarchy as linked child goals |
| Competency | A reusable skill/behavior definition referenced by review templates |
| 9-box grid | A calculated performance × potential placement, not a stored field |
| Succession plan | Candidate pool with readiness levels attached to a critical position |

## 🔀 Related lessons on other tracks

- [Product Lead — Performance Management for PMs](https://sigilipelli.github.io/product-lead-mastery-path/level-2/03-performance-management/)

## Exercise

Design a condition rule (in plain language) for Meridian's review cycle
that requires calibration only for supervisory organizations with more
than 15 direct reports, skipping it for smaller teams. Then explain how
this condition rule's evaluation is mechanically identical to the
compensation-approval condition rule from Module 1, despite applying to a
completely different business process type.
