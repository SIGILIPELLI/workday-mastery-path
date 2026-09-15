---
description: "Compensation & Benefits Deep Dive — Level 1 Module 6 introduced compensation grades and ranges. This module goes deeper into how compensation packages are…"
---

# 04 · Compensation & Benefits Deep Dive

Level 1 Module 6 introduced compensation grades and ranges. This module
goes deeper into how compensation packages are actually assembled from
multiple plan types, and introduces benefits — a related but structurally
distinct configuration area.

## Compensation packages: grades, plans, and eligibility rules

A **Compensation Package** is the container assigned to a position or job
profile that determines which compensation **plans** a worker in that role
is eligible for. A package typically bundles several plan types:

| Plan type | Example |
|---|---|
| **Salary plan** | Base annual salary within the grade's range |
| **Hourly plan** | Hourly rate, for non-exempt roles |
| **Bonus plan** | Target bonus percentage, often tied to a performance cycle |
| **Stock/equity plan** | Grant guidelines for eligible levels |
| **Allowance plan** | Recurring non-salary payments (car allowance, remote-work stipend) |

A **Compensation Grade** (Level 1's building block) attaches to the salary
or hourly plan specifically, defining the min/mid/max range; the package
is the broader assembly that also decides *eligibility* — which plans
apply at all for a given job profile or management level.

## Guidelines, not hard caps

Compensation ranges are typically configured as **guidelines** rather than
hard system-enforced caps: Workday can be configured to warn (not block)
when a proposed salary falls outside the grade range, or to require
additional approval (Module 1's condition-rule mechanism) rather than
rejecting the transaction outright. This distinction matters because real
compensation decisions sometimes need to go outside guidelines for
retention or market reasons — the system's job is to make that visible and
governed, not to make it impossible.

## Benefits: plans, eligibility rules, and enrollment events

Benefits configuration is structurally similar to compensation but answers
a different question — not "how much is this worker paid" but "which
insurance/retirement/other benefit plans can this worker enroll in, and
when."

| Concept | Detail |
|---|---|
| **Benefit Plan** | A specific offering (e.g., "PPO Medical," "401(k)") with its own coverage/contribution rules |
| **Benefit Group** | A bundle of plans a population is eligible for, assigned via eligibility rules (often based on worker type, FTE, location) |
| **Enrollment Event** | The business-process-driven trigger for a worker to make elections — new hire, open enrollment, or a qualifying life event |

## Qualifying life events and enrollment windows

Benefits enrollment isn't always open — most changes are gated to specific
windows:

| Trigger | Window behavior |
|---|---|
| **New hire enrollment** | A fixed window (e.g., 30 days) from hire date, triggered automatically by the Hire BP |
| **Annual open enrollment** | A company-wide window, same for everyone, run once a year |
| **Qualifying Life Event (QLE)** | Marriage, birth, divorce — opens a short ad-hoc window tied to the life event's effective date, requiring supporting documentation in most configurations |

A worker who misses a window generally cannot enroll or change elections
until the next qualifying trigger — this rule enforcement is itself
handled through the benefits enrollment business process's condition
rules, the same mechanism from Module 1.

## Worked example: compensation and benefits for a Meridian promotion

Continuing Jordan Ellis's promotion to Assistant Manager (Level 1 Module
5): the promotion changes Jordan's compensation grade, which cascades
through both systems:

1. **Compensation package re-evaluation** — Assistant Manager's job
   profile carries a different, higher compensation package than Store
   Associate, so the Job Change BP (Module 1) evaluates Jordan's new
   eligible plans: base salary against the new grade's range, plus new
   eligibility for a bonus plan Store Associates don't receive.
2. **Benefits eligibility re-check** — if the promotion also changes
   Jordan's FTE or worker classification, it can trigger a benefits
   eligibility change, since eligibility rules commonly key off FTE
   thresholds (e.g., a benefits group requiring 30+ hours/week).
3. **Enrollment event, if triggered** — a benefits eligibility *increase*
   opens a limited enrollment window for the newly eligible plans, while a
   pure compensation change with no FTE change wouldn't touch benefits at
   all.

Note the two systems are related but not the same trigger: a raise alone
usually doesn't open a benefits window; a change in benefits *eligibility*
does.

## How It Actually Works

Both compensation and benefits resolve access to specific plans the same
way security resolves access to data (Level 1 Module 8) — via an
eligibility rule evaluated against the worker's current attributes, not a
static assignment stored on the worker.

**A compensation package's plan eligibility is recalculated at every
compensation-related event, not fixed at hire.** When Jordan is promoted,
Workday doesn't need a manual "unassign old plans, assign new plans" step
— the Job Change business process re-evaluates which compensation package
applies given Jordan's *new* position's job profile, and separately
re-evaluates which specific plans within that package Jordan is eligible
for given attributes like level and worker type. This is the same
resolve-at-request-time pattern as security domain grants: eligibility is
a live computation, not a stored roster.

**Benefits eligibility is a boolean function of current worker attributes,
evaluated on a schedule (nightly) and on every BP that could change an
eligibility-relevant attribute.** A benefits eligibility rule might read
"FTE ≥ 0.75 AND worker type = Employee." Nothing forces this rule to be
re-evaluated only when someone explicitly clicks "update benefits" — any
transaction that changes FTE (a Job Change, an FTE-adjustment BP) is
configured to trigger the same eligibility re-check, and a scheduled
batch process re-evaluates it tenant-wide as a safety net for changes made
outside a tracked business process (e.g., certain integration-driven
updates). This is why a promotion that happens to bump FTE from 0.6 to 1.0
can trigger a benefits enrollment window even though nobody directly
touched a "benefits" screen — the FTE change alone satisfied the
eligibility rule's condition.

**Enrollment windows are enforced by effective-dated event records, which
is what lets the system reject a late election without needing a human to
manually check dates.** A Qualifying Life Event enrollment event carries
its own trigger date and a calculated deadline (trigger date + configured
window length). Any election submission is checked against that stored
deadline at submission time; once past it, the same condition-rule
mechanism from Module 1 routes the submission to a "requires HR override"
path rather than silently accepting it — enforcing enrollment-window
policy is just one more condition rule attached to one more business
process step.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Compensation Package | The container deciding which comp plans a role is eligible for |
| Compensation Grade | The min/mid/max range attached to a salary/hourly plan |
| Benefit Group | A bundle of benefit plans assigned via eligibility rules |
| Enrollment Event | The BP-triggered window in which elections can be made |
| Qualifying Life Event | A life change that opens an ad-hoc, time-limited enrollment window |

## Exercise

Sketch an eligibility rule (in plain language) for a new "Wellness
Stipend" allowance plan at Meridian that should apply only to full-time
corporate employees (not store or DC hourly staff) above a certain job
level. Then explain which single worker attribute change would be most
likely to accidentally make a worker gain or lose eligibility for this
plan, and why that's worth testing in Sandbox before go-live.
