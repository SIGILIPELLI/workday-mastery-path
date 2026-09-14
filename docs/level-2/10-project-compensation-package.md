# 10 · Project — Configure a Compensation Package

This module is a project, not a new concept lesson: apply Level 2's
compensation, eligibility, and security concepts together by designing a
full compensation package for a new job profile at Meridian Outfitters,
the way a configurer would scope one before building it in a tenant.

## The scenario

Meridian Outfitters is launching a new job profile, **Regional Merchandising
Specialist** — a field role between Store Associate and District Manager
level, responsible for visual merchandising standards across a small
cluster of stores. It needs its own compensation package, since it doesn't
cleanly fit the existing Store Associate or Assistant Manager packages
from Module 4's worked example.

## Part 1 — Define the compensation grade

Using Level 1 Module 6 and Module 4's concepts, specify:

- A compensation grade name and its position in Meridian's overall grade
  ladder (above Store Associate, below Assistant Manager — where exactly,
  and why).
- A min/mid/max salary range for the grade, expressed as reasonable
  numbers relative to the two neighboring grades you're placing it
  between.
- Whether the range should be enforced as a hard cap or a guideline
  (Module 4's distinction), and what condition-rule-driven approval should
  exist for exceptions (Module 1's mechanism) — for example, requiring
  regional VP approval for any offer above the range max.

## Part 2 — Assemble the compensation package

Decide which plan types (Module 4's package concept) this role is
eligible for, and justify each inclusion or exclusion:

- Salary plan — yes/no, tied to the grade from Part 1.
- Bonus plan — should this field-based role have a bonus plan, and if so,
  what would it reasonably be tied to (e.g., cluster-level merchandising
  compliance scores) versus a company-wide metric?
- Allowance plan — this role likely travels between stores; would a
  travel/mileage allowance plan make sense, and how would eligibility be
  scoped so store-based roles that don't travel aren't inadvertently
  included?
- Stock/equity plan — justify inclusion or exclusion based on job level.

## Part 3 — Eligibility rule and approval routing

Write the eligibility rule (plain-language, Module 4-style) that
determines which workers or positions this compensation package applies
to, being specific enough that it wouldn't also accidentally sweep in an
unrelated job profile at a similar pay level. Then design the approval
routing (Module 1-style) for compensation change requests using this
package: who approves a starting offer within range, and who additionally
approves one above range.

## Part 4 — Security scoping

Using Level 1 Module 8 and Module 7's deep-dive concepts, specify:

- Which security group(s) should have View access to Regional
  Merchandising Specialist compensation data, and at what scope
  (company-wide, or constrained to the specialist's assigned region).
- Whether an intersection group (Module 7) is warranted here, and if so,
  what the two constituent groups would be.
- One domain this compensation data should sit in, and why keeping it
  separate from (or combined with) the general Worker Data: Compensation
  domain from Level 1 Module 8 makes sense for this specific role.

## Part 5 — Report to verify

Sketch a custom report (Module 5-style) that Meridian's compensation team
could use to audit this new package after launch — for example, a report
listing every worker assigned to the new grade with their current salary,
compa-ratio (Module 5's calculated field), and whether their salary falls
above, within, or below the guideline range. Specify the report type,
data source, at least one calculated field, and one prompt you'd add so
different regional partners can scope the same report to their own
region.

## Deliverable

Write up your answers to Parts 1–5 as a single structured document (grade
definition, package table, eligibility rule text, approval routing
diagram or table, security grants table, and report spec) — the same
shape of artifact a real Workday configurer would hand to a project lead
before building any of it in a Sandbox tenant. There's no single correct
answer; the goal is internally consistent, well-justified design decisions
that correctly apply every Level 2 concept module to one concrete new job
profile.
