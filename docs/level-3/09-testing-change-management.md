# 09 · Testing & Change Management in Workday

Every module so far has referenced "test in Sandbox before production" in
passing. This module makes that discipline explicit: Workday's tenant
model, release cadence, and the testing practices that keep configuration
changes safe at scale.

## The multi-tenant model: Sandbox, Preview, Production

A Workday customer typically operates more than one **tenant** (an
isolated instance of their configuration and data):

| Tenant type | Purpose |
|---|---|
| **Sandbox (Preview/non-Preview)** | Where configuration changes and integrations are built and tested |
| **Preview tenant** | Specifically refreshed ahead of upcoming Workday releases, for testing against the *next* release's changes before it reaches Production |
| **Production** | The live tenant real transactions, real workers, and real payroll run in |

Configuration changes are built and validated in Sandbox, then migrated
(via Workday's tenant migration tooling, moving specific configuration
objects rather than the whole tenant) to Production once tested — mirroring
the pending/activate discipline from Level 3 Module 2, but at the level of
moving configuration between tenants entirely.

## Workday's release cadence

Workday ships **feature releases** on a regular schedule (historically
twice yearly, alongside more frequent smaller updates), and — distinct
from most on-premise software — every tenant is upgraded together on the
same schedule; a customer cannot indefinitely stay on an old version.
This is why the Preview tenant matters operationally: it's refreshed with
the *upcoming* release ahead of time specifically so a customer can
validate their configuration and integrations against new behavior before
that release reaches their Production tenant on the fixed schedule.

## Change management practices

| Practice | Purpose |
|---|---|
| **Change impact analysis** | Before a config change, identify every BP, report, and integration that could be affected |
| **Regression test scripts** | A documented set of scenarios re-run after any significant change, to confirm nothing broke |
| **User Acceptance Testing (UAT)** | Business stakeholders (not just configurers) validate a change behaves as intended before go-live |
| **Release readiness review** | Ahead of each feature release, reviewing what's changing and whether any tenant configuration needs adjustment |

## Tenant setup and configuration migration

Workday's **Tenant Setup** tooling packages specific configuration
objects (a business process definition, a set of security policies, a
report) for migration between tenants, rather than requiring every
change to be manually re-keyed in each tenant. This is what makes the
Sandbox-build, Production-migrate workflow practical at scale — a
configurer builds and iterates in Sandbox, then migrates the finished,
tested configuration as a defined package rather than reproducing dozens
of manual steps in Production by hand.

## Worked example: testing a feature release at Meridian

Ahead of an upcoming Workday feature release affecting business process
notification behavior, Meridian's Workday team:

1. **Reviews release notes** identifying that notification step behavior
   (Level 2 Module 1's step type) is changing in a way that could affect
   their custom Works Council notification step (Level 2 Module 1's
   worked example).
2. **Tests in Preview tenant**, which has already been refreshed with the
   upcoming release, running their existing regression test script's
   Termination BP scenarios (including the German-worker Works Council
   case) to confirm the notification still fires correctly under the new
   release behavior.
3. **Identifies a gap**: the new release changes how notification
   recipients are resolved in an edge case Meridian's regression script
   didn't originally cover (a worker with no assigned HR Partner). Adds
   this case to the regression script.
4. **UAT with the compensation/HR team**, confirming the Termination flow
   still matches business expectations, not just technical correctness.
5. **Sign-off and no urgent remediation needed** — since Meridian tested
   ahead of the fixed release date, they had time to adjust before the
   release reached Production automatically.

## How It Actually Works

Because every Workday tenant runs the same versioned platform code, a
Preview tenant showing new release behavior is not a simulation or a
separate testing product — it's a genuine instance of the exact
software Production will run once the release date arrives, which is
what makes Preview testing a reliable predictor of production behavior.

**A tenant migration moves a configuration object's definition, not a
live snapshot of Sandbox transaction data — Production's actual worker
and transaction data is untouched by a migration.** When Meridian
migrates a tested BP definition change from Sandbox to Production, the
migration package contains the BP definition object (steps, condition
rules) itself, not any of the Sandbox tenant's test worker records or
test transactions. This separation is what makes iterative Sandbox
testing safe — configurers can create, break, and recreate test data
freely in Sandbox without any risk of that test data reaching Production,
because migration explicitly scopes to configuration objects, not the
full tenant contents.

**Regression test scripts encode specific condition-rule branches as
scenarios precisely because a shared platform-wide release can change
behavior at the branch-evaluation level, not just at the top-level
feature being marketed in release notes.** Meridian's regression script
specifically re-tests the German-worker Works Council branch (a condition
rule, Level 2 Module 1) rather than just confirming "Termination BP still
completes," because a release change to a lower-level mechanism (how
notification steps resolve or a calculation function's rounding behavior)
can alter one specific branch's outcome while every other branch of the
same BP definition behaves identically. Testing only the "happy path"
would have missed exactly this kind of narrow, condition-rule-specific
change.

**Fixed-schedule, tenant-wide releases are what make Preview-ahead-of-
Production testing necessary rather than optional — there's no tenant-
level choice to delay the upgrade and test at leisure afterward.** Because
every tenant is upgraded together on Workday's schedule (unlike
customer-controlled-timing on-premise upgrades), a gap discovered only
after Production has already been upgraded is discovered too late to
prevent live impact. This is the structural reason Preview testing
against the *next* release, well ahead of that fixed date, is treated as
a required practice rather than a nice-to-have — there is no "hold off on
upgrading until we're ready" lever available at the tenant level.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Sandbox tenant | Isolated instance for building and testing configuration |
| Preview tenant | Refreshed ahead of the next release, for pre-production validation |
| Tenant migration | Moving specific configuration objects between tenants, not live data |
| Regression test script | Documented scenarios re-run after changes to confirm nothing broke |

## Exercise

Meridian's Workday team is about to migrate the "Executive Compensation
Committee" security segment change from Level 3 Module 2 into Production.
List the regression test scenarios you'd want covered before migrating
(referencing specific security groups and domains from Module 2), and
explain why testing this in Sandbox first, rather than configuring
directly in Production, matters even though the change itself is
"just" a security policy update.
