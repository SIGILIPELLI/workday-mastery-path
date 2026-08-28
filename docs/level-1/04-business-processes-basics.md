# 04 · Business Processes Basics

If Module 3's organizations and workers are Workday's *nouns*, business
processes are its *verbs* — the configured workflows that actually move
data from one state to another, with approvals along the way. Nearly
everything you do in Workday that isn't pure viewing (hiring, transferring,
requesting time off, changing compensation) runs through a **Business
Process (BP)**. This module builds the conceptual framework for how BPs
work, before Module 5 uses that framework on real staffing transactions.

## What a business process actually is

A **Business Process** is a configured definition, attached to a specific
transaction type (Hire, Termination, Request Time Off, Change Job, and
hundreds more), that defines:

1. **Steps** — the ordered stages the transaction moves through
2. **Approvals** — who must sign off at each step, and in what role
3. **Conditional routing** — rules that change the path based on the data
   in the transaction itself
4. **Notifications** — who gets told what, and when

Critically, the BP definition is **separate from the transaction data**. The
"Hire" business process definition is configured once (by an HRIS admin,
covered further in Level 2 Module 1); every individual hire that happens
afterward is a new *instance* of that same definition, routed the same way
unless conditional rules change its path.

## Anatomy of a BP: steps

A business process is built from an ordered sequence of **step types**:

| Step type | What happens |
|---|---|
| **Action** | Someone performs a task — e.g., "Enter Job Details" |
| **Approval** | A person or role must approve before the process continues |
| **To Do** | A non-blocking task assigned for tracking (doesn't halt the process) |
| **Notification** | An informational message sent to someone, doesn't require action |
| **Approval Chain (multiple approvers)** | A sequence of approvals — e.g., manager, then HR partner, then finance |

Each step is assigned to a **security group or role** — not a named
individual — so the same BP definition automatically routes to "whoever is
the manager of this position" or "whoever is in the HR Partner role for
this supervisory org," regardless of who that turns out to be for any given
transaction.

## Conditional routing — the same BP, different paths

Real business processes rarely follow one fixed path for every instance.
Workday supports **condition rules** that branch the process based on data
in the transaction. For example, Meridian Outfitters' "Request Compensation
Change" business process might include:

| Condition | Extra step triggered |
|---|---|
| Proposed increase is greater than 10% | Adds a VP-level approval step |
| Worker is in a Distribution Center org | Adds an approval step for the DC Director |
| Proposed increase is within standard merit guidelines | Skips the extra approval entirely, routes straight to HR Partner review |

This is what lets a single "Request Compensation Change" BP definition
correctly handle both a routine 3% merit increase (fast, few approvals) and
an exceptional 25% market adjustment (slower, more scrutiny), without
needing two separate business processes.

## Approvals: what an approver actually sees and can do

When a business process step reaches an approval, it lands in the
approver's Inbox (Module 2). From there, the approver can typically:

| Action | Effect |
|---|---|
| **Approve** | Moves the process to its next step |
| **Send Back** | Returns the process to an earlier step for correction (e.g., "Send Back to Enter Job Details" if the wrong position was selected) |
| **Deny** | Stops and cancels the process entirely |
| **Add To Do** | Delegates a follow-up action to someone else without blocking the approval itself |

Every action, on every step, is logged. Module 8 covers who has security
access to *see* that history, but the fact of full auditability — every
approval, every send-back, every denial timestamped and attributed — is a
foundational property of how BPs work, not an add-on feature.

## Worked example: Meridian's Hire business process, step by step

Here's a simplified version of what happens when a hiring manager at
Meridian's Distribution Center West initiates a Hire for a new warehouse
associate position:

1. **Action — Enter Hire Details**: The hiring manager selects the vacant
   position, enters the candidate's name, start date, and proposed hire
   type (Employee).
2. **Action — Propose Compensation**: The HR Partner enters the proposed
   pay rate, referencing the position's assigned compensation grade
   (Module 6 covers this in depth).
3. **Condition check**: Is the proposed pay rate within the grade's
   standard range? — Yes, for this hire.
   - *(If No, a "Compensation Approval — Exception" step would be inserted
     here, routing to a compensation analyst before continuing.)*
4. **Approval — DC Director**: Alan Cho, the DC West Director, reviews and
   approves the hire.
5. **Approval — HR Operations**: A second-line HR approval confirms
   policy compliance (background check completed, offer letter issued).
6. **Action — Complete**: The new hire's record is finalized; the worker's
   Workday profile is created effective their start date, and downstream
   notifications fire — payroll setup, IT provisioning ticket, benefits
   eligibility calculation.

Notice step 3: if the hiring manager had instead proposed a pay rate 15%
above the position's standard range, the *same* Hire business process would
have automatically inserted an extra approval step — no one had to remember
to route it differently, because the condition rule handles that
automatically based on the data entered in step 2.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Business Process (BP) | A configured workflow definition attached to a transaction type |
| Step | One stage in a BP — Action, Approval, To Do, or Notification |
| Condition rule | Logic that branches a BP's path based on transaction data |
| Approve / Send Back / Deny | The three core actions an approver can take on an approval step |
| BP instance | One specific occurrence of a BP definition being run (e.g., "this hire") |

## Exercise

Sketch, as a numbered list like the worked example above, the business
process you'd expect for a **Termination** at Meridian Outfitters. Include
at least five steps mixing Action and Approval types, and design at least
one condition rule that changes the path (for example: involuntary vs.
voluntary termination routing differently, or a termination for someone
with direct reports requiring an extra reassignment step). For each
approval step, name which role (not which specific person) would own it.
