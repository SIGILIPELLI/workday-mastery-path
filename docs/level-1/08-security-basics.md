# 08 · Security Basics

Every module so far has quietly depended on security: which worklets you
see (Module 2), which reports you can run (Module 7), which approvals land
in your Inbox (Module 4). This module makes that dependency explicit by
covering Workday's security model conceptually — security groups vs.
domains — without touching the actual configuration screens, which is a
Level 2+ topic.

## The two-sided security model: domains and security groups

Workday security answers two separate questions, each governed by a
different building block:

| Question | Governed by |
|---|---|
| **"What can be done, and to what data?"** | **Security Domains** — a categorized bucket of related actions/data (e.g., "Compensation Data," "Worker Data: Public") |
| **"Who can do it?"** | **Security Groups** — a defined population of users (e.g., "HR Partner," "Manager," "Compensation Analyst") |

A **Domain Security Policy** connects the two: it grants a specific
security group specific levels of access (View, Modify, or both) to a
specific domain. Nothing is secured by attaching permissions directly to an
individual user — everything routes through this domain ↔ group
relationship, which is what makes Workday security auditable and
consistent at scale.

## Security domains — what's being protected

A domain groups related data and actions together so they can be secured
as one unit rather than permission-by-permission. A few illustrative
domains relevant to what you've already learned:

| Domain (illustrative) | Covers |
|---|---|
| **Worker Data: Public Worker Reports** | Basic worker info most employees can see about colleagues (name, title, org) |
| **Worker Data: Compensation** | Pay rate, compensation history — much more restricted |
| **Business Process: Hire** | Who can initiate/approve Hire transactions |
| **Organization Data: Organization Management** | Who can view/edit the org hierarchy itself |

Notice compensation data sits in its own domain, separate from general
worker data — this is deliberate: an HR Partner might have broad access to
worker data but need a separate, more restrictive grant to see compensation
figures, and Workday's domain model makes that distinction directly
configurable rather than all-or-nothing.

## Security groups — who gets access

A **security group** defines a population of users, and groups come in a
few conceptual varieties:

| Security group type | How membership is determined |
|---|---|
| **Role-based** | Membership follows an assigned role, e.g., "HR Partner" for whoever is assigned that role on a given supervisory org |
| **Job-based** | Membership follows job profile/position, e.g., anyone in a "Store Manager" job profile |
| **User-based** | Explicit named individuals — used sparingly, since it doesn't automatically update as people change roles |
| **Intersection/aggregation groups** | Combinations of other groups, for fine-grained scoping |

Role-based groups are the workhorse of most tenants: because membership
follows the *role* rather than the *person*, when Priya (Module 2's HR
Business Partner) is replaced by someone else next year, the new hire
automatically inherits the correct access the moment they're assigned the
HR Partner role for that org — no manual security re-grant required.

## Putting it together: why Priya sees what she sees

Recall Priya from Module 2, who saw an Inbox with pending approvals and
used related actions to view compensation history. Both of those
experiences trace directly back to domain/group grants:

| Priya's action | Domain involved | Why she could do it |
|---|---|---|
| Approving a Change Job in her Inbox | Business Process: Job Change | She's in the "HR Partner" security group, granted Approve access on this domain for her supervisory org's scope |
| Viewing an employee's compensation history | Worker Data: Compensation | Her HR Partner group is also separately granted View access on this domain |
| NOT seeing a "Configure Business Process" worklet | Business Process Administration | She is *not* in the HRIS Admin security group, which is the only group granted access to this domain |

That last row matters: security is as much about what's correctly *hidden*
as what's correctly shown. A well-configured tenant ensures Priya can do
her HR Partner job fully without ever being able to reach configuration
screens meant for the HRIS admin team — not because anyone distrusts her,
but because scoping access to what a role needs is the baseline discipline
that keeps a large tenant auditable.

## Worked example: scoping a new security group at Meridian

Meridian Outfitters is creating a new role: **Store Operations Analyst**, a
regional role that needs to view (but not approve) staffing and
compensation data across all stores in their region, without touching
corporate or distribution center data. Sketching the access:

| Domain | Access level | Scope |
|---|---|---|
| Worker Data: Public Worker Reports | View | All workers in the analyst's assigned region |
| Worker Data: Compensation | View | All workers in the analyst's assigned region |
| Business Process: Hire, Job Change, Termination | View only (not Approve) | All workers in the analyst's assigned region |
| Organization Data: Organization Management | View | All orgs in the analyst's assigned region |

Note the pattern: every row is scoped to "the analyst's assigned region,"
not the whole company — and every access level is View, never Modify or
Approve, since this role's job is analysis, not transaction execution. This
is exactly the kind of table a security architect fills in during
Level 2's Security Groups Deep Dive, before it's ever built inside the
tenant.

## How It Actually Works

Every access decision in Workday — whether a worklet tile renders, whether
a related action appears, whether a report even runs — resolves down to
the same evaluation performed against the same domain/group grant records,
executed fresh at the moment of the request rather than cached from login.

**Access control is evaluated as an intersection, not a single lookup:
group membership × domain grant × scope.** When Priya opens an employee's
compensation history, Workday doesn't check a single "can Priya see this"
flag. It resolves, in order: (1) which security groups Priya currently
belongs to, by evaluating her role assignments against the live
organizational data from Module 3 (a role-based group's membership is
itself a query, re-run each time, not a static list); (2) which Domain
Security Policies grant those groups access to the "Worker Data:
Compensation" domain, and at what level (View, Modify); (3) whether the
grant's configured **scope** — often "own supervisory organization and
subordinates" — covers the specific worker instance being requested, by
walking the same supervisory-org relationship tree from Module 3. Only if
all three resolve favorably does the action succeed. This is why revoking
Priya's HR Partner role assignment (a Module 3/5-style organizational
change) instantly removes her compensation access with zero separate
security administration step — the group membership that access depended
on was never a static grant, it was a live query against her current role.

**Domains exist to let one grant cover many related actions/fields at
once, because checking permission field-by-field or action-by-action
wouldn't scale.** A domain bundles a set of secured items (specific report
fields, specific business process actions, specific worker attributes)
that share a sensitivity profile, so a single Domain Security Policy
grant/revoke changes access to the entire bundle atomically. This is the
mechanism behind compensation living in its own domain separate from
general worker data: it lets a tenant grant broad View access to ordinary
worker attributes while withholding the compensation bundle specifically,
using one additional policy rather than hundreds of individual field-level
grants.

**Scope is what turns a role-based grant into a properly bounded one
rather than an all-or-nothing switch.** The same "HR Partner" security
group definition, with the same domain grants, produces different visible
data for two different HR Partners because the grant's scope clause is
evaluated against *each user's own* organizational position — "their
assigned supervisory organizations and subordinates" resolves differently
per person. This is exactly why the Store Operations Analyst worked example
above scopes every row to "the analyst's assigned region": the scope clause
is what prevents one role definition, reused for every regional analyst,
from accidentally granting company-wide visibility to all of them.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Security Domain | A bucket of related data/actions being protected |
| Security Group | A defined population of users |
| Domain Security Policy | The grant connecting a group to a domain at a given access level |
| Role-based group | Membership follows an assigned role, not a named person |
| View vs. Modify/Approve | The access levels a group can be granted per domain |

## Exercise

Using the table format from the worked example, design the security access
for a new "Benefits Coordinator" role at Meridian that should be able to
View all workers' benefits enrollment data company-wide, Modify (process)
benefits enrollment changes company-wide, but have no access at all to
compensation data or the ability to approve any staffing business process.
List at least three domains with their access levels and scope, and explain
in one sentence why compensation data is deliberately excluded from this
role's grants.
