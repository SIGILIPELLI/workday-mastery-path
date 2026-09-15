---
description: "Payroll Fundamentals — This module covers Workday Payroll conceptually — how pay is actually calculated from the staffing, compensation, absence, and time…"
---

# 04 · Payroll Fundamentals

This module covers Workday Payroll conceptually — how pay is actually
calculated from the staffing, compensation, absence, and time tracking
inputs covered in earlier modules, without depending on any specific
country's payroll rules.

## Payroll inputs: what a pay calculation consumes

A payroll run doesn't invent data — it consumes already-resolved records
from other modules as inputs:

| Input | Source module |
|---|---|
| Base compensation | Level 1 Module 6 / Level 2 Module 4 (compensation plans) |
| Worked/overtime hours | Level 2 Module 9 (Time Tracking, calculated tags) |
| Paid absence hours | Level 2 Module 9 (Absence, calculated balances) |
| Tax elections, deductions | Worker-maintained payroll election data |
| One-time payments (bonus, correction) | Manually entered or business-process-triggered |

This input-consuming design is deliberate: payroll's job is to correctly
calculate and disburse pay from data that other modules have already
validated and approved, not to re-derive or second-guess whether a time
block or absence request was legitimate.

## Pay components and the earning/deduction model

Workday Payroll builds gross-to-net pay from **earnings** and
**deductions**, each defined as its own configurable object:

| Concept | Detail |
|---|---|
| **Earning** | A category of pay (Regular, Overtime, Bonus, Holiday) with its own calculation rule |
| **Deduction** | A category of withholding (tax, benefits premium, garnishment) with its own calculation rule and priority order |
| **Pay Component Group** | A named bundle of earnings/deductions used for reporting and calculation grouping |

Deduction **priority order** matters when gross pay isn't sufficient to
cover every deduction in full (e.g., a worker with a large one-time
correction reducing their check) — configured priority determines which
deductions are taken in full, prorated, or skipped for that pay period,
rather than leaving the outcome ambiguous.

## The payroll processing cycle

| Stage | What happens |
|---|---|
| **Pay period definition** | The recurring calendar (weekly, biweekly, monthly) driving when payroll runs |
| **Input collection/lockdown** | Time, absence, and one-time payment inputs are gathered and typically locked from further edits |
| **Gross-to-net calculation** | Earnings and deductions are calculated per worker |
| **Off-cycle vs. on-cycle** | On-cycle runs follow the regular calendar; off-cycle runs handle corrections or urgent payments outside it |
| **Payroll settlement/disbursement** | Final pay is disbursed (direct deposit, check) and results post to Financials (Level 3 Module 3's journal entries) |

## Retroactive pay

A **retroactive (retro) pay** situation arises when a compensation change
is approved with a past effective date — a raise approved this month but
effective last month, for example. Payroll must calculate the difference
between what was actually paid and what should have been paid for the
retroactive period, and issue the delta, typically in the next available
payroll run rather than reopening and reissuing a prior, already-
disbursed pay period.

## Worked example: retro pay for Jordan's promotion at Meridian

Jordan Ellis's promotion to Assistant Manager (Level 1 Module 5, Level 2
Module 4) is approved with an effective date two pay periods in the past,
because the Job Change business process took longer than expected to
route through approvals:

1. **Payroll detects the retro situation** — the compensation change's
   effective date falls within an already-processed, closed pay period.
2. **Retro calculation** — payroll calculates the difference between
   Jordan's old salary and new salary for the affected retroactive
   period(s), factoring in any hours-based components (overtime
   calculated on the new, higher rate, if applicable) for those periods.
3. **Delta included in the next on-cycle run** — rather than reopening the
   closed pay periods, the retro amount appears as a distinct earning line
   ("Retro Pay Adjustment") in Jordan's next regular paycheck.
4. **Financials posting** — the retro amount posts to the same cost
   center worktag (Level 3 Module 3) Jordan's regular pay would have used
   in the original periods, keeping historical financial reporting
   accurate to when the cost was actually incurred, not just when it was
   paid.

## How It Actually Works

Payroll calculation is a batch process that reads a snapshot of approved,
locked inputs for a specific period and applies configured calculation
rules — it doesn't recompute anything from first principles each pay
period, and closed periods are never edited in place.

**Input lockdown exists because payroll must calculate against a
stable, unchanging snapshot — not live data that could shift mid-
calculation.** Time Tracking and Absence balances (Level 2 Module 9) are
live, continuously recalculated values right up until a pay period locks.
Locking freezes that period's inputs into an immutable snapshot precisely
so a payroll calculation started at 2pm and a worker's late time-entry
edit at 2:05pm don't produce a race condition — the calculation runs
against the frozen snapshot, and any post-lock correction becomes input to
a *later* calculation (an off-cycle run, or the next on-cycle period),
never a silent retroactive change to a calculation already run.

**Retro pay is calculated as a delta against historical closed-period
data, then applied as new pay in a currently open period — closed
periods themselves are never reopened or rewritten.** When Jordan's raise
is retroactively effective, payroll doesn't go back and edit the two
already-disbursed paychecks. It reads those closed periods' historical
snapshot data (what was actually calculated and paid), recomputes what
*should* have been paid using the new compensation record's effective
date, and the difference becomes a new earning line item processed in the
next open period. This mirrors the effective-dated, additive discipline
from Level 1 Module 5 — the past record isn't overwritten, a new
compensating record is added on top of it.

**Deduction priority order is what makes gross-pay-insufficient scenarios
deterministic rather than ambiguous.** When gross pay for a period can't
cover every configured deduction in full, the calculation engine
processes deductions in their configured priority sequence — often
statutory taxes first, then benefits premiums, then voluntary deductions
— taking each in full until gross pay is exhausted, then prorating or
skipping lower-priority ones per their configured shortfall handling.
This is a configuration decision made once per deduction type, not a
judgment call made ad hoc during each affected pay run.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Earning / Deduction | Configurable categories building gross-to-net pay |
| Pay period lockdown | Freezing inputs into an immutable snapshot before calculation |
| Off-cycle run | A payroll run outside the regular calendar, for corrections/urgent pay |
| Retro pay | A calculated delta for a compensation change with a past effective date |

## Exercise

A Meridian worker's time entry for a shift two weeks ago is corrected
*after* that pay period has already locked and been paid. Using the
mechanism above, describe how this correction should be processed —
should the closed period be reopened, and if not, what happens instead?
Reference the retro pay pattern in your answer even though this is a
time-tracking correction rather than a compensation change.
