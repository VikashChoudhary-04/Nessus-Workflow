# Choosing the Workflow

## Objective

Learn how to choose the appropriate Nessus assessment workflow before configuring a scan.

By the end of this file, you should be able to:

* Start with an assessment question instead of a scan template.
* Distinguish discovery from vulnerability assessment.
* Distinguish unauthenticated from authenticated assessment.
* Recognize when configuration/compliance assessment is a different objective.
* Understand why the same target can require different assessments.
* Choose a workflow based on scope, objective, access, and constraints.
* Avoid changing scan settings before understanding the problem.
* Explain why a particular workflow was selected.

---

## 1. The Most Important Question

Do not begin with:

> "Which Nessus template should I select?"

Begin with:

> **"What am I trying to determine?"**

The workflow should follow the assessment objective.

Use this model:

```text
AUTHORIZATION
     ↓
SCOPE
     ↓
ASSESSMENT QUESTION
     ↓
AVAILABLE ACCESS
     ↓
ENVIRONMENT / CONSTRAINTS
     ↓
WORKFLOW
     ↓
CONFIGURATION
```

The template is a means to perform the assessment.

It is not the assessment objective itself.

---

# 2. Why Workflow Selection Matters

Different assessment questions require different approaches.

For example:

### Question A

> "Which hosts and services are exposed on this network?"

This is primarily a **discovery** question.

### Question B

> "What vulnerabilities can Nessus identify on this web server from an external perspective?"

This is a **vulnerability assessment** question.

### Question C

> "What additional vulnerabilities can be identified when Nessus can authenticate to this server?"

This is an **authenticated vulnerability assessment** question.

### Question D

> "Does this system comply with the required configuration baseline?"

This is a **configuration/compliance** question.

The target may be identical.

The assessment question is different.

Therefore the workflow can be different.

---

# 3. Core Workflow Categories

At a high level, think in terms of these categories:

```text
Nessus Assessment
│
├── Discovery
│
├── Vulnerability Assessment
│     ├── Unauthenticated
│     └── Authenticated
│
├── Configuration / Compliance
│
└── Specialized / Edition-Specific Workflows
```

Exact template names and available workflows vary by:

* Nessus version.
* Nessus edition.
* Licensing.
* Platform.
* Available plugins.
* Product configuration.

Therefore:

> **Learn the purpose of a workflow, not just its current UI name.**

---

# 4. Discovery

Discovery answers questions about the environment itself.

Typical questions:

* Which hosts are reachable?
* Which systems respond?
* Which ports/services appear exposed?
* What technologies or services are visible?
* What should be included in a later vulnerability assessment?

The important distinction is:

```text
Discovery
   ↓
"What is there?"
```

versus:

```text
Vulnerability Assessment
   ↓
"What security weaknesses can be identified?"
```

Discovery can support vulnerability assessment, but they are not the same objective.

---

# 5. When to Choose Discovery

Choose a discovery-oriented workflow when the immediate objective is to establish or verify the target environment.

Examples:

### New Lab

You have:

```text
192.168.56.0/24
```

but do not yet know which hosts are active.

The immediate question is:

> "Which authorized hosts are actually present?"

Discovery is appropriate.

### Existing Asset List

You already have a verified list of systems.

The immediate question is:

> "What vulnerabilities exist on these systems?"

A dedicated vulnerability assessment may be more appropriate.

---

# 6. Vulnerability Assessment

A vulnerability assessment asks:

> **"What vulnerabilities or security weaknesses can Nessus identify under the configured assessment conditions?"**

The result depends on:

* Target.
* Network visibility.
* Workflow.
* Plugins/checks.
* Credentials.
* Configuration.
* Target state.
* Scanner behavior.
* Environmental conditions.

Therefore:

```text
Vulnerability Assessment
≠
Universal proof that no vulnerabilities exist
```

The assessment provides evidence within its defined scope and methodology.

---

# 7. Unauthenticated Assessment

An unauthenticated assessment evaluates the target without providing host-level credentials.

Conceptually:

```text
Scanner
   │
   │ Network interaction
   ↓
Target
```

This can approximate an external or limited-access perspective.

It can identify issues that are observable without privileged system access.

However, it may not provide the same visibility as an authenticated assessment.

---

# 8. Authenticated Assessment

An authenticated assessment provides Nessus with credentials or another supported authentication mechanism so it can obtain additional information from the target.

Conceptually:

```text
Scanner
   │
   ├── Network assessment
   │
   └── Authentication
          ↓
       Target
```

Potentially available information can be broader because Nessus may be able to inspect aspects of the host that are not externally observable.

But:

> **Authenticated does not automatically mean "better."**

It answers a different assessment question.

---

# 9. Unauthenticated vs Authenticated

Use this conceptual comparison:

| Question                       | Unauthenticated                  | Authenticated                                         |
| ------------------------------ | -------------------------------- | ----------------------------------------------------- |
| External visibility            | Strong relevance                 | Less representative                                   |
| Host-level visibility          | Limited                          | Potentially greater                                   |
| Credential dependency          | No                               | Yes                                                   |
| Configuration inspection       | More limited                     | Potentially broader                                   |
| Patch/software visibility      | May be inferred                  | Potentially more direct                               |
| Authentication troubleshooting | Not applicable                   | Required                                              |
| Operational complexity         | Lower                            | Higher                                                |
| Best question                  | "What is externally observable?" | "What can be identified with authorized host access?" |

This is not a ranking.

The correct choice depends on the assessment objective.

---

# 10. Do Not Automatically Choose Authenticated

A common beginner assumption is:

> "If credentials are available, I should always use them."

That is not necessarily correct.

Suppose the objective is:

> "What could an unauthenticated attacker identify against this exposed server?"

Using credentials changes the assessment perspective.

In that situation, an unauthenticated assessment may be the relevant workflow.

Now consider:

> "Which missing patches and installed software vulnerabilities can be identified using authorized administrative access?"

Authentication becomes highly relevant.

The objective determines the workflow.

---

# 11. Configuration and Compliance

Configuration/compliance assessment is different from ordinary vulnerability discovery.

A vulnerability assessment asks:

> "What security weaknesses can be identified?"

A configuration/compliance assessment may ask:

> "Does this system meet a defined configuration or security baseline?"

Examples can include:

* Organizational configuration requirements.
* Security hardening requirements.
* Industry baselines.
* Policy requirements.
* Specific compliance checks.

The exact checks and supported standards depend on the Nessus edition, plugins, and available content.

Do not assume that every compliance framework is available in every Nessus installation.

---

# 12. Specialized Workflows

Some environments require more specialized assessment approaches.

Examples may involve:

* Particular technologies.
* Specialized applications.
* Specific platforms.
* Configuration standards.
* Edition-specific capabilities.

The correct process is:

```text
Assessment Question
      ↓
Required Evidence
      ↓
Available Nessus Capability
      ↓
Appropriate Workflow
```

Not:

```text
Interesting Template
      ↓
Run It
      ↓
Figure Out Why
```

---

# 13. The Workflow Selection Algorithm

Use this process every time.

## Step 1 — Define the Objective

Write one sentence:

> "I need to determine __________."

Examples:

> "I need to determine which authorized hosts are reachable."

> "I need to determine which externally observable vulnerabilities exist on the target."

> "I need to determine which vulnerabilities can be identified with authorized host credentials."

> "I need to determine whether the host satisfies the required configuration baseline."

---

## Step 2 — Determine the Perspective

Ask:

```text
What access does the assessment represent?
```

Possible perspectives include:

* Network-only.
* Unauthenticated.
* Authenticated.
* Configuration/compliance.
* Specialized.

---

## Step 3 — Determine the Required Evidence

Ask:

> "What information do I need Nessus to collect to answer the question?"

For example:

```text
Question:
"What services are exposed?"

Required evidence:
Host reachability + service exposure
```

Another example:

```text
Question:
"What missing software updates can be identified?"

Required evidence:
Software / package / patch information
```

This may make authentication relevant.

---

## Step 4 — Consider Constraints

Check:

* Authorization.
* Target scope.
* Network access.
* Credentials.
* Time window.
* Production sensitivity.
* Scanner capacity.
* Required depth.
* Operational impact.

A theoretically useful workflow may still be inappropriate if it violates the assessment constraints.

---

## Step 5 — Select the Workflow

Only now choose the corresponding Nessus workflow/template.

---

# 14. Workflow Selection Example 1

### Situation

You have one authorized lab VM.

You want to know:

> "What vulnerabilities are visible to Nessus without credentials?"

### Reasoning

```text
Objective:
Vulnerability assessment

Perspective:
Unauthenticated

Target:
Authorized lab VM

Credentials:
Not required

Workflow:
Unauthenticated/general vulnerability assessment
```

Do not add credentials simply because they are available.

---

# 15. Workflow Selection Example 2

### Situation

You need to assess an authorized Windows system and identify weaknesses that require host-level visibility.

### Reasoning

```text
Objective:
Vulnerability assessment

Perspective:
Authenticated

Target:
Authorized Windows system

Credentials:
Available and authorized

Workflow:
Authenticated vulnerability assessment
```

The next task is credential configuration.

That belongs to a later file.

---

# 16. Workflow Selection Example 3

### Situation

You are given an authorized network range but do not have a reliable asset inventory.

Your first question is:

> "Which systems are reachable?"

### Reasoning

```text
Objective:
Host discovery

Known targets:
Network range

Required evidence:
Reachable hosts

Workflow:
Discovery-oriented assessment
```

After discovery, you may perform additional vulnerability assessments against identified targets.

---

# 17. Workflow Selection Example 4

### Situation

Your organization provides a required configuration baseline and asks:

> "Does this authorized server meet the baseline?"

### Reasoning

```text
Objective:
Configuration/compliance

Required evidence:
Configuration state

Workflow:
Configuration/compliance assessment
```

Do not substitute a generic vulnerability scan and assume it answers the same question.

---

# 18. One Target Can Require Multiple Workflows

Consider a single server:

```text
TARGET
192.168.56.20
```

You could have:

```text
Assessment 1
→ Discovery

Assessment 2
→ Unauthenticated vulnerability assessment

Assessment 3
→ Authenticated vulnerability assessment

Assessment 4
→ Configuration/compliance assessment
```

These are not necessarily redundant.

Each answers a different question.

The professional skill is knowing why each assessment exists.

---

# 19. Avoid "Scan Everything"

A beginner often thinks:

> "I should enable everything."

This creates several problems.

It may:

* Increase assessment duration.
* Increase traffic.
* Increase complexity.
* Produce more information than needed.
* Make troubleshooting harder.
* Make result interpretation harder.
* Increase operational impact.
* Fail to answer the actual assessment question more effectively.

Use the smallest appropriate workflow that can answer the question.

This does not mean choosing an artificially weak scan.

It means:

> **Match assessment depth to assessment purpose.**

---

# 20. Workflow vs Configuration

These are related but different.

### Workflow

Defines the overall assessment approach.

### Configuration

Defines how that workflow is executed.

Think:

```text
Question
   ↓
Workflow
   ↓
Configuration
   ↓
Execution
```

For example:

```text
Question:
"What vulnerabilities are externally observable?"

Workflow:
Unauthenticated vulnerability assessment

Configuration:
Target + discovery + assessment settings + plugin behavior + other relevant controls
```

Do not solve a workflow-selection problem by randomly changing configuration values.

---

# 21. What If No Template Looks Perfect?

This is normal.

Do not choose randomly.

Use:

```text
What is my objective?
        ↓
Which available workflow is closest?
        ↓
Does it provide the required evidence?
        ↓
Can configuration make it appropriate?
        ↓
Are there important limitations?
```

If no available workflow can answer the question adequately, document the limitation rather than pretending that the assessment answers it.

---

# 22. Version and Edition Awareness

Nessus capabilities and interface options can differ between versions and editions.

Therefore, when selecting a workflow:

1. Look at the workflows actually available in your installation.
2. Read the workflow/template description.
3. Determine its intended purpose.
4. Confirm that the required capability is available.
5. Check whether licensing or edition affects it.
6. Avoid copying instructions written for a different Nessus environment without verification.

Your repository should teach transferable reasoning, not dependency on one UI screenshot.

---

# 23. Workflow Selection Record

For important assessments, record:

```text id="v1h7ca"
Assessment Question:
Target:
Authorized Scope:
Assessment Perspective:
Required Evidence:
Credentials Available:
Operational Constraints:
Selected Workflow:
Why This Workflow:
Important Limitations:
Expected Result:
```

The most important field is:

```text
Why This Workflow
```

You should be able to explain the decision to another assessor.

---

# 24. Practical Exercise 1 — Classify the Question

For each question, classify the primary workflow.

### A

> "Which hosts are reachable in my authorized lab subnet?"

Answer:

**Discovery**

### B

> "Which vulnerabilities are externally observable on this authorized server?"

Answer:

**Unauthenticated vulnerability assessment**

### C

> "Which vulnerabilities can be identified using authorized host credentials?"

Answer:

**Authenticated vulnerability assessment**

### D

> "Does this server meet the required hardening baseline?"

Answer:

**Configuration/compliance assessment**

### E

> "Which findings from yesterday's scan remain after remediation?"

Answer:

**Follow-up/retest assessment workflow**

The final example also requires comparison and result analysis; workflow selection should follow the actual retest objective.

---

# 25. Practical Exercise 2 — Explain Your Decision

Choose one authorized lab target.

Write:

```text
Assessment Question:
```

Then answer:

```text
What perspective does this assessment require?
What evidence do I need?
Do I need authentication?
What operational constraints exist?
Which available Nessus workflow fits?
Why?
What would make me choose a different workflow?
```

Do not proceed until your reasoning is clear.

---

# 26. Practical Exercise 3 — Same Target, Different Questions

Use one authorized lab VM.

Create three hypothetical assessment requests:

### Request 1

> "Assess the target from a network-only perspective."

### Request 2

> "Assess the target using authorized credentials."

### Request 3

> "Determine whether the target meets a specified configuration baseline."

For each, identify:

```text
Question
Perspective
Required Evidence
Workflow Category
Credential Requirement
Expected Output
```

The target remains the same.

The workflow changes because the question changes.

---

# 27. Practical Exercise 4 — Reject a Bad Workflow

Imagine someone gives you this instruction:

> "Run the biggest scan available against the server and enable everything."

Your task is to respond as an assessor.

Ask:

1. What is the assessment objective?
2. What evidence is required?
3. What perspective is required?
4. Is authentication needed?
5. What is the authorized scope?
6. What operational impact is acceptable?
7. Which workflow actually answers the question?

The correct professional response is not to blindly maximize scan scope.

It is to define the assessment first.

---

# 28. Decision Matrix

Use this as a quick reference:

| Assessment Question                                       | Primary Workflow                         |
| --------------------------------------------------------- | ---------------------------------------- |
| Which authorized hosts are reachable?                     | Discovery                                |
| Which services are externally visible?                    | Discovery / network-oriented assessment  |
| Which vulnerabilities are visible without credentials?    | Unauthenticated vulnerability assessment |
| Which vulnerabilities can be identified with host access? | Authenticated vulnerability assessment   |
| Does the system meet a defined configuration baseline?    | Configuration/compliance assessment      |
| Did remediation remove a previously identified issue?     | Retest / follow-up assessment            |
| Which workflow should I choose?                           | Start with the assessment question       |

The exact Nessus template used for each category depends on the capabilities available in your version and edition.

---

# 29. Common Mistakes

## Mistake 1 — Choosing by Name Alone

Bad reasoning:

> "This template sounds advanced, so I'll use it."

Better:

> "This workflow provides the evidence required by my assessment question."

---

## Mistake 2 — Always Using Credentials

Bad reasoning:

> "Authenticated scans find more, so they are always preferable."

Better:

> "Authentication changes the assessment perspective and must match the objective."

---

## Mistake 3 — Enabling Everything

Bad reasoning:

> "More plugins means a better assessment."

Better:

> "The assessment should provide appropriate coverage for the defined objective while respecting operational constraints."

---

## Mistake 4 — Confusing Discovery With Vulnerability Assessment

Bad reasoning:

> "I discovered a port, therefore I performed a vulnerability assessment."

Better:

> "Discovery established exposure; a vulnerability assessment answers a different question."

---

## Mistake 5 — Treating Templates as Permanent

Bad reasoning:

> "This is the template I always use."

Better:

> "I choose the workflow based on the current objective and environment."

---

## Mistake 6 — Ignoring Edition Differences

Bad reasoning:

> "A tutorial showed this option, so my installation must have it."

Better:

> "I verify the capability in my actual Nessus version and edition."

---

# 30. Professional Decision Rule

Before selecting a Nessus workflow, complete this sentence:

> **"I am choosing this workflow because I need Nessus to collect __________ from __________ under __________ conditions."**

Example:

> "I am choosing an unauthenticated vulnerability assessment because I need Nessus to identify vulnerabilities observable against an authorized server without host credentials."

Another:

> "I am choosing an authenticated vulnerability assessment because I need Nessus to assess the authorized host with additional visibility provided by valid credentials."

If you cannot complete this sentence clearly, you probably have not defined the assessment objective well enough.

---

# 31. Workflow Selection Checklist

Before creating the assessment:

```text
[ ] What is the assessment question?
[ ] What is the authorized scope?
[ ] What perspective is required?
[ ] What evidence is required?
[ ] Are credentials required?
[ ] What operational constraints exist?
[ ] Which workflow matches the objective?
[ ] Does the workflow exist in my Nessus edition/version?
[ ] Are there important limitations?
[ ] Can I explain why I selected it?
```

---

# 32. Completion Criteria

You have completed this file when you can independently:

* Start with an assessment question rather than a template.
* Explain the difference between discovery and vulnerability assessment.
* Explain unauthenticated and authenticated assessment perspectives.
* Identify when configuration/compliance is the appropriate objective.
* Recognize that one target can require multiple different assessments.
* Select a workflow based on required evidence.
* Avoid unnecessary "scan everything" configurations.
* Account for Nessus version and edition differences.
* Explain why a selected workflow matches the assessment objective.
* Recognize when no available workflow adequately answers the question.

The final test is:

> **Given a new authorized assessment request, can you identify the question, required evidence, assessment perspective, operational constraints, and appropriate Nessus workflow before touching the detailed scan settings?**

If yes, you are ready to move from simply running scans to deliberately designing assessments.
