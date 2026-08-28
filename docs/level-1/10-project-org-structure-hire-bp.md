# 10 · Project — Design a Simple Org Structure & Hire Business Process

This capstone project pulls together every Level 1 module into one paper
deliverable: designing a supervisory organization structure and its Hire
business process for a new fictional company. There's no tenant access
required — this is the same kind of design document a Workday analyst
would draft *before* any actual configuration begins, and it's the
artifact a real project would review with stakeholders first.

## The scenario

You're the HRIS analyst for **Harborview Coffee Roasters**, a growing
specialty coffee company that just decided to implement Workday HCM. They
have:

- A **headquarters** function (Finance, Marketing, People Ops — about 20
  people)
- A **roastery/production** operation (about 45 people: roasting,
  packaging, quality control)
- A **retail** arm with **6 café locations** (about 90 people total, roughly
  15 per café: baristas, shift leads, café managers)

Your job is to design their supervisory organization structure and the
Hire business process for one specific role, and document both clearly
enough that a stakeholder unfamiliar with Workday could review and approve
your design.

## Part 1 — Design the supervisory organization structure

Using the org-tree format from Level 1 Module 3, sketch Harborview's full
supervisory organization hierarchy. Requirements:

- A clear top-level org for the whole company
- At least three major branches (Headquarters, Roastery/Production, Retail)
- Retail must show individual café sub-orgs (at least 3 of the 6, to keep
  the exercise scoped) each with at least one further sub-org or role
  grouping underneath (e.g., "Café Team" or split into "Front of House" /
  "Back of House" if you prefer more granularity)
- For each top-level branch, state the **staffing model** (Position
  Management or Job Management) you'd choose, and justify it in one
  sentence per branch, using Module 3's decision criteria (headcount
  control needs vs. fluid/high-turnover roles)

## Part 2 — Design the Hire business process for "Café Barista"

Café Barista is Harborview's highest-volume, highest-turnover role — likely
dozens of hires per year across 6 locations. Design its Hire business
process:

1. List the **steps** (minimum 5), using Module 4's step types (Action,
   Approval, To Do, Notification), in order.
2. For each Approval step, name the **role** (not a specific person) that
   owns it.
3. Design **at least one condition rule** that changes the process's path
   based on transaction data — for example, a proposed pay rate above the
   role's standard grade range, or a rehire (someone who worked at
   Harborview before) requiring an extra HR review step.
4. State the **compensation grade and basis** you'd assign to Café Barista
   (referencing Module 6's grade table format), and the **pay range** you'd
   set for it.

## Part 3 — Security sketch

Using Module 8's domain/group framework, sketch the security access for
the **Café Manager** role (who will initiate hires for their own café) at a
minimum covering:

- Which domain(s) they need Approve or Action access to for the Hire
  business process you designed in Part 2
- Which domain(s) they should only have View access to (or no access at
  all) — specifically, whether a Café Manager should be able to see
  compensation data for staff *outside* their own café, and why or why not

## Deliverable format

Write this up as a single document (a markdown file, a doc, or even a
plain text file works) with three clearly labeled sections matching Parts
1-3 above. Use tables wherever the earlier modules used them — an org tree
as an indented list or diagram, business process steps as a numbered list,
security access as a table with Domain / Access Level / Scope columns, the
same structure Module 8's worked example used.

## Exercise

Complete Parts 1, 2, and 3 above for Harborview Coffee Roasters. Then write
a short (3-5 sentence) "design rationale" paragraph at the end, as if
presenting this to Harborview's VP of People, explaining the single
decision in your design you expect them to push back on (for example: a
staffing model choice, a condition rule threshold, or a security
restriction) and how you'd defend it using the concepts from this level.
