---
title: "Workday Cheat Sheet: Quick Reference, Beginner to Advanced"
description: "A free Workday cheat sheet: 36 quick-reference tables from every level of the Workday Mastery Path, each linked to its full lesson."
---

# Workday Cheat Sheet

Quick-reference tables from **36 lessons** of the free Workday Mastery Path, in course order from beginner to advanced. Each section links to the full lesson with explanations and runnable examples.

## Level 1 · Entry

### [01 · What Is Workday?](level-1/01-what-is-workday.md)

| Concept | Key idea |
|---|---|
| SaaS | Workday hosts and upgrades the software; you never manage servers |
| Tenant | Your organization's isolated, configured instance of Workday |
| HCM | The people pillar — orgs, workers, staffing, comp, talent |
| Financials | The money pillar — GL, AP/AR, procurement, projects |
| Two releases/year | Every tenant upgrades on the same schedule automatically |
| Configuration over code | Most behavior is set through config screens, not custom code |

### [02 · Navigating the Workday UI](level-1/02-navigating-the-workday-ui.md)

| I want to... | Use this |
|---|---|
| See my pending approvals | Inbox worklet |
| Find a specific person or task | Global search bar |
| Do something to a specific worker/org/position | Navigate to it, then its related actions icon |
| See what a worklet gives me access to | Click the worklet tile itself |
| Approve/deny/send back a transaction | Open it from Inbox, review detail, then act |

### [03 · Core HCM Concepts](level-1/03-core-hcm-concepts.md)

| Term | One-line definition |
|---|---|
| Organization | A grouping container — supervisory, cost center, company, region, custom |
| Supervisory Organization | The management-hierarchy org that positions/jobs and workers sit inside |
| Position Management | Staffing model with persistent, numbered positions (filled or vacant) |
| Job Management | Staffing model with on-the-fly job requisitions, no persistent vacant record |
| Employee | Worker type: on payroll |
| Contingent Worker | Worker type: not on payroll (contractor/temp) |

### [04 · Business Processes Basics](level-1/04-business-processes-basics.md)

| Term | One-line definition |
|---|---|
| Business Process (BP) | A configured workflow definition attached to a transaction type |
| Step | One stage in a BP — Action, Approval, To Do, or Notification |
| Condition rule | Logic that branches a BP's path based on transaction data |
| Approve / Send Back / Deny | The three core actions an approver can take on an approval step |
| BP instance | One specific occurrence of a BP definition being run (e.g., "this hire") |

### [05 · Staffing Fundamentals](level-1/05-staffing-fundamentals.md)

| Transaction | What it does | Common approval pattern |
|---|---|---|
| Hire | Creates a new worker record into a position/job | Hiring manager → HR Partner/Ops |
| Transfer | Moves a worker to a different supervisory org | Losing manager → receiving manager |
| Job Change (promotion) | Role + often org change | Manager → HR → Compensation (if grade changes) |
| Termination (voluntary) | Ends employment, worker-initiated | Manager → HR exit checklist |
| Termination (involuntary) | Ends employment, employer-initiated | HR/Legal review → Manager executes |

### [06 · Compensation Basics](level-1/06-compensation-basics.md)

| Term | One-line definition |
|---|---|
| Compensation Package | The bundle of pay plans assigned to a worker |
| Comp Basis | The unit pay is expressed in — hourly, salaried, unit-based |
| Compensation Grade | A defined pay range (min/mid/max) tied to a job profile/level |
| Grade Profile | A grade plus its eligible compensation plans |
| Change Reason | The categorized cause of a pay change (merit, promotion, market, equity) |

### [07 · Reporting Basics](level-1/07-reporting-basics.md)

| Term | One-line definition |
|---|---|
| Standard report | A pre-built report available out of the box |
| Custom report | A tenant-built report using the report writer (Level 2+) |
| As Of date | The point in time a report reconstructs organizational/worker state for |
| Prompt | A filter input requested before a report runs |
| Export | Sending report output (typically to Excel) for further analysis |

### [08 · Security Basics](level-1/08-security-basics.md)

| Term | One-line definition |
|---|---|
| Security Domain | A bucket of related data/actions being protected |
| Security Group | A defined population of users |
| Domain Security Policy | The grant connecting a group to a domain at a given access level |
| Role-based group | Membership follows an assigned role, not a named person |
| View vs. Modify/Approve | The access levels a group can be granted per domain |

### [09 · Integration Basics Overview](level-1/09-integration-basics-overview.md)

| Term | One-line definition |
|---|---|
| Integration | Automated data flow keeping Workday and an external system in sync |
| Outbound | Data flowing from Workday to another system |
| Inbound | Data flowing from another system into Workday |
| EIB | Workday's configuration-based, no-code integration builder |
| Workday Studio / Connectors | More advanced integration tools for complex needs (Level 3) |

## Level 2 · Intermediate

### [01 · Advanced Business Process Configuration](level-2/01-advanced-bp-configuration.md)

| Concept | One-line definition |
|---|---|
| BP definition | An ordered sequence of steps for one BP type/subtype combination |
| Condition rule | A reusable calculated-field expression driving step or eligibility branching |
| Approval step | Requires an explicit approve/deny/send-back decision |
| Action/to-do step | Requires data entry/confirmation, no gating decision |
| Notification step | Informational only, cannot block the process |
| Routing rule | The logic that resolves an approval step's approver(s) dynamically |

### [02 · Organization Management Deep Dive](level-2/02-organization-management-deep-dive.md)

| Term | One-line definition |
|---|---|
| Cost Center | Organization type driving budget/ledger rollup |
| Company | Organization type representing the legal employing entity |
| Matrix Organization | A secondary, non-supervisory reporting relationship |
| Reorganization | The effective-dated BP that changes org hierarchy relationships |
| Inheritance | A worker's cost center/company is resolved by walking the live org hierarchy, not stored per worker |

### [03 · Advanced Staffing](level-2/03-advanced-staffing.md)

| Concept | One-line definition |
|---|---|
| Concurrent jobs | A worker holding more than one active position assignment simultaneously |
| Primary position | The assignment driving default benefits/manager/Inbox routing |
| Staffing model conversion | Changing an org from Job Management to Position Management (or vice versa) |
| Leave of Absence | A non-terminating status change with an expected return |

### [04 · Compensation &amp; Benefits Deep Dive](level-2/04-compensation-benefits-deep-dive.md)

| Term | One-line definition |
|---|---|
| Compensation Package | The container deciding which comp plans a role is eligible for |
| Compensation Grade | The min/mid/max range attached to a salary/hourly plan |
| Benefit Group | A bundle of benefit plans assigned via eligibility rules |
| Enrollment Event | The BP-triggered window in which elections can be made |
| Qualifying Life Event | A life change that opens an ad-hoc, time-limited enrollment window |

### [05 · Advanced Reporting](level-2/05-advanced-reporting.md)

| Term | One-line definition |
|---|---|
| Data source | The starting business object and its reachable related fields |
| Calculated field | A computed value derived from other fields, evaluated at run time |
| Prompt | A runtime-selectable filter parameter, itself security-scoped |
| Matrix report | A report type for cross-tabulated, subtotaled summaries |

### [06 · Workday Studio &amp; EIB Basics](level-2/06-workday-studio-eib-basics.md)

| Term | One-line definition |
|---|---|
| EIB | A wizard-configured, code-free integration for bulk load or extract |
| Inbound EIB | Loads data into Workday, submitting one BP instance per row |
| Outbound EIB | Extracts data out of Workday, sourced from a report |
| Workday Studio | A visual, connector-based IDE for complex integration logic |
| Validate-only run | Checks each row against BP rules without committing transactions |

### [07 · Security Groups Deep Dive](level-2/07-security-groups-deep-dive.md)

| Term | One-line definition |
|---|---|
| Intersection group | Membership requires belonging to all listed groups |
| Aggregation group | Membership requires belonging to any listed group |
| Constrained group | Scope limited to the role holder's own org and subordinates (typically) |
| Security segment | A data-value-based filter applied on top of domain/scope |

### [08 · Talent &amp; Performance Management](level-2/08-talent-performance-management.md)

| Term | One-line definition |
|---|---|
| Cascaded goal | An org-level goal propagated down the hierarchy as linked child goals |
| Competency | A reusable skill/behavior definition referenced by review templates |
| 9-box grid | A calculated performance × potential placement, not a stored field |
| Succession plan | Candidate pool with readiness levels attached to a critical position |

### [09 · Absence &amp; Time Tracking](level-2/09-absence-time-tracking.md)

| Term | One-line definition |
|---|---|
| Accrual rule | How an absence balance builds over time |
| Time calculation tag | Rule classifying raw time blocks into regular/overtime/etc. |
| Time block | A single recorded segment of worked time |
| Calculated balance | A leave balance derived from accrual and usage records, not stored directly |

## Level 3 · Advanced

### [01 · Advanced Integrations](level-3/01-advanced-integrations.md)

| Term | One-line definition |
|---|---|
| WWS (SOAP) | Workday's comprehensive, XML-based web services API |
| REST | Workday's newer, resource-oriented JSON API, growing coverage |
| ISU / ISSG | Integration System User and its dedicated security group |
| Cloud Connect | Pre-built, vendor-specific integration templates |
| Transform step | Reshapes data between source and target format mid-pipeline |

### [02 · Advanced Security Configuration](level-3/02-advanced-security-configuration.md)

| Term | One-line definition |
|---|---|
| Domain Security Policy (DSP) | Grants a group access to a data domain, staged as pending until activated |
| Business Process Security Policy | Governs who can initiate/approve/cancel a specific BP type |
| Security segment | A data-value predicate layered on top of a domain's base grant |
| View Security for Securable Item | Audit tool listing every group granted access to a given domain/segment |

### [03 · Workday Financials Basics](level-3/03-workday-financials-basics.md)

| Term | One-line definition |
|---|---|
| Worktag | A flexible, multi-dimensional tag (Cost Center, Project, etc.) attached to financial transactions |
| Chart of Accounts | The traditional ledger account classification, used alongside worktags |
| Three-way match | Comparing requisition, PO, and invoice data before releasing payment |
| Journal Entry | A direct accounting record, either transaction-generated or manually entered |

### [04 · Payroll Fundamentals](level-3/04-payroll-fundamentals.md)

| Term | One-line definition |
|---|---|
| Earning / Deduction | Configurable categories building gross-to-net pay |
| Pay period lockdown | Freezing inputs into an immutable snapshot before calculation |
| Off-cycle run | A payroll run outside the regular calendar, for corrections/urgent pay |
| Retro pay | A calculated delta for a compensation change with a past effective date |

### [05 · Advanced Calculated Fields](level-3/05-advanced-calculated-fields.md)

| Term | One-line definition |
|---|---|
| Related field lookup | A calculated field pulling a value from a related business object |
| Aggregate function | A calculated field summarizing a collection of related objects (Count, Sum) |
| Calculated field chaining | One calculated field referencing another as an input |
| Evaluation frequency | How often a calculation runs — the key cost driver for complex fields |

### [06 · Recruiting Module Deep Dive](level-3/06-recruiting-module-deep-dive.md)

| Term | One-line definition |
|---|---|
| Job Requisition | The request to fill a role, related to a Position or job profile |
| Candidate Pipeline | Configurable stages tracking an applicant's progress |
| Offer | Proposed compensation/start date captured against a candidate, pre-Hire |
| Hire conversion | Creating a new Worker object and Hire BP instance from an accepted offer |

### [07 · Learning Module Basics](level-3/07-learning-module-basics.md)

| Term | One-line definition |
|---|---|
| Learning Program | An ordered course sequence tracked as one curriculum |
| Assignment rule | An eligibility-rule-style trigger auto-enrolling a population |
| Content Provider integration | External learning content surfaced in Workday's catalog |
| Competency bump | A course-completion-triggered update to a talent competency record |

### [08 · Workday Extend Basics](level-3/08-workday-extend-basics.md)

| Term | One-line definition |
|---|---|
| Custom Object | A tenant-defined data structure with fields/relationships, structurally like delivered objects |
| Orchestration | Custom workflow logic that can call standard Workday BPs and web services |
| App/Page Builder | The custom UI a worker interacts with, built from delivered components |
| Extend Security | Custom objects secured via the same domain/group/scope model as delivered objects |

### [09 · Testing &amp; Change Management in Workday](level-3/09-testing-change-management.md)

| Term | One-line definition |
|---|---|
| Sandbox tenant | Isolated instance for building and testing configuration |
| Preview tenant | Refreshed ahead of the next release, for pre-production validation |
| Tenant migration | Moving specific configuration objects between tenants, not live data |
| Regression test script | Documented scenarios re-run after changes to confirm nothing broke |

## Level 4 · Master

### [01 · Enterprise Workday Architecture](level-4/01-enterprise-workday-architecture.md)

| Term | One-line definition |
|---|---|
| Single-tenant consolidation | Default architecture: one tenant representing the whole enterprise |
| Multi-tenant architecture | Isolated tenants, used for M&A transition or deliberate separation |
| Configuration review board | Governance layer approving cross-cutting configuration changes |
| Org hierarchy mapping | Fitting an acquired entity's structure into the existing hierarchy |

### [02 · Multi-Country/Global Deployments](level-4/02-multi-country-global-deployments.md)

| Term | One-line definition |
|---|---|
| Localization | Country-specific configuration layered onto shared, global object/BP definitions |
| Statutory template | A delivered starting-point configuration for a jurisdiction's compliance baseline |
| Wave-based rollout | Deploying one region/country group at a time, reusing prior waves' patterns |
| Pilot country | A first, lower-risk deployment validating the overall approach before scaling |

### [03 · Advanced Workday Financials](level-4/03-advanced-workday-financials.md)

| Term | One-line definition |
|---|---|
| Multi-book accounting | One transaction posting to multiple ledger books under different rules |
| Intercompany transaction | A transaction between two entities within the same enterprise, auto-generating matched entries |
| Elimination entry | Removes intercompany activity from consolidated totals to avoid double-counting |
| Currency translation | Converting local-currency results into the enterprise reporting currency at consolidation |

### [04 · Workday Prism Analytics](level-4/04-workday-prism-analytics.md)

| Term | One-line definition |
|---|---|
| Data Source | Raw data loaded into Prism, from Workday or external systems |
| Dataset | A transformed, materialized table built for analysis |
| Data Change Task | A transformation step (join, filter, aggregate) building a dataset |
| Discovery Board | Prism's dashboard/exploration layer for a finished dataset |

### [05 · Advanced Extend Development](level-4/05-advanced-extend-development.md)

| Term | One-line definition |
|---|---|
| Schema versioning | Treating custom object field changes as governed, impact-assessed changes |
| Custom app inventory | A portfolio-level record of apps, owners, and access grants |
| Direct API integration | An Extend app calling an external API synchronously within its own workflow |
| Reusable orchestration component | Shared workflow logic multiple apps call rather than duplicate |

### [06 · Workday Certification Prep](level-4/06-workday-certification-prep.md)

| Term | One-line definition |
|---|---|
| Certification track | A functional-area-specific Workday certification path |
| Scenario-based question | An exam question testing applied configuration judgment over recall |
| Worked example bank | A personal set of self-authored practice scenarios for exam prep |
| Cross-module scenario | A practice or exam scenario spanning more than one functional area |

### [07 · Change Management for Workday Rollouts](level-4/07-change-management-rollouts.md)

| Term | One-line definition |
|---|---|
| Stakeholder map | Identifying groups affected by a change and their specific concerns |
| Workaround closure | Deliberately retiring a legacy path once new-process adoption is stable |
| Go-live support | Elevated support availability immediately following a rollout |
| Reinforcement communication | Post-go-live follow-up addressing observed adoption friction |

### [08 · Implementation Lifecycle &amp; Project Management for Workday](level-4/08-implementation-lifecycle-pm.md)

| Term | One-line definition |
|---|---|
| Architect phase | The design-decision phase, including flagging high-cost-to-change choices |
| Functional workstream | A design-and-build team scoped to one functional area (HCM, Financials, etc.) |
| High-cost-to-change decision | A foundational design choice expensive to revisit after go-live |
| Hypercare | Elevated post-go-live support before transitioning to steady-state |

### [09 · Career Growth: Workday Analyst/Consultant Path](level-4/09-career-growth-analyst-consultant.md)

| Term | One-line definition |
|---|---|
| HRIS Analyst | In-house, day-to-day tenant administration and reporting role |
| Functional Consultant | Partner-employed, area-specific implementation and configuration expertise |
| Solution Architect | Cross-functional design authority spanning multiple modules |
| Hands-on practice | Real (or Sandbox) tenant configuration experience, distinct from reading alone |

---

Go deeper with the full [Workday Mastery Path](index.md) curriculum or test yourself with the [interview questions](interview-questions.md).
