---
title: "Final Quiz"
description: "Test what you have learned across all four levels of Workday Mastery Path with a 20-question final quiz."
---

# Final Quiz

20 questions drawn from all four levels of this course. You need **70% (14 out of 20)** to pass —
you can retake it as many times as you like. Your best result is saved in this browser.

<div id="mp-quiz-root"></div>

<script type="application/json" id="mp-quiz-data">
[
  {
    "question": "A compensation grade has min 60,000, midpoint 80,000 and max 100,000. What are compa-ratio and range penetration for 92,000 (2 decimals)?\n```python\nlo,mid,hi=60000,80000,100000\nsal=92000\nprint(round(sal/mid,2), round((sal-lo)/(hi-lo),2))\n```\n",
    "options": [
      "0.87 0.8",
      "1.15 0.8",
      "1.15 1.15",
      "0.8 1.15"
    ],
    "correctIndex": 1,
    "explanation": "Compa-ratio compares to the midpoint; penetration places the salary within the range. (A Python model of the concept using made-up data; Workday itself was not available.) Verified by running it."
  },
  {
    "question": "A 5% merit increase takes effect 1 July on a 60,000 salary. What is the in-year (6 of 12 months) cost? \n```python\nprint(60000*0.05*6/12)\n```\n",
    "options": [
      "2500.0",
      "1500.0",
      "3000.0",
      "750.0"
    ],
    "correctIndex": 1,
    "explanation": "Prorate the annual increase for the months remaining. (A Python model of the concept using made-up data; Workday itself was not available.) Verified by running it."
  },
  {
    "question": "Gross 6,000; pre-tax deduction 500; tax 18% of taxable pay; post-tax deduction 200. What is net pay?\n```python\ngross=6000; pre=500\ntax=(gross-pre)*0.18\nprint(round(gross-pre-tax-200,2))\n```\n",
    "options": [
      "4500.0",
      "4410.0",
      "4800.0",
      "4310.0"
    ],
    "correctIndex": 3,
    "explanation": "Pre-tax deductions reduce taxable income; post-tax come out afterwards. (A Python model of the concept using made-up data; Workday itself was not available.) Verified by running it."
  },
  {
    "question": "A user is in groups {A, B}. Is an intersection group of (A, C) satisfied, and an aggregation group of (A, C)?\n```python\nu={'A','B'}\nprint({'A','C'}<=u, bool({'A','C'}&u))\n```\n",
    "options": [
      "False True",
      "True True",
      "True False",
      "False False"
    ],
    "correctIndex": 0,
    "explanation": "Intersection is AND; aggregation is OR. (A Python model of the concept using made-up data; Workday itself was not available.) Verified by running it."
  },
  {
    "question": "A journal has debits 1,500 and credits 900 + 600. Is it balanced?\n```python\nprint(1500==900+600)\n```\n",
    "options": [
      "False",
      "None",
      "1500",
      "True"
    ],
    "correctIndex": 3,
    "explanation": "Debits must equal credits. (A Python model of the concept using made-up data; Workday itself was not available.) Verified by running it."
  },
  {
    "question": "What is effective dating?",
    "options": [
      "Audit logs only",
      "Storing changes with the date they take effect, preserving history and allowing future-dated changes",
      "Backups",
      "Timestamps only"
    ],
    "correctIndex": 1,
    "explanation": "Entry date records when it was entered; effective date when it applies."
  },
  {
    "question": "What is the difference between a supervisory organisation and a cost centre?",
    "options": [
      "Reverse",
      "None",
      "Same",
      "Supervisory orgs organise management of people; cost centres are financial worktags"
    ],
    "correctIndex": 3,
    "explanation": "Both are organisation types."
  },
  {
    "question": "A BP step should run only for raises over 10%. What do you configure?",
    "options": [
      "A report",
      "A prompt",
      "A new tenant",
      "A condition on that step (possibly using a calculated field)"
    ],
    "correctIndex": 3,
    "explanation": "Conditions make routing dynamic."
  },
  {
    "question": "Why do security edits sometimes seem to have no effect?",
    "options": [
      "Colour",
      "Cache",
      "Pending security policy changes must be activated with the Activate Pending Security Policy Changes task",
      "Bug"
    ],
    "correctIndex": 2,
    "explanation": "A very common pitfall."
  },
  {
    "question": "What does domain security control compared with BP security?",
    "options": [
      "Nothing",
      "Same",
      "Reports",
      "Access to data and tasks in a functional area, whereas BP security controls who initiates and acts on process steps"
    ],
    "correctIndex": 3,
    "explanation": "Both must be right."
  },
  {
    "question": "When is EIB the right tool?",
    "options": [
      "Never",
      "Straightforward bulk loads or extracts without complex logic",
      "Always",
      "Payroll only"
    ],
    "correctIndex": 1,
    "explanation": "Studio is for complex integrations."
  },
  {
    "question": "Why test an integration in a sandbox first?",
    "options": [
      "Style",
      "Colour",
      "A wrong load into production can affect many records and payroll",
      "Law"
    ],
    "correctIndex": 2,
    "explanation": "Rehearse and validate."
  },
  {
    "question": "What is an integration system user?",
    "options": [
      "A role",
      "A report",
      "A non-human account with least-privilege security used to run integrations",
      "A person"
    ],
    "correctIndex": 2,
    "explanation": "Keep it scoped."
  },
  {
    "question": "What is the purpose of parallel payroll testing?",
    "options": [
      "Law",
      "Colour",
      "Compare new results with the existing payroll to find configuration differences before go-live",
      "Speed"
    ],
    "correctIndex": 2,
    "explanation": "Reconcile variances."
  },
  {
    "question": "What are worktags?",
    "options": [
      "Reports",
      "Roles",
      "Jobs",
      "Tags such as cost centre and region attached to transactions for reporting and control"
    ],
    "correctIndex": 3,
    "explanation": "Central to Financials."
  },
  {
    "question": "What does regression testing mean around Workday releases?",
    "options": [
      "New features",
      "Security audit",
      "Load testing",
      "Re-testing existing behaviour after an update to ensure nothing broke"
    ],
    "correctIndex": 3,
    "explanation": "Workday updates twice a year."
  },
  {
    "question": "What drives successful adoption of a Workday rollout?",
    "options": [
      "Software only",
      "Colours",
      "Reports",
      "Sponsorship, communication, training and support for affected people"
    ],
    "correctIndex": 3,
    "explanation": "Technology alone is not enough."
  },
  {
    "question": "What belongs in an integration specification?",
    "options": [
      "Logos",
      "Colours",
      "Names",
      "Purpose, data flow, field mapping, schedule, security, error handling and test cases"
    ],
    "correctIndex": 3,
    "explanation": "Developers and testers depend on it."
  },
  {
    "question": "What is the best way to build configuration skill?",
    "options": [
      "Hands-on practice in a sandbox with realistic scenarios",
      "Watching",
      "Skipping",
      "Reading only"
    ],
    "correctIndex": 0,
    "explanation": "Practice beats passive study."
  },
  {
    "question": "What shows an implementation plan is credible?",
    "options": [
      "Colours",
      "Realistic estimates, dependencies, risks with owners and clear acceptance criteria",
      "Length",
      "Format"
    ],
    "correctIndex": 1,
    "explanation": "Evidence over claims."
  }
]
</script>

## Coding challenges

Try these on your own computer, then compare your output. Nothing you write is sent anywhere.

### A business-process router with conditions, delegation and send-back

Model a Change Job business process: steps with conditions (HR Partner only for raises or grade changes, VP Finance only above 10%), delegation of one approver to another, a 'send back' decision that stops the process and a final effective date. This is a Python teaching model with made-up people and rules; Workday itself was not available, and in Workday you configure this in the Business Process definition.

**Starter code**

```python
# A Workday-style business process router (a teaching model, not Workday)
from dataclasses import dataclass, field

@dataclass
class Step:
    name: str
    role: str
    condition: object = None          # callable(txn) -> bool; None means always

@dataclass
class Txn:
    kind: str
    worker: str
    data: dict
    log: list = field(default_factory=list)

DELEGATES = {"vp_finance": "cfo"}        # vp_finance is on leave, the cfo acts for them
APPROVERS = {"manager": "ana", "hr_partner": "bo", "vp_finance": "cy", "cfo": "di"}

CHANGE_JOB = [
    Step("Manager approval", "manager"),
    Step("HR Partner approval", "hr_partner", lambda t: t.data.get("change_pct", 0) > 0 or t.data.get("new_grade")),
    Step("VP Finance approval", "vp_finance", lambda t: t.data.get("change_pct", 0) > 10),
]

# TODO: route(txn, steps, decisions) -> "complete" | "sent back" | "denied"
#   for each step: if it has a condition that is false, log "skip <step name>" and continue
#   otherwise find the approver (apply DELEGATES to the role first, then look up APPROVERS), take the decision for that role
#   (default "approve"), log "<step name>: <who> <decision>"; "send back" logs "-> sent back to initiator" and stops,
#   "deny" logs "-> denied, transaction cancelled" and stops
#   after the last step log "-> complete, effective <date>"

cases = [
    Txn("Change Job", "Eli", {"change_pct": 4, "effective": "2026-04-01"}),
    Txn("Change Job", "Flo", {"change_pct": 12, "effective": "2026-04-01"}),
    Txn("Change Job", "Gus", {"change_pct": 0, "effective": "2026-05-01"}),
    Txn("Change Job", "Hal", {"change_pct": 20, "new_grade": "G7", "effective": "2026-06-01"}),
]
decisions = [{}, {}, {}, {"hr_partner": "send back"}]
for t, d in zip(cases, decisions):
    result = route(t, CHANGE_JOB, d)
    print(f"{t.worker}: {result}")
    for line in t.log:
        print("   ", line)
```

**Expected output**

```text
Eli: complete
    Manager approval: ana approve
    HR Partner approval: bo approve
    skip VP Finance approval
    -> complete, effective 2026-04-01
Flo: complete
    Manager approval: ana approve
    HR Partner approval: bo approve
    VP Finance approval: di approve
    -> complete, effective 2026-04-01
Gus: complete
    Manager approval: ana approve
    skip HR Partner approval
    skip VP Finance approval
    -> complete, effective 2026-05-01
Hal: sent back
    Manager approval: ana approve
    HR Partner approval: bo send back
    -> sent back to initiator
```

<details><summary>Show solution and explanation</summary>

<pre><code># A Workday-style business process router (a teaching model, not Workday)
from dataclasses import dataclass, field

@dataclass
class Step:
    name: str
    role: str
    condition: object = None          # callable(txn) -&gt; bool; None means always

@dataclass
class Txn:
    kind: str
    worker: str
    data: dict
    log: list = field(default_factory=list)

DELEGATES = {&quot;vp_finance&quot;: &quot;cfo&quot;}        # vp_finance is on leave, the cfo acts for them
APPROVERS = {&quot;manager&quot;: &quot;ana&quot;, &quot;hr_partner&quot;: &quot;bo&quot;, &quot;vp_finance&quot;: &quot;cy&quot;, &quot;cfo&quot;: &quot;di&quot;}

CHANGE_JOB = [
    Step(&quot;Manager approval&quot;, &quot;manager&quot;),
    Step(&quot;HR Partner approval&quot;, &quot;hr_partner&quot;, lambda t: t.data.get(&quot;change_pct&quot;, 0) &gt; 0 or t.data.get(&quot;new_grade&quot;)),
    Step(&quot;VP Finance approval&quot;, &quot;vp_finance&quot;, lambda t: t.data.get(&quot;change_pct&quot;, 0) &gt; 10),
]

def route(txn, steps, decisions):
    for step in steps:
        if step.condition and not step.condition(txn):
            txn.log.append(f&quot;skip {step.name}&quot;)
            continue
        who = APPROVERS[DELEGATES.get(step.role, step.role)]
        decision = decisions.get(step.role, &quot;approve&quot;)
        txn.log.append(f&quot;{step.name}: {who} {decision}&quot;)
        if decision == &quot;send back&quot;:
            txn.log.append(&quot;-&gt; sent back to initiator&quot;)
            return &quot;sent back&quot;
        if decision == &quot;deny&quot;:
            txn.log.append(&quot;-&gt; denied, transaction cancelled&quot;)
            return &quot;denied&quot;
    txn.log.append(&quot;-&gt; complete, effective &quot; + txn.data[&quot;effective&quot;])
    return &quot;complete&quot;

cases = [
    Txn(&quot;Change Job&quot;, &quot;Eli&quot;, {&quot;change_pct&quot;: 4, &quot;effective&quot;: &quot;2026-04-01&quot;}),
    Txn(&quot;Change Job&quot;, &quot;Flo&quot;, {&quot;change_pct&quot;: 12, &quot;effective&quot;: &quot;2026-04-01&quot;}),
    Txn(&quot;Change Job&quot;, &quot;Gus&quot;, {&quot;change_pct&quot;: 0, &quot;effective&quot;: &quot;2026-05-01&quot;}),
    Txn(&quot;Change Job&quot;, &quot;Hal&quot;, {&quot;change_pct&quot;: 20, &quot;new_grade&quot;: &quot;G7&quot;, &quot;effective&quot;: &quot;2026-06-01&quot;}),
]
decisions = [{}, {}, {}, {&quot;hr_partner&quot;: &quot;send back&quot;}]
for t, d in zip(cases, decisions):
    result = route(t, CHANGE_JOB, d)
    print(f&quot;{t.worker}: {result}&quot;)
    for line in t.log:
        print(&quot;   &quot;, line)
</code></pre>

<p>Each step is evaluated in order: false conditions skip, delegation changes who acts, and decisions can stop the transaction. Hal&#x27;s 20% raise with a grade change triggers HR Partner approval, who sends it back, so the VP step never runs. Verified by running the Python solution.</p>
</details>

### Compensation analytics and security-group evaluation

Compute compa-ratio and range penetration, apply a merit matrix (performance band by compa band), report the merit budget used, build a headcount matrix and evaluate intersection and aggregation security groups. Everything is made-up illustrative data in a Python model; real merit matrices, grades and security design are organisation-specific and configured in Workday.

**Starter code**

```python
# Compensation analytics and security-group evaluation (a teaching model, not Workday)
GRADES = {"G4": (50000, 70000, 90000), "G5": (65000, 85000, 105000), "G6": (80000, 100000, 120000)}
WORKERS = [
    ("Ana", "Sales", "G4", 62000, 4.0), ("Bo", "Sales", "G5", 90000, 3.0), ("Cy", "Eng", "G5", 78000, 4.5),
    ("Di", "Eng", "G6", 118000, 3.5), ("Eli", "Eng", "G4", 52000, 2.0), ("Flo", "Ops", "G6", 95000, 5.0),
]
MERIT = {  # (performance band, compa band) -> percent
    ("high", "low"): 6.0, ("high", "mid"): 4.5, ("high", "high"): 3.0,
    ("mid", "low"): 4.0, ("mid", "mid"): 3.0, ("mid", "high"): 2.0,
    ("low", "low"): 1.5, ("low", "mid"): 0.0, ("low", "high"): 0.0,
}

# TODO: compa(sal, grade) = salary / midpoint;  penetration(sal, grade) = (sal - min) / (max - min);  band(value, cuts) -> low / mid / high
# TODO: for each worker print name, grade, compa, penetration, merit % from MERIT[(performance band with cuts 3.0/4.5, compa band with cuts 0.9/1.1)]
#       and the new salary (rounded); then the merit budget used (percent and amount)
# TODO: a headcount matrix by department and grade
# TODO: security groups: intersection(*groups) = users in ALL groups, aggregation(*groups) = users in ANY group;
#       can_view_comp = Comp Admins OR (HR Partner AND Sales Org members); show that pat is denied
users = {"mia": {"HR Partner", "Sales Org"}, "ned": {"HR Partner"}, "oli": {"Sales Org", "Comp Admin"}, "pat": set()}
```

**Expected output**

```text
name  grade compa  pen   merit%  new salary
Ana   G4     0.89  0.30    4.0  64480
Bo    G5     1.06  0.62    3.0  92700
Cy    G5     0.92  0.33    4.5  81510
Di    G6     1.18  0.95    2.0  120360
Eli   G4     0.74  0.05    1.5  52780
Flo   G6     0.95  0.38    4.5  99275
merit budget used: 3.25% (16105)
Eng    {'G4': 1, 'G5': 1, 'G6': 1}
Ops    {'G6': 1}
Sales  {'G4': 1, 'G5': 1}
intersection(HR Partner, Sales Org): ['mia']
aggregation(HR Partner, Comp Admin): ['mia', 'ned', 'oli']
can view compensation: ['mia', 'oli'] | pat denied: True
```

<details><summary>Show solution and explanation</summary>

<pre><code># Compensation analytics and security-group evaluation (a teaching model, not Workday)
GRADES = {&quot;G4&quot;: (50000, 70000, 90000), &quot;G5&quot;: (65000, 85000, 105000), &quot;G6&quot;: (80000, 100000, 120000)}
WORKERS = [
    (&quot;Ana&quot;, &quot;Sales&quot;, &quot;G4&quot;, 62000, 4.0), (&quot;Bo&quot;, &quot;Sales&quot;, &quot;G5&quot;, 90000, 3.0), (&quot;Cy&quot;, &quot;Eng&quot;, &quot;G5&quot;, 78000, 4.5),
    (&quot;Di&quot;, &quot;Eng&quot;, &quot;G6&quot;, 118000, 3.5), (&quot;Eli&quot;, &quot;Eng&quot;, &quot;G4&quot;, 52000, 2.0), (&quot;Flo&quot;, &quot;Ops&quot;, &quot;G6&quot;, 95000, 5.0),
]
MERIT = {  # (performance band, compa band) -&gt; percent
    (&quot;high&quot;, &quot;low&quot;): 6.0, (&quot;high&quot;, &quot;mid&quot;): 4.5, (&quot;high&quot;, &quot;high&quot;): 3.0,
    (&quot;mid&quot;, &quot;low&quot;): 4.0, (&quot;mid&quot;, &quot;mid&quot;): 3.0, (&quot;mid&quot;, &quot;high&quot;): 2.0,
    (&quot;low&quot;, &quot;low&quot;): 1.5, (&quot;low&quot;, &quot;mid&quot;): 0.0, (&quot;low&quot;, &quot;high&quot;): 0.0,
}

def compa(sal, grade):
    return sal / GRADES[grade][1]

def penetration(sal, grade):
    lo, mid, hi = GRADES[grade]
    return (sal - lo) / (hi - lo)

def band(value, cuts):
    return &quot;low&quot; if value &lt; cuts[0] else &quot;high&quot; if value &gt;= cuts[1] else &quot;mid&quot;

print(&quot;name  grade compa  pen   merit%  new salary&quot;)
total_before = total_after = 0
for name, dept, grade, sal, perf in WORKERS:
    c = compa(sal, grade)
    pct = MERIT[(band(perf, (3.0, 4.5)), band(c, (0.9, 1.1)))]
    new = round(sal * (1 + pct / 100))
    total_before += sal; total_after += new
    print(f&quot;{name:5} {grade:5} {c:5.2f} {penetration(sal, grade):5.2f} {pct:6.1f}  {new}&quot;)
print(f&quot;merit budget used: {100 * (total_after - total_before) / total_before:.2f}% ({total_after - total_before})&quot;)

# headcount matrix report: department x grade
matrix = {}
for _, dept, grade, _, _ in WORKERS:
    matrix.setdefault(dept, {}).setdefault(grade, 0)
    matrix[dept][grade] += 1
for dept in sorted(matrix):
    print(f&quot;{dept:6}&quot;, {g: matrix[dept][g] for g in sorted(matrix[dept])})

# security groups: users belong to role-based groups; intersection = AND, aggregation = OR
users = {&quot;mia&quot;: {&quot;HR Partner&quot;, &quot;Sales Org&quot;}, &quot;ned&quot;: {&quot;HR Partner&quot;}, &quot;oli&quot;: {&quot;Sales Org&quot;, &quot;Comp Admin&quot;}, &quot;pat&quot;: set()}
intersection = lambda *gs: {u for u, g in users.items() if set(gs) &lt;= g}
aggregation = lambda *gs: {u for u, g in users.items() if set(gs) &amp; g}
print(&quot;intersection(HR Partner, Sales Org):&quot;, sorted(intersection(&quot;HR Partner&quot;, &quot;Sales Org&quot;)))
print(&quot;aggregation(HR Partner, Comp Admin):&quot;, sorted(aggregation(&quot;HR Partner&quot;, &quot;Comp Admin&quot;)))
can_view_comp = aggregation(&quot;Comp Admin&quot;) | intersection(&quot;HR Partner&quot;, &quot;Sales Org&quot;)
print(&quot;can view compensation:&quot;, sorted(can_view_comp), &quot;| pat denied:&quot;, &quot;pat&quot; not in can_view_comp)
</code></pre>

<p>The matrix rewards high performers low in range and holds back those already high in range, and the budget check shows the total cost. Intersection groups are set-subset (AND) tests and aggregation groups are overlaps (OR). Verified by running the Python solution.</p>
</details>
