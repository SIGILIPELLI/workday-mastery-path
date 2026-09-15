---
description: "Security Groups Deep Dive — Level 1 Module 8 introduced domains and groups conceptually. This module goes deeper into the specific security group types…"
---

# 07 · Security Groups Deep Dive

Level 1 Module 8 introduced domains and groups conceptually. This module
goes deeper into the specific security group types, how scope is actually
configured, and the aggregation patterns used to compose fine-grained
access from simpler building blocks.

## Security group types, in configuration detail

Building on Level 1's four conceptual categories, here's what configuring
each actually involves:

| Group type | Configuration mechanism |
|---|---|
| **Role-Based Security Group** | Defined by naming an *organization role* (e.g., "HR Partner"); membership auto-resolves to whoever holds that role, per org, per Module 1's live-query pattern |
| **Job-Based Security Group** | Defined by naming one or more job profiles or management levels; membership auto-resolves to anyone currently in a matching job |
| **User-Based Security Group** | An explicit, named list of individual users, maintained manually |
| **Intersection Security Group** | Members must belong to *all* of two or more specified groups simultaneously |
| **Aggregation Security Group** | Members belong to *any* of two or more specified groups (a union) |

## Scoping: unconstrained vs. constrained groups

A role-based or job-based group can be **unconstrained** (grants apply
company-wide) or **constrained** by an organizational scope — most
commonly "the role holder's own supervisory organization and its
subordinates." Constrained groups are what make the same "HR Partner"
role definition produce different visible data per person, exactly as
Level 1 Module 8 described. Configuring the constraint correctly (self and
subordinates, vs. self only, vs. a named organization type boundary) is
one of the most consequential decisions a security architect makes,
because getting it wrong either over-exposes data or silently blocks
legitimate access.

## Intersection and aggregation groups — composing access

Intersection and aggregation groups let a configurer build precise access
without inventing a brand-new role-based group for every nuance:

| Pattern | Example |
|---|---|
| **Intersection** | "HR Partners" ∩ "assigned to the EMEA region" → an intersection group for EMEA-specific HR Partner access, without redefining what "HR Partner" means globally |
| **Aggregation** | "Store Manager" ∪ "Assistant Manager" → one group for a domain grant that both roles should equally have, without duplicating the grant twice |

Using intersection/aggregation groups instead of duplicating base role
definitions keeps the underlying role and job-profile definitions clean
and reusable — the composition, not the base building blocks, is where
the fine-grained nuance lives.

## Segment-based security (an advanced scoping pattern)

Beyond organizational scope, **security segments** let particular fields
or transactions be restricted further based on data values rather than
org hierarchy — for example, restricting who can view a specific
"executive compensation" segment of pay data even among users who
otherwise have general compensation View access. This pattern exists for
exactly the cases where organizational scope alone isn't the right
boundary (a VP's compensation shouldn't be visible to every HR Partner who
has general compensation View, regardless of which org that VP sits in).

## Worked example: Meridian's regional HR Partner intersection group

Recall Level 1 Module 8's Store Operations Analyst example. Now Meridian
wants a *more* restricted variant: "EMEA HR Partner" access that should
combine the standard HR Partner domain grants with a hard region boundary,
without redefining the global HR Partner role:

1. **Base role-based group**: "HR Partner" (already exists, unconstrained
   definition, scoped per-holder to their own org and subordinates as
   usual).
2. **Region-tagging group**: "EMEA Region Assignment" — a role-based or
   custom group whose membership reflects assignment to EMEA-tagged
   organizations.
3. **Intersection group**: "HR Partner ∩ EMEA Region Assignment" — created
   as a new security group referencing both of the above.
4. **Domain grant**: The EMEA-specific compensation segment's Domain
   Security Policy grants View access to the *intersection* group, not to
   the base HR Partner group.

Result: an HR Partner outside EMEA never sees the EMEA-restricted
compensation segment, and an EMEA-assigned person who isn't an HR Partner
still can't see it either — only the intersection satisfies both grants.

## How It Actually Works

An intersection or aggregation group has no independent membership list of
its own — its membership is computed at evaluation time from its
constituent groups' membership, using the same live-query pattern that
resolves role-based membership in the first place.

**Intersection and aggregation are boolean operations evaluated over
already-computed membership sets, not separately maintained rosters.**
When Workday checks whether a specific user belongs to the "HR Partner ∩
EMEA Region Assignment" group, it doesn't consult a stored list for that
composite group. It independently resolves membership in "HR Partner" (a
live role-query, per Level 1 Module 8) and membership in "EMEA Region
Assignment" (another live query), then returns true only if the user
appears in both result sets. This is why the composite group's membership
updates automatically and instantly when either underlying group's
resolution changes — reassigning someone's role, or reassigning their
regional tag — with zero maintenance on the intersection group definition
itself.

**Scope constraints on a role-based group are applied as an additional
filter on top of the domain grant's population, evaluated per grant use,
not baked into the group's membership.** A "constrained to own org and
subordinates" HR Partner group doesn't have a smaller membership list than
an unconstrained one — the *group* still resolves to every HR Partner
company-wide. The constraint is applied when a specific domain grant is
evaluated for a specific data instance: is *this* worker's org within
*this* particular HR Partner's own-org-and-subordinates scope. This
two-stage evaluation (who's in the group, then does the scope cover this
specific instance) is exactly the "group × domain × scope" intersection
Level 1 Module 8 described, made concrete.

**Security segments add a third filtering dimension — a data-value
predicate — evaluated alongside, not instead of, domain and scope.** An
executive-compensation segment attaches a condition (e.g., "job level ≥
executive") to a subset of the compensation domain's data. A user's
overall access to view compensation still requires the domain grant and
org scope from Level 1 Module 8 to pass first; the segment then further
restricts which *specific records* within that already-scoped population
are visible, based on the record's own data rather than the org tree.
This layering is why an HR Partner with broad compensation View access
can still be correctly blocked from seeing one VP's pay figure sitting
inside their own scope.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Intersection group | Membership requires belonging to all listed groups |
| Aggregation group | Membership requires belonging to any listed group |
| Constrained group | Scope limited to the role holder's own org and subordinates (typically) |
| Security segment | A data-value-based filter applied on top of domain/scope |

## Exercise

Design an aggregation group for Meridian combining "Store Manager" and
"District Manager" job-based groups so both can be granted the same View
access to a new "Store Performance Dashboard" domain in one grant.
Then design a *separate* intersection group that would restrict a
different, more sensitive domain to only District Managers who are also
tagged to the "West Region." Explain in one sentence why these call for
two different composition patterns.
