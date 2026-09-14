# 01 · Advanced Business Process Configuration

Level 1's Module 4 treated a business process (BP) as a black box: steps
route, conditions branch, approvals land in an Inbox. This module opens the
box — how a BP definition is actually assembled from steps, condition
rules, and routing logic, and how a configurer changes that behavior
without touching code.

## The anatomy of a BP definition

Every Workday business process type (Hire, Job Change, Termination,
Compensation Change, and dozens more) has a **BP definition** built from an
ordered sequence of **steps**, each of which is one of a small set of step
types:

| Step type | Purpose |
|---|---|
| **Action step** | Someone performs a task (fill in data, make a decision) |
| **Approval step** | Someone approves or sends back the transaction |
| **Notification step** | Informs someone, without requiring action |
| **Approval chain** (multi-step) | A sequence of approvals, often role-based, that can itself branch |

Steps are attached to the BP definition for a specific **business process
type + subtype**, and a tenant can maintain multiple definitions of the
same BP type — for example, a "Hire" definition scoped to one country and
a different "Hire" definition scoped to another, each with its own step
sequence, selected by the **condition rules** covered next.

## Condition rules — the branching engine

A **condition rule** is a reusable, named boolean expression evaluated
against fields on the business process instance (and the objects it
touches) at runtime. Two things happen with a condition rule once defined:

- **Step conditions** — attach the rule to a step so that step only runs
  when the rule evaluates true (Level 1's compensation-approval-on-
  promotion example is exactly this).
- **Eligibility rules on the BP definition itself** — decide which of
  several BP definitions for the same type applies to a given transaction,
  based on organization, worker type, or country.

Condition rules are built from **calculated fields** (Module 5 goes deep on
these) — for a promotion example, the rule compares
`Proposed_Position.Compensation_Grade` to
`Current_Position.Compensation_Grade` and returns true when they differ.
Because the rule is a named, reusable object, the same "grade changed"
rule can drive step conditions across Job Change, Transfer, and Request
Compensation Change definitions without being rewritten three times.

## Approval chains and dynamic routing

An approval step doesn't have to name one fixed approver. Workday resolves
approvers dynamically using **routing rules** — most commonly, roles
resolved relative to the worker or organization on the transaction:

| Routing pattern | Resolves to |
|---|---|
| **"Manager"** | The worker's current manager, walked from the supervisory org hierarchy |
| **"Manager's manager"** | One level further up the same hierarchy |
| **Role on organization** | Whoever holds the "HR Partner" role assignment for the relevant supervisory org |
| **Specific security group** | Any member of a named group (used when the approval isn't tied to one org) |

Because routing is resolved at runtime against live organizational data,
reorganizing Meridian Outfitters' supervisory org hierarchy (Level 1,
Module 3) automatically changes who future approvals route to — the BP
definition's step doesn't need editing when a manager changes, only when
the *rule for finding* the approver needs to change.

## To-Do vs. approval steps, and the notification step

Configurers distinguish an **action/to-do step** (someone must enter or
confirm data — no approve/deny decision) from an **approval step** (an
explicit approve, deny, or send-back decision that can halt or reroute the
process). A **notification step** runs in parallel with no gating power at
all — it tells someone the transaction happened, but can't block it.
Mixing these up is a common early configuration mistake: putting a data
correction on an approval step means every approver can silently edit
data they were only supposed to be approving.

## Worked example: adding a compliance step at Meridian

Meridian Outfitters' legal team asks that any Termination for a worker in
their EU entity route through an extra "Works Council Notification" step
before the termination becomes effective. A configurer's approach:

1. **Add a condition rule** — `Worker.Country = "Germany"` (or, more
   robustly, a rule referencing the worker's legal entity rather than a
   raw country string, so it survives future entity restructuring).
2. **Insert a notification step** into the Termination BP definition,
   positioned after HR's involuntary-termination review step and before
   the effective-date processing step, with the condition rule attached so
   it only fires for in-scope workers.
3. **Test in Preview/Sandbox** before promoting to production — confirming
   the step fires for a German test worker and does *not* fire for a US
   test worker on the same BP definition.

Note what didn't change: the Hire, Transfer, and other BP definitions are
untouched, and non-German terminations see zero difference in their
routing.

## How It Actually Works

A business process instance is not a script executing top to bottom — it's
a **state machine** whose current position is one specific step, advanced
by events (a submission, an approval, a denial), with the *next* step
determined fresh each time by evaluating that step's condition rules
against the instance's current data.

**Condition rules are evaluated per-instance, not baked in at design
time.** When the BP definition is saved, the step sequence and each step's
attached condition rules are stored — but nothing about which branches
will actually fire is decided until a real transaction is submitted. At
each step boundary, Workday evaluates the relevant condition rule's
calculated-field expression against that specific instance's data (which
worker, which position, which proposed values) and only then decides
whether to enter, skip, or branch at the next step. This is why the same
Termination BP definition can silently do nothing extra for a US worker
and insert a Works Council step for a German worker — one definition,
many possible execution paths, decided per-instance at runtime.

**Approver resolution is a live query against the org model, executed at
the moment the step becomes active — not when the transaction started.**
If a transaction sits in a "Manager Approval" step for three days and the
worker's manager changes on day two (a Transfer processed elsewhere),
Workday resolves "Manager" fresh at that step's activation, which can mean
different approvers see the request depending on exact timing. This
matters operationally: configurers who need approval stability sometimes
deliberately capture and freeze an approver at submission time rather than
relying on live resolution, precisely because live resolution *can* change
mid-flight.

**Denials and sends-back rewind the state machine rather than restarting
it.** A "send back" step doesn't create a new BP instance — it moves the
existing instance's current-step pointer back to an earlier step, keeping
all prior step history, comments, and the same unique BP transaction ID.
This is why an audit trail on a single Hire transaction can show a
send-back-and-resubmit cycle as one continuous history rather than two
disconnected transactions — the state machine's identity persists across
the rewind.

## Cheat sheet

| Concept | One-line definition |
|---|---|
| BP definition | An ordered sequence of steps for one BP type/subtype combination |
| Condition rule | A reusable calculated-field expression driving step or eligibility branching |
| Approval step | Requires an explicit approve/deny/send-back decision |
| Action/to-do step | Requires data entry/confirmation, no gating decision |
| Notification step | Informational only, cannot block the process |
| Routing rule | The logic that resolves an approval step's approver(s) dynamically |

## Exercise

Design a condition-rule-driven change to Meridian's Hire BP: any hire into
a position with an annual salary above a threshold should route through an
extra VP-level approval step that doesn't apply to hires below the
threshold. Sketch (1) the condition rule's logic in plain language, (2)
where in the existing step sequence the new approval step should be
inserted, and (3) one edge case you'd test in Sandbox before promoting to
production (e.g., a hire whose salary is proposed *after* the routing
decision was made).
