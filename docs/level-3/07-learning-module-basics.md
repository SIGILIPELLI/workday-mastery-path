---
description: "Learning Module Basics — This module introduces Workday Learning — course/content management, assignment rules, and how completion data connects back to…"
---

# 07 · Learning Module Basics

This module introduces Workday Learning — course/content management,
assignment rules, and how completion data connects back to the talent and
compliance concepts from Level 2 Module 8.

## Courses, programs, and content

| Concept | Detail |
|---|---|
| **Course** | A single learning unit — video, document, in-person session, or external content reference |
| **Learning Program** | An ordered collection of courses forming a curriculum (e.g., a new-manager onboarding program) |
| **Content Provider integration** | External content libraries (LinkedIn Learning-style providers) surfaced inside Workday Learning's catalog rather than hosted natively |

Programs, not just individual courses, are how a structured multi-step
curriculum (onboarding, compliance certification renewal) is represented
— a program tracks a learner's progress across its entire course sequence
as one entity, not as several unrelated individual completions.

## Assignment rules — pushing learning to the right population

Rather than relying on workers to browse and self-enroll, Workday Learning
supports **required learning assignment rules** that automatically enroll
a defined population based on attributes — the same eligibility-rule
pattern from Level 2 Module 4's benefits eligibility, applied to learning
content:

| Assignment trigger | Example |
|---|---|
| **New hire onboarding** | Auto-assigned the moment a Hire BP completes (Level 1 Module 5) |
| **Job profile/role-based** | Anyone in "Store Manager" job profile assigned a management compliance course |
| **Recurring/renewal** | An annual compliance course reassigned automatically on a recurrence schedule |
| **Event-triggered** | A promotion into a management role (Job Change BP) auto-assigns a manager-transition course |

## Completion tracking and compliance reporting

Each assignment carries a **due date** and a **completion status**, and
overdue required learning is reportable using the same custom-report
mechanics from Level 2 Module 5 — a compliance report showing every
worker with an overdue required course, filterable by region or manager,
built the same way any other custom report is.

## Connecting Learning to Talent

Completed learning can feed into the Talent and Performance concepts from
Level 2 Module 8 — for example, a competency (Level 2 Module 8) can be
configured to gain a proficiency-level bump upon completion of a specific
course or program, connecting a discrete learning event to the broader
talent profile used in calibration and succession planning.

## Worked example: manager-transition training at Meridian

When Jordan Ellis is promoted to Assistant Manager (Level 1 Module 5,
Level 2 Module 4), Meridian wants Jordan automatically enrolled in a
"New Manager Essentials" learning program:

1. **Assignment rule configured**: triggered by a Job Change BP
   transitioning a worker *into* a management-tier job profile (evaluated
   via the same condition-rule field-comparison logic from Level 2 Module
   1 — old job profile's management flag = false, new job profile's
   management flag = true).
2. **Auto-enrollment** fires the moment Jordan's promotion is approved and
   effective, assigning the "New Manager Essentials" program with a
   30-day completion due date.
3. **Progress tracked** across the program's course sequence; Jordan's
   manager (the District Manager) can see completion status via a
   direct-reports learning report, scoped by the same manager-hierarchy
   security pattern from Level 1 Module 8.
4. **Competency bump** — completing the program's "New Manager
   Fundamentals" course automatically raises Jordan's "Team Leadership"
   competency (Level 2 Module 8) by one proficiency level, feeding into
   Jordan's next talent calibration cycle.

## How It Actually Works

Learning assignment rules are evaluated the same way benefits eligibility
rules and BP condition rules are — as boolean expressions run against a
worker's current attributes, triggered either by a specific event or a
scheduled re-evaluation, not maintained as manually curated enrollment
lists.

**An event-triggered assignment rule listens for the same underlying
attribute change a benefits eligibility re-check (Level 2 Module 4) would
react to — the two mechanisms are the same trigger pattern applied to
different downstream actions.** Jordan's promotion changing their job
profile's management flag is exactly the kind of attribute change that
Level 2 Module 4 established triggers a benefits eligibility re-check;
here, the identical event also satisfies the Learning assignment rule's
condition. This isn't a coincidence of similar-sounding features — both
benefits eligibility and learning assignment are built on the platform's
general pattern of "any transaction that changes a relevant attribute
re-evaluates every rule that depends on that attribute," so one Job
Change transaction can simultaneously trigger a benefits recheck, a
learning assignment, and a compensation package re-evaluation (Level 2
Module 4), each independently, from the single underlying attribute
change.

**A recurring compliance course's "renewal" is a new assignment record
generated on a schedule, not a reset of the prior completion record.**
When an annual compliance course reassigns itself each year, Workday
doesn't clear or overwrite last year's completion — it creates a new,
dated assignment record for the new period, while the prior year's
completion record remains intact for historical audit purposes (proving,
for a compliance audit two years later, that the worker completed the
required training in each specific year, not just "at some point"). This
is the same additive, effective-dated discipline from Level 1 Module 5,
applied to compliance history instead of staffing history.

**A competency proficiency bump from course completion is configured as a
triggered update to a talent record, not a live calculated value — which
is why it persists even if the learning assignment rule that originally
triggered enrollment later changes.** Unlike a 9-box placement (Level 2
Module 8's live calculation from current rating records), a competency
level raised by course completion is written as its own dated competency
record at the moment of completion. If Meridian later changes which
program triggers on promotion, that change has no retroactive effect on
competency records already written for workers who completed the old
program — the completion event, once recorded, is a historical fact like
any other effective-dated record in the system.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Learning Program | An ordered course sequence tracked as one curriculum |
| Assignment rule | An eligibility-rule-style trigger auto-enrolling a population |
| Content Provider integration | External learning content surfaced in Workday's catalog |
| Competency bump | A course-completion-triggered update to a talent competency record |

## Exercise

Design an assignment rule for a new Meridian food-safety compliance
course that should auto-assign to any worker whose job profile involves
food handling, re-assign annually, and auto-assign again immediately if a
worker transfers into a qualifying job profile mid-year. Explain which
part of this rule is event-triggered and which part is schedule-triggered,
using the distinction drawn above.
