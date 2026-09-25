# Policies, Templates, and Plugins

## Objective

Learn how Nessus templates, policies, and plugins fit into assessment design, and how to select and control assessment coverage without blindly enabling everything.

By the end of this file, you should be able to:

* Explain the difference between a template, policy, and plugin.
* Understand how reusable configuration relates to an individual assessment.
* Understand that plugins are the actual security checks behind many Nessus findings.
* Choose plugin coverage based on the assessment objective.
* Understand plugin families at a practical level.
* Recognize the risks of indiscriminately enabling or disabling plugins.
* Understand how plugin updates can affect assessment results.
* Troubleshoot unexpected results by reviewing plugin configuration.
* Build reproducible assessment configurations.
* Explain why a particular template, policy, or plugin configuration was selected.

---

# 1. Three Concepts to Separate

Three Nessus concepts are easy to confuse:

```text id="m8s3r1"
TEMPLATE
   ↓
Starting point for an assessment

POLICY
   ↓
Reusable assessment configuration

PLUGIN
   ↓
Individual security check / detection logic
```

They operate at different levels.

A useful mental model is:

```text id="r4v7n2"
Assessment Objective
       ↓
Template / Workflow
       ↓
Policy / Configuration
       ↓
Plugin Selection
       ↓
Execution
       ↓
Findings
```

The exact behavior and availability of these objects can vary by Nessus version and edition.

---

# 2. What Is a Template?

A template is a predefined starting point for creating a particular type of assessment.

Conceptually:

```text id="q1d7x9"
Template
   ↓
Initial Assessment Structure
   ↓
Customize if necessary
   ↓
Run Assessment
```

Templates help you avoid building every assessment configuration from zero.

But:

> **A template is a starting point, not a substitute for assessment reasoning.**

---

# 3. Choosing a Template

Do not choose a template because:

* Its name sounds advanced.
* It contains more options.
* Someone else always uses it.
* It appears first in the UI.

Choose it because:

> **Its intended purpose matches your assessment question.**

Use:

```text id="j5c2k8"
Question
   ↓
Required Evidence
   ↓
Workflow
   ↓
Template
```

---

# 4. What Is a Policy?

A policy is a reusable configuration that can define how assessments should be performed.

Depending on your Nessus version and edition, a policy may contain configuration such as:

* Discovery behavior.
* Assessment behavior.
* Plugin selections.
* Credentials.
* Performance-related options.
* Other scan configuration.

The exact capabilities vary.

Think of a policy as:

> **A reusable assessment configuration designed to produce consistent execution.**

---

# 5. Why Policies Matter

Imagine an organization repeatedly assesses similar Linux servers.

Without reusable configuration:

```text id="a3f8w6"
Assessment 1
Configure manually

Assessment 2
Configure manually

Assessment 3
Configure manually
```

This can create inconsistency.

With a carefully maintained policy:

```text id="y2m6d4"
Approved Policy
      ↓
Assessment 1
Assessment 2
Assessment 3
```

This improves repeatability.

But a policy should be reviewed before reuse.

---

# 6. Policy Reuse Does Not Remove Review

A common mistake is:

> "This policy worked last month, so I can use it forever."

Environments change.

Review:

```text id="e7n4v2"
Objective
↓
Targets
↓
Credentials
↓
Plugins
↓
Performance
↓
Safety
↓
Version / Edition
```

before important assessments.

A reusable configuration is useful only while it remains appropriate.

---

# 7. Template vs Policy

Use this practical distinction:

| Concept    | Main Purpose                              |
| ---------- | ----------------------------------------- |
| Template   | Starting point for creating an assessment |
| Policy     | Reusable assessment configuration         |
| Plugin     | Individual check/detection mechanism      |
| Assessment | Actual execution against targets          |

Think:

```text id="s7h3k9"
Template
   ↓
Create Assessment

Policy
   ↓
Reusable Configuration

Plugins
   ↓
Checks Performed

Assessment
   ↓
Execution
```

Do not treat these as interchangeable terms.

---

# 8. What Is a Plugin?

A plugin is a piece of Nessus detection logic used to identify or assess a particular condition.

Conceptually:

```text id="v6p2a8"
Target
   ↓
Plugin
   ↓
Check
   ↓
Evidence
   ↓
Finding
```

Plugins are the mechanisms through which Nessus performs many individual security checks.

You do not need to memorize thousands of plugin IDs.

You need to understand how plugin selection affects assessment coverage.

---

# 9. Plugin Families

Plugins are organized into logical families/categories.

Depending on your Nessus version and plugin set, these can represent areas such as:

* Operating systems.
* Databases.
* Web servers.
* Network devices.
* Applications.
* Services.
* Configuration.
* Malware.
* Compliance.
* Other technologies or security checks.

The exact categories and available plugins change over time.

---

# 10. Plugin Selection by Objective

Start with:

```text id="p7n4c1"
Assessment Objective
      ↓
Relevant Technology
      ↓
Required Checks
      ↓
Plugin Coverage
```

For example:

### Web Server Assessment

Relevant coverage may include:

* Web server checks.
* Web application/service checks.
* Operating-system checks where appropriate.
* Related network/service checks.

### Database Assessment

Relevant coverage may include:

* Database-specific checks.
* Operating-system checks where appropriate.
* Configuration/security checks supported by the environment.

The exact plugin coverage should be determined from the available Nessus content and assessment objective.

---

# 11. Do Not Treat Plugin Count as Quality

Suppose:

```text id="t6h2r9"
Scan A → 5,000 plugins
Scan B → 2,000 plugins
```

You cannot conclude that Scan A is automatically better.

The relevant questions are:

* What objective did each scan have?
* Which technologies were present?
* Which plugins were relevant?
* Was authentication available?
* Were there operational constraints?
* Were the plugins current?
* Did the assessment actually provide the required evidence?

The goal is appropriate coverage.

Not the largest number.

---

# 12. Enabling Everything

The "enable everything" strategy can create problems.

Potential consequences include:

* Longer scan duration.
* More network activity.
* Increased target load.
* More irrelevant checks.
* Greater troubleshooting complexity.
* More difficult result interpretation.

More checks can be useful when they are relevant.

They are not automatically useful simply because they exist.

---

# 13. Disabling Plugins

The opposite mistake is disabling large groups without understanding the consequences.

For example:

```text id="q4c7m1"
Disable entire family
       ↓
Faster scan
       ↓
Potentially missing important checks
```

Before disabling a plugin or family, ask:

> "What assessment coverage am I intentionally giving up?"

If you cannot answer that, do not disable it.

---

# 14. Plugin Dependencies and Interactions

Some checks depend on information obtained by earlier discovery or detection steps.

Conceptually:

```text id="k3r8b2"
Discover Service
      ↓
Identify Technology
      ↓
Run Relevant Checks
      ↓
Generate Finding
```

Therefore, disabling one category can sometimes affect the usefulness of later checks.

The exact behavior is plugin- and version-dependent.

This is another reason to make controlled changes.

---

# 15. Plugin Updates

Nessus plugins are updated over time.

Updates can introduce:

* New vulnerability checks.
* Updated detection logic.
* Improved evidence.
* Changed plugin behavior.
* New technology coverage.
* Retired or modified checks.

Therefore:

> **The same scan configuration can produce different results at different points in time.**

The environment may be identical while the detection content has changed.

---

# 16. Plugin Version Awareness

When investigating a significant result difference, consider:

```text id="f9n3a2"
Target Changed?
       ↓
Configuration Changed?
       ↓
Credentials Changed?
       ↓
Environment Changed?
       ↓
Plugin Content Changed?
```

Do not assume that a changed result must have been caused by the target.

---

# 17. Plugin Updates and Reproducibility

For important assessments, record enough context to explain the scan.

For example:

```text id="m7w4d1"
Assessment Date:
Nessus Version:
Edition:
Plugin Update State:
Workflow:
Policy:
Target:
Credentials:
Important Plugin Configuration:
```

The exact plugin/version reporting available depends on your product version.

---

# 18. Plugin Families vs Individual Plugins

For most assessment design, start with plugin families/categories.

Example:

```text id="d5s8x2"
Question:
Assess an authorized Linux server

↓ 

Relevant Technology:
Linux

↓

Plugin Coverage:
Linux / Unix-related checks
+ Relevant network/service checks
+ Other objective-specific checks
```

Only move to individual plugin investigation when you have a specific reason.

Examples:

* Investigating one finding.
* Troubleshooting a missing detection.
* Validating plugin behavior.
* Understanding false positives.
* Comparing scan behavior.

---

# 19. When to Inspect an Individual Plugin

Inspect a specific plugin when:

* A finding needs investigation.
* You need to understand detection logic.
* A plugin produces unexpected evidence.
* You suspect a false positive.
* You suspect a missed detection.
* You need to understand affected products/versions.
* You are validating whether a plugin applies to your target.

Do not inspect thousands of plugins simply to become familiar with Nessus.

Learn through real assessment questions.

---

# 20. Plugin IDs

Plugin IDs can be useful for precise investigation.

A plugin identifier can help you:

* Locate a specific check.
* Compare the same check across assessments.
* Investigate plugin output.
* Search documentation.
* Discuss a finding with another assessor.

But:

> **Plugin ID memorization is not a professional skill.**

The important skill is understanding what the plugin did and what evidence it produced.

---

# 21. Plugin Output

When investigating a plugin finding, examine:

```text id="x8v4p7"
Plugin Identity
      ↓
Description
      ↓
Affected Target
      ↓
Evidence / Output
      ↓
Severity
      ↓
References
      ↓
Remediation
```

Plugin output is particularly important because it can explain why Nessus reported the condition.

---

# 22. Policy Design Principle

A reusable policy should represent a deliberate assessment strategy.

Document:

```text id="u7q2m8"
Policy Name:
Purpose:
Assessment Type:
Target Type:
Authentication:
Plugin Coverage:
Performance:
Important Exclusions:
Known Limitations:
Owner:
Review Date:
```

Do not create policies merely to increase the number of saved objects.

---

# 23. Naming Policies

Use descriptive names.

Bad:

```text id="c4j7n2"
scan1
newscan
test
policy2
```

Better:

```text id="a6r3k9"
Linux-Authenticated-VA
External-Web-Assessment
Network-Discovery-Lab
```

The exact naming convention is up to you.

The important property is clarity.

---

# 24. Naming Templates and Assessments

Names should answer:

> "What is this for?"

For example:

```text id="m1x8c5"
External-Web-Assessment
```

is more useful than:

```text id="q2z7p4"
Scan-01
```

Good names make scan history easier to understand.

---

# 25. Policy Versioning

When a reusable policy changes materially, record the change.

Example:

```text id="p8v3s6"
Policy:
External-Web-Assessment

Version:
2

Change:
Added relevant web-service plugin coverage

Reason:
Assessment objective expanded

Date:
YYYY-MM-DD
```

The exact versioning mechanism can simply be documentation if Nessus does not provide the workflow you want.

---

# 26. Policy Drift

A policy can slowly become different from the original purpose.

Example:

```text id="j4m8x2"
Original:
External Web Assessment

Later changes:
+ Internal checks
+ Credentials
+ Database plugins
+ Aggressive performance
+ Miscellaneous exclusions
```

Eventually the policy no longer clearly represents the original assessment.

Avoid this by reviewing reusable configurations periodically.

---

# 27. Templates, Policies, and Customization

Use this decision process:

```text id="k6v9d3"
Start With Template
        ↓
Does It Match Objective?
        │
   ┌────┴────┐
  YES       NO
   │          │
Continue    Choose another
   │          │
   ↓          ↓
Apply appropriate policy/configuration
        ↓
Review
        ↓
Launch
```

Do not assume a template must be used unchanged.

---

# 28. Configuration Reuse vs Configuration Copying

Reuse is valuable when:

* The objective is repeated.
* The environment is similar.
* The assessment method is stable.
* The policy is maintained.

Copying without review is dangerous when:

* The target changes.
* The objective changes.
* Authentication changes.
* Plugins change.
* Operational constraints change.

Always validate the copied configuration against the new assessment.

---

# 29. Practical Exercise 1 — Identify the Three Layers

Open your Nessus installation.

Locate examples of:

```text id="w7c4a1"
Template:
____________________

Policy:
____________________

Plugin:
____________________
```

Then explain:

```text id="x1m5k8"
What does each one control?
When would I use it?
How do they relate?
```

Do not worry if the exact UI labels differ.

The goal is conceptual recognition.

---

# 30. Practical Exercise 2 — Choose a Template

Take this assessment request:

> "Assess an authorized Linux lab server for vulnerabilities visible without host credentials."

Determine:

```text id="r2d7n5"
Objective:
Perspective:
Target:
Authentication:
Workflow:
Appropriate Template:
Why:
```

Then create the assessment without unnecessarily modifying unrelated settings.

---

# 31. Practical Exercise 3 — Create a Reusable Policy

In your authorized lab, design a reusable configuration for:

> "Authenticated vulnerability assessment of Linux lab servers."

Document:

```text id="e6p3y9"
Policy Name:
Purpose:
Target Type:
Authentication:
Plugin Coverage:
Performance:
Important Exclusions:
Known Limitations:
```

Do not include credentials or secrets.

---

# 32. Practical Exercise 4 — Plugin Family Investigation

Choose one technology present in your lab.

For example:

```text id="b5w8r2"
Linux
Windows
Web Server
Database
```

Identify relevant plugin categories.

Record:

```text id="g2x6m4"
Technology:
Relevant Plugin Family:
Why Relevant:
Example Plugin:
What It Checks:
Evidence It Produces:
```

The goal is understanding, not memorizing IDs.

---

# 33. Practical Exercise 5 — Controlled Plugin Change

In an isolated authorized lab:

1. Run a baseline assessment.
2. Record important findings.
3. Change one relevant plugin category.
4. Run the assessment again.
5. Compare the result.

Record:

```text id="z8q3m7"
Baseline Coverage:
Changed Coverage:
Expected Difference:
Actual Difference:
Findings Affected:
Interpretation:
```

This demonstrates how plugin selection affects evidence.

---

# 34. Practical Exercise 6 — Plugin Investigation

Choose one finding from your lab.

Record:

```text id="s4n6v1"
Plugin ID:
Plugin Name:
Affected Host:
Finding:
Evidence:
Detection Logic / Basis:
References:
Remediation:
```

Then answer:

> "What information did this plugin use to produce the finding?"

This prepares you for the results-analysis section later.

---

# 35. Practical Exercise 7 — Policy Drift

Take your reusable lab policy.

Intentionally imagine three changes:

```text id="n5v8k2"
Change 1:
Authentication added

Change 2:
New plugin category added

Change 3:
Performance settings changed
```

Ask:

> "Does this policy still represent its original purpose?"

If not, redesign or split the policy.

---

# 36. Common Mistakes

## Mistake 1 — Treating Templates as Magic

Bad:

> "The template automatically guarantees the right assessment."

Better:

> "The template provides a starting point that must still be reviewed."

---

## Mistake 2 — Enabling Every Plugin

Bad:

> "More plugins always means more security."

Better:

> "Plugin coverage should match the assessment objective and operational constraints."

---

## Mistake 3 — Disabling Plugin Families for Speed

Bad:

> "This makes the scan faster, so it is better."

Better:

> "The speed improvement must be weighed against the coverage lost."

---

## Mistake 4 — Ignoring Plugin Updates

Bad:

> "The configuration is identical, so the results must be directly comparable."

Better:

> "Plugin content is part of the assessment context."

---

## Mistake 5 — Reusing Policies Blindly

Bad:

> "This policy worked before."

Better:

> "This policy is reviewed against the current objective and environment."

---

## Mistake 6 — Memorizing Plugin IDs Instead of Understanding Findings

Bad:

> "I know plugin 12345."

Better:

> "I understand what the plugin checks, what evidence it uses, and what the finding means."

---

# 37. Troubleshooting Unexpected Plugin Results

If a finding is missing or unexpected, investigate:

```text id="c7m2q8"
Correct Target?
      ↓
Correct Workflow?
      ↓
Correct Credentials?
      ↓
Relevant Plugin Family Enabled?
      ↓
Plugin Current?
      ↓
Plugin Applicable?
      ↓
Target Evidence Available?
      ↓
Network Conditions?
      ↓
Plugin Output?
```

This gives you a structured path instead of random configuration changes.

---

# 38. Missing Finding

If you expected Nessus to detect a condition but it did not, ask:

1. Was the target reachable?
2. Was the relevant service discovered?
3. Was the relevant plugin enabled?
4. Was the plugin applicable?
5. Was authentication required?
6. Did authentication succeed?
7. Did the target state change?
8. Did plugin content change?
9. Is the expected detection actually supported?

Do not immediately label the result a Nessus failure.

---

# 39. Unexpected Finding

If Nessus reports something unexpected:

1. Open the finding.
2. Identify the plugin.
3. Read the description.
4. Examine evidence/output.
5. Determine the detection basis.
6. Check whether the target matches the affected condition.
7. Determine whether validation is required.

The plugin is an important starting point for understanding why Nessus reported the condition.

---

# 40. Plugin Configuration Record

For important assessments:

```text id="h4v7n9"
## Plugin Configuration

### Objective
[Assessment question]

### Workflow
[Workflow]

### Plugin Families
[Relevant families]

### Included Coverage
[Important inclusions]

### Excluded Coverage
[Important exclusions]

### Reason for Exclusions
[Why]

### Plugin Update Context
[Relevant information]

### Important Limitations
[Known limitations]
```

Do not claim that a plugin category is available unless it is actually present in your Nessus installation.

---

# 41. Professional Decision Rule

Before finalizing plugin configuration, complete:

> **"I am including this plugin coverage because it is relevant to __________. I am excluding this coverage because __________. The expected effect on the assessment is __________."**

If you cannot explain the decision, reconsider it.

---

# 42. Reproducibility Checklist

For an important assessment:

```text id="u9k3p6"
[ ] Nessus version recorded
[ ] Edition recorded
[ ] Template/workflow recorded
[ ] Policy recorded
[ ] Target recorded
[ ] Credentials context recorded
[ ] Important plugin configuration recorded
[ ] Important exclusions recorded
[ ] Performance changes recorded
[ ] Assessment date recorded
[ ] Significant limitations recorded
```

---

# 43. Completion Criteria

You have completed this file when you can independently:

* Explain the difference between templates, policies, and plugins.
* Choose a template based on an assessment objective.
* Explain the purpose of reusable policies.
* Explain what plugins do.
* Use plugin families to reason about assessment coverage.
* Avoid treating plugin count as a measure of quality.
* Explain the risks of indiscriminate plugin inclusion or exclusion.
* Understand why plugin updates can affect results.
* Investigate individual plugins when necessary.
* Recognize policy drift.
* Maintain reproducible configuration records.
* Troubleshoot missing or unexpected findings using plugin context.

The final test is:

> **Given an authorized assessment request, can you choose an appropriate workflow/template, determine whether a reusable policy is suitable, understand the relevant plugin coverage, justify important inclusions or exclusions, and explain how those choices affect the evidence Nessus can produce?**

If yes, you are ready to learn how to turn these configurations into **repeatable scheduled assessments**.
