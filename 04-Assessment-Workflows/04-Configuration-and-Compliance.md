# Configuration and Compliance

## Objective

Learn how to use Nessus for **configuration and compliance-oriented assessments** and understand how these assessments differ from ordinary vulnerability scanning.

By the end of this workflow, you should be able to:

* Distinguish vulnerability assessment from configuration/compliance assessment.
* Define a configuration or compliance question before scanning.
* Determine when authenticated access is required.
* Select an appropriate workflow based on the assessment objective.
* Understand policies, configuration checks, and compliance-oriented checks.
* Interpret failed checks without confusing them with vulnerabilities.
* Separate technical evidence from compliance conclusions.
* Investigate unexpected or ambiguous results.
* Document exceptions and limitations.
* Decide when remediation, validation, or another assessment is required.

---

# 1. What Is a Configuration and Compliance Assessment?

A vulnerability assessment primarily asks:

> What security weaknesses can be identified?

A configuration/compliance assessment asks a different question:

> Does the system satisfy a defined configuration, control, or compliance requirement?

Conceptually:

```text
AUTHORIZATION
      ↓
REQUIREMENT / BASELINE
      ↓
TARGET
      ↓
ASSESSMENT CONFIGURATION
      ↓
CHECKS
      ↓
EVIDENCE
      ↓
PASS / FAIL / OTHER RESULT
      ↓
INVESTIGATION
      ↓
REMEDIATION
      ↓
RETEST
```

The distinction is important because:

```text
Vulnerability
    ≠
Configuration Deviation
    ≠
Compliance Violation
```

These concepts can overlap, but they are not interchangeable.

---

# 2. Start With the Requirement

Do not begin by choosing a compliance template.

Begin with the requirement.

Examples:

```text
Requirement:
SSH configuration must follow the approved organizational baseline.
```

or:

```text
Requirement:
Systems in scope must satisfy specified security configuration controls.
```

or:

```text
Requirement:
A particular benchmark configuration must be assessed on authorized systems.
```

The assessment should then answer:

```text
Requirement
    ↓
Expected State
    ↓
Actual State
    ↓
Evidence
    ↓
Result
```

---

# 3. Configuration vs Compliance

These terms are related but should remain conceptually separate.

## Configuration Assessment

Focuses on the actual technical state of the system.

Examples:

* Security setting enabled or disabled.
* Service configuration.
* Password policy.
* System parameter.
* Network configuration.
* Local security control.
* Installed component configuration.

The question is:

> What is configured on the system?

---

## Compliance Assessment

Focuses on whether the system satisfies a defined requirement, benchmark, policy, or control set.

The question becomes:

> Does the observed state satisfy the applicable requirement?

Therefore:

```text
Configuration Assessment
        ↓
Technical State
        ↓
Compliance Assessment
        ↓
Requirement Comparison
```

---

# 4. Vulnerability vs Configuration Finding

Consider a server with an insecure configuration.

A vulnerability assessment might report:

```text
Potential security weakness
```

A configuration assessment might report:

```text
Required setting is not configured according to baseline
```

A compliance assessment might report:

```text
Control requirement is not satisfied
```

The same underlying system condition may appear in multiple contexts.

However, the interpretation and reporting purpose can differ.

---

# 5. Why the Assessment Question Matters

Consider these three questions:

### Question A

> Does the host expose a known remotely detectable vulnerability?

Potential workflow:

```text
Vulnerability Assessment
```

### Question B

> Is the host configured according to the approved security baseline?

Potential workflow:

```text
Configuration Assessment
```

### Question C

> Does the host satisfy a specified compliance benchmark?

Potential workflow:

```text
Compliance-Oriented Assessment
```

The target may be identical.

The assessment question is different.

---

# 6. Define the Baseline

Before running a configuration assessment, identify the expected state.

A baseline can come from:

* Organizational policy.
* Security standard.
* Approved hardening guide.
* Regulatory requirement.
* Industry benchmark.
* Technical configuration standard.
* An assessment-specific requirement.

Do not invent a baseline during result interpretation.

Write down:

```text
Baseline:
Approved security configuration standard

Scope:
Authorized Linux servers

Required Evidence:
Relevant configuration values

Assessment Objective:
Determine baseline conformity
```

---

# 7. Baseline Is Not Automatically Universal

A configuration that is appropriate for one environment may be inappropriate for another.

For example:

```text
Control:
Disable a service
```

might be reasonable for:

```text
Web server
```

but inappropriate for:

```text
System whose business function requires that service
```

Therefore:

> A failed baseline check does not automatically mean the configuration should be changed.

You must understand:

```text
Requirement
+
System Role
+
Approved Exception
+
Business Need
```

before making a remediation decision.

---

# 8. Authentication Is Often Important

Many configuration checks require access to local system state.

Therefore authenticated assessment is often necessary.

The conceptual model is:

```text
Target
  ↓
Authentication
  ↓
Configuration Evidence
  ↓
Control Evaluation
```

However, the exact authentication requirements depend on the assessment type and target.

Do not assume every compliance-oriented check requires the same account privileges.

---

# 9. Configuration and Compliance Workflow

Use:

```text
ASSESSMENT QUESTION
       ↓
REQUIREMENT
       ↓
BASELINE
       ↓
TARGET
       ↓
AUTHENTICATION
       ↓
WORKFLOW
       ↓
CHECKS
       ↓
EVIDENCE
       ↓
RESULT
       ↓
INVESTIGATION
       ↓
REMEDIATION
       ↓
RETEST
```

This differs from a normal vulnerability workflow because the **expected state** is central to the assessment.

---

# 10. Choosing the Workflow

Nessus templates, policies, compliance capabilities, and available checks can vary by:

* Nessus edition.
* Product version.
* License.
* Platform.
* Plugin availability.
* Current content.

Therefore, do not memorize a fixed UI path.

Instead:

1. Identify the requirement.
2. Determine whether the requirement is configuration-oriented or compliance-oriented.
3. Identify the supported Nessus workflow.
4. Review its scope.
5. Review required credentials.
6. Review applicable checks.
7. Confirm the resulting evidence will answer the requirement.

The exact template name is secondary to the assessment objective.

---

# 11. Configuration Assessment Preparation

Before launching, document:

```text
Assessment:
Configuration Baseline Review

Target:
Authorized server

Baseline:
Approved hardening configuration

Authentication:
Authorized assessment account

Expected Evidence:
Configuration values and control states

Success Condition:
Required controls can be evaluated

Limitations:
Approved exceptions must be reviewed separately
```

This creates a clear assessment boundary.

---

# 12. Compliance Policies and Checks

A compliance-oriented workflow may evaluate multiple controls.

Conceptually:

```text
Compliance Policy
       ↓
Control 1
Control 2
Control 3
Control 4
       ↓
Evidence
       ↓
Result
```

Each check should be understood as:

```text
Requirement
    ↓
Expected Condition
    ↓
Observed Condition
    ↓
Evaluation
```

Do not treat a policy as a mysterious collection of pass/fail buttons.

Understand what it is actually checking.

---

# 13. Read the Check Before Trusting the Result

When a control fails, ask:

1. What requirement is being evaluated?
2. What value or condition was expected?
3. What did Nessus observe?
4. What evidence supports the result?
5. Is the requirement applicable to this host?
6. Is there an approved exception?
7. Does the result require manual confirmation?

This prevents blind remediation.

---

# 14. Pass Does Not Mean "Secure"

Suppose a compliance assessment reports:

```text
Control:
PASS
```

This means that the evaluated condition satisfied that particular check.

It does **not** mean:

```text
System is secure.
```

Similarly:

```text
PASS
```

does not mean:

```text
No vulnerabilities exist.
```

A compliance assessment is bounded by its controls.

---

# 15. Fail Does Not Automatically Mean "Vulnerable"

Suppose:

```text
Control:
Password policy does not match baseline.
```

The technical condition may be a configuration deviation.

Whether it represents a vulnerability depends on the broader context.

Therefore:

```text
Compliance Failure
        ↓
Understand Requirement
        ↓
Understand Technical State
        ↓
Assess Security Impact
```

Do not collapse the categories.

---

# 16. Understand Result Categories

Depending on the workflow and content, results may include concepts such as:

* Pass.
* Fail.
* Warning.
* Not Applicable.
* Informational.
* Error.
* Unable to determine.

Exact result terminology may vary.

The important skill is to understand **why the check received its result**.

---

# 17. "Unable to Determine" Is Important

Suppose a control cannot be evaluated.

Do not automatically report:

```text
FAIL
```

and do not automatically report:

```text
PASS
```

Instead, determine why the check could not be completed.

Possible causes include:

* Authentication failure.
* Missing permissions.
* Unsupported target.
* Missing configuration evidence.
* Plugin/content limitation.
* Network issue.
* Target-specific behavior.

An unevaluated control is different from a failed control.

---

# 18. Approved Exceptions

Real environments frequently contain approved exceptions.

For example:

```text
Baseline:
Disable Service X

Target:
Service X enabled

Initial interpretation:
FAIL
```

But suppose:

```text
Approved Exception:
Service X required for documented business function.
```

The correct professional response is not automatically:

> Disable the service.

Instead:

```text
Observed State
      ↓
Baseline Deviation
      ↓
Check Exception Register
      ↓
Approved Exception?
      ↓
YES
      ↓
Document Exception
```

Compliance requires context.

---

# 19. Exceptions Are Not Findings to Hide

An exception should remain visible in the assessment record.

Document:

```text
Control:
Service configuration

Observed:
Service enabled

Baseline:
Service should be disabled

Exception:
Approved business requirement

Exception Owner:
<owner>

Expiration / Review:
<date>

Assessment Treatment:
Documented exception
```

Do not manipulate scan results simply to make compliance percentages look better.

---

# 20. False Positives and Context

A compliance check can produce a result that appears incorrect because:

* The environment uses a different configuration path.
* The requirement does not apply.
* The system role differs.
* The plugin interpretation does not match local implementation.
* The target changed after assessment.
* An exception exists.
* Required evidence was incomplete.

Investigate before labeling a result as incorrect.

Use:

```text
Observed Result
      ↓
Evidence
      ↓
Requirement
      ↓
Applicability
      ↓
Exception
      ↓
Conclusion
```

---

# 21. Configuration Drift

Configuration assessments become particularly valuable when performed repeatedly.

Conceptually:

```text
Approved Baseline
      ↓
Assessment
      ↓
Remediation
      ↓
Retest
      ↓
Approved State
      ↓
Time
      ↓
Configuration Drift
      ↓
Assessment
```

A system can become non-compliant after a change even if nobody intentionally weakened it.

Potential causes include:

* Software updates.
* Administrative changes.
* New services.
* Policy changes.
* Infrastructure changes.
* Deployment processes.
* Temporary troubleshooting.
* Configuration automation changes.

Recurring assessment helps identify this drift.

---

# 22. Baseline Changes Require Deliberate Review

Suppose a control repeatedly fails.

Before changing the system, ask:

> Is the system wrong, or has the baseline changed?

The investigation should include:

```text
Current Baseline
      ↓
Current Configuration
      ↓
System Role
      ↓
Recent Change
      ↓
Approved Exception
      ↓
Corrective Action
```

Do not permanently modify the baseline simply to eliminate failures.

---

# 23. Plugin and Content Updates

Configuration and compliance checks depend on assessment content.

Changes to plugin or compliance content can affect results.

Therefore, when comparing assessments, consider:

* Nessus version.
* Plugin/content updates.
* Policy changes.
* Baseline changes.
* Target changes.
* Credential changes.

A changed result does not necessarily mean the target changed.

The assessment itself may have changed.

---

# 24. Compare Like With Like

For meaningful comparison, record:

```text
Assessment Date
Nessus Version
Relevant Plugin / Content State
Policy / Baseline
Target
Credential Scope
Important Configuration
```

Then compare.

Conceptually:

```text
Assessment A
     ↓
Same Target?
     ↓
Same Baseline?
     ↓
Same Policy?
     ↓
Comparable Authentication?
     ↓
Comparable Content?
     ↓
Meaningful Comparison
```

---

# 25. Practical Lab 1 — Configuration Baseline

## Objective

Assess an authorized lab host against a defined configuration baseline.

## Conditions

* Authorized system.
* Defined baseline.
* Appropriate authentication.
* Non-destructive assessment.

## Tasks

1. Define the baseline.
2. Define the target.
3. Identify required credentials.
4. Choose an appropriate workflow.
5. Configure the assessment.
6. Review applicable checks.
7. Launch.
8. Review pass/fail results.
9. Investigate at least one failed control.
10. Identify the observed evidence.
11. Determine whether the failure is applicable.
12. Check for an approved exception.
13. Determine remediation or documentation action.

## Record

```text
Baseline:
Target:
Authentication:
Controls Assessed:
Passed:
Failed:
Unable to Determine:
Exceptions:
Evidence:
Remediation:
Retest Required:
```

---

# 26. Practical Lab 2 — Investigate a Failed Control

## Scenario

A control reports:

```text
FAIL
```

Do not immediately change the system.

Work through:

```text
FAIL
 ↓
What Requirement?
 ↓
What Was Expected?
 ↓
What Was Observed?
 ↓
What Evidence Supports It?
 ↓
Does It Apply?
 ↓
Is There an Exception?
 ↓
What Is the Security / Compliance Impact?
 ↓
What Action Is Appropriate?
```

Record:

```text
Control:
Requirement:
Expected:
Observed:
Evidence:
Applicability:
Exception:
Impact:
Recommended Action:
```

---

# 27. Practical Lab 3 — Pass vs Vulnerability

Find a control that passes.

Then identify whether the target still contains vulnerabilities that the configuration check does not address.

Your objective is to demonstrate:

```text
Compliance PASS
        +
Vulnerability Assessment
        ↓
Different Questions
```

Write:

```text
Control Passed:
What It Proved:
What It Did Not Prove:
Other Assessment Needed:
```

---

# 28. Practical Lab 4 — Compliance Failure With Exception

## Scenario

A control fails because the target configuration differs from the baseline.

You discover an approved exception.

Document:

```text
Control:
Observed Configuration:
Baseline Requirement:
Exception:
Exce
```
