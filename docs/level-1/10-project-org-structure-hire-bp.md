---
description: "Project — Design a Simple Org Structure & Hire Business Process — This capstone project pulls together every Level 1 module into one paper deliverable…"
---

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

## How It Actually Works

This project asks you to design three artifacts on paper, but each one maps
directly onto a real object structure inside Workday — understanding that
structure is what separates a design that merely *looks* plausible from one
that would actually configure cleanly in a tenant.

**A supervisory organization is a chain of manager-position references, not
a folder hierarchy.** Workday doesn't store "Harborview Coffee Roasters" as
a tree of nested containers the way a file system does. Each supervisory
org is its own business object whose only structural link to the org above
it is a pointer to that parent org's manager position. When you sketch
Café #1 as a sub-org of Retail, what that really means in the tenant is:
Café #1's org record carries a "reports to" reference resolving to the
Retail org's manager position, and every worker hired into Café #1
inherits Retail's and Headquarters' attributes (cost center rollups,
default security group assignments, applicable business process
definitions) through that chain of references — not by copy, but by live
lookup evaluated at transaction time. This is why choosing Position
Management vs. Job Management per branch (Part 1) is a structural decision,
not a preference: it determines whether the org's roles are pre-defined
position objects with headcount limits (each hire fills an existing
position record) or whether hires create job requisition instances against
an unbounded job profile — and that choice is set once on the supervisory
org and inherited by every sub-org and worker beneath it, exactly like the
manager-reference chain itself.

**Business process steps execute against an ordered, conditionally-pruned
graph, not a fixed checklist.** When you design the Café Barista Hire
process in Part 2, the step list you write down is really the *default*
path through a directed step sequence; Workday evaluates each condition
rule at runtime, in step order, against the specific transaction's data —
proposed pay rate, worker rehire status, org assignment — and prunes or
inserts steps based on the result. Your rehire-triggers-extra-review
condition isn't a note attached to the process description; structurally
it's a rule object bound to a specific step transition, evaluated once the
transaction reaches that point, that reads the candidate's prior-worker
history attribute and either advances to the default next step or reroutes
to the inserted HR-review step. Because the rule reads live transaction
data rather than being hardcoded per role, the same Hire business process
definition can serve Café Barista, Roastery Technician, and a Headquarters
Finance hire simultaneously — each transaction just evaluates the same
condition rules against different data and takes a different path through
the identical graph.

**Security domain access is evaluated per-domain, per-transaction-step, not
per-user-globally.** The Café Manager security sketch in Part 3 isn't
granting a person a role label; it's assigning their user account's
security group membership Action or View access on specific domains (Hire
Business Process, Compensation data), and that access is checked fresh
every time a transaction touches that domain — not cached against the
person. This is precisely why the compensation-visibility question in Part
3 matters structurally: if a Café Manager's security group has View access
scoped to "workers in their own supervisory organization" rather than
tenant-wide, the same domain-permission rule that lets them approve a hire
in their own café automatically evaluates to *no access* the moment the
transaction or report crosses into a different café's org — no separate
rule is needed to block it, because the org-hierarchy scope from the first
point above is the same mechanism the security evaluation reads to decide
what's in scope.

## 🔀 Related lessons on other tracks

- [AI Manager — 10 · Project — Design an MLOps Process for a Team](https://sigilipelli.github.io/ai-manager-mastery-path/level-2/10-project-mlops-process/)
- [Product Lead — 08 · Basic Org Design for Product Teams](https://sigilipelli.github.io/product-lead-mastery-path/level-1/08-basic-org-design/)
- [Product Manager — 10 · Capstone — Full Product Strategy + Org Design](https://sigilipelli.github.io/product-manager-mastery-path/level-4/10-capstone-strategy-org-design/)

## Exercise

Complete Parts 1, 2, and 3 above for Harborview Coffee Roasters. Then write
a short (3-5 sentence) "design rationale" paragraph at the end, as if
presenting this to Harborview's VP of People, explaining the single
decision in your design you expect them to push back on (for example: a
staffing model choice, a condition rule threshold, or a security
restriction) and how you'd defend it using the concepts from this level.
