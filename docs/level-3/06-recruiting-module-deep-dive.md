---
description: "Recruiting Module Deep Dive — This module covers Workday Recruiting — the module governing job requisitions, candidate pipelines, and offers — and how it…"
---

# 06 · Recruiting Module Deep Dive

This module covers Workday Recruiting — the module governing job
requisitions, candidate pipelines, and offers — and how it hands off into
the Hire business process covered back in Level 1 Module 5.

## Job requisitions vs. positions

A **Job Requisition** is Recruiting's request to fill a role, and it
relates to — but is distinct from — the `Position` object from Level 1
Module 3's staffing models:

| Staffing model | Requisition relationship |
|---|---|
| **Position Management** | A requisition is opened *against* a specific vacant position number |
| **Job Management** | A requisition opens for a job profile within a supervisory org, with no specific position number pre-existing |

This distinction matters operationally: in Position Management, a
recruiter can see exactly which position they're filling (and its
pre-existing compensation grade, Level 1 Module 6) from the moment the
requisition opens; in Job Management, those specifics are determined at
hire time.

## The candidate pipeline

A **Candidate Pipeline** tracks applicants through configurable **stages**
— the recruiting-specific analog of a business process's steps, though
candidate movement between stages is typically more flexible (a recruiter
can move a candidate backward or skip stages) than the strict forward
routing of a Hire BP:

| Stage (illustrative) | Purpose |
|---|---|
| **Applied** | Initial application received |
| **Screen** | Recruiter or automated screening review |
| **Interview** | One or more structured interview rounds |
| **Offer** | Offer extended, pending acceptance |
| **Hire** | Accepted offer, ready to convert into a Hire BP |

Each stage can have its own configured actions (scheduling interviews,
triggering candidate communications) and its own security scoping — a
hiring manager typically sees only candidates for their own open
requisitions, using the same domain/group/scope mechanism from Level 1
Module 8, applied to candidate data instead of worker data.

## Offers and the hire conversion

An **Offer** captures proposed compensation and start date before the
candidate has become a worker — it references the same compensation
grade and package concepts from Level 1 Module 6 / Level 2 Module 4, but
against a *candidate* record, not yet a `Worker` object. Once an offer is
accepted, converting the candidate into an employee triggers the Hire
business process (Level 1 Module 5), carrying forward the offer's agreed
compensation, position, and start date as the Hire transaction's initial
proposed values — the recruiter doesn't re-enter data the offer already
captured.

## Sourcing and requisition-level reporting

Recruiting tracks **source** (referral, job board, internal mobility) per
application, enabling reporting on which channels actually convert to
hires — a report built with the same custom-report mechanics from Level
2 Module 5, using Recruiting-specific data sources (Job Requisition,
Candidate) instead of Worker.

## Worked example: filling the Regional Merchandising Specialist role at Meridian

Continuing Level 2 Module 10's new job profile — Meridian now needs to
actually hire someone into it:

1. **Requisition opened** against a specific vacant position (Position
   Management), pre-populated with the compensation package designed in
   Level 2 Module 10.
2. **Candidates apply and move through pipeline stages** — screen,
   interview with the District Manager, final interview with the
   Regional VP.
3. **Offer extended** to the selected candidate, referencing the position's
   compensation grade; because the proposed salary happens to be above
   the grade's guideline maximum (Level 2 Module 4's guideline-not-cap
   pattern), the offer routes through the exception-approval condition
   rule designed back in Level 2 Module 10, Part 1.
4. **Offer accepted → Hire BP triggered**, carrying forward the approved
   above-range salary, position, and start date without re-entry;
   standard Hire approval routing (Level 1 Module 5) still applies on top.

## How It Actually Works

Recruiting data (requisitions, candidates, offers) lives as its own set
of business objects related to, but distinct from, the `Worker` and
`Position` objects — the Hire conversion is the specific mechanism that
creates the bridge between a candidate record and a worker record.

**A requisition against a vacant position is a relationship record
between the `Job_Requisition` object and the `Position` object, the same
relational pattern used everywhere else in the platform — it doesn't
duplicate the position's own attributes.** When a requisition displays
the position's compensation grade, it's not because the requisition
stores its own copy of that grade — it resolves the grade by following
the requisition's reference to its position, then the position's
reference to its compensation package (Level 1 Module 6's grade
assignment). This is why updating the position's compensation package
(say, a grade range adjustment) automatically reflects in any open
requisition against that position, without the requisition needing to be
edited.

**Hire conversion works by creating a new `Worker` object and copying
forward specific offer-stage data as the initial values of the Hire
business process instance — the candidate record and the worker record
remain two distinct, linked objects, not one object that changes type.**
Accepting an offer doesn't transform the `Candidate` object into a
`Worker` object in place; it creates a new `Worker` object and initiates
a Hire BP instance (Level 1 Module 5's mechanism) pre-populated with the
offer's compensation, position, and start date as proposed values, while
retaining a reference back to the original candidate and requisition
records for reporting continuity (source tracking, time-to-fill metrics)
even after the hire completes.

**The above-guideline-salary condition rule fires identically whether the
proposed salary originates from a manually entered Hire or an accepted
Recruiting offer, because both ultimately populate the same proposed-
compensation field the condition rule reads.** Level 2 Module 1's
exception-approval condition rule was built to evaluate a proposed salary
against a grade maximum regardless of where that proposed value came
from. Recruiting's offer-to-hire handoff simply supplies that field's
value from the accepted offer instead of a recruiter typing it in fresh
— the condition rule itself has no awareness of, or dependency on, which
path populated the field it's evaluating.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Job Requisition | The request to fill a role, related to a Position or job profile |
| Candidate Pipeline | Configurable stages tracking an applicant's progress |
| Offer | Proposed compensation/start date captured against a candidate, pre-Hire |
| Hire conversion | Creating a new Worker object and Hire BP instance from an accepted offer |

## Exercise

Design a candidate pipeline stage configuration for a Meridian store
management role that should require two interview stages before an offer
can be extended, with a condition rule blocking movement to Offer if
either interview stage's feedback hasn't been submitted. Explain why this
gating is conceptually the same mechanism as a business process step
condition (Level 2 Module 1), even though candidate pipeline movement is
generally more flexible than strict BP step routing.
