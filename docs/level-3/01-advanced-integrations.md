# 01 · Advanced Integrations

Level 2 Module 6 introduced EIB and previewed Workday Studio. This module
goes deeper into Studio-built integrations, the connector model, and the
web services layer that both EIBs and Studio integrations ultimately run
on top of.

## Workday's web services layer: SOAP and REST

Every integration into or out of Workday — whether built via EIB, Studio,
or a fully external system — ultimately calls one of Workday's published
**web services**:

| API style | Characteristics |
|---|---|
| **SOAP (Workday Web Services / WWS)** | The original, comprehensive API surface — one WSDL-defined service per functional area (Human Resources, Financial Management, etc.), XML request/response |
| **REST** | Newer, resource-oriented endpoints for a growing subset of objects, JSON request/response, generally lighter-weight for simpler integrations |

A configurer or integration developer chooses the API style based on
what the target system or use case expects, and on whether the specific
operation needed is available as REST yet — SOAP still covers a broader
surface area for complex transactional operations.

## Studio integrations in more depth

Building on Level 2's introduction, a real Studio integration assembly
typically chains:

1. **A trigger/get connector** — pulls source data, often a Workday Web
   Services "Get" operation or an external system connector (database,
   SFTP file).
2. **One or more transform steps** — XSLT (or Studio's Groovy scripting
   support) reshaping data between the source and target schema.
3. **A put/send connector** — delivers to the target, which might be a
   Workday "Put" operation (writing back into Workday) or an external
   system's API/file drop.
4. **Error and exception handling steps** — catching failures at any stage
   and routing them to a notification or retry path rather than letting
   the integration fail silently.

Unlike an EIB, a Studio assembly can call multiple different systems
within one integration run and apply branching logic mid-pipeline — the
capability gap Level 2 flagged as the reason to reach for Studio at all.

## Integration System Security: ISUs and ISSGs

Integrations don't authenticate as a human user — they run under an
**Integration System User (ISU)**, a special account type with no login
UI access, granted permissions through an **Integration System Security
Group (ISSG)**, the integration-specific analog of the security groups
from Level 1/Level 2 Module 7. Scoping an ISU's ISSG tightly (access only
to the domains the specific integration needs) follows the same
least-privilege discipline as human security group design — an
integration compromised or misconfigured with overly broad access is a
larger blast radius than a single human's overreach, since it runs
unattended and at volume.

## Middleware and connector patterns

Many real integrations don't connect Workday directly to a target system
but route through **middleware** (an iPaaS, or Workday's own Cloud
Connect pre-built templates for common third-party systems — benefits
carriers, payroll vendors). Cloud Connect integrations trade
configurability for speed: they're pre-built Studio-equivalent templates
for a specific vendor's expected format, configured rather than built
from scratch, at the cost of less flexibility than a custom Studio
assembly.

## Worked example: a benefits carrier feed for Meridian

Meridian needs to send weekly benefits enrollment data (Level 2 Module 4)
to their external medical carrier, in the carrier's proprietary flat-file
format, with error records requiring HR review before resend:

1. **Evaluate Cloud Connect first** — if the carrier is a supported
   Cloud Connect template, configure it directly rather than building
   custom Studio.
2. **If not supported, build in Studio**: a Get connector against the
   benefits enrollment report data (Level 2 Module 5's reporting engine,
   accessed as a web service call), a transform step reshaping records
   into the carrier's exact field order and encoding, and a Put/file-
   delivery connector via SFTP.
3. **Exception handling**: any record failing the carrier's required-field
   validation is routed to an error queue and triggers a notification step
   (Module 1's BP-style notification concept, here inside an integration
   rather than a business process) to the benefits team, rather than
   silently dropping the record or failing the entire batch.
4. **ISU/ISSG**: a dedicated Integration System User scoped only to the
   benefits enrollment domain, not general worker data, runs the
   integration.

## How It Actually Works

Every integration, regardless of the tool used to build it, ultimately
resolves to the same underlying web service operations and the same
security evaluation as a human-driven transaction — the tooling differs,
the enforcement layer doesn't.

**An ISU is still evaluated through the domain/group/scope mechanism from
Level 1 Module 8 — integrations don't bypass security, they're just a
different kind of "user."** When the benefits feed integration calls a
Get operation for enrollment data, Workday checks the calling ISU's ISSG
domain grants exactly as it would check a human's security group grants
for the same data. This is why over-scoping an ISU (granting broader
access than the integration strictly needs, often done for convenience
during build) is a genuine security risk rather than a harmless
shortcut — the ISU's credentials, if compromised, carry exactly the
access it was granted, evaluated the same way a stolen human login would
be.

**A Studio transform step doesn't create new data — it re-expresses data
already retrieved via a web service call into a different shape, and
errors surface at the step where a required transformation can't be
satisfied.** The carrier feed's transform step takes each record returned
by the Get connector and maps it into the target flat-file layout;
when a record is missing a field the carrier's format requires, the
transform step itself can flag that failure, because it has visibility
into both the source shape and the target requirement simultaneously. This
localized failure detection — at the transform, not after final delivery
— is what makes per-record exception routing possible instead of an
all-or-nothing batch failure.

**Middleware/Cloud Connect templates are pre-assembled Studio-equivalent
logic, configured through parameters instead of built visually — the
same web service calls happen underneath.** Choosing Cloud Connect over
custom Studio doesn't mean Workday talks to the carrier through a
different mechanism; it means Workday (or Anthropic's stand-in analogy:
a pre-written recipe) already encodes the Get/transform/Put pipeline for
that specific vendor's known format, exposed as a configuration form
rather than a visual canvas. This is exactly why Cloud Connect templates
are faster to implement but less flexible — the underlying pipeline
shape is fixed by the template author, not the tenant's configurer.

## Cheat sheet

| Term | One-line definition |
|---|---|
| WWS (SOAP) | Workday's comprehensive, XML-based web services API |
| REST | Workday's newer, resource-oriented JSON API, growing coverage |
| ISU / ISSG | Integration System User and its dedicated security group |
| Cloud Connect | Pre-built, vendor-specific integration templates |
| Transform step | Reshapes data between source and target format mid-pipeline |

## Exercise

Design the ISSG scope (which domains, at what access level) for a new ISU
that will run a nightly outbound integration sending only active workers'
names and work email addresses to Meridian's IT provisioning system.
Explain in one paragraph why this ISU should not simply reuse the broader
ISSG already scoped for the benefits carrier feed above, even though both
integrations touch worker data.
