# Nessus Workflow

A practical, workflow-driven guide for learning and mastering **Tenable Nessus** from absolute beginner to independent professional vulnerability assessment.

This repository is designed to teach **how to operate Nessus**, not merely how to memorize its features.

The goal is simple:

> Given a new, authorized Nessus assessment, determine what to do, perform it safely, understand the results, troubleshoot problems, make defensible decisions, document the work, and determine what to do next.

---

## Learning Philosophy

This repository follows:

```text
Goal
 ↓
Situation
 ↓
Question
 ↓
Decision
 ↓
Action
 ↓
Expected Result
 ↓
Actual Result
 ↓
Next Decision
```

The emphasis is on **workflow thinking and operational decision-making**.

You will progressively learn to answer:

* What am I assessing?
* Is it authorized and in scope?
* What question am I trying to answer?
* Which Nessus workflow fits the objective?
* Do I need authentication?
* Which targets should be assessed?
* Which configuration actually matters?
* What should remain at the default?
* Is the scan safe to execute?
* What should I monitor?
* Are the results complete?
* Which findings require investigation?
* Which findings require validation?
* How should findings be prioritized?
* What evidence should be documented?
* How should the results be reported?
* How should remediation be verified?
* What should I do if the expected result does not occur?

---

## Learning Progression

The repository progresses through increasingly independent work:

```text
Absolute Beginner
       ↓
Nessus Mental Model
       ↓
UI Familiarity
       ↓
Installation & Initialization
       ↓
First Assessment
       ↓
Scan Configuration
       ↓
Assessment Workflows
       ↓
Authenticated Assessment
       ↓
Results Analysis
       ↓
Finding Validation
       ↓
Prioritization
       ↓
Professional Reporting
       ↓
Troubleshooting
       ↓
Professional Assessment Workflow
       ↓
Decision-Based Labs
       ↓
Independent Assessments
       ↓
Final Capstone
       ↓
Independent Nessus Operator
```

The level of guidance intentionally decreases as the repository progresses.

### Stage 1 — Guided

You receive:

* exact workflow
* UI path
* actions
* expected results
* troubleshooting guidance

### Stage 2 — Partially Guided

You receive:

* objective
* environment
* constraints
* success criteria

You determine the appropriate Nessus workflow.

### Stage 3 — Independent

You receive only:

* assessment scenario
* scope
* objective
* constraints
* success criteria

You determine the complete workflow yourself.

---

## Core Nessus Mental Model

The repository builds an internal model of how the major Nessus components relate:

```text
Authorization
     ↓
Scope
     ↓
Assessment Question
     ↓
Target
     ↓
Scan Configuration
     ↓
Plugins
     ↓
Credentials
     ↓
Scanner
     ↓
Assessment
     ↓
Findings
     ↓
Evidence
     ↓
Risk / Priority
     ↓
Report
     ↓
Remediation
     ↓
Retest
     ↓
Verification
```

Changing one part can affect another.

For example:

```text
Credentialed Assessment
        ↓
Authentication succeeds
        ↓
More host-level information becomes available
        ↓
Additional checks can be performed
        ↓
Assessment coverage changes
```

Therefore, Nessus configuration is treated as a **decision**, not a collection of settings to memorize.

---

## Repository Structure

```text
Nessus-Workflow/
│
├── README.md
│
├── 00-Foundations/
│   ├── 01-Nessus-Mental-Model.md
│   └── 02-Lab-and-Safety.md
│
├── 01-Setup-and-UI/
│   ├── 01-Installation-and-Initialization.md
│   └── 02-UI-Navigation.md
│
├── 02-First-Assessment/
│   ├── 01-First-Scan.md
│   └── 02-Scan-Lifecycle.md
│
├── 03-Assessment-Configuration/
│   ├── 01-Choosing-the-Workflow.md
│   ├── 02-Targets-and-Discovery.md
│   ├── 03-Scan-Settings.md
│   ├── 04-Credentials.md
│   ├── 05-Policies-Templates-and-Plugins.md
│   └── 06-Scheduling-and-Recurring-Assessments.md
│
├── 04-Assessment-Workflows/
│   ├── 01-Discovery.md
│   ├── 02-Unauthenticated-Vulnerability-Assessment.md
│   ├── 03-Authenticated-Vulnerability-Assessment.md
│   ├── 04-Configuration-and-Compliance.md
│   └── 05-Edition-Specific-Workflows.md
│
├── 05-Results-and-Validation/
│   ├── 01-Reading-Results.md
│   ├── 02-Investigating-Findings.md
│   ├── 03-Validating-Findings.md
│   └── 04-Comparing-and-Tracking-Results.md
│
├── 06-Prioritization-and-Reporting/
│   ├── 01-Prioritizing-Findings.md
│   ├── 02-Professional-Reporting.md
│   └── 03-Remediation-and-Retest.md
│
├── 07-Troubleshooting/
│   ├── 01-Scanner-and-Platform-Problems.md
│   ├── 02-Network-and-Target-Problems.md
│   ├── 03-Credential-and-Authentication-Problems.md
│   └── 04-Scan-and-Result-Problems.md
│
├── 08-Professional-Workflow/
│   ├── 01-End-to-End-Assessment.md
│   ├── 02-Operational-Considerations.md
│   └── 03-Assessment-Documentation.md
│
├── 09-Decision-Labs/
│   ├── 01-Guided-Decisions.md
│   ├── 02-Partial-Information.md
│   └── 03-Independent-Assessments.md
│
└── 10-Capstone/
    ├── 01-Capstone-Brief.md
    ├── 02-Capstone-Success-Criteria.md
    └── 03-Operator-Checklist.md
```

The structure is intentionally compact.

Files will only be added when they provide a distinct learning or operational capability.

---

## How to Use This Repository

Do not read the repository like a textbook.

For each practical workflow:

```text
1. Understand the objective
2. Identify the situation
3. Determine what information is available
4. Make the required decision
5. Perform the workflow
6. Observe the result
7. Compare expected vs actual behavior
8. Troubleshoot if necessary
9. Analyze the outcome
10. Determine the next action
```

When a workflow becomes familiar, stop following the instructions mechanically.

Instead ask:

> "If I encountered this situation in a real assessment, what would I do?"

---

## Decision-Making Model

The repository repeatedly uses this structure:

```text
Situation
    ↓
Question
    ↓
Decision
    ↓
Action
    ↓
Expected Result
    ↓
Actual Result
    ↓
Next Action
```

For branching situations:

```text
IF X
    → Perform A

ELSE IF Y
    → Perform B

ELSE
    → Investigate C
```

The objective is to make these decisions independently by the end of the repository.

---

## Troubleshooting Model

Failures are treated as part of normal Nessus operation.

The standard troubleshooting process is:

```text
Symptom
   ↓
Expected Behavior
   ↓
Observed Behavior
   ↓
Difference
   ↓
Likely Causes
   ↓
Checks
   ↓
Decision
   ↓
Fix
   ↓
Verification
```

Do not simply restart a failed scan.

Determine **why** it failed.

---

## Professional Assessment Lifecycle

The repository ultimately connects individual Nessus tasks into a complete assessment:

```text
Authorization
    ↓
Scope
    ↓
Preparation
    ↓
Assessment Planning
    ↓
Target Selection
    ↓
Workflow Selection
    ↓
Credential Planning
    ↓
Configuration
    ↓
Safety / Impact Review
    ↓
Scanning
    ↓
Monitoring
    ↓
Results Analysis
    ↓
Finding Validation
    ↓
Prioritization
    ↓
Reporting
    ↓
Remediation
    ↓
Retesting
    ↓
Verification
```

Not every assessment follows exactly the same path.

The repository therefore teaches when and why the workflow branches.

---

## Safety and Authorization

All practical scanning must be performed only against:

* systems owned by you
* systems for which you have explicit authorization
* intentionally vulnerable laboratory environments

Authorization and scope come **before scanning**.

Never use Nessus against systems merely because they are reachable.

---

## Version and Edition Awareness

Nessus capabilities, scan templates, UI elements, licensing, and workflows can vary between:

* Nessus editions
* product versions
* operating systems
* licensing configurations
* enabled capabilities
* plugins

This repository therefore does not treat every Nessus feature as universally available.

When a workflow depends on a specific capability, verify it against the current Tenable documentation and the Nessus edition/version being used.

The repository prioritizes **transferable workflow knowledge** over fragile memorization of UI labels.

---

## Minimality Rule

Every file must answer:

> **Would removing this file reduce the user's ability to operate Nessus independently?**

If not, the content should be merged or removed.

The repository deliberately avoids:

* unnecessary theory
* giant command lists
* feature encyclopedias
* duplicate explanations
* arbitrary plugin memorization
* repetitive screenshots
* generic cybersecurity fundamentals
* files created only to make the repository appear larger

---

## Practical Learning Standard

Completing a file does not mean memorizing it.

A skill is considered learned when you can:

```text
Understand the objective
        ↓
Choose the appropriate workflow
        ↓
Perform it
        ↓
Interpret the result
        ↓
Handle unexpected behavior
        ↓
Explain your decision
        ↓
Determine the next action
```

---

## Final Independence Standard

The repository is complete only when you can receive a new authorized Nessus assessment and independently determine:

1. What to do first
2. What information to gather
3. What is in scope
4. Which assessment workflow is appropriate
5. Whether authentication is required
6. How targets should be handled
7. Which configuration matters
8. What should remain at default
9. Whether the assessment is safe to execute
10. How to run the assessment
11. How to monitor it
12. Whether the results are complete
13. How to investigate findings
14. Which findings require validation
15. How to prioritize findings
16. What evidence to document
17. How to report the results
18. How to troubleshoot unexpected behavior
19. How to verify remediation
20. What the next action should be

The final capstone intentionally removes step-by-step instructions.

---

## Completion Goal

The goal of this repository is not:

> "I know where the Nessus buttons are."

The goal is:

> **"I can independently operate Nessus as part of a professional vulnerability-assessment workflow."**

---

## Disclaimer

Use Nessus only against systems and environments that you own or are explicitly authorized to assess.

This repository is intended for authorized security assessment, vulnerability management, and controlled laboratory environments.
