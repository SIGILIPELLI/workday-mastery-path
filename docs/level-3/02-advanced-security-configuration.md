# 02 · Advanced Security Configuration

Level 2 Module 7 covered security group types and composition. This
module covers configuring security at the tenant level: managing domain
security policies as versioned objects, business process security
policies, and the audit tooling used to verify a security model is
correctly locked down.

## Domain Security Policies as versioned, activated objects

A **Domain Security Policy (DSP)** isn't edited in place the way a typo
gets fixed in a document — changes to a DSP are made in a **draft/pending**
state and only take effect tenant-wide once explicitly **activated**
(via the "Activate Pending Security Policy Changes" task), which requires
naming a comment describing the change for the audit trail. Multiple DSP
edits across different domains and groups can be staged as pending changes
and activated together in a single batch, rather than each change going
live the instant it's saved.

## Business Process Security Policies

Distinct from data-domain DSPs, a **Business Process Security Policy**
governs who can *initiate*, *approve*, or *cancel* a specific business
process type — the security layer underneath the routing/condition-rule
mechanics from Level 2 Module 1. A worker might have View access to
Job Change data (a domain grant) but no ability to actually *initiate* a
Job Change transaction unless the corresponding BP security policy grants
that action to their security group — the two layers (data access,
transactional action) are configured and activated independently.

## The Security Audit toolset

Workday provides dedicated tools for verifying a security configuration
rather than requiring manual trial-and-error:

| Tool | Purpose |
|---|---|
| **View Security for Securable Item** | Shows every security group currently granted access to a specific domain, and at what level |
| **Compare Security Between Two Security Groups** | Diffs two groups' domain grants, useful for verifying a new group doesn't accidentally under- or over-grant relative to an existing similar one |
| **View User Access** (where enabled) | Shows what a *specific user* can actually see/do, resolved through all their group memberships at once |

Running these audits *before* activating a pending security change is
what catches the difference between "the config I intended" and "the
access this actually produces once every group membership and scope
resolves together" — a gap that's easy to introduce with intersection
and aggregation groups (Level 2 Module 7) if one constituent group's
scope was set up differently than assumed.

## Segment-based security in configuration

Level 2 Module 7 introduced security segments conceptually. Configuring
one involves defining the segment (the data-value predicate, e.g., "job
level = Executive"), attaching it to the relevant domain, and then
granting specific groups access to that segment specifically — separate
from, and layered on top of, the domain's base grant. A group with base
domain access but no segment grant sees every record *except* the ones
matching the segment's predicate.

## Worked example: locking down executive compensation at Meridian

Meridian's security team needs to ensure only a small "Executive
Compensation Committee" security group can view compensation data for
job level "Executive," while all other HR Partners keep normal
compensation visibility for everyone else:

1. **Define a security segment**: `Job_Level = "Executive"`, attached to
   the Worker Data: Compensation domain (Level 1 Module 8).
2. **Configure the segment's DSP** as a pending change: grant View to the
   "Executive Compensation Committee" security group; do not grant it to
   the general "HR Partner" role-based group.
3. **Audit before activating**: run "Compare Security Between Two Security
   Groups" comparing "HR Partner" against "Executive Compensation
   Committee" on this specific domain, confirming HR Partner shows no
   segment grant.
4. **Activate** the pending change with a descriptive comment ("restrict
   executive comp visibility per compliance request, Q3").
5. **Post-activation audit**: use View Security for Securable Item on the
   segment to confirm only the intended group appears.

## How It Actually Works

Security configuration changes are staged and applied as their own
governed, auditable transaction — the same "don't overwrite, add a dated
record and make the transition explicit" discipline that runs through
staffing (Level 1 Module 5) and organization (Level 2 Module 2) data
applies here to the security model itself.

**A pending security change doesn't affect live access evaluation until
activation — draft and active states are genuinely separate, evaluated
states, not just a UI convenience.** Between saving a DSP edit and
activating it, every real access check tenant-wide continues resolving
against the previously active policy version, exactly as before. This
is what makes staging multiple related DSP changes together safe: a
configurer can build out an entire coordinated security redesign (new
segment, new group grants, adjusted BP security policy) across several
edits and only expose users to the *complete, coherent* new state at one
activation moment, rather than a half-migrated state where some grants
have changed and others haven't.

**Segment grants intersect with base domain grants rather than replacing
them, which is why a group needs both to see segment-restricted data.**
An HR Partner with base View access to Worker Data: Compensation, but no
grant on the "Executive" segment, doesn't get an error or a blocked
screen when viewing an executive's record — the underlying query simply
excludes records matching the segment predicate from what's returned,
the same way Level 1 Module 8's scope clause silently narrows a result
set rather than throwing an access-denied response. This is a deliberate
design choice: the same report or worklet renders correctly for every
viewer, showing more or fewer rows depending on their resolved
grants, rather than needing separate report versions per security
posture.

**Business Process Security Policies and Domain Security Policies are
independent grant surfaces evaluated together, which is why "can view"
and "can initiate" are configured, activated, and audited as two
separate concerns.** A worker's ability to submit a Job Change is checked
against the Job Change BP Security Policy's initiate-action grant for
their security groups; a completely separate check against the relevant
domain's DSP governs whether they can *see* particular fields on that
same transaction once submitted. This split is why a manager can be
correctly granted the ability to initiate a compensation change for their
own team while still being blocked from viewing compensation history
outside their own transaction — two different policies, two different
grants, evaluated for two different questions.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Domain Security Policy (DSP) | Grants a group access to a data domain, staged as pending until activated |
| Business Process Security Policy | Governs who can initiate/approve/cancel a specific BP type |
| Security segment | A data-value predicate layered on top of a domain's base grant |
| View Security for Securable Item | Audit tool listing every group granted access to a given domain/segment |

## Exercise

Meridian's security team wants to grant a new "Regional Comp Analyst"
group View access to compensation data only for job levels below
Director, across all regions. Sketch the segment definition, the DSP
grant, and the audit step you'd run before activation to confirm this
group cannot see Director-and-above compensation data anywhere in the
company.
