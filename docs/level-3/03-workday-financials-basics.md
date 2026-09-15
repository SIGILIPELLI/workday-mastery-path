---
description: "Workday Financials Basics — This module introduces Workday Financial Management — a separate but architecturally related product to HCM, sharing the same…"
---

# 03 · Workday Financials Basics

This module introduces Workday Financial Management — a separate but
architecturally related product to HCM, sharing the same underlying
platform concepts (business processes, security domains, organizations)
applied to accounting and spend data instead of worker data.

## The chart of accounts and worktags

Where HCM organizes data around workers and supervisory orgs, Financials
organizes around the **chart of accounts** and a distinctive Workday
concept: **worktags**.

| Concept | Detail |
|---|---|
| **Ledger Account** | The traditional accounting classification (Revenue, Expense, Asset, etc.) |
| **Worktag** | A flexible, multi-dimensional tag attached to a transaction (Cost Center, Project, Grant, Location, Spend Category) |
| **Journal** | The record of a financial transaction, always carrying a ledger account plus one or more worktags |

Worktags are what let Workday Financials answer questions traditional
chart-of-accounts-only systems struggle with, like "total spend on
Project X across every cost center that contributed to it" — because
Project is its own worktag dimension, independent of which cost center
recorded the spend, rather than requiring a separate account code per
project.

## Financial organizations

Financials introduces its own organization types, related to but distinct
from HCM's supervisory orgs (Level 1 Module 3 / Level 2 Module 2):

| Financial org type | Purpose |
|---|---|
| **Company** | The legal entity for financial reporting (shared concept with HCM's Company org) |
| **Cost Center** | Same worktag as referenced from HCM, the primary budget-tracking dimension |
| **Business Unit** | Financial reporting grouping, often crossing multiple cost centers |
| **Fund / Grant** (sector-specific) | Used in public sector/nonprofit tenants for restricted-funding tracking |

The shared **Company** and **Cost Center** organization types are the
concrete link between HCM and Financials in one tenant — a worker's cost
center assignment (Level 2 Module 2) is the same worktag dimension a
Financials journal entry would use to record that worker's payroll
expense.

## Core financial business processes

Financials transactions run through the same BP engine (Level 2 Module
1) as HCM transactions, with its own transaction types:

| BP type | Purpose |
|---|---|
| **Requisition / Purchase Order** | Procurement workflow — request, approve, order |
| **Supplier Invoice** | Recording and approving an incoming invoice for payment |
| **Journal Entry** | Direct accounting entries, often adjustments |
| **Expense Report** | Employee-submitted reimbursable expenses |

Each carries the same condition-rule-driven approval routing concept —
a Purchase Order above a spend threshold routing to an additional
approval level works exactly like Level 2 Module 1's salary-threshold
example, just evaluated against a spend amount and cost center worktag
instead of a proposed salary.

## Worked example: a store supply purchase at Meridian

Meridian's Boise store needs new store fixtures, a $4,000 purchase:

1. **Requisition** created by the store manager, tagged with Cost Center
   "Boise Store 118" and Spend Category "Store Fixtures" worktags.
2. **Condition rule evaluates the spend amount** — above the store-level
   manager's approval limit, so it routes to the District Manager for
   approval (the same routing-rule concept from Level 2 Module 1, applied
   to a spend threshold).
3. **Purchase Order issued** to the fixture supplier upon approval.
4. **Supplier Invoice received and matched** against the PO (a three-way
   match: requisition, PO, invoice, verifying quantities and amounts
   agree) before payment is released.
5. **Journal entries generated automatically**, recording the expense
   against the Boise Store 118 cost center and Store Fixtures spend
   category — the same worktags carried since the original requisition.

## How It Actually Works

Financials transactions carry their organizing worktags end-to-end through
every downstream step, which is what lets a single purchase be traced and
reported on consistently from initial request through final ledger entry
without re-tagging at each stage.

**A worktag attached at requisition time propagates automatically through
PO, invoice, and journal entry, because each downstream transaction
references the originating one rather than re-deriving its own tags.**
The Boise store's cost center and spend category tags aren't re-entered
at the Purchase Order or Supplier Invoice stage — those transactions
inherit them from the requisition they trace back to. This is
structurally similar to how a Job Change transaction in HCM carries
forward the worker and position context from the original hire rather
than requiring re-identification at every staffing event: the underlying
platform pattern (a chain of related, effective-dated transactional
records referencing prior ones) is the same one Level 1 Module 5
described, applied to procurement instead of staffing.

**A three-way match works by comparing worktag-and-amount data across
three independently created records, flagging discrepancies rather than
blocking automatically resolving them.** The system doesn't recompute
what the invoice "should" say — it retrieves the requisition's approved
quantities/amounts, the PO's issued quantities/amounts, and the invoice's
billed quantities/amounts, and applies a comparison rule. A mismatch (the
supplier billed for a different quantity than ordered) routes the
transaction to an exception-handling approval step — the same
condition-rule-driven branching from Level 2 Module 1, evaluated against
a discrepancy flag instead of a salary threshold.

**Cost Center being a worktag shared between HCM and Financials is what
makes a single tenant's payroll expense automatically land in the correct
financial ledger bucket without a separate manual mapping step.** When a
worker's pay is processed (Level 3 Module 4 covers payroll specifically),
the payroll result references that worker's current cost center
assignment — resolved via the same live organization-hierarchy lookup
from Level 2 Module 2 — and posts to the financial ledger using that same
worktag value. This is the concrete mechanism behind "HCM and Financials
share a platform": it isn't marketing language, it's the same
organization object being referenced by transactions in both functional
areas.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Worktag | A flexible, multi-dimensional tag (Cost Center, Project, etc.) attached to financial transactions |
| Chart of Accounts | The traditional ledger account classification, used alongside worktags |
| Three-way match | Comparing requisition, PO, and invoice data before releasing payment |
| Journal Entry | A direct accounting record, either transaction-generated or manually entered |

## Exercise

Sketch the worktag combination (cost center, spend category, and one
additional worktag of your choosing, e.g. Project) you'd expect on a
requisition for a new point-of-sale system rollout affecting five
Meridian stores at once. Explain why a single "Project: POS Rollout"
worktag, cutting across all five stores' individual cost centers, lets
Meridian's finance team report total rollout spend as one number even
though the expense actually posts against five different cost centers.
