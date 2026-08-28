# 09 · Integration Basics Overview

Workday doesn't operate in isolation — payroll vendors, benefits carriers,
badge/access systems, and dozens of other third-party systems need worker
data flowing in and out of the tenant. This module covers integrations at
a purely conceptual level: what an EIB is, what integrations generally do,
and why they matter — no tenant access or configuration is needed, and
Level 3 Module 1 is where hands-on integration configuration concepts
begin.

## Why integrations exist

Even though Workday is the system of record for worker data, most
organizations run other specialized systems alongside it — a benefits
carrier's own enrollment portal, a badge-access system for physical
security, a specialized payroll provider in a country Workday doesn't
directly process payroll for, an applicant tracking system for parts of
recruiting. **Integrations** keep those systems synchronized with Workday
without requiring anyone to manually re-enter the same data twice.

| Direction | Example |
|---|---|
| **Outbound** (Workday → external system) | New hire data sent to a benefits carrier the day after Hire completes |
| **Inbound** (external system → Workday) | Timekeeping punches from a third-party time clock system loaded into Workday for payroll processing |
| **Bidirectional** | A badge-access system that both receives new-hire provisioning data from Workday and sends back badge deactivation confirmations |

## EIB — Enterprise Interface Builder, conceptually

The **EIB (Enterprise Interface Builder)** is Workday's built-in,
configuration-based tool for building integrations without custom code —
the most common starting point for moving data in or out of a tenant. At a
conceptual level, an EIB does three things:

1. **Extracts or accepts data** — either pulling data out of Workday (for
   an outbound EIB) using a report as its data source, or accepting an
   incoming file (for an inbound EIB) formatted to match what Workday
   expects.
2. **Transforms it** — reshaping field names, formats, and codes into
   whatever the target system (or Workday itself, for inbound) requires.
3. **Delivers or loads it** — sending the output file to its destination
   (an SFTP server, an email, a connector to another system) or loading
   incoming data into Workday as actual transactions.

| EIB characteristic | What it means in practice |
|---|---|
| No-code / config-based | Built through guided screens, not a programming language |
| Report-driven (outbound) | An outbound EIB's data source is typically a custom report you've already built |
| Scheduled or on-demand | Can run automatically on a schedule (nightly, weekly) or be triggered manually |
| Best for | Simpler, well-structured, often flat-file (e.g., CSV/fixed-width) integrations |

For more complex integration needs — real-time delivery, complex
conditional transformation logic, or connecting to systems with
sophisticated APIs — Workday offers **Workday Studio** and pre-built
**Connectors**, which Level 3 Module 1 covers. This module intentionally
stays at the EIB/conceptual level, since that's the right starting depth
for understanding *what integrations do* before learning *how to build
one*.

## What flows through a typical integration

Tying back to Modules 3, 5, and 6, a new-hire outbound integration
typically carries:

| Data category | Example fields |
|---|---|
| Identity | Name, employee ID, date of birth (if the receiving system needs it) |
| Organizational | Supervisory org, cost center, location |
| Employment | Hire date, job profile, employee type |
| Compensation (if in scope) | Base pay, pay frequency — often *excluded* from integrations to systems that don't need it, per the security-domain thinking from Module 8 |

That last row matters: a well-designed integration sends only the fields
the receiving system actually needs — a badge-access system needs identity
and location, not compensation; a benefits carrier needs identity,
employment status, and benefit-plan eligibility fields, not necessarily
full compensation history. Over-sharing data through an integration is a
common real-world security and compliance mistake.

## Worked example: Meridian's new-hire-to-badge-system integration

When Jordan Ellis (Module 5) was hired at Distribution Center West, an
outbound EIB fired the next morning to Meridian's physical badge-access
vendor:

1. **Trigger**: The EIB runs on a nightly schedule, picking up all Hires
   completed in the last 24 hours (its data source is a custom report
   filtered to "New Hires, Status = Completed, Hire Date = yesterday").
2. **Fields extracted**: Employee ID, legal name, location (Distribution
   Center West), start date, and job profile (used by the badge vendor to
   determine which building zones Jordan's badge should access).
3. **Transform**: The EIB reformats Workday's internal location code
   ("DCW-01") into the badge vendor's expected site code ("SITE-4402"), a
   simple field-mapping transformation.
4. **Delivery**: The transformed file is dropped onto the badge vendor's
   SFTP server, which their own system picks up and processes to
   provision Jordan's badge before his first day.
5. **What's deliberately excluded**: Compensation, benefits elections, and
   emergency contact info are not part of this integration's field list —
   the badge vendor has no legitimate need for them, consistent with
   Module 8's least-access principle applied to integrations rather than
   human security groups.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Integration | Automated data flow keeping Workday and an external system in sync |
| Outbound | Data flowing from Workday to another system |
| Inbound | Data flowing from another system into Workday |
| EIB | Workday's configuration-based, no-code integration builder |
| Workday Studio / Connectors | More advanced integration tools for complex needs (Level 3) |

## Exercise

Design (at a conceptual level, no configuration needed) an outbound EIB
integration from Meridian Outfitters to a benefits carrier, triggered by
new hires. List: the trigger/schedule, at least five fields you'd include,
at least two fields you'd deliberately exclude and why, and one
transformation you'd expect to need (e.g., converting a Workday internal
code into a format the carrier expects).
