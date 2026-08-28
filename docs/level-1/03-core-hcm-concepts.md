# 03 · Core HCM Concepts

Everything in Workday HCM — every hire, every approval, every report —
sits on top of a handful of foundational objects: organizations, the
supervisory hierarchy, positions vs. jobs, and workers. Get these four
concepts solid and the rest of the course clicks into place; get them
fuzzy and every later module will feel harder than it needs to be.

## Organizations — Workday's grouping structure

An **organization** (org) in Workday is a container used to group workers,
positions, or other data for a purpose — reporting lines, cost allocation,
security scoping, or headcount planning. Workday ships several standard
organization types, and most tenants use most of them simultaneously,
layered on top of the same workers:

| Org type | Purpose | Example at Meridian Outfitters |
|---|---|---|
| **Supervisory Organization** | The "who reports to whom" management hierarchy — the primary org type for staffing | "Distribution Center West," managed by the DC West Director |
| **Cost Center** | Financial grouping for budgeting and expense allocation | "Retail Operations — West Region" |
| **Company** | Legal entity | "Meridian Outfitters, Inc." vs. a newly acquired subsidiary |
| **Region / Location Hierarchy** | Geographic grouping | "West Region" containing several store locations |
| **Custom Organization** | Tenant-specific groupings that don't fit standard types | "Sustainability Initiative" (a cross-functional team spanning multiple supervisory orgs) |

The critical distinction: a worker belongs to **exactly one supervisory
organization** at a time (their management chain), but can simultaneously
roll up into a cost center, a company, and a region — because those are
different lenses on the same worker, not competing hierarchies.

## Supervisory organizations — the backbone of staffing

The **supervisory organization** is the org type that everything about
"who manages whom" and "where do I hire this position" is built on. Each
supervisory org has exactly one **manager** and contains a set of
**positions or jobs** (Level 1 Module 5 covers hiring into them), and
supervisory orgs nest inside each other to form the company's overall
management hierarchy.

Picture Meridian Outfitters' supervisory org tree, simplified:

```
Meridian Outfitters (top-level org)
├── Corporate Functions
│   ├── Finance
│   └── HR Operations
├── Retail Operations
│   ├── West Region
│   │   ├── Store 104 - Denver
│   │   └── Store 118 - Boise
│   └── East Region
│       ├── Store 022 - Columbus
│       └── Store 057 - Pittsburgh
└── Distribution Center West
    ├── Warehouse Operations
    └── Inbound Logistics
```

Each box is its own supervisory organization with its own manager. A
transfer from Store 104 to Store 118 is, underneath, a change of
supervisory organization assignment for that worker — which is exactly why
Module 4's business processes and Module 5's staffing transactions are
built around moving workers between these boxes.

!!! info "Supervisory orgs can restructure without losing history"
    When Meridian reorganizes — say, splitting "Retail Operations" into
    three regions instead of two — Workday preserves the organization's
    history. A report run "as of" a date before the reorg still shows the
    old structure; nothing is silently deleted or overwritten. This
    matters enormously for reporting integrity, covered further in
    Module 7.

## Positions vs. Jobs — the two staffing models

Workday tenants choose one of two **staffing models** per supervisory
organization (a large tenant can mix both across different parts of the
org):

| | **Position Management** | **Job Management** |
|---|---|---|
| What's tracked | Individual, numbered **positions** (e.g., Position #40218, "Store Associate") that exist whether filled or vacant | Just **job requisitions** created when needed — no persistent position record when vacant |
| Best fit for | Roles needing tight headcount control — a fixed budget of "40 warehouse associate positions" | Fluid, high-volume, or less structured roles |
| What happens when someone leaves | The position stays open, ready to be refilled | The requisition is simply closed; a new one is opened when needed |
| Typical use at Meridian | Corporate and distribution center roles (headcount-controlled) | Seasonal retail associate roles (fluid, high-turnover) |

This choice is made per supervisory organization when it's configured, and
it shapes how the Hire and Staffing business processes actually behave —
Position Management requires selecting an existing (vacant) position to
hire into; Job Management lets you create the job on the fly during hire.
Module 5 walks through both flows concretely.

## Workers — the person record

A **worker** is Workday's umbrella term for anyone the system tracks in a
staffing capacity, split into two categories:

| Worker type | Definition | Example |
|---|---|---|
| **Employee** | On the organization's own payroll | A full-time Meridian store manager |
| **Contingent Worker** | Not on payroll — contractor, temp, or vendor-supplied | A seasonal warehouse temp supplied through a staffing agency |

Both worker types can have a supervisory organization, a position, a
manager, and be routed through business processes — but contingent workers
typically have a reduced data footprint (no compensation package, no
benefits enrollment) and are usually converted to full employee records
through a formal "Convert Contingent Worker to Employee" business process
if they're hired on permanently, rather than having their existing record
silently reclassified.

## Worked example: mapping Meridian's DC West to the four concepts

Distribution Center West at Meridian Outfitters, mapped out:

| Concept | DC West's answer |
|---|---|
| Supervisory org | "Distribution Center West," managed by Director Alan Cho |
| Sub-orgs | "Warehouse Operations" and "Inbound Logistics," each with their own manager reporting to Alan |
| Cost center | "DC Operations — West," a financial grouping shared with the region's Retail Operations for consolidated budget reporting |
| Staffing model | Position Management — DC roles are headcount-controlled against an approved budget of 220 positions |
| Worker mix | 195 employees, 25 contingent (seasonal peak-season warehouse temps supplied through a staffing agency) |

This is the exact kind of table an HRIS analyst fills in during discovery
for any new organization being built out in a tenant — and it's the
template the Module 10 project will ask you to complete for a structure of
your own design.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Organization | A grouping container — supervisory, cost center, company, region, custom |
| Supervisory Organization | The management-hierarchy org that positions/jobs and workers sit inside |
| Position Management | Staffing model with persistent, numbered positions (filled or vacant) |
| Job Management | Staffing model with on-the-fly job requisitions, no persistent vacant record |
| Employee | Worker type: on payroll |
| Contingent Worker | Worker type: not on payroll (contractor/temp) |

## Exercise

Using the org tree diagram for Meridian Outfitters above, sketch (as a
simple indented list, like the diagram) a supervisory org structure for
"East Region" with at least two stores under it, each store having at
least one sub-team (e.g., "Sales Floor" and "Stockroom"). For each org in
your sketch, note whether you'd choose Position Management or Job
Management as its staffing model, and justify the choice in one sentence
per org.
