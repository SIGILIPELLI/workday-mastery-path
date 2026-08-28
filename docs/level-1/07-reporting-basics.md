# 07 · Reporting Basics

Every concept covered so far — organizations, workers, business processes,
compensation — eventually gets consumed through a report. This module
covers Workday's standard reporting model: what a standard report is, how
to run and filter one, and why "as of" dating (previewed in Module 5)
matters so much for accurate historical reporting.

## Standard reports vs. custom reports

Workday ships with hundreds of **standard reports** — pre-built,
pre-configured reports covering common needs (headcount, turnover,
compensation review, org charts) that any tenant can use immediately
without building anything. Custom reports, built with Workday's report
writer (using calculated fields, covered starting in Level 2 Module 5), let
an organization report on exactly its own data shapes and terminology.

This module is scoped to standard reports — running one, understanding its
prompts, and filtering its output — since that's what the overwhelming
majority of day-to-day Workday users (managers, HR partners) actually do.
Building custom reports is a Level 2+ skill.

| Report category | Example standard reports |
|---|---|
| **Headcount & organization** | Headcount, Organization Chart, Worker Directory |
| **Staffing** | New Hires, Terminations, Transfers |
| **Compensation** | Compensation Review, Total Rewards Statement |
| **Time & absence** | Time Off Balance, Absence Summary |

## Running a standard report

The general pattern for running any standard report:

1. **Find it** — type the report's name into global search (Module 2);
   Workday surfaces matching reports you have security access to run.
2. **Fill in prompts** — most reports open a prompt screen asking for
   filter criteria before running: an organization, a date or "as of" date,
   a worker type, or a date range.
3. **Run** — the report executes and returns a data grid (or, for some
   reports, a chart/summary view).
4. **Refine** — most report outputs support additional in-grid filtering,
   sorting, and column selection without re-running the whole report from
   scratch.
5. **Export or share** — most reports can be exported (commonly to Excel)
   for further analysis outside Workday.

## "As of" dating — the concept that makes historical reporting correct

Because Workday preserves history rather than overwriting it (Module 3 and
5 both mentioned this), most organization- and worker-related reports
accept an **"As Of" date** prompt. Running the same report with different
"As Of" dates can produce genuinely different — and both *correct* —
results:

| Report | As Of Jan 1 | As Of Jul 1 (after a reorg) |
|---|---|---|
| Headcount by supervisory org | Shows Jordan under "Distribution Center West" | Shows Jordan under "Retail Operations — Store 118," reflecting the transfer from Module 5 |
| Organization Chart | Shows the pre-reorg management structure | Shows the current structure |

Getting "As Of" wrong is one of the most common real-world reporting
mistakes: someone pulls a headcount report today intending to reconstruct
what a department looked like six months ago, forgets to set the As Of
date, and gets *today's* structure applied retroactively to historical
counting — silently producing a wrong answer that looks plausible.

## Filtering a report

Beyond the As Of date, most standard reports offer additional filter
prompts, commonly:

| Filter type | Example |
|---|---|
| **Organization** | Limit to "Distribution Center West" only, including or excluding sub-orgs |
| **Worker type** | Employees only, or include contingent workers |
| **Date range** | New hires between two dates, rather than a single as-of snapshot |
| **Status** | Active workers only vs. include terminated |

A well-formed report request states all four dimensions explicitly rather
than accepting defaults blindly — "Active employees only, in Distribution
Center West including sub-orgs, as of today" is a precise, reproducible
request; "just run the headcount report" is not.

## Worked example: answering a real question with a standard report

Meridian's VP of HR Operations asks: *"How many people did Distribution
Center West hire in Q1, and how does that compare to how many left?"*

An HR analyst answers this with two standard reports, run carefully:

1. **New Hires report**, filtered to Organization = "Distribution Center
   West" (including sub-orgs "Warehouse Operations" and "Inbound
   Logistics"), Date Range = Jan 1–Mar 31, Worker Type = Employee (the
   question is about hiring, not adding contingent workers). Result: 14
   hires.
2. **Terminations report**, same organization and sub-org scope, same date
   range, Status = both Voluntary and Involuntary included (the question
   asked "how many left," not "how many resigned"). Result: 9 terminations.
3. The analyst reports back: *"DC West hired 14 and lost 9 in Q1 — net
   headcount growth of +5, with a 61% hire-to-loss ratio."* They also note
   the scope explicitly (which sub-orgs, which worker types, which date
   range) so the VP can trust the number without re-deriving it themselves.

Notice the analyst didn't just run "the headcount report" — they picked the
two reports that actually answer the *hiring vs. attrition* question asked,
and stated their filter choices out loud, which is the habit that
separates a trustworthy report from a technically-correct-but-ambiguous
one.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Standard report | A pre-built report available out of the box |
| Custom report | A tenant-built report using the report writer (Level 2+) |
| As Of date | The point in time a report reconstructs organizational/worker state for |
| Prompt | A filter input requested before a report runs |
| Export | Sending report output (typically to Excel) for further analysis |

## Exercise

Using the four filter dimensions from this module (organization, worker
type, date range, status), write out the exact filter values you'd use to
answer this question: *"How many involuntary terminations happened in
Meridian's Retail Operations (West Region) in the last 12 months, employees
only?"* Then write one sentence describing what would go wrong with the
answer if the analyst forgot to exclude contingent workers, and one
sentence describing what would go wrong if they forgot to restrict to
involuntary only.
