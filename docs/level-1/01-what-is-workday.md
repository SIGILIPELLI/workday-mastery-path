# 01 · What Is Workday?

Workday is one of the most common enterprise systems in the working world,
yet most people who use it every day couldn't explain what it actually is
beyond "the place I request time off." This module builds a real mental
model: what kind of software Workday is, how its cloud/tenant architecture
works, and why that architecture shapes almost every decision later modules
will cover — from how business processes route approvals to why
integrations behave the way they do.

## What Workday actually is

Workday is a **cloud-based Enterprise Resource Planning (ERP) suite**,
built primarily around two pillars:

| Pillar | What it covers | Examples of what lives here |
|---|---|---|
| **HCM (Human Capital Management)** | Everything about the workforce | Organizations, workers, positions, staffing, compensation, benefits, talent, recruiting, learning, time & absence |
| **Financials** | Everything about the money | General ledger, accounts payable/receivable, procurement, projects, grants, banking |

Many organizations run only HCM, some run only Financials, and large
enterprises often run both on a single, unified tenant — which is one of
Workday's core selling points: a hire in HCM and a cost-center allocation in
Financials reference the *same* underlying organizational and worker data,
rather than being synced between two separate systems.

This course is HCM-focused through Level 2, introduces Financials
fundamentals in Level 3, and covers advanced Financials and enterprise
architecture in Level 4.

!!! info "This is not a coding course"
    Workday is configured, not programmed, for the vast majority of its
    functionality — through business process definitions, security groups,
    calculated fields, and configuration screens inside the application
    itself. Where actual code appears (Workday Studio, Extend), it's covered
    conceptually in Level 3-4 without requiring you to write it.

## SaaS, single-tenant-per-customer, and what that means for you

Workday is delivered as **Software as a Service (SaaS)**: your organization
does not install or host Workday on its own servers. Instead, Workday runs
the application in its own data centers, and your company is provisioned a
**tenant** — a logically isolated instance of the software containing your
organization's own configuration and data, running on a shared underlying
codebase that every Workday customer runs.

| Traditional on-premise ERP | Workday (SaaS) |
|---|---|
| Your IT team hosts servers and applies patches | Workday hosts everything; you never touch a server |
| Upgrades are large, infrequent, risky projects | Workday pushes two feature releases per year to every tenant automatically |
| Heavy custom code is common and accumulates over years | Configuration-first; custom code is deliberately limited (Extend apps, EIB/Studio integrations) |
| Each customer can be on a different version | Every tenant is always on the current version — no "we're stuck on the old version" problem |

This has a practical consequence worth internalizing early: **you cannot
permanently break Workday's underlying code**, and you don't need to worry
about "which version" a screenshot or instruction refers to the way you
might with on-premise software. What you *can* do is misconfigure your
tenant's business processes, security, or data — which is exactly what the
rest of this course teaches you to do correctly.

## The tenant concept, illustrated

Picture three completely unrelated companies — a hospital network, a
software startup, and a national retail chain — all running Workday. Each
has its own tenant: its own organizations, its own workers, its own
business process configurations, its own security groups. None of them can
see or affect another's data, even though all three tenants are running on
the same underlying Workday application code, upgraded on the same
twice-yearly release schedule.

Throughout this course, we'll follow one running example company:
**Meridian Outfitters**, a mid-sized outdoor-gear retailer with about 1,200
employees across corporate offices, distribution centers, and retail
stores. Meridian's tenant is what later modules will build out — its
organizational hierarchy, its Hire business process, its security model —
so that concepts connect to one consistent, evolving scenario rather than a
new example every module.

## Core object vocabulary (a preview)

You'll go deep on each of these starting in Module 3, but it helps to see
the map before the territory:

| Term | One-line definition |
|---|---|
| **Worker** | A person Workday tracks — employee or contingent worker |
| **Position / Job** | The "seat" a worker sits in, defined by Workday's staffing model (Position Management vs. Job Management) |
| **Organization (Org)** | A grouping structure — supervisory, cost center, company, region, and others |
| **Business Process (BP)** | The configured workflow that routes a transaction (like a hire or a promotion) through steps and approvals |
| **Security Group / Domain** | Who can see or do what, and to which data |
| **Tenant** | Your organization's isolated instance of Workday |

## Worked example: sizing up Meridian Outfitters' Workday footprint

Before any configuration work happens, an analyst joining a project
typically answers a short set of orientation questions. Here's how that
looks for Meridian:

| Question | Answer for Meridian Outfitters |
|---|---|
| Which pillars are in scope? | HCM only for now; Financials is a planned Phase 2 in 18 months |
| How many workers, roughly? | ~1,200 across corporate, 4 distribution centers, and 85 retail stores |
| What's the supervisory shape? | Deep in retail (store manager → assistant managers → associates), flat at corporate |
| Any acquisitions/mergers pending? | One small regional retailer being acquired next year — its data will need to be merged into Meridian's tenant |
| Who are the tenant's system admins? | A 3-person HRIS team, reporting to the VP of HR Operations |

Framing a new system this way — pillars in scope, headcount, org shape,
future change, and who owns configuration — is the same first pass a real
Workday analyst runs on day one of any engagement, and it's the lens the
rest of this course will keep coming back to.

## How It Actually Works

Everything in Workday — every worker, position, organization, business
process, and report — is ultimately a row (or set of related rows) in an
**object-based data model**, not a set of flat database tables you'd
recognize from a traditional relational HR system. Workday calls these
**business objects**: `Worker`, `Position`, `Supervisory_Organization`,
`Compensation_Package`, and thousands more. Each business object type
defines a fixed set of attributes and, critically, a fixed set of
**relationships** to other business object types — a `Worker` object
relates to a `Position` object, which relates to a `Supervisory
Organization` object, which relates to a `Location` object, and so on. This
web of typed relationships is what lets Workday answer a question like "who
are all the workers in DC West as of last March" without you writing a
join — the relationship between `Worker` and `Supervisory_Organization` is
already part of the object model, and Workday's calculation engine walks it
for you.

The single-tenant-per-customer architecture matters here for a subtle
reason beyond "your data is isolated": every tenant runs against the
**same object model definitions**. When Workday ships a feature release,
it can extend the business object schema (add a new attribute to `Worker`,
say) for every tenant simultaneously, because no tenant has forked the
underlying schema with custom columns the way an on-premise system might
accumulate over years. What *does* vary per tenant is the **configuration
layer** sitting on top of that fixed object model — which business
processes are active, how security groups are defined, which calculated
fields exist. This is precisely why "you cannot permanently break Workday's
underlying code": the object model and its relationships are platform-level
and immutable from a customer's seat; only the configuration referencing
those objects is yours to change.

This distinction — fixed object model plus a customer-owned configuration
layer — is the mechanism behind nearly everything covered later in this
course. A business process doesn't invent a new way to move data; it
defines an approval path for an **event** that creates or changes instances
of business objects (a Hire event creates a `Worker` and a `Position`
assignment). A security group doesn't invent new data boundaries; it grants
or denies access to instances of business objects and their relationships.
Once you can see Workday as "a fixed graph of business object types, with a
per-tenant configuration layer governing who can trigger changes to
instances of that graph and how those changes route for approval," the rest
of the platform stops looking like a collection of unrelated screens and
starts looking like one consistent mechanism applied to different object
types.

## Cheat sheet

| Concept | Key idea |
|---|---|
| SaaS | Workday hosts and upgrades the software; you never manage servers |
| Tenant | Your organization's isolated, configured instance of Workday |
| HCM | The people pillar — orgs, workers, staffing, comp, talent |
| Financials | The money pillar — GL, AP/AR, procurement, projects |
| Two releases/year | Every tenant upgrades on the same schedule automatically |
| Configuration over code | Most behavior is set through config screens, not custom code |

## Exercise

Answer the same five orientation questions from the worked example above,
but for an organization you know well (your employer, a past employer, or a
hypothetical company you invent with at least 200 people). Write one
sentence per answer. Then, in one paragraph, explain whether that
organization would most benefit from running HCM only, Financials only, or
both on a single Workday tenant — and why, based on how connected its people
data and financial data would need to be.
