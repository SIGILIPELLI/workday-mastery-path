---
description: "Workday Prism Analytics — This module covers Workday Prism Analytics — the platform's data-lake and advanced-analytics layer for blending Workday data…"
---

# 04 · Workday Prism Analytics

This module covers Workday Prism Analytics — the platform's data-lake
and advanced-analytics layer for blending Workday data with external data
sources, going beyond what the standard reporting engine (Level 2 Module
5) is designed for.

## Why Prism exists alongside standard reporting

Level 2 Module 5's custom reports are excellent at querying and
presenting Workday's own transactional data live. What they're not built
for: blending Workday data with large volumes of **external, non-Workday
data** (point-of-sale transaction logs, external market data, data from
other enterprise systems) at scale, or performing heavier analytical
transformations than a calculated field (Level 3 Module 5) reasonably
supports. **Prism** fills that gap as a dedicated data-preparation and
analytics layer within the same tenant.

## Prism's core building blocks

| Concept | Purpose |
|---|---|
| **Data Source** | Raw data loaded into Prism — from a Workday report, an external file, or another connected system |
| **Dataset** | A defined, transformed table built from one or more data sources, ready for analysis |
| **Data Change Tasks** | Transformation steps (join, filter, aggregate) applied to build a dataset from raw sources |
| **Discovery Board** | Prism's visual analytics/dashboarding layer for exploring a finished dataset |

## Blending Workday and external data

A typical Prism use case joins a Workday-native dataset (e.g., store
staffing and labor cost, sourced from a standard report) with an external
dataset (e.g., daily point-of-sale revenue per store, loaded from the
retail systems team) to answer a question neither system alone can — for
example, labor cost as a percentage of revenue, per store, per day.
Because the join happens inside Prism rather than requiring a separate
external BI tool to pull from two disconnected sources, the resulting
dataset can still respect Workday's security model for the Workday-native
portion of the data.

## Prism vs. exporting to an external BI tool

Some enterprises still export Workday data to an external business
intelligence platform for advanced analytics. Prism's advantage is
keeping the analysis **inside the security boundary already established**
for the tenant — a Prism dataset built from compensation data, for
instance, can still be scoped so only appropriately privileged users can
build or view dashboards against it, using the same domain/group model
(Level 1 Module 8) rather than needing a parallel access-control setup in
a separate BI product.

## Worked example: labor cost vs. revenue at Meridian

Meridian's finance and operations leadership want a per-store, per-day
view of labor cost as a percentage of revenue — a metric no single
existing report can produce, since labor cost lives in Workday and
revenue lives in the point-of-sale system:

1. **Workday-native data source**: a custom report (Level 2 Module 5)
   extracting daily labor hours and cost by store, using Time Tracking
   (Level 2 Module 9) and compensation data.
2. **External data source**: a daily revenue-by-store file loaded from
   the point-of-sale system into Prism.
3. **Data Change Task**: join the two sources on store identifier and
   date, then calculate `Labor Cost % = Labor_Cost / Revenue` as a
   dataset-level transformation.
4. **Discovery Board**: a dashboard surfacing this metric by store,
   region, and trend over time, security-scoped so regional operations
   leaders see only their own region's stores — reusing the same
   organization-based scoping concept from Level 1 Module 8, applied to
   a Prism dataset instead of a standard report.
5. **Outcome**: operations leadership identifies stores with
   disproportionately high labor cost relative to revenue, feeding into
   staffing model decisions (Level 2 Module 3) for those specific stores.

## How It Actually Works

Prism operates as a separate data-processing layer from the live
transactional query engine standard reports use, which is precisely what
lets it handle heavier joins and external data volumes without competing
with live transactional performance, while still inheriting the tenant's
security model where the data originated from Workday.

**A Prism dataset is materialized (computed and stored as its own table)
rather than recalculated live on every view, which is the opposite
tradeoff from a standard report's live-query model.** Level 2 Module 5
established that a custom report recalculates against live data every
time it runs. A Prism dataset, once built via its Data Change Tasks, is
processed and stored as a static table until explicitly refreshed —
appropriate for Prism's use case, since joining large external datasets
live on every dashboard view would be far too slow, and the underlying
external data (yesterday's point-of-sale revenue) isn't changing minute
to minute the way live transactional Workday data can. This is a
deliberate architectural difference, not an oversight: Prism trades
some data freshness for the ability to handle scale and complexity a
live query engine isn't designed for.

**Security scoping on a Prism dataset built from Workday-sourced data can
still enforce the originating domain/group/scope grants, because the
Workday-native portion of the dataset carries its security context
forward into Prism rather than being flattened into an anonymous table.**
When the labor cost report's Workday-native source data (compensation,
staffing) is loaded into Prism, the platform can preserve the security
context needed to continue scoping visibility by the same domain grants
(Level 1 Module 8) that governed it inside Workday — which is the
specific design reason Prism is positioned as staying "inside the
security boundary" rather than being equivalent to exporting to an
ungoverned external spreadsheet, where that context would be lost the
moment the export happened.

**Joining external, non-Workday data alongside Workday data works because
Prism treats both as equally valid data sources feeding into the same
dataset-building pipeline — external data isn't a second-class or
differently-processed input.** The point-of-sale revenue file and the
Workday-native labor report are both simply "data sources" from Prism's
perspective, joined by a Data Change Task the same way two Workday-native
sources would be. This uniform treatment is what makes Prism genuinely
useful for blended analytics rather than being restricted to Workday
data alone — the join, filter, and aggregate transformation mechanisms
don't distinguish where a given data source originated.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Data Source | Raw data loaded into Prism, from Workday or external systems |
| Dataset | A transformed, materialized table built for analysis |
| Data Change Task | A transformation step (join, filter, aggregate) building a dataset |
| Discovery Board | Prism's dashboard/exploration layer for a finished dataset |

## Exercise

Design a Prism dataset for Meridian blending Workday's Recruiting
time-to-fill data (Level 3 Module 6) with an external dataset of local
unemployment rates by region, to analyze whether hiring speed correlates
with local labor market conditions. Identify the join key you'd use, and
explain why this analysis is better suited to Prism than to a standard
custom report, referencing the live-query-vs-materialized distinction
above.
