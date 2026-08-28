# 05 · Staffing Fundamentals

With organizations, workers, and business processes established, this
module walks through the three most common staffing transactions — hire,
transfer, and termination — conceptually, showing how each one exercises
the concepts from Modules 3 and 4 together. You won't need tenant access to
follow along: each walkthrough is written as a "what happens on screen and
why" narrative you can trace against the concepts already covered.

## Hire — creating a new worker record

A **Hire** transaction creates a brand-new worker record in Workday. The
exact data entry screens differ slightly depending on staffing model
(Module 3), but the conceptual shape is consistent:

| Stage | What's captured |
|---|---|
| **Position/job selection** | Which vacant position (Position Management) or new requisition (Job Management) the hire fills |
| **Worker identity** | Legal name, personal information, worker type (Employee or Contingent Worker) |
| **Employment details** | Start date, supervisory organization (inherited from the position), employee type (full-time/part-time), location |
| **Compensation** | Proposed pay, referencing the position's compensation grade (Module 6) |
| **Routing** | The Hire business process (Module 4) carries this through approvals |

A hire at Meridian Outfitters' DC West into a Position-Management org means
the hiring manager must pick an *existing vacant position number* — say,
Position #40218 — rather than inventing a new one; the position, its job
profile, and its compensation grade already exist and were created when DC
West's headcount was budgeted.

## Transfer — moving a worker between organizations

A **Transfer** moves an existing worker from one supervisory organization
to another — the Workday-native way of handling "this person now reports
somewhere else." Transfers come in two conceptual flavors:

| Type | What changes |
|---|---|
| **Move within the same job profile** | Same type of role, different org — e.g., a Store Associate moving from Store 104 to Store 118 |
| **Job Change combined with transfer** | Org *and* role change simultaneously — e.g., a Store Associate promoted into an Assistant Manager role at a different store |

Workday treats a pure transfer and a promotion-with-transfer as related but
distinct transaction types (both fall under the broader "Job Change"
business process family), because a pure transfer typically requires
lighter approval (a receiving-manager sign-off) while a promotion usually
requires compensation approval as well, per the conditional routing
concept from Module 4.

!!! info "Transfers don't erase history"
    When a worker transfers, Workday doesn't overwrite their old
    assignment — it creates a new, dated staffing event. Running a report
    "as of" a date before the transfer still correctly shows their old
    supervisory org and manager, which matters for historical headcount and
    tenure reporting (Module 7).

## Termination — ending a worker's employment

A **Termination** ends a worker's active employment, and — like hire and
transfer — is driven by a business process with its own conditional
routing. The two broad termination categories shape that routing:

| Category | Typical routing difference |
|---|---|
| **Voluntary** (resignation, retirement) | Standard exit-checklist approvals; often a longer notice-period lead time before the effective date |
| **Involuntary** (performance, reduction in force) | Usually adds HR/Legal review steps before the transaction can even be initiated by a manager |

A termination also triggers a set of **downstream effects** that aren't
part of the BP steps themselves but are consequences of the worker's status
changing: benefits eligibility ends (subject to legally required
continuation offers), payroll processes a final paycheck, system access
should be deprovisioned (often via an integration, previewed in Module 9),
and the worker's former position becomes vacant again (in Position
Management) or the requisition needed to backfill can be opened (in Job
Management).

## Worked example: a full staffing lifecycle for one worker at Meridian

Follow **Jordan Ellis** through Meridian Outfitters' staffing transactions
over roughly 18 months:

1. **Hire** — Jordan is hired into Position #40218, "Warehouse Associate,"
   at Distribution Center West, starting January. The Hire BP routes
   through the DC Director and HR Operations approvals from Module 4's
   worked example.
2. **Transfer** — After eight months, Jordan requests a move to Store 118
   (Boise) as a Store Associate — a role and org change, so this runs as a
   Job Change (transfer + role change combined), requiring both the losing
   manager's acknowledgment and the receiving store manager's approval.
3. **Promotion (Job Change)** — A year later, Jordan is promoted to
   Assistant Manager at the same store. Because this crosses a
   compensation-grade boundary, the conditional routing from Module 4
   inserts a compensation approval step alongside the standard promotion
   approval.
4. **Termination (Voluntary)** — Eighteen months after hire, Jordan resigns
   to relocate. The Termination BP routes through the standard exit
   checklist; Jordan's Assistant Manager position becomes vacant, appearing
   again in the store's open positions for the next hire.

Each of these four transactions is a distinct BP instance, but all four
reference the *same* underlying worker record, which is why a "worker
history" report on Jordan (Module 7) can show every one of these events on
a single timeline.

## Cheat sheet

| Transaction | What it does | Common approval pattern |
|---|---|---|
| Hire | Creates a new worker record into a position/job | Hiring manager → HR Partner/Ops |
| Transfer | Moves a worker to a different supervisory org | Losing manager → receiving manager |
| Job Change (promotion) | Role + often org change | Manager → HR → Compensation (if grade changes) |
| Termination (voluntary) | Ends employment, worker-initiated | Manager → HR exit checklist |
| Termination (involuntary) | Ends employment, employer-initiated | HR/Legal review → Manager executes |

## Exercise

Write a four-event staffing lifecycle (like Jordan Ellis's above) for a
fictional worker of your own at Meridian Outfitters, but change at least
one detail: make the transfer a pure lateral move instead of a
promotion-combined transfer, and make the termination involuntary instead
of voluntary. For each event, note which approvals you'd expect and why,
referencing the conditional-routing concept from Module 4.
