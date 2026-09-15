---
description: "Advanced Extend Development — Level 3 Module 8 introduced Extend's building blocks. This module covers enterprise-scale Extend development practices…"
---

# 05 · Advanced Extend Development

Level 3 Module 8 introduced Extend's building blocks. This module covers
enterprise-scale Extend development practices: application lifecycle
management, API-first integration patterns between custom apps and
external systems, and governance for a portfolio of custom apps rather
than a single one.

## Application lifecycle management for Extend apps

A single custom app (Level 3 Module 8's Equipment Request example) is
manageable informally. A portfolio of dozens of custom apps across an
enterprise needs the same tenant-migration and testing discipline (Level
3 Module 9) applied deliberately to custom-object and orchestration
changes, plus:

| Practice | Purpose |
|---|---|
| **Versioned custom object schemas** | Changing a custom object's fields is itself a governed change, since existing app logic and reports may depend on the prior shape |
| **App-level access review** | Periodically re-auditing which security groups can access which custom app's domain, the same audit discipline from Level 3 Module 2 |
| **Deprecation process** | Retiring an unused custom app deliberately (data retention decisions, notifying remaining users) rather than silently abandoning it |

## API-first patterns: Extend apps calling external systems

Beyond orchestrating calls into Workday's own business processes (Level 3
Module 8), an Extend app can call **external** APIs directly — letting a
custom app inside Workday integrate with a non-Workday system as part of
its own workflow, rather than requiring a separate Studio integration
(Level 3 Module 1) running independently alongside it.

| Pattern | When it fits |
|---|---|
| **Extend app calls external API directly** | The external call is part of the *app's own* interactive workflow (e.g., checking real-time inventory before submitting a request) |
| **Extend orchestration triggers a separate Studio integration** | The external interaction is better modeled as an independent, asynchronous, scheduled process |

Choosing between these is a similar decision to Level 2 Module 6's EIB-
vs-Studio guide: an app needing a real-time, in-workflow external check
calls the API directly; a batch/scheduled external interaction is better
handled as its own separate integration.

## Governance for a custom app portfolio

At enterprise scale, a Center of Excellence (Level 4 Module 8 covers this
organizational concept) typically maintains a **custom app inventory** —
what exists, who owns it, what security groups can access it, and when it
was last reviewed — because custom apps, unlike delivered functionality,
have no vendor-driven maintenance; an unmaintained custom app can quietly
accumulate technical debt or unreviewed security grants (Level 3 Module 2)
that nobody is actively responsible for catching.

## Worked example: expanding Meridian's equipment request app

Meridian's Equipment Request app (Level 3 Module 8) has been running for
a year and now needs two enhancements: real-time stock-level checking
against the vendor's inventory API, and expansion to a second custom app
(a facilities ticket app) that should share some of the same underlying
approval orchestration logic:

1. **Schema versioning**: before adding a new field to `Equipment_Request`
   for "preferred vendor," the team reviews existing reports and
   orchestration steps referencing the object to confirm nothing breaks
   — the same change-impact-analysis discipline from Level 3 Module 9,
   applied to a custom object instead of a delivered one.
2. **Direct API integration**: since checking vendor stock levels is part
   of the app's own real-time submission workflow (a worker shouldn't
   submit a request for an item that's out of stock), the app calls the
   vendor's inventory API directly at submission time, rather than via a
   separate scheduled Studio integration.
3. **Shared orchestration logic**: the approval-routing orchestration
   built for Equipment Request (resolving the requester's manager via
   organization hierarchy, Level 2 Module 2) is refactored into a
   reusable orchestration component the new Facilities Ticket app also
   calls, rather than duplicating the same manager-resolution logic in a
   second app.
4. **Portfolio governance**: both apps are added to the enterprise's
   custom app inventory, with named owners and an annual access review
   scheduled for their respective security domains.

## How It Actually Works

Extend applications at scale face the same core tension enterprise
architecture (Level 4 Module 1) identified for the whole tenant: shared,
reusable components are efficient but require deliberate scoping and
governance, precisely because a change to something shared ripples
everywhere it's used.

**Refactoring shared orchestration logic into a reusable component works
because an orchestration, like a calculated field (Level 3 Module 5), is
a referenceable object other logic can call rather than a copy-pasted
script.** When the Facilities Ticket app calls the same manager-resolution
orchestration the Equipment Request app uses, it isn't duplicating that
logic — it's invoking the same underlying orchestration object, the exact
composability pattern Level 3 Module 5 described for calculated fields
referencing other calculated fields. This is why fixing a bug in how the
shared orchestration resolves managers (say, handling a worker with no
current manager assignment) automatically fixes it for both apps calling
it, without either app's own logic needing to change.

**Direct external API calls from an Extend app's real-time workflow and a
separate scheduled Studio integration are both ultimately the same web
services mechanism (Level 3 Module 1) — the difference is *when* and
*from where* the call happens, not what kind of call it is.** Choosing to
call the vendor's inventory API directly from the app's submission
workflow, rather than via a separate integration, is a latency and
workflow-coupling decision: the app needs the stock check's result
*before* the worker can proceed, which only a synchronous, in-workflow
call satisfies. A batch integration polling stock levels overnight would
technically use the same kind of API call but couldn't satisfy a
real-time submission-blocking requirement, because its result wouldn't
be available at the moment the worker is filling out the form.

**Schema versioning discipline exists because a custom object's fields are
referenced by name from multiple, potentially unrelated places (reports,
orchestrations, other custom objects' relationships) — exactly the same
reason Level 3 Module 9's regression testing exists for delivered BP
changes, applied to custom object structure instead.** Removing or
renaming a field on `Equipment_Request` without checking dependents risks
silently breaking a report or orchestration step that referenced the old
field name — a custom object gets no special protection from this risk
just because a tenant's own developers built it; the same
change-impact-analysis discipline applies precisely because nothing about
being "custom" makes a downstream reference less real.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Schema versioning | Treating custom object field changes as governed, impact-assessed changes |
| Custom app inventory | A portfolio-level record of apps, owners, and access grants |
| Direct API integration | An Extend app calling an external API synchronously within its own workflow |
| Reusable orchestration component | Shared workflow logic multiple apps call rather than duplicate |

## Exercise

Meridian wants a third custom app (a "Shift Swap Request" tool for store
staff) that also needs manager approval. Describe whether it should reuse
the existing shared manager-resolution orchestration component from the
worked example or need its own, and identify one schema or security
question you'd check before wiring it in, referencing the change-impact
discipline above.
