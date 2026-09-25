# UI Navigation

## Objective

Become comfortable navigating Nessus without needing someone to tell you where a feature is.

The goal is not to memorize the position of every button.

The goal is to understand:

```text
Where am I?
   ↓
What does this area control?
   ↓
When would I use it?
   ↓
What decision does it affect?
```

By the end of this file, you should be able to navigate the Nessus interface confidently enough to create, inspect, modify, monitor, and review assessments without following a screenshot tutorial.

---

# 1. The UI Mental Model

Think of the Nessus interface as a set of operational areas:

```text
Nessus
 │
 ├── Assessments
 │     ├── Create
 │     ├── Configure
 │     ├── Run
 │     ├── Monitor
 │     └── Review
 │
 ├── Reusable Configuration
 │     ├── Templates
 │     ├── Policies
 │     ├── Plugins
 │     └── Credentials
 │
 ├── Results
 │     ├── Findings
 │     ├── Evidence
 │     ├── History
 │     └── Reports / Exports
 │
 └── Administration
       ├── Scanner
       ├── Settings
       ├── Product State
       └── Other Available Controls
```

The exact labels and layout can change between Nessus versions and editions.

Learn the **function of an area**, not just its current label.

---

# 2. Before You Click

Do not explore the interface by randomly changing settings.

Use:

```text
Locate
   ↓
Read
   ↓
Understand
   ↓
Change only when necessary
   ↓
Observe
```

At this stage, exploration should be mostly read-only.

The objective is familiarity, not configuration.

---

# 3. Identify the Main Navigation

Open Nessus and identify the primary navigation areas available in your installation.

Do not use this file to guess the exact labels.

Your installed version is the source of truth.

Record what you actually see:

```text
Primary Navigation:

1. __________________________
2. __________________________
3. __________________________
4. __________________________
5. __________________________
6. __________________________
```

There may be fewer or more items depending on the product/version.

---

# 4. Assessment Area

Find the area used to manage scans.

Determine how Nessus allows you to:

* view existing scans
* create a scan
* inspect scan state
* open completed scan results
* access scan history
* manage recurring assessments where supported

Record:

```text
Assessment / Scan Area:
____________________________
```

### Decision Question

If someone tells you:

> "Create a new vulnerability assessment."

Where would you start?

```text
Answer:
____________________________
```

Do not continue until you can find the answer directly in your own Nessus interface.

---

# 5. Scan Creation

Locate the control used to create a new scan.

Do not launch anything yet.

Open the scan creation workflow and observe the available templates/options.

Record:

```text
Create Scan:
____________________________
```

Now identify the available starting templates.

Do not attempt to memorize every template.

Instead categorize them:

```text
Discovery
Vulnerability Assessment
Compliance / Configuration
Specialized / Edition-Specific
Other
```

Record examples actually visible in your installation:

```text
Discovery:
____________________________

Vulnerability:
____________________________

Compliance / Configuration:
____________________________

Specialized:
____________________________
```

---

# 6. Template Selection

A template is a starting point for an assessment workflow.

The important question is not:

> "Which template name do I remember?"

The important question is:

> **"Which template best matches my assessment objective?"**

For example:

```text
Objective:
Find reachable systems
        ↓
Look for discovery-oriented workflow
```

versus:

```text
Objective:
Identify vulnerabilities on a host
        ↓
Look for vulnerability-assessment workflow
```

Template selection will be studied in detail later.

For now, learn where templates are found.

---

# 7. Scan Configuration Area

Open a scan configuration screen without launching the scan.

Identify the major configuration categories available to you.

Depending on the template/version, these may include areas related to:

* basic information
* discovery
* assessment
* report
* advanced behavior
* credentials
* compliance
* plugins

Record the categories visible in your installation:

```text
Configuration Areas:

________________________________
________________________________
________________________________
________________________________
________________________________
```

Do not change anything yet.

---

# 8. Basic Information

Locate the area containing basic scan information.

Determine where you can identify things such as:

* scan name
* target
* description
* scheduling information where applicable
* other basic scan metadata

Ask:

> "If I need to determine what this scan is supposed to assess, where would I look?"

Record:

```text
Location:
____________________________
```

---

# 9. Targets

Locate where targets are configured.

Do not enter an unauthorized target.

Use your controlled lab only.

Determine how Nessus represents target input.

Possible forms may include:

* individual IP addresses
* hostnames
* ranges
* subnets
* other supported target definitions

Record:

```text
Target Configuration:
____________________________
```

### Decision Question

If the assessment scope changes, which part of the scan configuration should you inspect first?

```text
Answer:
____________________________
```

---

# 10. Discovery

Locate the discovery-related configuration.

Determine where Nessus allows you to control discovery behavior for the selected workflow.

Do not change advanced discovery settings yet.

The purpose of this exercise is simply:

```text
Find it
 ↓
Understand that it controls discovery
```

Record:

```text
Discovery Configuration:
____________________________
```

---

# 11. Assessment

Locate the assessment-related configuration.

This area controls how Nessus performs vulnerability assessment activities for the selected workflow.

Determine where it exists in your version.

Record:

```text
Assessment Configuration:
____________________________
```

Later, you will learn how to decide whether a setting here actually needs to change.

---

# 12. Credentials

Locate the credential configuration area.

Do not enter real production credentials.

For now, determine:

```text
Where are credentials configured?
____________________________
```

Identify the authentication categories available to your edition/version.

Record only categories you actually see:

```text
Credential Types:

1. __________________________
2. __________________________
3. __________________________
4. __________________________
```

Do not attempt to memorize the complete list.

The skill is knowing:

> **Where do I go when this assessment requires authentication?**

---

# 13. Plugins

Locate the plugin configuration area.

Determine how Nessus presents plugin information.

Depending on the current interface, you may encounter:

* plugin families
* individual plugins
* enabled/disabled states
* search/filter functionality
* plugin descriptions

Record:

```text
Plugin Configuration:
____________________________
```

Do not disable plugins merely for practice.

Plugin selection can materially change assessment coverage.

---

# 14. Policies

Locate the area where policies are created or managed, if available in your edition.

Determine:

```text
Where are policies?
____________________________

How do I create one?
____________________________

How do I edit one?
____________________________
```

Do not create a production-style policy yet.

We will study policies later.

The current goal is navigation.

---

# 15. Results

Find a completed or available scan in your lab.

Open its results.

Determine how Nessus allows you to view:

* affected hosts
* findings
* severity
* plugin information
* evidence/details
* remediation information
* other available result data

Record:

```text
Results Area:
____________________________
```

The important navigation path is:

```text
Scan
 ↓
Results
 ↓
Finding
 ↓
Evidence / Details
```

You will use this path repeatedly throughout the repository.

---

# 16. Finding Details

Open one finding from a completed lab scan.

Identify where Nessus displays information such as:

```text
Finding Name
Severity
Affected Asset
Plugin Information
Description
Evidence / Output
Remediation
References
```

Not every finding will display exactly the same information.

Record what your version provides:

```text
Finding Details Include:

________________________________
________________________________
________________________________
________________________________
```

Do not treat the finding title as sufficient information.

Later you will learn how to investigate the underlying evidence.

---

# 17. Scan Status

Find a scan that is running or inspect the available scan status/history information.

Determine how Nessus communicates states such as:

```text
Created
Running
Completed
Paused
Stopped
Failed
```

The exact states depend on the version/workflow.

Record the states you can actually observe:

```text
Scan States:
________________________________
________________________________
________________________________
```

Ask:

> "How would I know whether a scan is still running?"

Record:

```text
Answer:
____________________________
```

---

# 18. Scan History

Locate scan history.

Determine how Nessus allows you to find previous assessments.

Ask:

```text
Can I identify when a scan ran?
Can I reopen previous results?
Can I distinguish different scan executions?
Can I compare current and previous assessment information?
```

Record:

```text
History Location:
____________________________
```

This becomes important later for retesting and recurring assessments.

---

# 19. Reports and Exports

Locate the available report/export functionality.

Do not export everything yet.

Determine:

```text
Where are reports generated?
____________________________

Where are exports performed?
____________________________
```

Record the output formats available in your edition/version:

```text
Available Formats:
________________________________
________________________________
________________________________
```

The exact formats may vary by version and product.

Later you will learn which output is appropriate for different situations.

---

# 20. Scheduling

Locate scheduling functionality where available.

Determine:

```text
Where is scheduling configured?
____________________________
```

Do not create a recurring production-style schedule.

For now, understand where the control exists and what information it requests.

---

# 21. Administration and Scanner Settings

Locate the administration/settings area available to your account.

Do not change anything unnecessarily.

Identify where you can find information related to:

* scanner configuration
* product state
* updates
* system settings
* user/account administration
* other available administrative controls

Record:

```text
Administration / Settings:
____________________________
```

The objective is recognition, not modification.

---

# 22. Version and Edition Awareness Exercise

Now perform a navigation exercise.

Find:

```text
Nessus Version
Nessus Edition / Product
Scanner State
```

Record where each is displayed:

```text
Version:
____________________________

Edition:
____________________________

Scanner State:
____________________________
```

This will become useful whenever documentation and your interface do not appear identical.

---

# 23. UI Search Exercise

Without looking at this document, find the following:

### Task 1

Find where a new scan is created.

```text
Location:
____________________________
```

### Task 2

Find where scan targets are configured.

```text
Location:
____________________________
```

### Task 3

Find where credentials are configured.

```text
Location:
____________________________
```

### Task 4

Find plugin configuration.

```text
Location:
____________________________
```

### Task 5

Find scan results.

```text
Location:
____________________________
```

### Task 6

Find detailed information for a vulnerability.

```text
Location:
____________________________
```

### Task 7

Find report/export functionality.

```text
Location:
____________________________
```

### Task 8

Find scanner/product settings.

```text
Location:
____________________________
```

If you needed this file to answer these tasks, repeat them until you can navigate directly.

---

# 24. UI Decision Exercise

The goal of this exercise is to connect **assessment questions to UI locations**.

### Situation A

You need to change the target.

Where do you go?

```text
Answer:
____________________________
```

### Situation B

You need to configure SSH credentials.

Where do you go?

```text
Answer:
____________________________
```

### Situation C

You need to inspect why Nessus reported a vulnerability.

Where do you go?

```text
Answer:
____________________________
```

### Situation D

You need to determine whether the scan actually completed.

Where do you go?

```text
Answer:
____________________________
```

### Situation E

You need to export assessment data.

Where do you go?

```text
Answer:
____________________________
```

### Situation F

You need to create a recurring assessment.

Where do you look?

```text
Answer:
____________________________
```

---

# 25. The "Find → Explain → Use" Method

For every important Nessus UI area, use this three-stage method.

## Stage 1 — Find

Can you locate it without instructions?

```text
Can I find it?
```

## Stage 2 — Explain

Can you explain what it controls?

```text
What does it affect?
```

## Stage 3 — Use

Can you use it correctly in a realistic assessment?

```text
When should I change it?
```

A feature is not considered learned until all three are true.

---

# 26. UI Familiarity Matrix

Maintain this mental model:

| Area            | Question You Should Be Able to Answer     |
| --------------- | ----------------------------------------- |
| Scans           | Where do I create and manage assessments? |
| Templates       | Which workflow can I start from?          |
| Targets         | What systems am I assessing?              |
| Discovery       | How is host/service discovery controlled? |
| Assessment      | What checks should Nessus perform?        |
| Credentials     | How will Nessus authenticate?             |
| Plugins         | Which checks are available/selected?      |
| Policies        | What reusable configuration should I use? |
| Results         | What did Nessus find?                     |
| Finding Details | Why did Nessus report it?                 |
| History         | What happened in previous assessments?    |
| Reports/Exports | How do I preserve or communicate results? |
| Scheduling      | How do I repeat the assessment?           |
| Administration  | How do I manage the scanner/product?      |

The exact UI labels can change.

The questions remain useful.

---

# 27. What Not to Memorize

Do not spend time memorizing:

* exact button positions
* exact menu wording
* every plugin ID
* every configuration field
* every template name
* every report format
* every administrative option

Instead remember:

```text
Assessment
→ Find scan controls

Configuration
→ Find the relevant setting category

Authentication
→ Find credentials

Detection
→ Find plugins

Results
→ Find findings and evidence

Operations
→ Find scanner/settings
```

If a label changes between versions, the mental model still works.

---

# 28. UI Troubleshooting

### Problem

A feature shown in documentation is missing.

### Do not immediately assume the installation is broken.

Check:

```text
Nessus Version
        ↓
Nessus Edition
        ↓
License
        ↓
Feature availability
        ↓
Current Tenable documentation
```

---

### Problem

A menu looks different from a tutorial.

Check:

```text
Tutorial Version
        ↓
Your Nessus Version
        ↓
Your Edition
        ↓
Current Documentation
```

Do not blindly reproduce an old tutorial.

---

### Problem

You cannot find a setting.

Use:

```text
What does the setting control?
        ↓
Which configuration category should contain it?
        ↓
Search/navigate within that category
        ↓
Verify the current documentation
```

This is more reliable than remembering a screenshot.

---

# 29. Practical Challenge — UI Scavenger Hunt

Without following the earlier sections, find all of these in your current Nessus installation:

```text
[ ] Scan creation
[ ] Scan templates
[ ] Scan targets
[ ] Discovery settings
[ ] Assessment settings
[ ] Credentials
[ ] Plugins
[ ] Policies
[ ] Scan results
[ ] Finding details
[ ] Scan history
[ ] Reports/exports
[ ] Scheduling
[ ] Scanner/product settings
[ ] Version information
[ ] Edition/product information
```

Do not modify anything unless the task specifically requires it.

---

# 30. Practical Challenge — Explain Before Clicking

Choose any configuration area.

Before changing anything, answer:

```text
What does this area control?
____________________________

Why might an assessment require changing it?
____________________________

What could happen if I change it incorrectly?
____________________________

Would the default probably be sufficient?
____________________________
```

Then inspect the actual setting.

This creates the habit:

> **Understand first. Change second.**

---

# 31. Practical Challenge — Find the Path

Starting from the Nessus home/interface, determine the path you would follow for each task.

### Create an assessment

```text
Start
 ↓
________________
 ↓
________________
```

### Configure credentials

```text
Start
 ↓
________________
 ↓
________________
```

### Inspect a vulnerability

```text
Start
 ↓
________________
 ↓
________________
```

### Review a previous scan

```text
Start
 ↓
________________
 ↓
________________
```

### Export results

```text
Start
 ↓
________________
 ↓
________________
```

Do this directly in your installed Nessus interface.

---

# 32. UI Independence Test

Close this file.

Open Nessus.

Without looking back, locate:

```text
1. Scan creation
2. Scan templates
3. Target configuration
4. Credentials
5. Plugins
6. Policies
7. Results
8. Finding details
9. Scan history
10. Reporting/export
11. Scheduling
12. Scanner settings
13. Version/edition information
```

You do not need to remember exact labels.

You need to be able to **find the correct area using the function it performs**.

---

# 33. Completion Criteria

This file is complete when you can navigate Nessus without needing a step-by-step UI tutorial.

You should be able to:

* locate scan creation
* identify appropriate template categories
* locate targets
* locate discovery settings
* locate assessment settings
* locate credentials
* locate plugins
* locate policies
* locate scan results
* open vulnerability details
* find scan history
* locate reports/exports
* locate scheduling
* locate scanner/product settings
* identify your Nessus edition
* identify your Nessus version
* distinguish UI differences caused by version or edition
* explain what a UI area controls
* determine when a UI setting might matter
* avoid changing settings without a reason

The final skill is:

```text
Assessment Requirement
        ↓
Identify What Must Be Changed
        ↓
Know Where That Control Lives
        ↓
Change Only What Is Necessary
        ↓
Observe the Result
```

You are now ready to perform your first complete Nessus assessment.
