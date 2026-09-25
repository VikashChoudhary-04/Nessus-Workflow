# Assessment Documentation

## Objective

Learn how to create complete, accurate, reproducible documentation for a Nessus assessment.

Professional assessment documentation should allow another qualified operator to understand:

* Why the assessment was performed
* What was authorized
* What was assessed
* How it was assessed
* Which scanner was used
* Which credentials or authentication perspective were involved
* What configuration was used
* What actually happened during execution
* What was covered
* What was not covered
* Which findings were identified
* How important findings were investigated
* Which findings were validated
* How findings were prioritized
* What remediation was requested
* What was retested
* What remains unresolved
* What should happen next

The objective is not to create paperwork for its own sake.

The objective is:

> **To preserve enough accurate context that the assessment can be understood, defended, compared, repeated, and acted upon.**

---

# 1. The Documentation Mental Model

Think of assessment documentation as the evidence trail connecting the assessment lifecycle.

```text id="5j6z0m"
AUTHORIZATION
      ↓
SCOPE
      ↓
OBJECTIVE
      ↓
WORKFLOW
      ↓
CONFIGURATION
      ↓
EXECUTION
      ↓
COVERAGE
      ↓
RESULTS
      ↓
INVESTIGATION
      ↓
VALIDATION
      ↓
PRIORITIZATION
      ↓
REPORT
      ↓
REMEDIATION
      ↓
RETEST
      ↓
VERIFICATION
```

Every major transition should leave enough evidence to explain what happened.

---

# 2. Why Documentation Matters

Good documentation supports:

* Reproducibility
* Accountability
* Troubleshooting
* Result comparison
* Remediation tracking
* Retesting
* Auditability
* Team handoff
* Historical analysis
* Management communication
* Incident investigation where relevant

Poor documentation creates uncertainty.

For example:

```text id="8g1d5n"
"Scan found 20 vulnerabilities."
```

does not answer:

* What was scanned?
* When?
* From where?
* With which scanner?
* Authenticated or unauthenticated?
* Which configuration?
* Which plugin/content state?
* Was coverage complete?
* Were findings validated?
* What does "20" represent?

A professional record answers these questions.

---

# 3. Documentation Is Not the Same as Reporting

These are related but different.

### Assessment documentation

Captures the complete operational and technical record.

### Professional report

Communicates the important results to the intended audience.

Use:

```text id="s4t7q2"
Assessment Documentation
        ↓
Evidence / Analysis
        ↓
Professional Report
```

The report should be derived from the assessment record.

Do not let the report become the only record of what happened.

---

# 4. The Documentation Lifecycle

Use this model:

```text id="h7m0x8"
PLAN
  ↓
RECORD
  ↓
EXECUTE
  ↓
UPDATE
  ↓
ANALYZE
  ↓
REPORT
  ↓
REMEDIATE
  ↓
RETEST
  ↓
CLOSE
```

Documentation should happen during the assessment, not entirely afterward.

---

# 5. Start Documentation Before the Scan

Before launching Nessus, record:

```text id="p8v3n2"
Authorization
Scope
Objective
Assessment question
Target set
Exclusions
Scanner
Scanner position
Workflow
Credentials
Configuration
Assessment window
Operational constraints
Stop conditions
Expected coverage
```

This creates the baseline against which actual execution can be compared.

---

# 6. Authorization Record

A professional authorization record should establish:

```text id="q5r9m1"
Who authorized the assessment?
What systems are authorized?
What activity is authorized?
When is it authorized?
What systems are excluded?
What restrictions apply?
```

Example:

```text id="f8c2d4"
Authorization source:
<approved authorization>

Authorized scope:
<approved target set>

Excluded scope:
<approved exclusions>

Assessment window:
<approved window>

Permitted workflow:
<vulnerability assessment / discovery / etc.>

Restrictions:
<operational constraints>
```

Do not invent authorization details.

---

# 7. Scope Record

Scope documentation should be precise enough to determine whether an asset belongs in the assessment.

Record:

* IP addresses
* Hostnames
* FQDNs
* Ranges
* CIDRs
* Applications where applicable
* Exclusions
* Network boundaries
* Scanner position
* Assessment window

Use the exact approved scope where possible.

---

# 8. Scope Changes

If scope changes, preserve the original state.

Record:

```text id="m7p1k5"
Original scope:
<scope>

Change:
<what changed>

Reason:
<reason>

Requested by:
<authorized requester>

Approval:
<approval>

Effective time:
<time>

Assessment impact:
<impact>
```

Do not overwrite the original scope without preserving the change history.

---

# 9. Assessment Objective

Document the actual question.

Weak:

```text id="b8n5x1"
Objective:
Run Nessus scan.
```

Better:

```text id="r4q6z8"
Objective:
Identify vulnerabilities observable from the authorized
internal scanner position against the specified application
servers and determine which findings require validation
and remediation.
```

The objective should explain why the assessment exists.

---

# 10. Assessment Success Criteria

Define what a successful assessment means.

Examples:

```text id="g1y7p4"
All authorized targets assessed
+
Expected services evaluated
+
Required authentication achieved
+
Relevant vulnerability coverage available
+
Important findings investigated
+
Limitations documented
```

Success criteria should reflect the actual assessment objective.

---

# 11. Workflow Selection Record

Document:

```text id="w3j8c0"
Selected workflow:
<workflow>

Reason:
<assessment objective>

Required evidence:
<evidence>

Alternative considered:
<alternative>

Why not selected:
<reason>
```

This demonstrates that workflow selection was deliberate.

---

# 12. Target Record

Document the target set used by Nessus.

Example:

| Target | Type     | Expected | Observed | Notes |
| ------ | -------- | -------- | -------- | ----- |
|        | IP       |          |          |       |
|        | Hostname |          |          |       |
|        | CIDR     |          |          |       |

If dynamic targets are involved, record the relevant behavior.

---

# 13. Scanner Record

Record:

```text id="a9k2x6"
Scanner:
Name/identifier:
Nessus edition:
Nessus version:
Operating environment:
Network position:
Source network:
Assessment date:
```

Scanner identity matters for reproducibility.

---

# 14. Scanner Position

Document where the scanner sits relative to the target.

For example:

```text id="c7w4m2"
Scanner:
Internal security segment

Target:
Application server subnet

Path:
Security segment → firewall → application subnet
```

Do not assume scanner position is obvious from the target IP.

---

# 15. Credential Record

Never record secrets.

Record only what is needed to understand the assessment perspective.

Example:

```text id="v2h8n4"
Authentication:
Enabled

Method:
<authorized method>

Account:
<assessment account>

Privilege level:
<required/approved level>

Authentication result:
Successful / Partial / Failed

Coverage impact:
<description>

Secret:
Not recorded
```

---

# 16. Configuration Record

Document the important assessment configuration.

At minimum consider:

```text id="p6m0t3"
Target
Discovery
Assessment
Credentials
Plugins/content
Performance
Advanced settings
Scheduling
```

Do not necessarily record every UI field if it has no relevance.

Focus on configuration that affects:

* Coverage
* Safety
* Reproducibility
* Result interpretation

---

# 17. Configuration Minimality

Do not create documentation that is impossible to maintain.

The objective is not:

```text id="f3g7n8"
Record every button ever clicked
```

The objective is:

```text id="m8r2q4"
Record decisions and settings that materially affect
assessment behavior, safety, coverage, or interpretation.
```

This keeps documentation useful.

---

# 18. Configuration Snapshot

For repeatable assessments, maintain a configuration snapshot.

Example:

```text id="k4z7x1"
Assessment:
Internal Server Vulnerability Assessment

Targets:
<approved target set>

Workflow:
Authenticated vulnerability assessment

Discovery:
<configuration>

Assessment:
<configuration>

Credentials:
<method / account reference only>

Plugins/content:
<relevant state>

Performance:
<relevant settings>

Advanced:
<meaningful deviations from default>

Scanner:
<scanner>

Version:
<version>

Date:
<date>
```

Never store passwords or private keys.

---

# 19. Pre-Launch Checklist Record

Before launching, document the review.

```text id="y7p3c9"
Authorization:
Confirmed

Scope:
Confirmed

Targets:
Confirmed

Exclusions:
Confirmed

Scanner:
Available

Credentials:
Configured / Not required

Authentication:
Expected behavior confirmed

Configuration:
Reviewed

Impact:
Reviewed

Stop conditions:
Confirmed

Assessment window:
Confirmed
```

This creates evidence that the assessment was intentionally prepared.

---

# 20. Execution Record

During execution, capture meaningful events.

Record:

```text id="h2x5q8"
Launch time
Assessment state
Major errors
Authentication behavior
Target reachability issues
Operational impact
Scan pause/stop
Configuration changes
Scope changes
Unexpected events
Completion time
```

Do not attempt to record every insignificant UI event.

Record events that can affect interpretation.

---

# 21. Decision Log

A decision log is especially useful for unusual assessments.

Use:

| Time | Observation | Decision | Reason | Operator |
| ---- | ----------- | -------- | ------ | -------- |
|      |             |          |        |          |
|      |             |          |        |          |
|      |             |          |        |          |

Examples:

```text id="w8n3d7"
Observation:
Two targets became unreachable.

Decision:
Continue assessment.

Reason:
Remaining targets were unaffected and stop conditions were
not triggered.

Impact:
Two targets require follow-up assessment.
```

This preserves operational reasoning.

---

# 22. Coverage Record

After execution, document actual coverage.

Use:

```text id="e3m9r5"
Intended Targets:
<value>

Assessed Targets:
<value>

Missing Targets:
<value>

Expected Services:
<value>

Observed Services:
<value>

Authentication:
<status>

Plugin Coverage:
<status>

Major Limitations:
<limitations>
```

Coverage documentation is one of the most important parts of the assessment record.

---

# 23. Coverage Is Not the Same as Completion

Record separately:

```text id="x7j4p2"
Assessment Status:
Completed
```

and:

```text id="b9m6k3"
Coverage:
8 of 10 authorized hosts assessed
```

This prevents an execution status from being mistaken for assessment completeness.

---

# 24. Result Record

For each meaningful finding, record:

```text id="r5k8w1"
Finding:
<name>

Affected asset:
<asset>

Service:
<service>

Severity:
<severity>

Plugin:
<plugin identifier if relevant>

Evidence:
<evidence>

Applicability:
<analysis>

Impact:
<technical/environmental impact>

Validation:
<status>

Root cause:
<root cause>

Priority:
<priority rationale>

Remediation:
<action>

Retest:
<status>
```

Do not copy plugin output blindly into the final report.

Interpret it.

---

# 25. Evidence vs Interpretation

This distinction is fundamental.

### Evidence

What was actually observed.

Example:

```text id="d4x7p9"
Observed software version:
X.Y.Z
```

### Interpretation

What that observation means.

Example:

```text id="m1q8v3"
The detected version may be affected by the
identified vulnerability, subject to vendor
patch/backport verification.
```

Do not present interpretation as direct observation.

---

# 26. Confidence

Not every finding has the same evidence strength.

Document uncertainty where relevant.

Possible states:

```text id="s8n2k4"
Strong evidence
```

```text id="h6r4p1"
Supporting evidence
```

```text id="t7m3c9"
Indirect evidence
```

```text id="q2w8j5"
Conflicting evidence
```

```text id="v5d9x1"
Insufficient evidence
```

Use terminology appropriate to the organization's reporting standards.

---

# 27. Validation Record

For significant findings, document:

```text id="n8f3k7"
Finding:
<finding>

Reason validation was needed:
<reason>

Validation method:
<method>

Evidence obtained:
<evidence>

Result:
Confirmed / Not Applicable / False Positive /
Unresolved / Condition Changed

Impact on conclusion:
<impact>
```

Do not claim validation that was not performed.

---

# 28. Root Cause Record

Where related findings share a common cause, document it.

Example:

```text id="u5c8p2"
Root Cause:
Outdated shared software component

Affected Assets:
Server A
Server B
Server C

Related Findings:
<findings>

Recommended Remediation:
Upgrade the shared component to an approved version.

Verification:
Retest affected population.
```

This produces more useful remediation guidance than a long list of disconnected plugin findings.

---

# 29. Prioritization Record

Document why a finding received its priority.

Example:

```text id="z6k1m8"
Finding:
<finding>

Validity:
Supported

Asset:
<asset context>

Exposure:
<external/internal/restricted>

Technical impact:
<impact>

Exploitability:
<known evidence>

Business context:
<provided context>

Compensating controls:
<controls>

Priority rationale:
<reason>
```

Do not invent business information.

---

# 30. Remediation Record

A remediation record should answer:

```text id="j4r7x3"
What is wrong?
What is the root cause?
What should change?
Who owns the change?
What is the expected result?
When should it be completed?
How will it be verified?
```

Example:

| Field           | Value |
| --------------- | ----- |
| Finding         |       |
| Root cause      |       |
| Affected assets |       |
| Remediation     |       |
| Owner           |       |
| Expected result |       |
| Target date     |       |
| Retest method   |       |
| Status          |       |

---

# 31. Retest Record

Document:

```text id="m9v2c6"
Original finding:
<finding>

Original evidence:
<evidence>

Remediation:
<action>

Retest date:
<date>

Retest target:
<target>

Retest configuration:
<relevant configuration>

Retest result:
<result>

Comparison:
<before vs after>

Verification:
<verified state>
```

A retest should be reproducible enough for another operator to understand what changed.

---

# 32. Verification Record

Do not stop at:

> "Retest completed."

Record:

```text id="p3x8n5"
Expected state:
<expected>

Observed state:
<observed>

Evidence:
<evidence>

Conclusion:
<verified state>
```

This distinguishes execution from verification.

---

# 33. Handling Unresolved Findings

Unresolved is a legitimate result.

Example:

```text id="k7w4m1"
Finding:
Potential vulnerability

Evidence:
Version-based detection

Limitation:
Vendor backport status could not be verified

Validation:
Not conclusive

Status:
Unresolved

Next action:
Obtain vendor/package evidence
```

Do not force an unresolved finding into:

```text id="z2c9q6"
Confirmed
```

or:

```text id="y8f3m5"
False Positive
```

without evidence.

---

# 34. Handling False Positives

If a finding is determined to be a false positive, document why.

Record:

```text id="d6p1r8"
Finding:
<finding>

Original evidence:
<evidence>

Contradictory evidence:
<evidence>

Validation:
<method>

Conclusion:
False positive

Reason:
<evidence-supported reason>
```

A false-positive conclusion should be evidence-based.

---

# 35. Handling Not Applicable Findings

A finding may not apply because the required condition does not exist.

Example:

```text id="c5m9q2"
Finding:
Vulnerability affecting component X

Target:
Server A

Observed:
Component X is not installed.

Conclusion:
Not applicable.
```

Document the evidence supporting the conclusion.

---

# 36. Handling Changed Conditions

Sometimes the original finding was valid, but the target changed before validation.

Example:

```text id="h1x6p4"
Original:
Service X exposed

Current:
Service X removed

Conclusion:
Condition changed

Verification:
Current service inventory confirms removal
```

This is different from claiming the original finding was false.

---

# 37. Assessment Limitations

Every professional assessment should document meaningful limitations.

Examples:

* Unreachable hosts
* Authentication failures
* Permission limitations
* Excluded systems
* Network filtering
* Scanner placement
* Incomplete scan
* Unsupported target technology
* Missing plugin/content coverage
* Short assessment window
* Target changes during execution

Use:

```text id="s3n8k5"
Limitation:
<description>

Cause:
<cause>

Affected scope:
<scope>

Impact:
<impact>

Follow-up:
<action>
```

---

# 38. Documentation of Negative Results

A negative result requires context.

Avoid:

> "No vulnerabilities found."

Prefer:

> No vulnerabilities were identified within the authorized targets and assessment coverage achieved during the assessment window.

If coverage was incomplete:

> No findings were identified on the successfully assessed hosts; two authorized hosts were unreachable and remain unverified.

The wording should reflect actual evidence.

---

# 39. Executive Summary Documentation

The executive summary should answer:

* Why was the assessment performed?
* What was assessed?
* What were the major observations?
* What are the most important remediation themes?
* What limitations matter?
* What should happen next?

Avoid overwhelming executive readers with:

* Plugin IDs
* Raw scan output
* Long technical command sequences
* Every informational result

The executive summary should be concise and evidence-based.

---

# 40. Technical Summary

A technical audience may need:

* Scope
* Scanner
* Scanner position
* Workflow
* Authentication
* Configuration
* Coverage
* Findings
* Evidence
* Validation
* Remediation
* Retest status
* Limitations

Technical documentation should allow engineers to act on findings.

---

# 41. Documentation by Audience

Use layered documentation:

```text id="x4p8m7"
Executive
   ↓
What matters?
What is affected?
What needs action?

Security / Risk
   ↓
What is the risk?
How confident are we?
What remains open?

Technical
   ↓
What is affected?
What evidence exists?
How should it be fixed?
How is it verified?
```

The underlying assessment record should remain more detailed than any single audience-facing report.

---

# 42. Documentation Naming

Use consistent naming.

Example:

```text id="q7m2v4"
2026-09-25_Internal-Vulnerability-Assessment
```

or an organization-approved naming convention.

Include enough information to distinguish assessments.

Do not include sensitive secrets in filenames.

---

# 43. Assessment Versioning

For recurring or evolving assessments, track meaningful versions.

Example:

```text id="j6c9p1"
Assessment:
Internal Vulnerability Assessment

Version:
2026-09-25

Changes from previous:
- Added two application servers
- Updated credential reference
- Scanner moved to internal security segment
```

This helps explain result differences.

---

# 44. Configuration Change History

Maintain a change record:

| Date | Change | Reason | Approved By | Result |
| ---- | ------ | ------ | ----------- | ------ |
|      |        |        |             |        |
|      |        |        |             |        |
|      |        |        |             |        |

This is especially valuable when result behavior changes unexpectedly.

---

# 45. Evidence Preservation

Preserve evidence needed to support important conclusions.

Depending on the assessment, this may include:

* Nessus result exports
* Finding details
* Plugin output
* Configuration records
* Authentication status
* Target evidence
* Validation evidence
* Remediation evidence
* Retest results
* Decision logs

Follow organizational retention and security requirements.

---

# 46. Evidence Integrity

When evidence matters for audit or formal reporting, preserve enough context to establish:

```text id="r4n7c2"
What?
When?
Where?
How?
By whom?
Under which assessment conditions?
```

For example:

```text id="b6x1m9"
Finding:
<finding>

Target:
<target>

Assessment:
<assessment ID>

Date:
<date>

Scanner:
<scanner>

Evidence:
<evidence>
```

---

# 47. Avoid Over-Documentation

Documentation can become counterproductive if it contains huge amounts of irrelevant detail.

Avoid recording:

* Every UI click
* Every unchanged default
* Every routine observation
* Duplicate copies of the same evidence
* Sensitive secrets
* Irrelevant personal information

Use the rule:

> **Document what another qualified operator would need to understand, reproduce, troubleshoot, defend, or act on the assessment.**

---

# 48. Avoid Under-Documentation

The opposite problem is also common.

Weak:

```text id="y8f2q6"
Scan completed.
20 findings.
```

Better:

```text id="s1m7c4"
Authenticated vulnerability assessment completed against
the authorized application server population from the
internal scanner position.

Nine of ten authorized hosts were assessed successfully.
One host was unreachable and remains unverified.

Authenticated coverage was successful for seven hosts;
two hosts returned limited authentication evidence.

Significant findings were investigated and validated where
required. Remediation priorities were based on technical
impact, exposure, asset context, and available business
information.
```

The second record explains what actually happened.

---

# 49. Documentation Quality Test

Ask another operator:

> "Could you understand what happened without asking me?"

If the answer is no, identify the missing information.

The record should allow reconstruction of:

```text id="w5c9n2"
Why
↓
What
↓
How
↓
Where
↓
When
↓
Evidence
↓
Decision
↓
Outcome
```

---

# 50. Documentation Handoff Test

Give the assessment record to another qualified operator.

Ask them to determine:

1. What was authorized?
2. What was scanned?
3. What was excluded?
4. Which scanner was used?
5. From where?
6. Which workflow was selected?
7. Was authentication used?
8. What coverage was achieved?
9. What findings were important?
10. Which were validated?
11. What remediation was requested?
12. What was retested?
13. What remains open?

If they cannot answer, improve the documentation.

---

# 51. Practical Lab 1 — Document a Complete Assessment

## Objective

Create a complete assessment record for an authorized lab assessment.

## Procedure

Perform a normal Nessus assessment.

Document:

```text id="c8x4m1"
Authorization
Scope
Objective
Workflow
Targets
Scanner
Scanner position
Credentials
Configuration
Execution
Coverage
Findings
Validation
Prioritization
Remediation
Retest
Closure
```

Do not store secrets.

## Success Condition

Another person can reconstruct the assessment lifecycle from your documentation.

---

# 52. Practical Lab 2 — Document an Incomplete Assessment

## Objective

Practice documenting limitations honestly.

## Scenario

Your assessment produces:

```text id="h5r2q8"
10 authorized hosts
8 assessed
2 unreachable
```

## Task

Document:

* Original scope
* Actual coverage
* Missing targets
* Cause if known
* Impact
* Findings from assessed targets
* Required follow-up

## Success Condition

The record does not imply that all ten hosts were assessed.

---

# 53. Practical Lab 3 — Document Authentication Failure

## Objective

Practice documenting authentication problems without exposing secrets.

## Scenario

An authenticated assessment is configured.

Results:

```text id="p7x3m9"
Host A → authentication successful
Host B → authentication failed
Host C → authentication successful but limited permissions
```

## Task

Document:

* Authentication method
* Target populations
* Authentication states
* Evidence
* Coverage impact
* Troubleshooting performed
* Retest
* Remaining limitations

Never record the actual password.

---

# 54. Practical Lab 4 — Document a Finding Validation

## Objective

Create a defensible finding record.

## Procedure

Select an important authorized lab finding.

Document:

```text id="m2q8c5"
Finding
Asset
Service
Detection method
Evidence
Applicability
Reason validation was needed
Validation method
Validation evidence
Conclusion
Impact
Remediation
Retest plan
```

## Success Condition

Another operator can understand why the finding was considered supported, unresolved, or not applicable.

---

# 55. Practical Lab 5 — Document a Remediation and Retest

## Objective

Track a finding from detection through closure.

## Workflow

```text id="n6v1x8"
Finding
↓
Investigation
↓
Validation
↓
Remediation
↓
Retest
↓
Verification
```

Record evidence at every meaningful stage.

## Success Condition

You can demonstrate exactly why the final state was assigned.

---

# 56. Practical Lab 6 — Documentation Handoff

## Objective

Test documentation quality.

## Procedure

1. Complete an authorized assessment.
2. Document it.
3. Give the record to another learner/operator.
4. Do not verbally explain the assessment.
5. Ask them to reconstruct:

   * Scope
   * Objective
   * Workflow
   * Scanner
   * Authentication
   * Coverage
   * Findings
   * Limitations
   * Remediation
   * Retest
6. Record what they could not determine.

## Success Condition

The documentation stands on its own.

---

# 57. Practical Lab 7 — Documentation Review

## Objective

Perform a quality review before finalizing an assessment.

Use this sequence:

```text id="z5k7p3"
Accuracy
↓
Completeness
↓
Evidence
↓
Consistency
↓
Clarity
↓
Confidentiality
↓
Actionability
```

Check whether:

* Facts are accurate.
* Important information is present.
* Conclusions are evidence-supported.
* Sections do not contradict each other.
* Language is clear.
* Sensitive information is protected.
* Findings have actionable next steps.

---

# 58. Professional Assessment Documentation Template

Use this as a master template.

```text id="r8m3q6"
==================================================
ASSESSMENT IDENTIFICATION
==================================================

Assessment:
Assessment ID:
Version:
Date:
Operator:
Assessment owner:
Technical owner:
Reference:

==================================================
AUTHORIZATION
==================================================

Authorization source:
Authorized by:
Authorized scope:
Excluded scope:
Assessment window:
Permitted activity:
Operational restrictions:
Stop conditions:

==================================================
OBJECTIVE
==================================================

Assessment objective:
Assessment question:
Required evidence:
Success criteria:

==================================================
TARGETS
==================================================

Target definition:
Expected hosts:
Expected services:
Actual hosts:
Actual services:
Missing hosts:
Unexpected hosts:
Scope changes:

==================================================
SCANNER
==================================================

Scanner:
Edition:
Version:
Operating environment:
Network position:
Source network:
Relevant routing/segmentation:

==================================================
WORKFLOW
==================================================

Assessment type:
Workflow selected:
Reason:
Alternative considered:

==================================================
CREDENTIALS
==================================================

Authentication:
Method:
Account:
Privilege level:
Authentication result:
Coverage impact:

Secret values:
NOT RECORDED

==================================================
CONFIGURATION
==================================================

Target configuration:
Discovery:
Assessment:
Plugins/content:
Performance:
Advanced settings:
Scheduling:
Meaningful deviations from baseline:

==================================================
PRE-LAUNCH REVIEW
==================================================

Authorization:
Scope:
Targets:
Credentials:
Configuration:
Scanner:
Impact:
Stop conditions:
Assessment window:

==================================================
EXECUTION
==================================================

Launch time:
Completion/stop time:
Duration:
Assessment state:

Execution observations:
Errors:
Operational events:
Scope changes:
Configuration changes:
Decisions:

==================================================
COVERAGE
==================================================

Target coverage:
Host coverage:
Service coverage:
Authentication coverage:
Plugin/content coverage:
Known limitations:

==================================================
RESULTS
==================================================

Finding:
Affected asset:
Service:
Severity:
Plugin/reference:
Evidence:
Applicability:
Technical impact:
Environmental context:
Validation:
Root cause:
Priority:
Remediation:

==================================================
FINDING STATUS
==================================================

Supported findings:
False positives:
Not applicable:
Unresolved:
Condition changed:
Other:

==================================================
REMEDIATION
==================================================

Finding:
Root cause:
Remediation:
Owner:
Expected result:
Target date:
Status:

==================================================
RETEST
==================================================

Retest date:
Retest target:
Retest configuration:
Retest result:
Before/after comparison:
Verification evidence:

==================================================
LIMITATIONS
==================================================

Limitation:
Cause:
Affected scope:
Impact:
Follow-up:

==================================================
FINAL STATUS
==================================================

Assessment conclusion:
Residual findings:
Open issues:
Exceptions:
Required follow-up:
Closure status:

==================================================
DOCUMENTATION REVIEW
==================================================

Accuracy checked:
Coverage checked:
Evidence checked:
Confidentiality checked:
Consistency checked:
Reviewer:
Review date:
```

---

# 59. Documentation Security

Assessment documentation itself can become sensitive.

Protect:

* Internal IP addresses
* Hostnames
* Architecture information
* Vulnerability details
* Configuration data
* Credentials metadata
* Network paths
* Security control information
* Internal system names
* Assessment reports

Never publish confidential assessment records to a public GitHub repository.

This repository should contain **learning documentation and sanitized examples**, not real client or production assessment data.

---

# 60. Sanitizing Examples

When documenting real-world lessons for public use, replace:

```text id="b5m8x1"
10.20.15.43
```

with:

```text id="q4z7n2"
10.10.10.10
```

and:

```text id="x9k3c6"
prod-db-01.company.internal
```

with:

```text id="m7p2r8"
server01.example.local
```

Do not copy:

* Real credentials
* Real API keys
* Real tokens
* Private keys
* Internal customer data
* Confidential vulnerability reports
* Sensitive architecture diagrams
* Proprietary assessment output

into public training material.

---

# 61. Documentation Consistency

Use the same terminology throughout the assessment.

For example, if the assessment record calls a target:

```text id="c2v6m8"
Application Server A
```

do not later call it:

```text id="h4x9p1"
Server Alpha
```

unless both names are explicitly mapped.

Consistency makes comparison and review easier.

---

# 62. Dates and Time

Use unambiguous dates and times.

Record:

```text id="y6r3k8"
Date:
2026-09-25

Time:
09:30 IST

Timezone:
IST / UTC+05:30
```

This matters when correlating:

* Scan events
* Security alerts
* System changes
* Remediation
* Retesting
* Incident timelines

---

# 63. Documenting Unknowns

A professional operator does not fill gaps with guesses.

Use:

```text id="q9m2x7"
Unknown
```

or:

```text id="c4v8n1"
Not verified
```

or:

```text id="h7p3r5"
Not provided
```

depending on the situation.

Then identify whether the unknown requires follow-up.

---

# 64. Documenting Assumptions

Sometimes assumptions are unavoidable during planning.

Label them.

Example:

```text id="m8x1q6"
Assumption:
The supplied target list represents the current
authorized application server population.

Verification required:
Asset owner confirmation before launch.
```

Never allow an assumption to appear later as a confirmed fact.

---

# 65. Documentation Review Questions

Before finalizing an assessment record, ask:

### Scope

> Can I prove what was authorized?

### Objective

> Can I explain what question the assessment answered?

### Method

> Can I explain why this workflow was selected?

### Execution

> Can I reconstruct what happened?

### Coverage

> Can I explain what was and was not assessed?

### Findings

> Can I support important findings with evidence?

### Validation

> Can I explain how uncertainty was resolved?

### Prioritization

> Can I explain why remediation order was selected?

### Remediation

> Can I identify what should change?

### Retest

> Can I prove whether the condition changed?

### Closure

> Can I identify what remains open?

---

# 66. Documentation Quality Gate

Use this final gate:

```text id="p4x7m2"
ACCURATE?
   ↓
COMPLETE?
   ↓
REPRODUCIBLE?
   ↓
EVIDENCE-SUPPORTED?
   ↓
CONSISTENT?
   ↓
CONFIDENTIALITY-PROTECTED?
   ↓
ACTIONABLE?
   ↓
READY
```

If the answer is "no" at any important stage, correct the record before finalizing it.

---

# 67. Common Documentation Mistakes

## Mistake 1 — Documenting only findings

Why it fails:

It hides scope, methodology, coverage, and limitations.

Better:

> Document the complete assessment lifecycle.

---

## Mistake 2 — Recording only scan completion

Why it fails:

Completion does not prove coverage.

Better:

> Record actual coverage.

---

## Mistake 3 — Copying raw plugin output as the report

Why it fails:

Plugin output is evidence, not automatically professional interpretation.

Better:

> Interpret evidence and explain applicability.

---

## Mistake 4 — Hiding uncertainty

Why it fails:

It creates unsupported confidence.

Better:

> Record unresolved limitations explicitly.

---

## Mistake 5 — Recording secrets

Why it fails:

It creates unnecessary security exposure.

Better:

> Record credential metadata, never secret values.

---

## Mistake 6 — Overwriting scope changes

Why it fails:

Historical context disappears.

Better:

> Preserve the original state and record changes.

---

## Mistake 7 — Treating missing findings as remediation

Why it fails:

The finding may disappear because assessment visibility changed.

Better:

> Document the reason for the change.

---

## Mistake 8 — Inventing business context

Why it fails:

Security documentation becomes misleading.

Better:

> Mark unavailable context as unknown and obtain it from the appropriate owner.

---

## Mistake 9 — Mixing facts and conclusions

Why it fails:

Readers cannot tell what was observed versus interpreted.

Better:

```text id="x3n8v5"
Evidence
+
Interpretation
+
Confidence
```

---

## Mistake 10 — Publicly exposing real assessment data

Why it fails:

Assessment data can reveal sensitive infrastructure and vulnerabilities.

Better:

> Sanitize examples and keep real assessment records in approved systems.

---

# 68. Decision Rule

When documenting any Nessus assessment, ask:

```text id="j7m4q2"
1. What was authorized?
2. What was the objective?
3. What was in scope?
4. What was excluded?
5. Which workflow was selected?
6. Why was it selected?
7. Which scanner was used?
8. From where?
9. Were credentials used?
10. What configuration mattered?
11. What happened during execution?
12. What was actually covered?
13. What findings were identified?
14. What evidence supports them?
15. What was validated?
16. What remains uncertain?
17. How were findings prioritized?
18. What remediation was requested?
19. What was retested?
20. What was verified?
21. What remains open?
22. What should happen next?
```

If another qualified operator can answer these questions from the assessment record, the documentation is serving its purpose.

---

# 69. Completion Criteria

You have completed this workflow when you can independently:

* Create an assessment record before execution.
* Document authorization and scope accurately.
* Convert an assessment request into a measurable objective.
* Record workflow-selection reasoning.
* Document scanner identity and network position.
* Record authentication without exposing secrets.
* Capture meaningful configuration details.
* Record execution events and decisions.
* Document actual assessment coverage.
* Distinguish completion from coverage.
* Record findings with evidence.
* Separate evidence from interpretation.
* Document uncertainty and confidence.
* Record validation activities.
* Group findings by root cause.
* Document prioritization rationale.
* Track remediation.
* Record retest conditions and results.
* Verify remediation.
* Preserve limitations.
* Handle unknown information honestly.
* Maintain useful change history.
* Produce documentation that another operator can understand without verbal explanation.
* Protect sensitive assessment information.
* Create sanitized public examples safely.

The final skill is not:

> **"I can write a Nessus report."**

It is:

> **"I can create an accurate, evidence-supported, reproducible assessment record that explains what was authorized, what was done, what was observed, what was not covered, why conclusions were reached, what changed, and what should happen next."**

---

# Final Documentation Mental Model

Keep this model:

```text id="w1c7m9"
AUTHORIZATION
      ↓
SCOPE
      ↓
OBJECTIVE
      ↓
WORKFLOW
      ↓
TARGET
      ↓
SCANNER
      ↓
CREDENTIALS
      ↓
CONFIGURATION
      ↓
PRE-LAUNCH RECORD
      ↓
EXECUTION RECORD
      ↓
COVERAGE RECORD
      ↓
RESULTS
      ↓
EVIDENCE
      ↓
VALIDATION
      ↓
PRIORITIZATION
      ↓
REMEDIATION
      ↓
RETEST
      ↓
VERIFICATION
      ↓
LIMITATIONS
      ↓
FINAL STATUS
      ↓
FOLLOW-UP
```

The professional question is no longer:

> **"Did I write down the scan results?"**

It is:

> **"Could another qualified operator understand, reproduce, challenge, defend, and continue this assessment from the evidence and decisions captured in my documentation?"**
