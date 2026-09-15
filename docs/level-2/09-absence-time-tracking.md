---
description: "Absence & Time Tracking — This module covers two related but distinct Workday modules: Absence Management (time off, leave balances) and Time Tracking…"
---

# 09 · Absence & Time Tracking

This module covers two related but distinct Workday modules: Absence
Management (time off, leave balances) and Time Tracking (recording hours
worked), and how both feed into payroll processing (Level 3 Module 4).

## Absence plans and accrual

An **Absence Plan** defines one type of time off (Vacation, Sick,
Personal) and the rules governing how a worker earns and uses it:

| Concept | Detail |
|---|---|
| **Accrual rule** | How balance builds over time (e.g., X hours accrued per pay period) |
| **Accrual cap** | Maximum balance a worker can hold before further accrual stops |
| **Carryover rule** | How much unused balance rolls into the next year, if any |
| **Eligibility** | Which worker population this plan applies to (often FTE- or country-based, same eligibility-rule mechanism as Module 4's benefits) |

Absence plans are frequently country-specific — legally mandated sick
leave minimums, for example, vary widely by jurisdiction, so a global
tenant maintains multiple plan variants gated by the worker's country,
using the same condition-rule pattern from Module 1.

## Time off requests as a business process

Requesting time off runs through its own BP type (again, Module 1's
engine): a worker submits a request, it's checked against their current
accrued balance, and — depending on configuration — routes to a manager
for approval or auto-approves if within policy. A request that would take
the balance negative is either blocked or flagged for exception approval,
depending on the plan's configured rules.

## Time tracking: entering and validating worked hours

**Time Tracking** captures actual hours worked, primarily for
non-exempt/hourly workers whose pay depends on hours rather than a fixed
salary:

| Concept | Detail |
|---|---|
| **Time entry** | Manual entry, a time clock, or a calculated schedule-based default |
| **Time calculation tags** | Rules that classify entered time into pay-relevant categories (regular, overtime, night differential) |
| **Time block** | A single recorded segment (e.g., 9am–1pm) that time calculation processes |
| **Approval** | Manager review/approval before hours are released to payroll |

Time calculation tags are what let the same raw clock-in/clock-out data
produce correctly categorized regular vs. overtime hours without a human
manually splitting the entry — a worker who clocks 10 hours in a day in a
jurisdiction with daily overtime rules gets 8 regular + 2 overtime hours
calculated automatically from the same entered time block.

## How absence and time tracking connect to payroll

Both modules exist largely to feed accurate inputs to payroll (Level 3
Module 4 covers payroll processing itself): approved time-off balances
determine whether a pay period includes paid or unpaid leave, and approved
time blocks with their calculated tags determine regular/overtime pay
calculations. Getting the approval and calculation steps right *before*
a payroll run is what Level 3's payroll module will assume is already
in place.

## Worked example: overtime and a sick day at Meridian's DC

A Distribution Center West warehouse associate at Meridian:

1. **Clocks in/out** across a week using Time Tracking, including one day
   with a 10-hour shift due to a rush order.
2. **Time calculation tags apply automatically** — the local jurisdiction's
   daily-overtime rule (over 8 hours/day = overtime) splits that 10-hour
   block into 8 regular + 2 overtime hours without manual intervention.
3. **Requests a sick day** later that week via Absence Management — the
   request checks against their accrued sick balance (built up via the DC's
   accrual rule), and — being within policy and balance — auto-approves
   without manager intervention, per the DC's configured plan.
4. **Payroll input** — at period close, the worker's pay calculation pulls
   both the calculated regular/overtime hours from Time Tracking and the
   approved paid sick hours from Absence Management as separate, already-
   resolved inputs.

## How It Actually Works

Both modules produce the same shape of output for payroll: discrete,
approved, dated records that a payroll run reads as inputs rather than
recalculates from scratch — keeping the "what happened" (attendance,
absence) separate from the "how it's paid" (payroll's own rules).

**Time calculation tags apply rule logic to raw time blocks at the moment
they're processed, not at data-entry time — so a policy change can
recalculate historical, unprocessed blocks correctly.** When the
warehouse associate clocks a 10-hour day, the raw entry stored is simply
"9am–7pm" (or similar); the split into 8 regular + 2 overtime hours is
produced by evaluating the jurisdiction's overtime calculation rule
against that raw block, generally at a scheduled calculation step rather
than instantly at clock-out. This separation is why correcting an
overtime rule's threshold and reprocessing time already entered (but not
yet paid) retroactively recalculates the correct split, without needing
every affected time block re-entered by hand.

**Absence balances are a running total derived from accrual events and
usage events, not a single number a request decrements directly.** A
sick-day request doesn't subtract from one stored "current balance"
field; it's checked against a calculated balance (sum of all accrual
records to date, minus sum of all approved usage records to date), the
same additive, effective-dated pattern from Level 1 Module 5 applied to
leave balance instead of staffing. This is why an accrual-cap change or a
correction to a past accrual record correctly ripples forward into every
later balance calculation without a manual balance recalculation step —
the "current balance" was never stored, only computed.

**Both time tracking and absence route through the standard BP engine for
their approval steps, meaning the same eligibility and condition-rule
concepts from earlier modules govern auto-approval versus manager review
here too.** The sick-day auto-approval above isn't a special "absence
system" behavior — it's a condition rule (requested amount ≤ available
calculated balance) attached to the Time Off Request BP's approval step,
evaluated exactly the way Module 1's compensation-approval condition
rule was, just on absence-specific fields.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Accrual rule | How an absence balance builds over time |
| Time calculation tag | Rule classifying raw time blocks into regular/overtime/etc. |
| Time block | A single recorded segment of worked time |
| Calculated balance | A leave balance derived from accrual and usage records, not stored directly |

## Exercise

Design an absence plan eligibility rule (plain language) for a new
"Volunteer Day" paid time-off plan at Meridian that should apply only to
full-time corporate workers with at least one year of tenure. Then explain
why this plan's balance calculation, once built, would automatically
handle a worker crossing the one-year tenure mark mid-year without any
manual balance adjustment — referencing the calculated-balance mechanism
above.
