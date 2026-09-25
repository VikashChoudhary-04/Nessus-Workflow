# Nessus Mental Model

## Objective

Build the minimum mental model required to operate Nessus intelligently.

By the end of this file, you should understand how these components relate:

```text
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
```

You do **not** need to memorize every Nessus feature or setting.

You need to understand what each part does and how a change in one part can affect the assessment.

---

## 1. What Nessus Is Doing

At a practical level, Nessus answers questions about systems.

For example:

```text
What hosts are reachable?
What services are exposed?
What vulnerabilities can be detected?
What software/configurations are present?
Can the scanner authenticate?
What security weaknesses can be identified?
```

Nessus does this by combining:

* targets
* scan configuration
* plugins
* credentials where applicable
* scanner capabilities
* network access
* collected evidence

The resulting output is a set of findings that must then be interpreted.

A Nessus finding is **not automatically the same thing as a complete security conclusion**.

---

# 2. Target

A target is the system or systems Nessus is assessing.

Examples:

```text
192.168.1.10
192.168.1.20
192.168.1.0/24
server01.example.local
```

The target defines **where** the assessment is performed.

### Important distinction

A target being technically reachable does not mean it is authorized to be scanned.

Always establish:

```text
Authorization
    ↓
Scope
    ↓
Target
```

Never reverse this order.

---

# 3. Scan Configuration

The scan configuration determines **how Nessus performs the assessment**.

It can influence things such as:

* discovery behavior
* ports and services considered
* assessment behavior
* authentication
* plugins
* reporting
* performance
* scheduling
* other scanner behavior

The important lesson is:

> **Configuration exists to answer an assessment requirement.**

Do not change settings simply because they are available.

Ask:

```text
What problem am I trying to solve?
        ↓
Which setting affects that problem?
        ↓
Do I actually need to change it?
```

If the default behavior already answers the assessment question, changing the setting may provide no benefit.

---

# 4. Plugins

Plugins are the mechanisms Nessus uses to perform specific checks and produce findings.

Conceptually:

```text
Target
   ↓
Plugin executes a check
   ↓
Evidence is collected
   ↓
Nessus evaluates the result
   ↓
Finding may be produced
```

You do not need to memorize thousands of plugin IDs.

The professional skill is knowing how to:

1. identify the relevant plugin
2. understand what it checks
3. inspect its evidence
4. understand the affected target
5. determine whether the result requires validation

### Mental model

Think of plugins as:

> **Checks Nessus can perform against the target.**

---

# 5. Credentials

Credentials allow Nessus to perform checks that require authenticated access.

Without authentication, Nessus may be limited to what it can determine remotely.

With successful authentication, Nessus may be able to inspect additional information on the target.

Conceptually:

```text
Unauthenticated
      ↓
Externally observable information

Authenticated
      ↓
Additional host-level information
      ↓
Potentially broader assessment coverage
```

This does **not** mean:

> "Authenticated scanning is always better."

The correct question is:

> **Does the assessment objective require information that authentication provides?**

For example, if the objective is to understand what an external attacker can observe, an unauthenticated assessment may be appropriate.

If the objective is to identify host-level vulnerabilities or configuration issues, authentication may be necessary.

---

# 6. Scanner

The scanner is the component that performs the assessment.

Conceptually:

```text
Your configuration
       +
Targets
       +
Plugins
       +
Credentials
       ↓
Scanner
       ↓
Assessment activity
```

If the scanner cannot reach the target, authenticate, retrieve required information, or execute the necessary checks, the final results can be affected.

Therefore:

> **A scan result is only as meaningful as the conditions under which the scan was performed.**

---

# 7. Assessment

The assessment is the actual execution of the configured workflow against the selected targets.

A useful model is:

```text
Question
   ↓
Configuration
   ↓
Execution
   ↓
Observation
```

Example:

```text
Question:
Are vulnerabilities present on this authorized host?

        ↓

Configuration:
Select appropriate vulnerability assessment
        +
Target host
        +
Required credentials

        ↓

Execution:
Run the scan

        ↓

Observation:
Review detected findings and evidence
```

The assessment is therefore not just:

> "Run a scan."

It is:

> **Execute a specific assessment designed to answer a specific question.**

---

# 8. Findings

A finding is information Nessus reports as a result of its assessment.

A finding can represent different types of information, including:

* vulnerabilities
* informational observations
* detected services
* configuration conditions
* compliance results
* other plugin output

Do not assume that every result deserves the same treatment.

When you see a finding, ask:

```text
What was detected?
        ↓
Which asset is affected?
        ↓
Why did Nessus report it?
        ↓
What evidence supports it?
        ↓
Does it require validation?
        ↓
What action should follow?
```

---

# 9. Evidence

Evidence is what supports the finding.

Depending on the check, evidence may involve information such as:

* detected software
* versions
* service information
* configuration information
* authentication results
* network observations
* plugin output
* other data collected during the assessment

The important principle is:

> **Do not treat the finding title as the complete finding.**

Investigate the underlying information.

---

# 10. Risk and Priority

Nessus provides severity information, but professional remediation decisions require context.

A useful mental model is:

```text
Finding Severity
       +
Exploitability
       +
Asset Importance
       +
Exposure
       +
Evidence
       +
Business Context
       +
Existing Controls
       ↓
Remediation Priority
```

This does not mean you need to calculate your own scoring formula for every finding.

It means:

> **Severity is an important input to a decision, not the entire decision.**

For example, two systems can have the same vulnerability while having very different operational importance.

---

# 11. Report

A report turns assessment results into useful information for other people.

A professional report should make it possible to understand:

```text
What was assessed?
What was found?
Where was it found?
How was it detected?
What evidence supports it?
How significant is it?
What should be done?
What are the assessment limitations?
```

The report should be based on the actual assessment conditions.

For example, if authentication failed, that limitation matters.

---

# 12. Remediation

Remediation is the process of addressing a finding.

Depending on the finding, remediation might involve:

* patching
* upgrading software
* changing configuration
* disabling an unnecessary service
* changing access controls
* replacing insecure settings
* applying another appropriate corrective action

Nessus identifies and provides information about weaknesses.

It does not mean Nessus should automatically make changes to the target.

The assessment and remediation activities should remain clearly understood as separate steps.

---

# 13. Retest

After remediation, the question changes.

Initially:

```text
Is the weakness present?
```

After remediation:

```text
Has the weakness actually been resolved?
```

The retest provides evidence for that second question.

Conceptually:

```text
Initial Assessment
       ↓
Finding
       ↓
Remediation
       ↓
Retest
       ↓
Compare Results
       ↓
Verify
```

A remediation claim should not automatically be treated as verified merely because someone says the issue was fixed.

---

# 14. The Complete Mental Model

Put everything together:

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
Validation
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

This is the core mental model for the entire repository.

---

# 15. Why Results Can Be Incomplete

One of the most important concepts to understand early is:

> **No finding does not always mean no vulnerability.**

Results depend on the conditions of the assessment.

For example:

```text
Target unreachable
      ↓
Limited/no assessment

OR

Credentials fail
      ↓
Authenticated checks unavailable

OR

Required service inaccessible
      ↓
Relevant checks may not execute

OR

Relevant plugin/check unavailable
      ↓
Potential coverage limitation
```

Therefore, after every important assessment, ask:

```text
Did Nessus actually have the access and information required
to answer my assessment question?
```

If the answer is uncertain, investigate before drawing conclusions.

---

# 16. Configuration Changes Have Consequences

Consider this example:

```text
You expect authenticated results.
        ↓
Authentication fails.
        ↓
You continue using the results anyway.
```

The problem is not necessarily that Nessus is broken.

The problem may be that the assessment conditions do not match the intended assessment.

Another example:

```text
You increase scan aggressiveness.
        ↓
Scan impact increases.
        ↓
The target behaves unexpectedly.
```

The correct response is not:

> "More aggressive must be better."

Instead ask:

```text
Why was the setting changed?
What did it change?
Was the change necessary?
Did it introduce unwanted impact?
```

This reasoning pattern will be used throughout the repository.

---

# 17. Decision Model

For every Nessus task, use:

```text
1. What is my objective?
2. What is in scope?
3. What information do I already have?
4. What information am I missing?
5. Which Nessus workflow can answer the question?
6. Do I need authentication?
7. What configuration actually matters?
8. What could go wrong?
9. What should I expect to see?
10. What will I do if I do not see it?
```

This is more valuable than memorizing a fixed scan configuration.

---

# 18. Practical Exercise — Explain the Workflow

Before continuing, explain the following in your own words.

### Exercise 1

Complete this chain:

```text
Target
   ↓
__________
   ↓
Plugins
   ↓
__________
   ↓
Scanner
   ↓
__________
   ↓
Findings
```

---

### Exercise 2

You run a scan against an authorized Linux host.

You expected authenticated results, but Nessus reports results consistent with an unauthenticated assessment.

What should you investigate before assuming the target has no additional vulnerabilities?

Think about:

```text
Credentials
Authentication
Permissions
Network access
Scan configuration
Evidence
```

Do not immediately rerun the scan.

Determine **why** the expected assessment condition did not occur.

---

### Exercise 3

You receive a high-severity finding.

Should you immediately conclude:

> "This is the first vulnerability that must be remediated."

Explain what additional information you would want before determining remediation priority.

---

### Exercise 4

A remediation team says:

> "The vulnerability has been fixed."

What question should your Nessus workflow answer next?

---

# 19. Decision Checkpoint

Before moving forward, you should be able to answer these without looking back:

### Question 1

What determines **where** Nessus performs an assessment?

### Question 2

What determines **how** Nessus performs an assessment?

### Question 3

Why can authentication affect assessment coverage?

### Question 4

Why should you investigate evidence instead of relying only on a finding title?

### Question 5

Why does a scan containing no findings not necessarily prove that the target is secure?

### Question 6

What is the difference between:

```text
Finding
```

and:

```text
Remediation Priority
```

### Question 7

What should happen after remediation?

---

# 20. Completion Criteria

Do not consider this file complete merely because you have read it.

You should be able to explain the following relationship without referring to this document:

```text
Target
   ↓
Configuration
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
Validation
   ↓
Priority
   ↓
Report
   ↓
Remediation
   ↓
Retest
```

You should also understand:

* why authentication can change coverage
* why configuration affects results
* why scan conditions matter
* why findings require interpretation
* why severity alone does not determine remediation priority
* why remediation must be verified
* why unexpected results trigger investigation rather than blind reruns

Once these ideas are intuitive, you are ready to learn the actual Nessus interface.
