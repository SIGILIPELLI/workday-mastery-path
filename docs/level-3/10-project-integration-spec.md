---
description: "Project — Design an Integration Specification Document — This module is a project: write a full Integration Specification Document (Integration Spec) …"
---

# 10 · Project — Design an Integration Specification Document

This module is a project: write a full **Integration Specification
Document (Integration Spec)** — the artifact a Workday integration
developer would produce before building anything — applying Level 3's
integration, security, and change-management concepts to one concrete
scenario at Meridian Outfitters.

## The scenario

Meridian's new 401(k) retirement plan provider needs a **weekly outbound
file** of eligible workers' enrollment and contribution data, and
Meridian's own HRIS team needs a **way to detect and remediate failed
records** without waiting for the provider to report problems manually.

## Part 1 — Requirements and scope

Write a short requirements section covering:

- What data is needed (which fields, referencing Level 2 Module 4's
  benefits eligibility and enrollment concepts).
- The population: which workers are in scope (eligibility rule, plain
  language).
- Frequency and delivery mechanism (referencing Level 3 Module 1's
  EIB-vs-Studio decision guide — justify your choice).
- Whether a pre-built Cloud Connect template exists for this provider, and
  what you'd do if it doesn't (build custom Studio).

## Part 2 — Data mapping

Produce a field mapping table: Workday source field → target file field
name/position → any transformation needed (date format, code mapping for
plan type, etc.), the same shape of table Level 3 Module 1's transform
step concept implies.

## Part 3 — Error handling design

Specify:

- What counts as a record-level error (e.g., missing required
  contribution election) versus a batch-level failure (e.g., connectivity
  failure to the provider's SFTP endpoint).
- How each error type should be handled — referencing Level 3 Module 1's
  exception-routing concept (per-record queue vs. all-or-nothing batch
  failure).
- Who gets notified on failure, and through what mechanism.

## Part 4 — Security design

Specify:

- The Integration System User (ISU) and its Integration System Security
  Group (ISSG) scope — which domains, at what access level (Level 3
  Module 1's ISU/ISSG concept).
- Justify why this ISU should not reuse an existing, more broadly scoped
  ISU already in the tenant (referencing Level 3 Module 1's exercise
  reasoning).

## Part 5 — Testing and change management plan

Specify:

- What you'd test in Sandbox before this integration ever touches
  Production data (Level 3 Module 9's regression-script concept) —
  name at least three specific test scenarios, including at least one
  edge case (a worker who becomes eligible mid-week, a worker who
  terminates the same week the file runs).
- How you'd validate the very first live run without risking a bad file
  reaching the provider (e.g., a validate-only pass, referencing Level 2
  Module 6's validate-only concept).
- What ongoing monitoring or audit report (Level 2 Module 5-style custom
  report) would let Meridian's HRIS team confirm each week's file matched
  expectations after the fact.

## Deliverable

Assemble Parts 1–5 into one structured Integration Specification
document. There's no single correct answer — the goal is a coherent,
fully-justified spec that correctly applies the integration, security,
and change-management concepts from every Level 3 module to one concrete,
believable requirement, in the same form a real Workday integration
developer would hand to a technical lead for review before opening
Workday Studio or an EIB wizard for the first time.
