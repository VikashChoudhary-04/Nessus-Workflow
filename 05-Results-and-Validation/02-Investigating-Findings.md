# Investigating Findings

## Objective

Learn how to investigate individual Nessus findings systematically instead of accepting scanner output at face value.

By the end of this workflow, you should be able to:

* Determine exactly what a finding represents.
* Identify the affected host, service, component, and condition.
* Understand how Nessus detected the finding.
* Examine the evidence supporting the finding.
* Distinguish direct evidence from inferred evidence.
* Determine whether the finding applies to the target.
* Investigate unexpected findings.
* Investigate missing or questionable evidence.
* Identify common root causes across multiple findings.
* Decide whether a finding needs validation.
* Document an investigation so another analyst can reproduce the reasoning.

---

# 1. Why Finding Investigation Matters

A scanner produces findings.

An analyst determines what those findings mean.

The investigation process is:

```text id="d7w4kx"
FINDING
   ↓
HOST
   ↓
COMPONENT
   ↓
DETECTION METHOD
   ↓
EVIDENCE
   ↓
APPLICABILITY
   ↓
IMPACT
   ↓
CONFIDENCE
   ↓
VALIDATION
   ↓
ACTION
```

The goal is not to challenge every finding unnecessarily.

The goal is to understand enough evidence to make a defensible decision.

---

# 2. A Finding Is a Starting Point

Suppose Nessus reports:

```text id="v9m2rp"
Severity:
High

Finding:
Example Vulnerability
```

That is not yet a complete conclusion.

You still need to determine:

```text id="x3k8qa"
Which host?
Which service?
Which component?
What evidence?
How was it detected?
Does it apply?
What is the impact?
What should happen next?
```

Therefore:

```text id="a8f5zc"
Scanner Result
     ↓
Investigation
     ↓
Analyst Conclusion
```

---

# 3. The Finding Investigation Model

Use this model for important findings:

```text id="r5n8wy"
WHAT?
 ↓
WHERE?
 ↓
HOW DETECTED?
 ↓
WHAT EVIDENCE?
 ↓
DOES IT APPLY?
 ↓
HOW IMPORTANT?
 ↓
DOES IT NEED VALIDATION?
 ↓
WHAT NEXT?
```

Each question should be answered before the finding is treated as fully understood.

---

# 4. Step 1 — What Was Found?

Start with the exact condition.

Do not rely only on the title.

Read:

* Finding title.
* Description.
* Technical details.
* Affected component.
* Detection explanation.
* Plugin output.
* References where available.

Rewrite the finding in your own words.

For example:

```text id="e3h9mq"
Nessus reported:
An outdated service component was identified.

My interpretation:
The target appears to expose a component whose observed
version may fall within a vulnerable range.
```

This forces you to understand the result rather than copy it.

---

# 5. Step 2 — Where Is It?

Identify the exact affected asset.

Record:

```text id="q7m4zn"
Host:
<IP / hostname>

Port:
<port>

Protocol:
<protocol>

Service:
<service>

Component:
<software / configuration>

Finding:
<finding>
```

A finding without a clearly understood affected asset is difficult to remediate.

---

# 6. One Finding Can Affect Multiple Assets

Some findings may appear across several hosts.

Do not assume they represent identical conditions without checking.

For example:

```text id="p4k8cv"
Host A
 └── Finding X

Host B
 └── Finding X

Host C
 └── Finding X
```

The plugin may be identical while:

* Software versions differ.
* Exposure differs.
* Network location differs.
* Authentication state differs.
* Business importance differs.

Investigate at the asset level where necessary.

---

# 7. Step 3 — How Was It Detected?

Determine the detection method.

Possible evidence sources can include:

* Network service detection.
* Version identification.
* Configuration inspection.
* Authenticated host information.
* Protocol behavior.
* Certificate information.
* Software inventory.
* Other plugin-specific evidence.

The key question is:

> What did Nessus actually observe that caused this finding?

---

# 8. Direct vs Indirect Evidence

Evidence can vary in strength.

A useful conceptual model is:

```text id="u5v8jx"
Direct Evidence
     ↓
Specific Technical Evidence
     ↓
Strong Inference
     ↓
Weak Inference
     ↓
Needs Investigation
```

Examples of stronger evidence may include:

* Direct configuration state.
* Authenticated package information.
* Specific service response.
* Explicitly observed vulnerable configuration.

Examples of more indirect evidence may include:

* Fingerprinted version.
* Service banner.
* Heuristic identification.
* Inferred product state.

The exact strength depends on the plugin.

Do not assume that every finding has identical evidentiary strength.

---

# 9. Version Detection Requires Care

A common vulnerability-detection pattern is:

```text id="b6m2yw"
Observed Product
       ↓
Observed Version
       ↓
Known Affected Version Range
       ↓
Potential Vulnerability
```

This can be useful.

But version information may not always be perfectly reliable.

Consider:

* Backported security patches.
* Vendor-specific package versions.
* Modified builds.
* Hidden or altered banners.
* Distribution-specific versioning.
* Components bundled differently.
* Configuration affecting applicability.

Therefore:

> An observed version should be interpreted in the context of the target platform and evidence available.

---

# 10. Backported Patches

This is an important investigation concept.

A package may report a version that looks old while the vendor has incorporated a security fix without changing the upstream version in the way you expect.

Conceptually:

```text id="a9c4kf"
Observed Version
      ↓
Looks Vulnerable
      ↓
Check Vendor / Distribution Context
      ↓
Security Fix May Be Backported
```

This does not mean the Nessus finding is wrong.

It means the finding may require additional verification.

---

# 11. Step 4 — Read the Plugin Output

Plugin output often provides the most useful technical evidence.

Look for:

* Observed values.
* Detected versions.
* Service information.
* Configuration values.
* Target responses.
* Authentication state.
* Other supporting observations.

Extract the evidence that matters.

Do not simply copy the entire plugin output into your report.

---

# 12. Separate Evidence From Interpretation

Suppose plugin output says:

```text id="k7v3mz"
Detected version:
X.Y.Z
```

That is evidence.

Your conclusion might be:

```text id="z8q4wp"
The observed version appears to fall within the affected
range for the referenced vulnerability.
```

Do not present your interpretation as though Nessus directly observed the entire conclusion.

Use:

```text id="r4y8bc"
Observed:
X.Y.Z

Interpretation:
Version appears affected.

Validation:
Required / Recommended
```

---

# 13. Step 5 — Determine Applicability

A vulnerability can exist in software without applying to the target's actual configuration.

Ask:

```text id="g3w7nt"
Is the affected component installed?
        ↓
Is it active?
        ↓
Is the relevant functionality enabled?
        ↓
Is the affected interface reachable?
        ↓
Does the vulnerability apply to this version/build?
        ↓
Are there compensating controls?
```

The goal is:

> Determine whether the reported condition actually applies to the assessed target.

---

# 14. Exposure Matters

A vulnerable component may have different practical significance depending on exposure.

For example:

```text id="n8v2yc"
Vulnerable Service
      ↓
Internet Accessible
```

versus:

```text id="u4k7qx"
Vulnerable Service
      ↓
Internal Network Only
```

Both may require remediation.

But the surrounding risk context differs.

Do not confuse:

```text id="c6m9jr"
Vulnerability Existence
```

with:

```text id="y3f8pd"
Exposure
```

They are separate investigation dimensions.

---

# 15. Network Position Matters

When investigating a finding, identify where the scanner was positioned.

For example:

```text id="q2w8hm"
Scanner
   ↓
Firewall
   ↓
Target
```

A finding observed from one network location does not necessarily describe every possible network path.

Record the scanner perspective when it materially affects interpretation.

---

# 16. Authentication Context Matters

For authenticated findings, determine:

```text id="p9f3xk"
Was authentication successful?
        ↓
For which host?
        ↓
What permissions existed?
        ↓
What evidence became available?
```

An authenticated finding may be based on information unavailable to an unauthenticated scanner.

Conversely, an authentication failure can explain missing evidence.

---

# 17. Step 6 — Determine Impact

Once applicability is established, understand the potential impact.

Consider:

* Confidentiality.
* Integrity.
* Availability.
* Privilege implications.
* Data exposure.
* Service exposure.
* Lateral movement implications.
* Operational impact.
* Business importance.

Do not automatically infer maximum impact from a severity label.

Use the vulnerability's documented technical characteristics plus the target context.

---

# 18. Severity vs Impact vs Priority

Keep these concepts separate.

```text id="t7x5qj"
Severity
   ↓
Technical classification

Impact
   ↓
Potential consequence

Priority
   ↓
What should be addressed first
```

Priority is addressed more fully later.

During investigation, your goal is to understand the first two well enough to support later prioritization.

---

# 19. Step 7 — Determine Confidence

Ask:

> How confident am I that this finding accurately represents the target condition?

Consider:

* Evidence quality.
* Detection method.
* Target behavior.
* Version certainty.
* Authentication state.
* Configuration context.
* Conflicting evidence.
* Recent target changes.

Use a simple internal model:

```text id="j5r8nw"
Strong Evidence
     ↓
High Confidence

Incomplete / Indirect Evidence
     ↓
Requires Investigation

Conflicting Evidence
     ↓
Validation Required
```

This is an analytical judgment, not necessarily a Nessus field.

---

# 20. Conflicting Evidence

Suppose Nessus reports:

```text id="x8d4km"
Vulnerable Version
```

but another authorized source indicates:

```text id="m7q3wp"
Security patch applied
```

Do not immediately choose one.

Investigate:

```text id="c5n9vz"
Version Evidence
      +
Patch Evidence
      +
Vendor Packaging
      +
Current Target State
      ↓
Reconciled Interpretation
```

Conflicting evidence is a reason to investigate, not a reason to ignore one source.

---

# 21. Step 8 — Decide Whether Validation Is Needed

Not every finding requires the same validation effort.

Validation becomes more useful when:

* The finding has significant impact.
* Evidence is indirect.
* Applicability is uncertain.
* The finding conflicts with other evidence.
* The finding would drive an important remediation.
* The observed version may be misleading.
* A false positive would have significant operational cost.

Use:

```text id="k4x9mp"
Finding
  ↓
Evidence Strong?
  ├── YES → May Be Sufficient
  └── NO  → Investigate / Validate
```

---

# 22. Validation Is Not Exploitation

The objective is confidence.

Use the least-impact method that answers the question.

Possible validation approaches can include:

```text id="s7w2nf"
Plugin Evidence
      ↓
Configuration Verification
      ↓
Version / Package Verification
      ↓
Service Verification
      ↓
Non-Destructive Test
      ↓
Controlled Validation
```

Do not jump directly to exploitation simply because a finding is interesting.

---

# 23. Investigating a False Positive

A finding may eventually prove inaccurate.

A professional investigation should record:

```text id="y5m8qv"
Finding:
<finding>

Why It Appeared:
<scanner evidence>

Contradictory Evidence:
<evidence>

Investigation:
<steps>

Conclusion:
<confirmed / not applicable / false positive / unresolved>

Reason:
<technical explanation>
```

This is much more useful than simply marking:

```text id="x8v4kj"
False Positive
```

---

# 24. Investigating a Suspected False Negative

The same discipline applies when a finding is missing.

Ask:

```text id="f8m3qa"
Expected Condition
       ↓
Target Reachability
       ↓
Service Availability
       ↓
Authentication
       ↓
Plugin Applicability
       ↓
Plugin Execution
       ↓
Evidence
```

Possible conclusion:

```text id="c2r7md"
Not Detected
```

does not automatically mean:

```text id="a7k9pz"
Not Present
```

Document the uncertainty.

---

# 25. Related Findings

Multiple findings can originate from the same root cause.

Example:

```text id="q6t8wr"
Outdated Component
      ├── Vulnerability A
      ├── Vulnerability B
      └── Vulnerability C
```

Investigate:

> What single remediation could address multiple findings?

This is more useful operationally than treating every finding independently.

---

# 26. Root Cause Analysis

Use:

```text id="v4n7xm"
Findings
   ↓
Common Component?
   ↓
Common Configuration?
   ↓
Common Administrative Cause?
   ↓
Root Cause
   ↓
Remediation
```

Possible root causes include:

* Unsupported software.
* Missing updates.
* Incorrect configuration.
* Weak access controls.
* Unnecessary services.
* Deployment inconsistency.
* Configuration drift.
* Credential or permission problems.

Root cause analysis helps remediation teams work efficiently.

---

# 27. Finding Dependencies

Some findings may depend on another condition.

Conceptually:

```text id="z9c5vk"
Configuration Weakness
        ↓
Service Exposure
        ↓
Vulnerability
```

Or:

```text id="h3m8qx"
Outdated Component
        ↓
Multiple Vulnerabilities
```

Understanding dependencies helps prevent incorrect remediation.

---

# 28. Avoid Double Counting

Suppose five findings are all caused by:

```text id="b7p4nr"
One outdated software component
```

The report may legitimately contain five findings.

But remediation planning should recognize:

```text id="c9x6mt"
5 Findings
      ↓
1 Root Cause
      ↓
1 Remediation Workstream
```

This improves operational clarity.

---

# 29. Investigating High-Impact Findings

For a significant finding, use a deeper process:

```text id="m8q4zs"
Finding
 ↓
Affected Asset
 ↓
Affected Service
 ↓
Evidence
 ↓
Applicability
 ↓
Exposure
 ↓
Impact
 ↓
Confidence
 ↓
Validation
 ↓
Remediation
 ↓
Retest
```

Do not skip directly from:

```text id="v6p2nx"
High / Critical
```

to:

```text id="r3y7km"
Remediate immediately
```

without understanding the finding.

---

# 30. Investigating Findings Across Multiple Hosts

When the same plugin appears on multiple hosts, compare:

| Host   | Service | Evidence | Authentication | Exposure | Notes |
| ------ | ------- | -------- | -------------- | -------- | ----- |
| Host A |         |          |                |          |       |
| Host B |         |          |                |          |       |
| Host C |         |          |                |          |       |

Ask:

* Is the underlying condition identical?
* Are versions identical?
* Are configurations identical?
* Are exposure paths identical?
* Did authentication succeed everywhere?
* Are the remediation steps the same?

This prevents broad assumptions.

---

# 31. Practical Lab 1 — Complete Finding Investigation

Choose one significant finding.

Follow:

```text id="h7w3mp"
Finding
 ↓
Host
 ↓
Service
 ↓
Plugin
 ↓
Evidence
 ↓
Applicability
 ↓
Impact
 ↓
Confidence
 ↓
Validation
 ↓
Action
```

Record:

```text id="k9r5vd"
Finding:
Affected Host:
Affected Service:
Plugin:
Detection Method:
Evidence:
Applicability:
Exposure:
Impact:
Confidence:
Validation:
Recommended Action:
```

---

# 32. Practical Lab 2 — Version-Based Finding

Choose a version-related finding.

Investigate:

1. Observed product.
2. Observed version.
3. Target platform.
4. Package/vendor context.
5. Vulnerable version range.
6. Patch status if available.
7. Configuration context.
8. Whether validation is needed.

Record:

```text id="s3q8yf"
Observed Product:
Observed Version:
Target Platform:
Vendor / Distribution:
Reported Vulnerability:
Evidence:
Potential Complication:
Validation Method:
Conclusion:
```

This exercise is particularly useful for learning why version-based findings deserve context.

---

# 33. Practical Lab 3 — False Positive Investigation

Choose a finding that appears questionable in an authorized lab.

Investigate it without modifying the target unnecessarily.

Record:

```text id="n6p2xa"
Finding:
Initial Evidence:
Why It Appeared Questionable:
Additional Evidence:
Investigation:
Final Conclusion:
Reason:
```

The objective is not to prove Nessus wrong.

The objective is to learn how to investigate uncertainty.

---

# 34. Practical Lab 4 — Root Cause Analysis

Choose several findings from one host.

Create:

| Finding | Component | Common Cause? | Possible Shared Remediation |
| ------- | --------- | ------------- | --------------------------- |
|         |           |               |                             |
|         |           |               |                             |
|         |           |               |                             |

Then determine:

```text id="r4k8wv"
Multiple Findings
      ↓
Common Root Cause
      ↓
Shared Remediation
      ↓
Retest
```

---

# 35. Practical Lab 5 — Cross-Host Investigation

Select the same finding across at least two authorized hosts.

Compare:

```text id="q8m4dz"
Host A
 ↓
Evidence

Host B
 ↓
Evidence

Compare
 ↓
Same Condition?
```

Determine whether the same remediation should apply to both.

Document any differences.

---

# 36. Practical Lab 6 — Missing Finding Investigation

Choose an expected vulnerability that did not appear.

Investigate:

```text id="p7w5kc"
Expected Finding
       ↓
Target Reachability
       ↓
Service
       ↓
Authentication
       ↓
Plugin Applicability
       ↓
Plugin Execution
       ↓
Available Evidence
```

Final result should be one of:

* Condition appears absent.
* Condition may exist but was not detectable.
* Assessment coverage was insufficient.
* Additional validation is required.
* Target state changed.
* Investigation remains unresolved.

Do not force certainty.

---

# 37. Professional Finding Investigation Record

Use:

```text id="v5n8rq"
Finding ID / Plugin:
Assessment:
Date:
Host:
Port:
Protocol:
Service:
Component:
Authentication State:
Detection Method:
Evidence:
Applicability:
Exposure:
Technical Impact:
Confidence:
Related Findings:
Potential Root Cause:
Validation Performed:
Validation Result:
Remediation:
Retest Required:
Limitations:
Final Interpretation:
```

This becomes valuable evidence for:

* Reporting.
* Remediation tracking.
* Peer review.
* Retesting.
* Dispute resolution.

---

# 38. Common Mistakes

## Mistake 1 — Accepting the Finding Title as the Conclusion

The title is only the beginning.

---

## Mistake 2 — Ignoring the Affected Component

You need to know what actually requires attention.

---

## Mistake 3 — Ignoring Plugin Evidence

Evidence is central to understanding why the finding exists.

---

## Mistake 4 — Treating Version Detection as Absolute

Vendor packaging and backported patches can complicate interpretation.

---

## Mistake 5 — Treating Severity as Proof

Severity does not establish applicability.

---

## Mistake 6 — Treating Every Finding as Independent

Related findings can share a root cause.

---

## Mistake 7 — Calling Something a False Positive Without Investigation

A label is not an explanation.

---

## Mistake 8 — Treating Missing Findings as Proof of Absence

Detection depends on visibility and applicable checks.

---

## Mistake 9 — Validating Destructively

Use the least-impact method that provides sufficient confidence.

---

## Mistake 10 — Forgetting Authentication State

Authenticated and unauthenticated evidence are not interchangeable.

---

# 39. Decision Rule

For every important finding:

```text id="n4q7xs"
WHAT WAS FOUND?
      ↓
WHERE?
      ↓
HOW WAS IT DETECTED?
      ↓
WHAT EVIDENCE SUPPORTS IT?
      ↓
DOES IT APPLY?
      ↓
WHAT IS THE EXPOSURE?
      ↓
WHAT IS THE TECHNICAL IMPACT?
      ↓
HOW CONFIDENT ARE WE?
      ↓
DOES IT NEED VALIDATION?
      ↓
WHAT IS THE ROOT CAUSE?
      ↓
WHAT SHOULD HAPPEN NEXT?
```

If evidence conflicts:

```text id="p8k2mz"
Conflicting Evidence
       ↓
Investigate
       ↓
Validate
       ↓
Reconcile
       ↓
Document
```

If evidence is insufficient:

```text id="q6m9wt"
Insufficient Evidence
       ↓
Do Not Overstate
       ↓
Investigate or Validate
```

---

# 40. Finding Investigation Checklist

## Identification

* [ ] Finding identified.
* [ ] Host identified.
* [ ] Port/protocol identified.
* [ ] Service identified.
* [ ] Component identified.

## Detection

* [ ] Plugin identified.
* [ ] Detection method understood.
* [ ] Plugin output reviewed.
* [ ] Supporting evidence extracted.

## Applicability

* [ ] Component exists.
* [ ] Component is relevant.
* [ ] Vulnerability applies to observed version/build.
* [ ] Configuration considered.
* [ ] Exposure considered.
* [ ] Authentication state considered.

## Analysis

* [ ] Technical impact understood.
* [ ] Evidence strength considered.
* [ ] Confidence assessed.
* [ ] Related findings identified.
* [ ] Root cause considered.

## Validation

* [ ] Validation need considered.
* [ ] Validation authorized.
* [ ] Least-impact method selected.
* [ ] Validation result documented.

## Closure

* [ ] Remediation identified.
* [ ] Retest requirement identified.
* [ ] Limitations documented.
* [ ] Final interpretation recorded.

---

# 41. Completion Criteria

You have completed this workflow when you can independently:

* Investigate a Nessus finding from title to evidence.
* Identify the exact affected asset and component.
* Explain how the plugin detected the condition.
* Distinguish evidence from interpretation.
* Assess the reliability of available evidence.
* Recognize complications in version-based detection.
* Investigate applicability.
* Consider network and authentication context.
* Assess technical impact.
* Identify related findings.
* Perform basic root-cause analysis.
* Investigate suspected false positives.
* Investigate missing findings.
* Decide when validation is required.
* Document an investigation so another analyst can understand the reasoning.
* Avoid overstating what the scanner actually established.

The final test is:

> Given a Nessus finding that could affect an important system, can you independently determine what was detected, why Nessus reported it, what evidence supports it, whether it applies, how confident you are, whether validation is necessary, and what the appropriate next action is?

If yes, you can investigate Nessus findings rather than simply reading them.
