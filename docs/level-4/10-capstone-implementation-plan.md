---
description: "Capstone — Full Workday Implementation Plan — This is the path's capstone project: design a complete, end-to-end implementation plan for a realistic new…"
---

# 10 · Capstone — Full Workday Implementation Plan

This is the path's capstone project: design a complete, end-to-end
implementation plan for a realistic new Workday deployment, applying
concepts from every level — organization design, security, business
processes, compensation, reporting, integrations, and the
architecture/change-management/PM concepts from Level 4 — to one coherent
scenario.

## The scenario

Meridian Outfitters has acquired **Cascade Supply Co.** (introduced in
Level 4 Module 1), a 25-store regional retailer running its own separate
HR system (not Workday). Unlike Level 4 Module 1's tenant-consolidation
scenario, this time Cascade is being brought onto Workday **for the first
time** — a genuine implementation project, not a tenant migration. You
are the Solution Architect responsible for the implementation plan.

## Part 1 — Architecture and organization design

- Decide: does Cascade get its own tenant or join Meridian's existing
  consolidated tenant (Level 4 Module 1's decision framework)? Justify
  your choice.
- Design Cascade's supervisory org hierarchy as it would sit within (or
  alongside) Meridian's existing structure, including at least one
  decision about staffing model (Position vs. Job Management, Level 1
  Module 3) for Cascade's stores.
- Identify one high-cost-to-change decision (Level 4 Module 8) specific
  to this integration and flag how you'd get executive sign-off on it
  during the Architect phase.

## Part 2 — Security design

- Design the security group structure for Cascade's workers — do they
  join Meridian's existing role-based groups (Level 4 Module 1's
  extension pattern), or need new groups? Justify per group.
- Identify one domain where Cascade's data should be *more* restricted
  than Meridian's existing equivalent population during a transition
  period (e.g., Cascade compensation data visible only to a small
  integration team until data validation is complete), and design that
  as a security segment (Level 3 Module 2).

## Part 3 — Compensation and staffing

- Cascade's existing pay grades don't map cleanly onto Meridian's.
  Design the grade reconciliation approach (referencing Level 4 Module
  1's worked example) and specify how you'd handle Cascade workers whose
  current pay falls above the reconciled grade's guideline maximum
  (Level 2 Module 4's guideline-vs-cap distinction).
- Design the bulk hire/data-load approach for bringing all 25 stores'
  workers onto Workday on day one (Level 2 Module 6's EIB bulk-load
  pattern), including one validation check you'd run before the final
  load.

## Part 4 — Business processes and integrations

- Identify one Cascade-specific business process nuance (e.g., a
  different termination notice policy) that should become a condition-
  rule branch (Level 2 Module 1) on Meridian's existing BP definitions,
  rather than a separate BP definition.
- Cascade currently uses a third-party payroll vendor Meridian doesn't
  use. Decide (Level 3 Module 1's EIB-vs-Studio-vs-Cloud-Connect
  framework) how you'd integrate with it during a transition period
  before potentially migrating Cascade onto Meridian's payroll approach.

## Part 5 — Testing, rollout, and change management

- Specify at least three regression test scenarios (Level 3 Module 9)
  you'd run before migrating Cascade's configuration to Production,
  including at least one edge case involving a worker mid-transaction
  at go-live.
- Design a phased rollout plan (Level 4 Module 2's wave-based pattern,
  adapted from countries to stores/regions) rather than a single
  big-bang cutover for all 25 stores simultaneously — justify the phasing.
- Write a stakeholder map and communication plan (Level 4 Module 7) for
  Cascade's store managers, who are being introduced to an entirely new
  system, not just a changed process within a system they already know.

## Deliverable

Assemble Parts 1–5 into one complete Implementation Plan document — the
capstone artifact of this entire path. It should read as a coherent,
internally consistent plan a real Workday Solution Architect could hand
to an executive steering committee (Level 4 Module 8's governance
structure) for review, correctly applying and cross-referencing concepts
from Level 1 through Level 4. There is no single correct answer; the
goal is demonstrating that you can reason about a novel, multi-faceted
scenario the way this path's every prior worked example and exercise has
asked you to — and that you can now do it end to end, across the whole
platform, without a module boundary telling you which concept to reach
for next.

Congratulations on completing the Workday Mastery Path.
