# 02 · Navigating the Workday UI

Workday's interface looks sparse compared to older enterprise systems — a
search bar, a grid of icons, a profile picture — and that simplicity is
deliberate, but it hides a fair amount of structure. This module walks
through the four navigation mechanisms you'll use constantly: worklets, the
global search bar, related actions, and your inbox, using Meridian
Outfitters' tenant as the running example.

## The home page and worklets

When you log into Workday, you land on your **home page**, built from a
grid of **worklets** — square tiles, each representing an application area
(Pay, Time Off, Benefits, Talent, Team, Inbox, and so on). Which worklets
appear depends on your **security group** membership (covered in Module 8):
a store associate at Meridian sees Pay, Time Off, and Personal Information;
an HR partner sees those plus Staffing, Compensation, and Talent; an HRIS
admin sees a much larger set including configuration-oriented worklets.

| Worklet | What clicking it opens |
|---|---|
| **Inbox** | Your pending business process approvals and to-dos |
| **Team** (or "My Team") | Direct reports, for anyone with people management responsibility |
| **Pay** | Payslips, payment elections, tax documents |
| **Time Off** | Balances, requests, holiday calendars |
| **Benefits** | Current elections, open enrollment when active |
| **Talent** | Goals, performance reviews, career/skills data |
| **Personal Information** | Contact info, emergency contacts, legal name |

Worklets are themselves configurable — an admin can add, remove, and
reorder them for different security groups — so two employees at the same
company can have visually different home pages while looking at the same
underlying system.

## Global search — the fastest way to anything

The search bar at the top of every Workday screen is not just for finding
people. It's a universal entry point into:

1. **People** — type a name and Workday shows a live-filtered list of
   matching workers, each result linking to that worker's profile.
2. **Tasks and reports** — type a task name like "Hire" or a report name
   like "Compensation Review" and Workday surfaces matching actions you have
   security access to initiate, directly from the results dropdown.
3. **Business objects and instances** — searching "Distribution Center West"
   at Meridian would surface that specific organization as a result if it
   exists in the tenant.

!!! tip "Search by task name, not by menu-hunting"
    New users often try to find a worklet with a matching icon for what
    they want to do. Experienced Workday users skip that entirely and just
    type the task name — "Request Time Off," "Change Job," "Propose
    Compensation Change" — directly into search. This becomes the fastest
    habit to build once you know the standard task names, which later
    modules will introduce module by module.

## Related Actions — the "right-click menu" of Workday

Almost every object in Workday — a worker, an organization, a position, a
report — has a small **related actions** icon next to its name (an orange
or gray icon that looks like layered dots, sometimes shown as a cloud/gear
depending on release). Clicking it opens a categorized menu of every action
you have security access to perform *on that specific object*.

For example, clicking the related actions icon next to a worker's name at
Meridian might show categories like:

| Category | Example actions inside it |
|---|---|
| **Job Change** | Change Job, Transfer, Promote |
| **Compensation** | Request Compensation Change |
| **Talent** | Start Performance Review, Add Goal |
| **Organization** | View Org Chart, Change Organization Assignment |

This is the single most important navigation pattern to internalize: rather
than hunting through worklets for "where do I promote someone," you
navigate *to the worker* (via search) and then use *their* related actions
menu. The same pattern applies to organizations, positions, and almost
every other object type in the system.

## The inbox and business process steps

Your **Inbox** worklet lists every business process step currently
awaiting your action — an approval, a to-do, or a review step. Each inbox
item shows:

- The **business process type** (e.g., "Hire," "Request Time Off")
- The **initiator** and the **subject** (who started it, who it's about)
- Available actions: typically **Approve**, **Send Back**, **Deny**, or
  **Add To-Do** for delegated follow-up tasks

Clicking an inbox item opens the full transaction detail before you act on
it — for a Hire, that might mean seeing the proposed position, compensation,
and start date before approving. Module 4 covers how these steps get
configured into a business process definition in the first place; this
module is only about the surface-level experience of acting on one.

## Worked example: an HR partner's first ten minutes at Meridian

Priya is a new HR Business Partner at Meridian Outfitters supporting the
Distribution Center West region. Her first ten minutes in Workday look
like:

1. She lands on her home page, sees an **Inbox** worklet showing "3" —
   three pending items.
2. She clicks Inbox and sees two "Change Job" approvals for transfers within
   her region, and one "Request Time Off" delegated to her because the
   direct manager is out.
3. She approves the time-off request, and opens the first Change Job to
   review the proposed new position and reporting line before approving it.
4. She then wants to check a specific employee's current compensation
   ahead of a scheduled 1:1. She types the employee's name into global
   search, lands on their profile, and clicks the related actions icon next
   to their name to find **Compensation > View Compensation History**
   rather than hunting for a "Compensation" worklet — because as an HR
   Partner rather than a comp analyst, she may not have that worklet on her
   home page at all.

Notice that steps 3 and 4 use two different navigation habits — inbox for
*things routed to you*, and search + related actions for *things you go
looking for* — and a fluent Workday user switches between them constantly
without thinking about it.

## Cheat sheet

| I want to... | Use this |
|---|---|
| See my pending approvals | Inbox worklet |
| Find a specific person or task | Global search bar |
| Do something to a specific worker/org/position | Navigate to it, then its related actions icon |
| See what a worklet gives me access to | Click the worklet tile itself |
| Approve/deny/send back a transaction | Open it from Inbox, review detail, then act |

## Exercise

Pick three tasks you'd expect a brand-new employee, a people manager, and an
HRIS admin to each perform in their first week at Meridian Outfitters (nine
tasks total, three per role). For each task, write down which of the four
navigation mechanisms above (worklet, search, related actions, inbox) is the
most natural way to reach it, and justify your choice in one sentence.
