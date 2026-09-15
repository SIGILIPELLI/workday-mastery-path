---
description: "Advanced Staffing — Level 1 Module 5 covered hire, transfer, and termination as the three core staffing transactions under a single staffing model. This…"
---

# 03 · Advanced Staffing

Level 1 Module 5 covered hire, transfer, and termination as the three core
staffing transactions under a single staffing model. This module covers
what changes when staffing gets more complex: multiple concurrent
assignments, staffing model conversions, and the events that fall outside
the "one worker, one job" pattern.

## Concurrent jobs — one worker, multiple positions

Workday supports a worker holding **more than one active position
simultaneously** — common for part-time workers who split time across
departments, or academic/hourly settings. Each concurrent assignment is
its own position-assignment record with its own supervisory org, job
profile, and compensation, all hanging off the same single `Worker`
object.

| Concept | Detail |
|---|---|
| **Primary position** | The assignment driving default benefits eligibility, primary manager for Inbox routing, etc. |
| **Additional job(s)** | Extra concurrent assignments, each independently staffed and compensated |
| **Total compensation view** | Reporting can roll up pay across all concurrent assignments for one worker |

A Meridian Outfitters worker who is both a part-time Store Associate at
Store 104 and a part-time warehouse picker at a nearby DC would have two
separate position assignments, two separate supervisory-org relationships,
and two separate managers who each see that worker in their own team
roster — while HR reporting can still see them as a single person with
combined hours for overtime-eligibility purposes.

## Staffing model conversion

A supervisory org's staffing model (Position Management vs. Job Management,
from Level 1 Module 3) isn't necessarily fixed forever. Converting an org's
model — usually Job Management to Position Management, as an org matures
and wants tighter headcount control — is itself a governed change:

1. Every currently filled job in the org needs a **position created and
   backfilled** to match the existing assignment (a worker who was simply
   "in a job" now occupies a specific position number).
2. Future hires and transfers into the org now go through position-based
   staffing rules — a hiring manager must select a vacant position rather
   than open an unbounded requisition.
3. Historical records are unaffected — reports run for dates before the
   conversion still reflect the org's prior staffing model.

## Leave of absence and related non-terminating events

Some staffing events change a worker's status without ending employment
and without being a transfer:

| Event | Effect |
|---|---|
| **Leave of Absence** | Worker's status becomes inactive-but-employed; position may or may not be backfilled depending on policy |
| **Return from Leave** | Reverses the leave status, typically restoring the prior position (or a comparable one) |
| **Furlough** | Similar to leave, usually employer-initiated and often tied to a defined return date |

These matter for the same reason termination did in Level 1: each one
triggers downstream effects (benefits continuation rules, whether the
position shows as vacant) that are consequences of a status change rather
than steps explicitly coded into the leave business process itself.

## Worked example: a concurrent-job scenario at Meridian

Jordan Ellis (Level 1 Module 5's worked example, now an Assistant Manager)
takes on a second, part-time assignment coaching Meridian's seasonal
retail-training program, which sits in a different supervisory org
entirely:

1. **Additional Job** business process is initiated for Jordan, referencing
   the training program's supervisory org and its own position.
2. Jordan now has **two active position assignments**: Assistant Manager
   (primary) and Seasonal Trainer (additional), each with independent
   compensation.
3. Jordan's Assistant Manager manager continues seeing Jordan in the store
   roster; the training program lead separately sees Jordan in *their*
   roster — neither sees the other assignment unless granted broader
   reporting access.
4. When the training program ends, a **End Additional Job** transaction
   closes only that assignment; Jordan's primary Assistant Manager
   assignment is completely unaffected.

## How It Actually Works

Concurrent jobs are possible because a `Worker` object and a `Position`
assignment are separate objects related by a many-to-one — not
one-to-one — relationship record, the same additive relationship pattern
Level 1 Module 5 introduced for a single assignment, simply not
constrained to a maximum of one active record per worker.

**A worker having two active jobs is just two simultaneously valid
position-assignment records pointing at the same `Worker` object.** There
is no special "concurrent worker" object type — the underlying data model
never assumed one worker maps to one assignment. What Level 1 presented
as "the" staffing event was really the common case of exactly one active
assignment record; concurrent jobs are the same mechanism with the
uniqueness assumption relaxed. This is why ending Jordan's training
assignment doesn't touch the Assistant Manager assignment record at all —
they were always independent records that happened to reference the same
worker.

**A staffing model conversion works by generating position records to
match existing job-based assignments, then changing which rule set future
transactions are validated against — it does not rewrite history.** Job
Management orgs store staffing directly against the supervisory org and
job profile; Position Management orgs require an intermediate `Position`
object. Converting an org creates one `Position` record per currently
filled job, effective as of the conversion date, and points the existing
worker assignment at that new position going forward — while historical
assignment records before the conversion date remain job-based, exactly as
they were. A report spanning the conversion date sees the model change as
just another effective-dated fact, not a data migration that erased the
past.

**Leave of absence changes a status attribute on the assignment, exactly
like termination does — the difference is which attribute, and whether
the assignment record itself is expected to become active again.**
Termination sets an end-employment status that (absent rehire) is
expected to be permanent. Leave sets an inactive-but-employed status with
an expected return date, and Return from Leave simply flips that status
attribute back — using the same additive, non-destructive mechanism, just
without discarding the underlying assignment the way termination
conceptually retires a position.

## Cheat sheet

| Concept | One-line definition |
|---|---|
| Concurrent jobs | A worker holding more than one active position assignment simultaneously |
| Primary position | The assignment driving default benefits/manager/Inbox routing |
| Staffing model conversion | Changing an org from Job Management to Position Management (or vice versa) |
| Leave of Absence | A non-terminating status change with an expected return |

## Exercise

Design a concurrent-job scenario for a fictional Meridian worker of your
own who splits time between two departments with two different managers.
Then explain, referencing the "How It Actually Works" mechanism above,
why terminating one of the two assignments should never accidentally end
the worker's overall employment status.
