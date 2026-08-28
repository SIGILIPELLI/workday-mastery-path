# 06 · Compensation Basics

Compensation touches nearly every staffing event covered in Module 5 — a
hire proposes pay, a promotion often changes it, a termination stops it —
so this module builds the vocabulary Workday uses to structure pay:
compensation packages, comp basis, and grades. This is a conceptual
foundation; Level 2 Module 4 goes deeper into benefits and advanced
compensation configuration.

## Compensation Packages — the container for a worker's pay

A **Compensation Package** is the set of compensation plans assigned to a
worker, grouped together based on their job profile, worker type, and
sometimes location or grade. A package might bundle:

| Plan type | Example |
|---|---|
| **Base pay plan** | Hourly or salaried base rate |
| **Bonus/incentive plan** | Annual bonus target as a percentage of base |
| **Merit plan** | Eligibility for the annual merit increase cycle |
| **Allowance plan** | A fixed stipend — e.g., a car allowance for field roles |

At Meridian Outfitters, a Store Associate's compensation package might
contain only an hourly base pay plan, while a Regional Director's package
bundles a salaried base pay plan, an annual bonus plan targeted at 15% of
base, and a merit plan — because a package is assigned based on what's
appropriate for that population, not a one-size-fits-all default.

## Comp Basis — hourly vs. salaried, and how it's expressed

**Compensation basis** defines the unit pay is expressed and calculated in.
Workday primarily distinguishes:

| Basis | How pay is entered/calculated | Typical population |
|---|---|---|
| **Hourly** | A rate per hour, multiplied by hours actually worked (from time tracking) | Retail associates, warehouse workers |
| **Salaried** | A fixed periodic amount (annual salary, paid out per pay period) regardless of hours | Managers, corporate staff |
| **Unit-based / Commission** | Calculated from a driver like units sold or commission percentage | Sales roles (less common at Meridian, but standard for many retailers' sales-commission staff) |

This matters beyond payroll mechanics: comp basis affects overtime
eligibility calculations, how a raise is expressed ("+$1.50/hour" vs. "+3%
annual salary"), and which compensation plans a worker is even eligible to
be assigned, since a bonus plan defined as "% of annual base salary" isn't
meaningful for a purely hourly worker without additional configuration.

## Compensation Grades — structuring pay ranges

A **Compensation Grade** defines a pay range (minimum, midpoint, maximum)
associated with a job profile or level, used to keep pay decisions
consistent and auditable. Grades are what Module 4's conditional
routing example checked against ("is the proposed rate within the grade's
standard range?").

Meridian Outfitters' simplified grade structure for a few roles:

| Job Profile | Grade | Range (annual, salaried roles) |
|---|---|---|
| Store Associate | Grade 2 | $28,000 – $34,000 (hourly equivalent) |
| Assistant Store Manager | Grade 5 | $42,000 – $52,000 |
| Store Manager | Grade 7 | $58,000 – $74,000 |
| Regional Director | Grade 11 | $110,000 – $140,000 |

A **Grade Profile** can further attach the eligible compensation plans for
that grade — so Grade 7 and above might be the threshold at which a bonus
plan becomes part of the standard package, while Grades 1-4 receive base
pay only.

## Compensation change: merit, promotion, and market adjustment

Not every pay change is the same kind of event, and Workday tenants
typically configure distinct **compensation change reasons** so reporting
and approval routing can distinguish them:

| Change reason | Trigger | Typical approval weight |
|---|---|---|
| **Merit increase** | Annual performance-linked cycle | Lightest — usually pre-approved within budget guidelines |
| **Promotion** | Job Change to a higher grade | Moderate — manager + HR, sometimes compensation approval if it jumps multiple grades |
| **Market adjustment** | Pay brought in line with external market data, unrelated to performance | Moderate-to-heavy — usually requires a compensation analyst's sign-off |
| **Equity adjustment** | Correcting an internal pay gap (e.g., discovered via a pay equity audit) | Heaviest — typically requires HR leadership approval given legal sensitivity |

## Worked example: proposing a compensation change at Meridian

Recall Jordan Ellis's promotion to Assistant Manager from Module 5. Here's
how compensation basis, grade, and change reason interact in that single
transaction:

1. Jordan's prior role, Store Associate, was **Grade 2, hourly basis**,
   earning $16.50/hour (~$34,320 annualized).
2. The Assistant Manager role is **Grade 5, salaried basis** —
   meaning this promotion changes *both* the grade and the comp basis
   itself, not just the number.
3. The HR Partner proposes a salary of $45,000 — within Grade 5's
   $42,000–$52,000 range, so no compensation-exception approval is
   triggered per Module 4's condition rule.
4. The change reason is recorded as **Promotion**, not Merit — which
   matters for later reporting: Meridian's annual "merit budget spend"
   report should *not* include promotion-driven increases, or the merit
   cycle's true cost gets overstated.
5. Because Jordan's package changes from hourly-only to salaried-with-
   bonus-eligibility (Grade 5 crosses Meridian's bonus-eligibility
   threshold), the system also needs the bonus plan added to his package —
   a detail an HR Partner filling out this transaction needs to catch
   manually unless the tenant has configured grade-based automatic plan
   assignment.

## Cheat sheet

| Term | One-line definition |
|---|---|
| Compensation Package | The bundle of pay plans assigned to a worker |
| Comp Basis | The unit pay is expressed in — hourly, salaried, unit-based |
| Compensation Grade | A defined pay range (min/mid/max) tied to a job profile/level |
| Grade Profile | A grade plus its eligible compensation plans |
| Change Reason | The categorized cause of a pay change (merit, promotion, market, equity) |

## Exercise

Using the Meridian grade table above, design a compensation change scenario
for a Store Manager (Grade 7) being promoted to Regional Director (Grade
11). State: the comp basis before and after (if it changes), a specific
proposed salary within the Grade 11 range, the change reason you'd record,
and one thing about the worker's compensation package (beyond base pay)
that would need to be added or changed as part of this promotion. Justify
each choice in one sentence.
