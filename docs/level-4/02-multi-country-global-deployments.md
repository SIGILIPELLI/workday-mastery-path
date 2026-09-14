# 02 · Multi-Country/Global Deployments

This module covers what changes when a Workday tenant operates across
many countries at once — localization, statutory compliance, and the
phased rollout patterns enterprises use to go global without a single
disruptive big-bang launch.

## Localization: what varies by country

A single tenant's core object model (Worker, Position, Supervisory
Organization) doesn't change per country, but a substantial layer of
configuration does:

| Localized area | What varies |
|---|---|
| **Business processes** | Country-specific approval steps (Level 2 Module 1's Works Council example), statutory notice periods for termination |
| **Absence plans** | Legally mandated leave minimums (Level 2 Module 9), which vary widely by jurisdiction |
| **Payroll** | Tax calculation, statutory deductions, and pay frequency norms differ entirely by country (Level 3 Module 4) |
| **Data privacy** | Field-level restrictions on what can be collected/displayed, driven by local regulation (e.g., restrictions on displaying certain personal data broadly) |

The tenant handles this variation the same way it handles any other
conditional behavior — condition rules keyed on worker country or legal
entity (Level 2 Module 1's mechanism), not separate tenants per country.

## Statutory compliance patterns

Workday delivers a baseline of **country-specific configuration
packages** for many jurisdictions (statutory absence plan templates,
localized business process variants) that a tenant can adopt as a
starting point rather than building every country's compliance logic
from scratch. Even with a delivered package, most enterprises still
validate and adjust it against their specific legal counsel's guidance
before go-live — a delivered template reduces build effort, it doesn't
eliminate the compliance review step.

## Phased global rollout strategies

Rolling out Workday across many countries rarely happens all at once —
common patterns:

| Rollout strategy | Approach |
|---|---|
| **Wave-based by region** | Deploy one region's countries at a time, learning from each wave |
| **Module-first, geography-second** | Deploy Core HCM globally first, then layer Payroll or Financials country by country |
| **Pilot country then scale** | Validate the whole approach in one representative, lower-risk country before wider rollout |

Each wave reuses the prior wave's tested configuration patterns
(condition rules, absence templates) as a starting point, adjusting only
for what's genuinely different in the new country — rather than each wave
starting from zero.

## Worked example: Meridian's UK expansion

Meridian Outfitters, previously US-only, opens its first international
stores in the UK:

1. **Legal entity and Company org setup** (Level 3 Module 3) for the new
   UK entity, with its own Company-level financial reporting.
2. **Statutory absence plan adoption** — UK statutory sick pay and holiday
   entitlement rules differ substantially from Meridian's existing US
   plans (Level 2 Module 9); Meridian adopts Workday's delivered UK
   statutory template as a starting point, then reviews it with UK
   employment counsel.
3. **BP localization** — the Termination business process gets a UK-
   specific condition rule branch (structurally identical to Level 2
   Module 1's Germany Works Council example) reflecting UK notice-period
   requirements, without altering the US branch's existing behavior.
4. **Payroll**: since Meridian doesn't yet run Workday Payroll for the
   UK, they initially integrate to a third-party UK payroll provider via
   Studio (Level 3 Module 1) rather than standing up Workday Payroll for
   just one small new country — planned as a candidate to bring in-house
   later if the UK operation grows.
5. **Pilot-then-scale**: the first two UK stores go live as a pilot wave,
   with lessons about UK-specific reporting needs folded into
   configuration before opening further UK stores.

## How It Actually Works

Country variation in a single tenant is handled entirely through the
same condition-rule and eligibility-rule mechanisms this path has
covered throughout — there's no separate "localization engine"; country
is simply one more attribute a rule can be conditioned on.

**A country-specific BP branch and a role-based security scope are both
instances of the same underlying pattern: a rule evaluated against a
worker or transaction's current attributes, deciding which of several
possible configured behaviors applies.** The UK Termination notice-period
branch is built exactly like Level 2 Module 1's Germany Works Council
branch — a condition rule comparing the worker's country or legal entity
attribute, attached to a step in the same shared Termination BP
definition. This is precisely why global rollout doesn't require a
separate BP definition per country: one definition, with as many
condition-rule branches as there are genuinely different behaviors
needed, mirrors how Level 4 Module 1 described a single tenant serving
many business units through scoped configuration rather than
duplication.

**Adopting a delivered statutory template doesn't remove condition-rule
design work — it pre-builds the common-case rules, leaving
tenant-specific adjustment as configuration on top of a working
starting point.** Workday's UK statutory absence template ships with
already-built accrual rules and eligibility logic (Level 2 Module 9's
mechanism) matching common UK statutory minimums; Meridian's
configuration work is adjusting or extending that template for anything
Meridian-specific (a more generous vacation policy layered on top of
statutory minimums), not authoring the statutory logic itself from
scratch.

**A phased rollout is architecturally just a sequence of scoped
condition-rule and eligibility-rule additions to already-existing shared
BP and plan definitions — each wave doesn't need its own tenant or
duplicated configuration, only its own scoped rule branch.** Because the
underlying BP definitions, security groups, and calculated fields are
already shared platform-wide (Level 4 Module 1's architecture), adding
France as a rollout wave after the UK means adding France-specific
condition-rule branches to the same shared definitions the UK branch was
added to — the "wave" is a project-management framing around a series of
narrowly scoped configuration additions, not a technically separate
deployment.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Localization | Country-specific configuration layered onto shared, global object/BP definitions |
| Statutory template | A delivered starting-point configuration for a jurisdiction's compliance baseline |
| Wave-based rollout | Deploying one region/country group at a time, reusing prior waves' patterns |
| Pilot country | A first, lower-risk deployment validating the overall approach before scaling |

## Exercise

Meridian is planning its next international wave into Germany. Using the
UK worked example as a template, list the localized areas (absence,
business process, payroll) you'd expect to need country-specific
condition rules or templates for, and identify which existing shared BP
definition from earlier in this path (name the specific module) the
German Works Council branch would be added to.
