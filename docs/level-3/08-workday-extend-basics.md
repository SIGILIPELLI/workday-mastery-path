# 08 · Workday Extend Basics

This module introduces Workday Extend — the platform for building
custom applications that run natively inside Workday, sharing its data
model and security, rather than being a separate bolted-on system.

## Why Extend exists

Every module so far has covered configuring *existing* Workday
functionality — business processes, reports, security. Sometimes a
tenant's requirement isn't a variation of something Workday already
provides at all (a fully custom approval workflow tied to a bespoke data
shape, a purpose-built app for a niche internal process). **Extend** lets
that be built as a genuinely new application, still running inside the
Workday tenant, using the same underlying objects and security model
rather than exporting data to an external system to build it there.

## Extend building blocks

| Building block | Purpose |
|---|---|
| **Custom Object** | A tenant-defined data structure (fields, relationships) — the Extend analog of Workday's own delivered business objects |
| **Orchestration** | Workflow logic connecting custom objects to actions, including calling standard Workday business processes |
| **App (Page Builder)** | The custom user interface a worker interacts with, built from delivered UI components |
| **Extend Security** | Custom objects and apps are secured using the *same* domain/security-group model from Level 1 Module 8, not a separate permission system |

## Custom objects vs. delivered business objects

A **Custom Object** is structurally the same *kind* of thing as a
delivered object like `Worker` or `Position` — it has fields, it can have
relationships to other objects (including delivered ones), and it can be
the subject of a report or a security domain. What's different is who
defined it: Workday ships `Worker`; a tenant's Extend developer defines a
custom object like `Equipment_Request` for a purpose-built internal app.

## Orchestrations: connecting custom logic to Workday's engine

An **Orchestration** can call existing Workday business processes and web
services (Level 3 Module 1's API layer) from within custom application
logic — meaning an Extend app doesn't have to reimplement staffing,
approval routing, or notification mechanics from scratch. A custom app
that needs "get manager approval" can orchestrate a call into a
purpose-built lightweight BP rather than hand-rolling an approval engine
inside the custom app itself.

## Worked example: an equipment request app at Meridian

Meridian's IT team wants a simple internal app for store staff to request
replacement equipment (registers, handheld scanners) that doesn't map
cleanly to any delivered Workday transaction type:

1. **Custom Object**: `Equipment_Request` — fields for equipment type,
   quantity, justification, and a relationship back to the requesting
   `Worker` and their `Supervisory_Organization`.
2. **App/Page Builder**: a simple form surfaced to store staff, built from
   delivered UI components, referencing the custom object's fields.
3. **Orchestration**: on submission, calls a lightweight approval workflow
   routing to the requester's manager (resolved via the same live
   organization-hierarchy lookup from Level 2 Module 2, since
   `Equipment_Request` has a relationship to the worker's supervisory
   org), then — if approved — calls a Financials web service (Level 3
   Module 1) to create a Requisition for the approved equipment, tagged
   with the requester's cost center worktag (Level 3 Module 3).
4. **Security**: a Domain Security Policy is configured for the
   `Equipment_Request` custom object exactly as it would be for any
   delivered domain (Level 3 Module 2), granting store staff View/Create
   on their own requests and managers View/Approve on their team's.

## How It Actually Works

An Extend app is not an integration bolted onto Workday from outside —
custom objects live inside the same tenant data model, evaluated by the
same security engine, and reachable by the same reporting and web
services layer as delivered objects.

**A custom object's relationship to a delivered object (like `Worker` or
`Supervisory_Organization`) is the same relationship mechanism the
platform uses between any two delivered objects — which is what lets an
Orchestration resolve "the requester's manager" without custom code
re-implementing organization traversal.** `Equipment_Request`'s
relationship field pointing to a `Worker` isn't a copied worker ID with
custom lookup logic bolted on — it's a genuine object relationship the
platform's existing organization-hierarchy resolution (Level 2 Module 2)
can traverse the same way it resolves a Hire BP's approver. This is why
building the approval routing above required configuring an orchestration
step, not writing a manager-lookup function from scratch — the lookup
capability already existed at the platform level, for any object with a
worker relationship, delivered or custom.

**Extend security domains are evaluated through the identical domain ×
group × scope intersection from Level 1 Module 8 — a custom object gets
no special-cased security behavior for being custom.** Configuring who
can View/Create/Approve `Equipment_Request` records uses the same Domain
Security Policy mechanism (Level 3 Module 2) as any delivered domain,
which means the same audit tools (View Security for Securable Item,
Compare Security Between Two Security Groups) work identically against
custom domains. This uniformity is a deliberate platform design choice:
a security architect doesn't need a second skill set to secure custom
apps versus delivered functionality.

**An orchestration calling a delivered business process or web service
means custom app logic is composed from existing, already-governed
platform capability rather than duplicating it — changes to the
underlying BP (say, a new approval step added to whatever Financials
process creates the Requisition) automatically apply to the custom app's
downstream call, too.** Because the orchestration invokes the Financials
Requisition creation capability rather than re-implementing "create a
purchase request" logic inside the custom app, any governance change made
to Requisition approval routing (Level 3 Module 3) takes effect for
equipment requests originating from the custom app exactly as it would
for a manually created requisition — the custom app is a new front door,
not a parallel, disconnected system.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Custom Object | A tenant-defined data structure with fields/relationships, structurally like delivered objects |
| Orchestration | Custom workflow logic that can call standard Workday BPs and web services |
| App/Page Builder | The custom UI a worker interacts with, built from delivered components |
| Extend Security | Custom objects secured via the same domain/group/scope model as delivered objects |

## Exercise

Sketch the custom object fields and one orchestration step for a Meridian
"Store Maintenance Ticket" app, where staff report facility issues that
should route to a facilities team and, above a cost threshold, also
require regional approval. Identify which existing platform mechanism
(from an earlier module) the cost-threshold routing would reuse rather
than reimplement.
