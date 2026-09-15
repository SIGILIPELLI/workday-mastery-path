---
description: "Change Management for Workday Rollouts — Level 3 Module 9 covered technical testing and release discipline. This module covers the organizational side of…"
---

# 07 · Change Management for Workday Rollouts

Level 3 Module 9 covered technical testing and release discipline. This
module covers the **organizational** side of change management — getting
a workforce to actually adopt a new Workday capability, which is a
distinct problem from configuring it correctly.

## Why technical correctness isn't sufficient

A perfectly configured business process (Level 2 Module 1) or security
model (Level 1 Module 8) still fails to deliver value if the people
meant to use it don't understand it, don't trust it, or actively route
around it. Organizational change management addresses the adoption gap
that technical testing (Level 3 Module 9) doesn't cover — a regression
test confirms the system behaves correctly; it says nothing about whether
managers will actually use the new approval Inbox instead of emailing HR
directly out of habit.

## The stakeholder map

A rollout's change management plan typically identifies:

| Stakeholder group | Concern |
|---|---|
| **Executive sponsors** | Need visible commitment to the change, or lower-level resistance has no consequence |
| **Managers/approvers** | Need confidence the new process won't slow them down or expose them to mistakes |
| **End users/workers** | Need to understand what's different and why, especially if a familiar process changed |
| **HR/HRIS support team** | Needs to be prepared for a spike in support requests immediately post-go-live |

## Communication and training cadence

Effective rollouts stage communication rather than announcing a change
once at go-live:

| Stage | Purpose |
|---|---|
| **Early awareness** (weeks before) | "This is coming and why" — reduces surprise |
| **Detailed training** (just before go-live) | Role-specific walkthroughs — a manager's training differs from a general worker's |
| **Go-live support** (first days/weeks) | Elevated support availability, quick-reference guides |
| **Reinforcement** (weeks after) | Follow-up communication addressing common confusion points observed in early usage |

## Resistance patterns and mitigation

Common resistance patterns in Workday rollouts, and typical mitigations:

| Resistance pattern | Mitigation |
|---|---|
| **"The old way was faster"** | Show, don't just tell — a live side-by-side comparison often resolves this better than an explanation |
| **Workarounds** (e.g., continuing to email a paper form) | Close the workaround path deliberately (Module 1's BP security policy can restrict who can still process the old way) once new-process confidence is established |
| **"Nobody told me"** | A stakeholder map gap — usually means a group was missed in the communication plan, not that communication itself failed |

## Worked example: rolling out self-service Time Off requests at Meridian

Meridian is moving from a paper/email time-off request process to
Workday's self-service Time Off Request (Level 2 Module 9) across all
retail stores:

1. **Stakeholder map**: store managers (need confidence approving via
   Inbox won't be slower than a quick verbal "yes"), store associates
   (need to know how to submit a request on a shared store device),
   district managers (need visibility that adoption is actually
   happening).
2. **Early awareness**: a message from the VP of Retail Operations
   (executive sponsor) two weeks before go-live, explaining the change
   and why.
3. **Role-specific training**: a two-minute walkthrough video for
   associates (submit a request), a slightly longer session for managers
   (approve/deny via Inbox, understand balance visibility).
4. **Go-live support**: HRIS team staffs an extended-hours help line for
   the first two weeks, tracking common questions to fold into an FAQ.
5. **Workaround closure**: after a month of stable adoption, the old
   paper-form process is formally retired — not simply left running in
   parallel indefinitely, since a permanently available fallback
   undermines full adoption of the new self-service process.
6. **Reinforcement**: a follow-up message addressing the most common
   support-line question observed (how to view remaining balance before
   requesting), turning a support burden into a proactive communication.

## How It Actually Works

Organizational change management isn't a "soft" add-on disconnected from
the technical configuration work — the specific configuration decisions
made earlier in this path directly shape which change management
interventions are even available, and when.

**Closing a workaround path (like the paper time-off form) is
technically the same Business Process Security Policy mechanism from
Level 3 Module 2, applied as a deliberate change-management lever rather
than a pure security decision.** Retiring the old process isn't just an
announcement — it's implemented by removing the security grant that
allowed HR to process paper-form requests through whatever mechanism
handled them previously, forcing all requests through the new
self-service BP. This illustrates that "change management" and
"security configuration" aren't separate disciplines here — the same
grant-revocation mechanism that governs data access is the concrete lever
that makes a workaround stop being available, timed deliberately for
change-management reasons (giving adoption time to stabilize first)
rather than being flipped off on day one.

**Role-specific training content maps directly onto the security-group-
defined populations already established in the tenant, because "what
does this role need to know" and "what does this role's security group
grant them" are answering closely related questions.** The distinction
between associate training (how to submit) and manager training (how to
approve) isn't an arbitrary training-design choice — it follows directly
from the different BP Security Policy actions (Level 3 Module 2) granted
to each population: associates can initiate the Time Off Request, managers
can approve it. Training content that didn't respect this distinction
(teaching associates approval mechanics they have no access to use) would
be training time wasted on capability the audience doesn't have.

**Staged communication timing works because different stakeholder
concerns are resolved by different kinds of information, delivered at the
point each becomes actionable — not because more repetition alone drives
adoption.** An executive sponsorship message weeks before go-live
addresses legitimacy and reduces surprise; it doesn't teach anyone how to
actually submit a request, which is why detailed training is
deliberately sequenced closer to go-live when the information is
immediately actionable rather than abstract. This staging mirrors the
same "right information, right time" principle behind phased BP condition
rules (Level 2 Module 1) revealing only the step relevant to the current
transaction state — change management timing and BP step sequencing are
solving structurally similar problems for two different audiences (people
and transactions).

## Cheat sheet

| Term | One-line definition |
|---|---|
| Stakeholder map | Identifying groups affected by a change and their specific concerns |
| Workaround closure | Deliberately retiring a legacy path once new-process adoption is stable |
| Go-live support | Elevated support availability immediately following a rollout |
| Reinforcement communication | Post-go-live follow-up addressing observed adoption friction |

## Exercise

Meridian is rolling out the Regional Merchandising Specialist role and
compensation package designed back in Level 2 Module 10. Write a
stakeholder map and a staged communication plan for announcing this new
role and its reporting-line changes to the district managers whose
regions it will affect, identifying at least one existing workaround (if
any) this change would need to formally close.
