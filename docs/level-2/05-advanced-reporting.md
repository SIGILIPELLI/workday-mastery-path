---
description: "Advanced Reporting — Level 1 Module 7 covered running and reading standard reports. This module covers building custom reports — the report types…"
---

# 05 · Advanced Reporting

Level 1 Module 7 covered running and reading standard reports. This module
covers building **custom reports** — the report types, calculated fields,
and prompts that let a report answer a question no delivered report was
built for.

## Report types: the shape determines the capability

Workday's custom report builder offers several report types, and choosing
the right one determines what the report *can* do before a single field is
added:

| Report type | Best for |
|---|---|
| **Advanced** | The general-purpose workhorse — filters, sorting, grouping, most business needs |
| **Matrix** | Cross-tabulated summaries (e.g., headcount by region × job level) |
| **Composite** | Combining multiple sub-reports/data sources into one output |
| **Search** | Simple, fast lookups without heavy calculation |
| **BIRT/Report Writer (advanced)** | Highly formatted, print-ready layouts (offer letters, pay stubs) |

Picking Matrix for a "headcount by region and level" request is a design
decision, not a preference — an Advanced report can list every worker with
region and level as columns, but only a Matrix report natively cross-tabs
and subtotals both dimensions in one readable grid.

## Data sources and business objects

Every report is built against a **data source** — a defined starting
business object (Worker, Position, Supervisory Organization) plus the
related objects it can pull fields from through defined relationships.
Choosing "All Active and Terminated Workers" versus "Worker" as a data
source changes which population the report can even see before any filter
is applied — this is the report-level equivalent of the security scoping
from Level 1 Module 8, and in fact security is enforced on top of
whatever the data source technically permits.

## Calculated fields for reporting

A **calculated field** lets a report show a value that doesn't exist as a
stored field — computed from other fields using Workday's calculation
functions (concatenation, date math, lookups, conditional logic). Common
reporting calculated fields:

| Calculated field example | What it computes |
|---|---|
| **Tenure in years** | Date math between hire date and today (or termination date) |
| **Full name (Last, First)** | String concatenation of two stored fields |
| **Compa-ratio** | Current salary ÷ grade midpoint, expressed as a ratio |
| **Is Above Range flag** | Conditional: true if salary > grade maximum |

Module 5 in Level 3 covers calculated fields as their own topic in depth;
here, the point is that reports are one of the two main places (business
process condition rules being the other) where calculated fields get used.

## Prompts: letting the runner choose

A **prompt** turns a hardcoded filter into a runtime choice — instead of a
report always showing "Boise Store 118," a prompt lets whoever runs it
pick the store (or region, or date range) at run time, with the report's
underlying logic unchanged. Prompts are commonly security-scoped
themselves, so a Store Manager prompted for "Store" only sees their own
store(s) in the picker, consistent with Level 1 Module 8's scoping
concept.

## Worked example: a compa-ratio report for Meridian

Meridian's compensation team wants a report showing every worker whose
compa-ratio is below 0.85 (potentially underpaid relative to their grade),
grouped by region, with the region chosen at runtime:

1. **Data source**: Worker (active workers only).
2. **Calculated field**: `Compa-ratio = Current_Base_Salary /
   Compensation_Grade.Midpoint`, built once as a reusable calculated
   field.
3. **Filter**: `Compa-ratio < 0.85`.
4. **Prompt**: "Region" prompt, letting a regional HR partner run the same
   report scoped to just their region, with the picker itself limited to
   regions that partner has security access to.
5. **Report type**: Advanced, since the ask is a filtered list rather than
   a cross-tab.

The same report definition serves every regional HR partner at Meridian —
one build, many runtime scopes, because the prompt plus the underlying
security domain grants (Level 1 Module 8) jointly determine what each
runner actually sees.

## How It Actually Works

A custom report is a saved **query definition** — data source, fields,
filters, calculated-field expressions, and prompts — that gets re-executed
against live data every time it's run, layered underneath the same
security evaluation that governs every other access in the system.

**A calculated field is stored as an expression, not a value — it's
recomputed every single time the report runs.** The compa-ratio field
above has no stored "compa-ratio" column anywhere in Workday's data;
every time the report executes, it re-fetches each qualifying worker's
current salary and current grade midpoint and divides them at that
moment. This is why a compa-ratio report run today and again after next
month's compensation review can return completely different results with
zero report maintenance — the underlying salary and grade data changed,
and the calculation simply re-ran against the new values.

**Security is applied as a filter on top of the report's own logic, not
instead of it — a runner never sees data the report technically returns
but they lack a domain grant for.** The compa-ratio report's own filter
logic (`< 0.85`) determines the candidate population; independently, the
regional HR partner's security-group domain grants (Level 1 Module 8)
determine which of those candidates fall within their scope, using the
same domain/group/scope intersection mechanism described there. Both
layers run on every execution — the report doesn't "know" who's running
it and pre-filter accordingly; the security layer intersects with
whatever the report's own query would otherwise return.

**A prompt's picker options are themselves security-scoped, using the same
mechanism as the report's row-level security — which is why two different
people running the identical report definition see different prompt
choices.** The Region prompt isn't a static dropdown of all regions; its
option list is generated by querying which region-scoped organizations the
current runner has at least View access to, via the same domain-grant
resolution from Level 1 Module 8. A Store Operations Analyst scoped to one
region sees only that region in the prompt; an HRIS admin sees all of
them — from one saved report definition.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Data source | The starting business object and its reachable related fields |
| Calculated field | A computed value derived from other fields, evaluated at run time |
| Prompt | A runtime-selectable filter parameter, itself security-scoped |
| Matrix report | A report type for cross-tabulated, subtotaled summaries |

## Exercise

Design a Matrix report for Meridian showing headcount cross-tabbed by
region (rows) and job level (columns), with a prompt letting the runner
filter to active workers only or include terminated workers within the
last 12 months. Identify the data source you'd choose and one calculated
field the report would need that isn't a raw stored field.
