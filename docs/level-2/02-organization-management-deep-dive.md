---
description: "Organization Management Deep Dive — Level 1's Module 3 introduced supervisory organizations as the backbone of staffing. This module goes deeper into the…"
---

# 02 · Organization Management Deep Dive

Level 1's Module 3 introduced supervisory organizations as the backbone of
staffing. This module goes deeper into the *other* organization types
Workday maintains alongside supervisory orgs, and the mechanics of how a
reorganization actually gets executed without losing history.

## Beyond supervisory orgs: the other organization types

Supervisory organizations answer "who reports to whom." Several other
organization types answer different questions, and a worker or position
can belong to more than one simultaneously:

| Organization type | Answers |
|---|---|
| **Cost Center** | Which budget/ledger bucket does this worker's cost roll up to? |
| **Company** | Which legal entity employs this worker? |
| **Location Hierarchy** | Where physically does this worker/org sit? |
| **Matrix Organization** | A secondary, non-supervisory reporting line (e.g., dotted-line to a regional director) |
| **Region/Custom Organization** | Tenant-defined groupings for reporting (e.g., "West Region") that don't drive approvals |

A single worker at Meridian Outfitters' Boise store therefore sits inside
several organizational memberships at once: a supervisory org (reports to
the store manager), a cost center (Boise Store 118 budget), a company
(the US legal entity), and a location (the physical Boise address) — each
independently maintained, each usable as a reporting or security-scoping
dimension.

## Organization assignment: how a worker ends up "in" an org

Workers don't get organization membership assigned to them directly for
most of these types — they *inherit* it through their position or
supervisory org assignment:

- A **Cost Center** is typically assigned to a supervisory organization (or
  a position within it), and every worker in that org inherits the cost
  center by default, though position-level overrides are possible for
  matrixed cost allocation.
- **Company** is usually inherited from the supervisory org's own company
  assignment, which is why moving a worker across a company boundary is
  treated as a more significant event (often triggering a termination +
  rehire pattern rather than a simple transfer, for legal/payroll reasons).
- **Location** can be assigned at the position level directly (e.g., a
  remote worker's home location differs from their team's default).

## Reorganizations: splitting, merging, and moving orgs

A **reorganization** changes the org hierarchy itself, not just who's
staffed where. The common patterns:

| Reorg type | What happens |
|---|---|
| **Move** | An existing supervisory org is re-parented under a different superior org |
| **Split** | One org's staff and positions are divided into two new orgs |
| **Merge** | Two orgs' staff and positions combine into one |
| **Create/Inactivate** | A new org is stood up, or an existing one is retired (never deleted) |

Every one of these is executed through the **Reorganization** business
process type, which — like any BP — is effective-dated. This matters
because a reorg doesn't happen "in the system right now" so much as it
happens "as of a future or past effective date," letting a tenant plan a
January 1st reorg in November and preview its effects before it goes live.

## Worked example: splitting a Meridian region

Meridian Outfitters' "West Region" supervisory org has grown to 40 stores
and leadership decides to split it into "Pacific Northwest" and
"Southwest" as of the next fiscal year. The reorg:

1. **Create two new supervisory orgs**, "Pacific Northwest Region" and
   "Southwest Region," each with a designated manager, effective the
   fiscal-year start date.
2. **Move each existing store's supervisory org** under the appropriate
   new regional org (a bulk move, often done via EIB — Module 6 — rather
   than one-by-one).
3. **Reassign cost center and reporting-dimension defaults** if the split
   also implies new budget buckets per new region.
4. **Preview headcount and reporting impact** before the effective date,
   confirming Level 1 Module 7-style reports still reconcile (old West
   Region total headcount should equal the sum of the two new regions'
   headcount on the split date).

Because the change is effective-dated, running a headcount report for last
year still correctly shows the single "West Region," while the same report
run for next year shows the two new regions — no historical data was
rewritten.

## How It Actually Works

Every organization in Workday is itself an object with its own identity,
and a supervisory org's "parent" is not a fixed structural slot but an
effective-dated **relationship record** to another org object — the same
additive, effective-dated pattern Level 1 Module 5 described for staffing
events, applied one level up to the organizational structure itself.

**Moving an org doesn't edit the org — it adds a new parent-relationship
record with a future effective date.** When "Boise Store 118" moves from
"West Region" to "Pacific Northwest Region," Workday doesn't overwrite a
"parent org" field on the Boise org object. It inserts a new dated
organization-hierarchy record establishing Boise's new parent as of the
split's effective date, while the prior record (Boise under West Region,
valid up to that date) remains intact. Resolving "who is Boise's parent
org today" is, again, a calculated lookup: find the hierarchy record whose
effective date is the latest one on or before the date in question. This
is exactly why the historical headcount report above returns the old
structure for last year without any special handling — it's asking the
same "as of" question Level 1 Module 5 covered for staffing events, just
against organization-hierarchy records instead of worker-assignment
records.

**Inherited attributes (cost center, company) are resolved by walking the
live hierarchy at query time, not copied down and stored on the worker.**
A worker doesn't carry a stored "cost center" value that a reorg would need
to bulk-update. Instead, any process needing a worker's cost center walks
up from the worker's current supervisory org assignment to find the
nearest ancestor org with a cost center override, evaluated fresh every
time. This is why splitting West Region's cost center assignment
automatically re-derives correct cost allocation for every affected
worker the moment the org-level reorg is effective — no worker-by-worker
update was needed, because no worker-level copy of the value ever existed.

**A reorg being "just another business process" is what makes it
effective-dated, previewable, and auditable like every other transaction.**
Because Reorganization runs through the same BP engine (Module 1's state
machine) as Hire or Termination, it inherits condition rules (e.g.,
requiring Finance approval above a headcount threshold), an approval
chain, and a permanent transaction history — a reorg isn't a special
administrative override outside the normal governance model, it's the
same model applied to organization-hierarchy relationship records instead
of worker-assignment records.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Cost Center | Organization type driving budget/ledger rollup |
| Company | Organization type representing the legal employing entity |
| Matrix Organization | A secondary, non-supervisory reporting relationship |
| Reorganization | The effective-dated BP that changes org hierarchy relationships |
| Inheritance | A worker's cost center/company is resolved by walking the live org hierarchy, not stored per worker |

## Exercise

Meridian is merging two underperforming stores' supervisory orgs into one
as of next quarter. List the sequence of steps you'd take (in the pattern
of the split example above), and explain in one paragraph why a headcount
report run for a date *before* the merge should still show two separate
stores even after the merge BP has been fully processed and approved.
