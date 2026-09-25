# Comparing and Tracking Results

## Objective

Learn how to compare Nessus assessments over time and determine what actually changed.

By the end of this workflow, you should be able to:

* establish a meaningful baseline assessment
* determine whether two assessments are genuinely comparable
* identify new, recurring, resolved, and reappeared findings
* distinguish remediation from scope or configuration changes
* recognize assessment drift
* use result history to track remediation
* avoid misleading conclusions based only on finding counts
* determine when a comparison is invalid or requires qualification
* document changes in a way that supports remediation and retesting

---

## 1. Why Compare Assessments?

A single assessment tells you about a target at one point in time.

A comparison tells you how that state changed.

For example:

```text
Assessment A
    ↓
Findings identified
    ↓
Remediation
    ↓
Assessment B
    ↓
What changed?
```

The important question is not:

> "Did the number of findings go down?"

The better question is:

> "What changed in the assessed security state, and why?"

A lower finding count can result from:

* successful remediation
* hosts being removed from scope
* services no longer being reachable
* authentication failing
* plugins changing
* scanner position changing
* configuration changing
* assessment coverage changing
* findings being reclassified
* target inventory changing

Therefore:

```text
Finding Count Change
        ≠
Security State Change
```

A comparison is useful only when you understand what changed between the assessments.

---

# 2. The Comparison Mental Model

Use this model:

```text
REFERENCE ASSESSMENT
        ↓
COMPARABILITY CHECK
        ↓
TARGET / SCOPE CHECK
        ↓
WORKFLOW CHECK
        ↓
AUTHENTICATION CHECK
        ↓
CONFIGURATION CHECK
        ↓
PLUGIN / CONTENT CHECK
        ↓
RESULT COMPARISON
        ↓
INVESTIGATE CHANGES
        ↓
CLASSIFY CHANGE
        ↓
REMEDIATION DECISION
        ↓
RETEST / FOLLOW-UP
```

The comparison itself is not the conclusion.

It is evidence used to reach a conclusion.

---

# 3. Establish a Baseline

A baseline is an assessment selected as the reference point for future comparison.

For example:

```text
Baseline
2026-09-01
        ↓
Remediation
        ↓
Retest
2026-09-15
```

The baseline should represent a meaningful state.

Possible baselines include:

* initial assessment
* approved security baseline
* pre-remediation assessment
* post-remediation assessment
* recurring assessment reference point
* assessment after a major infrastructure change

Record why the assessment was selected as the baseline.

Example:

```text
Baseline Purpose:
Initial vulnerability assessment before remediation activity.
```

Do not assume that the oldest assessment is automatically the best baseline.

---

# 4. Determine Whether the Assessments Are Comparable

Before comparing results, ask:

```text
Are these assessments measuring approximately the same thing?
```

Check the following.

| Comparison Factor   | Questions                                             |
| ------------------- | ----------------------------------------------------- |
| Scope               | Are the same authorized targets included?             |
| Target state        | Are the same systems/services present?                |
| Scanner             | Was the same or equivalent scanner used?              |
| Network position    | Was the scanner operating from a comparable location? |
| Workflow            | Were the assessments designed for the same objective? |
| Authentication      | Was authentication configured and successful in both? |
| Credentials         | Were equivalent credentials/permissions used?         |
| Plugins             | Was relevant plugin/content coverage comparable?      |
| Nessus version      | Did the platform/version change?                      |
| Configuration       | Were important scan settings comparable?              |
| Policies            | Was the same or equivalent policy used?               |
| Timing              | Could environmental changes explain differences?      |
| Target availability | Were systems reachable in both assessments?           |

If major variables changed, the comparison may still be useful, but the difference must be qualified.

---

# 5. Compare Like With Like

A strong comparison tries to preserve the assessment conditions.

Conceptually:

```text
Same Scope
   +
Same Objective
   +
Comparable Configuration
   +
Comparable Authentication
   +
Comparable Coverage
   +
Comparable Target State
   =
More Meaningful Comparison
```

This does not mean every assessment must be identical.

It means you must understand differences before interpreting the results.

---

# 6. Scope Comparison

Start with scope.

Ask:

```text
Did the same assets exist in both assessments?
```

Example:

### Assessment A

```text
10.10.10.10
10.10.10.11
10.10.10.12
10.10.10.13
```

### Assessment B

```text
10.10.10.10
10.10.10.11
10.10.10.12
```

The missing host may explain part of the reduction in findings.

Therefore:

```text
Fewer Findings
        ↓
Check Scope
        ↓
Host Removed?
        ↓
Possible Scope Effect
```

Do not automatically classify the reduction as remediation.

---

# 7. Target Inventory Changes

Targets can change over time.

Examples:

* new server added
* server retired
* IP address changed
* hostname changed
* service removed
* service added
* cloud resource replaced
* development system promoted to production
* infrastructure migrated

Therefore, compare the target inventory before comparing individual findings.

Use:

```text
Target Inventory A
        ↓
Target Inventory B
        ↓
Added Assets
Removed Assets
Changed Assets
Unchanged Assets
```

This prevents infrastructure changes from being mistaken for remediation.

---

# 8. Compare Hosts First

A useful order is:

```text
Assessment
    ↓
Hosts
    ↓
Services
    ↓
Findings
```

Do not immediately compare thousands of findings.

First determine:

* which hosts are present in both
* which hosts are new
* which hosts disappeared
* which hosts changed significantly
* which hosts were unreachable
* which hosts were only partially assessed

This gives context to later finding changes.

---

# 9. Compare Services and Components

A host may remain in scope while its attack surface changes.

For example:

### Previous

```text
80/tcp   HTTP
443/tcp  HTTPS
22/tcp   SSH
```

### Current

```text
443/tcp  HTTPS
22/tcp   SSH
```

Port 80 disappearing may represent:

* intentional service removal
* firewall filtering
* service failure
* scanner visibility change
* configuration change

Do not automatically interpret the change as remediation.

Investigate why it changed.

---

# 10. Finding States

For tracking purposes, findings can be conceptually classified as:

```text
NEW
RECURRING
RESOLVED
REAPPEARED
CHANGED
UNRESOLVED
```

These categories describe change between assessments.

They should not be confused with Nessus severity levels.

---

## 10.1 New Finding

A finding is considered new when it appears in the current assessment but was not present in the reference assessment.

Example:

```text
Baseline:
CVE-A
CVE-B

Current:
CVE-A
CVE-B
CVE-C
```

`CVE-C` is a candidate new finding.

But investigate why it appeared.

Possible explanations:

* newly introduced vulnerability
* newly installed software
* newly exposed service
* plugin update
* improved detection
* changed configuration
* previously incomplete assessment

---

## 10.2 Recurring Finding

A recurring finding remains present across assessments.

```text
Baseline:
CVE-A

Current:
CVE-A
```

This indicates that the condition still exists.

However, confirm that:

* the same host is being assessed
* the relevant service remains present
* the finding represents the same condition
* the assessment perspective is comparable

---

## 10.3 Resolved Finding

A finding may be considered resolved when it was previously identified and is no longer detected under comparable assessment conditions.

```text
Baseline:
CVE-A
CVE-B
CVE-C

Current:
CVE-A
CVE-C
```

`CVE-B` is a candidate resolved finding.

Before concluding that remediation succeeded, investigate whether:

* the affected software was patched
* the service was removed
* the host changed
* the target left scope
* authentication changed
* the plugin changed
* the service became unreachable

---

## 10.4 Reappeared Finding

A finding can disappear and later return.

Example:

```text
Assessment 1:
CVE-A

Assessment 2:
No CVE-A

Assessment 3:
CVE-A
```

Possible explanations include:

* regression
* patch rollback
* software reinstallation
* configuration drift
* asset replacement
* newly exposed service
* assessment coverage changes

A reappeared finding deserves investigation rather than being treated simply as a new finding.

---

# 11. Finding Counts Are Context, Not Conclusions

Consider:

```text
Assessment A: 40 findings
Assessment B: 20 findings
```

It is tempting to conclude:

```text
Security improved.
```

That conclusion is not justified from the count alone.

Possible explanation:

```text
Assessment A
100 hosts
↓
Assessment B
60 hosts
```

The reduction may be caused partly by scope changes.

Another example:

```text
Assessment A
Unauthenticated

Assessment B
Authenticated
```

A finding count difference may reflect a different assessment perspective.

Therefore:

```text
Count
 ↓
Investigate Change
 ↓
Understand Cause
 ↓
Interpret Result
```

---

# 12. Severity Changes

Finding severity can change between assessments.

For example:

```text
Baseline:
Finding X → High

Current:
Finding X → Medium
```

Do not automatically interpret this as successful remediation.

Investigate:

* plugin/content changes
* environmental changes
* affected component changes
* detection changes
* configuration changes
* vendor information changes

Severity changes require the same evidence-driven approach as finding appearance/disappearance.

---

# 13. Plugin and Content Changes

Nessus detection content evolves.

A newer assessment may detect conditions that an older assessment did not.

This can produce:

```text
Old Assessment
     ↓
Plugin coverage A

New Assessment
     ↓
Plugin coverage B
```

Therefore:

```text
Finding appeared
        ≠
Vulnerability necessarily appeared
```

It may have become detectable.

Similarly:

```text
Finding disappeared
        ≠
Vulnerability necessarily disappeared
```

Detection logic or content may have changed.

When comparing important results, record relevant Nessus/plugin/content state where available.

---

# 14. Nessus Version Changes

Platform changes can affect assessment behavior.

For example:

```text
Assessment A
Nessus Version X

Assessment B
Nessus Version Y
```

A result difference should be interpreted with awareness of the version change.

Do not assume every difference is caused by the Nessus version.

Instead:

```text
Version Changed?
      ↓
Relevant to Finding?
      ↓
Investigate
```

Use official Tenable documentation when version-specific behavior needs confirmation.

---

# 15. Authentication Comparison

Authentication state is one of the most important comparison variables.

Example:

```text
Assessment A:
Authenticated

Assessment B:
Authentication Failed
```

A reduction in findings cannot safely be interpreted as remediation.

The second assessment may simply have lost visibility.

Always compare:

* authentication method
* account
* permissions
* authentication success
* target coverage
* evidence showing authenticated assessment

Remember:

```text
Authenticated Assessment
        ≠
Authentication Configured
```

---

# 16. Configuration Drift

Assessment configuration can change without anyone intentionally changing the assessment objective.

Examples:

* plugin families changed
* performance settings changed
* discovery behavior changed
* credential configuration changed
* policy changed
* exclusions changed
* target list changed
* scheduling changed
* scanner changed

This is configuration drift.

A useful tracking model is:

```text
Reference Configuration
        ↓
Current Configuration
        ↓
Differences
        ↓
Potential Result Impact
```

Configuration should therefore be treated as part of assessment evidence.

---

# 17. Remediation Tracking Workflow

Use this workflow after remediation activity:

```text
BASELINE
   ↓
FINDING IDENTIFIED
   ↓
REMEDIATION ACTION
   ↓
RETEST
   ↓
COMPARE
   ↓
INVESTIGATE DIFFERENCES
   ↓
VALIDATE
   ↓
CLASSIFY
```

Example:

```text
Baseline:
Host A
HTTPS
Finding: vulnerable component

Remediation:
Component updated

Retest:
Host A
HTTPS
Finding no longer detected

Validation:
Installed version and relevant evidence reviewed

Conclusion:
Finding no longer detected under comparable conditions
```

The important point is that the comparison supports the conclusion; it does not replace validation.

---

# 18. Tracking Remediation Across Multiple Assessments

For recurring assessments, maintain a longitudinal view.

Example:

| Finding   | Baseline | Retest 1 | Retest 2 | Current State |
| --------- | -------- | -------- | -------- | ------------- |
| Finding A | Present  | Present  | Absent   | Not detected  |
| Finding B | Present  | Present  | Present  | Recurring     |
| Finding C | Absent   | Present  | Present  | New/Recurring |
| Finding D | Present  | Absent   | Present  | Reappeared    |

This helps identify patterns.

For example:

```text
Present → Absent → Present
```

may indicate regression or environmental change.

Whereas:

```text
Present → Present → Present
```

indicates continued presence under the assessed conditions.

---

# 19. Compare at the Right Granularity

Different questions require different comparison levels.

### Assessment level

```text
Did overall coverage change?
```

### Host level

```text
Which systems changed?
```

### Service level

```text
Which exposed services changed?
```

### Finding level

```text
Which conditions changed?
```

### Evidence level

```text
Why did the condition change?
```

Use the smallest level necessary to answer the question.

---

# 20. Compare Like-for-Like Before Comparing Globally

Suppose:

```text
Assessment A:
100 hosts

Assessment B:
120 hosts
```

A global finding count may increase.

That does not necessarily mean existing systems became less secure.

Instead separate:

```text
Existing Hosts
+
New Hosts
```

Then compare:

```text
Existing Host Findings
```

separately from:

```text
New Host Findings
```

This produces a more meaningful interpretation.

---

# 21. Changed Scope Comparison

Sometimes the assessments are intentionally different.

For example:

```text
Assessment A:
Production servers

Assessment B:
Production + Development
```

The assessments are not directly equivalent.

That does not make the comparison useless.

Instead state the limitation:

```text
The current assessment includes additional development assets,
so total finding counts are not directly comparable to the baseline.
```

Then perform narrower comparisons where possible.

---

# 22. When a Comparison Is Not Valid

Do not force a comparison when major differences prevent meaningful interpretation.

Examples:

* completely different target population
* different assessment objective
* authentication state fundamentally different
* substantially different scanner position
* major configuration changes
* incomplete baseline
* incomplete current assessment
* major plugin/content differences affecting the relevant findings
* infrastructure completely redesigned

The correct response may be:

```text
Not directly comparable.
```

That is better than producing a misleading trend.

---

# 23. Tracking Assessment History

For recurring work, maintain an assessment history.

A useful record contains:

| Field                | Example                           |
| -------------------- | --------------------------------- |
| Assessment           | Production Weekly Scan            |
| Date                 | 2026-09-25                        |
| Scope                | Production web servers            |
| Objective            | Vulnerability assessment          |
| Scanner              | Authorized scanner                |
| Authentication       | SSH authenticated                 |
| Configuration        | Approved policy                   |
| Plugin/content state | Recorded                          |
| Previous baseline    | 2026-09-18                        |
| Major changes        | Two servers added                 |
| Findings changed     | 6 new, 3 resolved                 |
| Coverage issues      | One host unreachable              |
| Follow-up            | Investigate host and new findings |

This provides context for later analysis.

---

# 24. Assessment Drift

Assessment drift occurs when the assessment gradually changes from its intended design.

Examples:

```text
Original:
Authenticated production assessment

Later:
Authentication fails

Later:
New targets added

Later:
Plugin configuration changed

Later:
Scanner moved

Later:
Exclusions added
```

The scan may still execute successfully.

But the assessment may no longer answer the original question reliably.

Therefore:

```text
Successful Scan
        ≠
Consistent Assessment
```

Recurring assessments should periodically be reviewed for drift.

---

# 25. Comparing Scheduled Assessments

For recurring assessments, use a review cycle:

```text
SCHEDULED ASSESSMENT
        ↓
RESULTS
        ↓
COMPARE TO REFERENCE
        ↓
INVESTIGATE CHANGES
        ↓
REMEDIATION / FOLLOW-UP
        ↓
UPDATE TRACKING
        ↓
CHECK CONFIGURATION DRIFT
        ↓
NEXT ASSESSMENT
```

Do not treat recurring scans as automatic proof of continuous correctness.

---

# 26. Practical Lab 1 — Compare Two Equivalent Assessments

## Objective

Determine what changed between two assessments of the same lab target.

## Setup

Use an authorized lab target.

Perform:

```text
Assessment A
    ↓
Record results
    ↓
Make a controlled change
    ↓
Assessment B
```

The change should be known and safe.

## Tasks

Determine:

1. Did the same host remain in scope?
2. Did the same services remain visible?
3. Which findings remained?
4. Which findings disappeared?
5. Which findings appeared?
6. Did plugin/content state change?
7. Did authentication state remain equivalent?
8. Can the differences reasonably be attributed to the controlled change?

## Record

```text
Baseline:
Controlled Change:
Retest:
New Findings:
Recurring Findings:
Resolved Candidates:
Coverage Differences:
Validation:
Conclusion:
```

---

# 27. Practical Lab 2 — Remediation Lifecycle

## Objective

Track a finding from identification through retest.

Workflow:

```text
Initial Assessment
       ↓
Finding
       ↓
Remediation
       ↓
Retest
       ↓
Comparison
       ↓
Validation
```

## Tasks

Record:

* original host
* affected service/component
* original evidence
* remediation performed
* retest date
* current evidence
* comparison result
* validation result

Do not record credentials or secrets.

---

# 28. Practical Lab 3 — Changed Scope

## Scenario

Assessment A covers:

```text
Host A
Host B
Host C
```

Assessment B covers:

```text
Host A
Host B
Host C
Host D
```

Assessment B contains more findings.

## Task

Determine whether the increase can be interpreted as a deterioration of the original environment.

Do not use total finding count alone.

Separate:

```text
Existing Assets
```

from:

```text
New Assets
```

Then compare the original population independently.

---

# 29. Practical Lab 4 — Authentication Change

## Scenario

Assessment A:

```text
Authenticated
```

Assessment B:

```text
Authentication failed
```

The second assessment reports fewer findings.

## Task

Determine whether the reduction can be classified as remediation.

Your answer should identify:

* authentication state
* evidence of coverage
* whether the same assessment perspective was maintained
* whether the finding reduction is interpretable

The goal is to recognize loss of visibility as a possible explanation.

---

# 30. Practical Lab 5 — Recurring Assessment Tracking

Run several assessments against an authorized lab environment.

Create a table:

| Date  | Scope | Auth | New | Recurring | Resolved | Reappeared | Coverage Issue |
| ----- | ----- | ---- | --: | --------: | -------: | ---------: | -------------- |
| Day 1 | Same  | Yes  |   — |         — |        — |          — | None           |
| Day 2 | Same  | Yes  |   2 |         5 |        1 |          0 | None           |
| Day 3 | Same  | Yes  |   1 |         4 |        2 |          1 | None           |

The exact values will depend on your lab.

Your goal is to understand the lifecycle, not reproduce the example numbers.

---

# 31. Practical Lab 6 — Plugin Content Change

## Objective

Understand how detection content can affect longitudinal comparisons.

## Tasks

1. Record relevant plugin/content state for an assessment.
2. Perform a later assessment.
3. Determine whether relevant content changed.
4. Identify findings that appeared or disappeared.
5. Investigate whether the content change could affect interpretation.
6. Avoid automatically labeling the result as remediation or regression.

The objective is attribution.

---

# 32. Practical Lab 7 — Find the Real Cause of a Difference

## Scenario

A finding disappeared.

Possible explanations:

```text
A. Vulnerability was remediated
B. Host left scope
C. Service disappeared
D. Authentication failed
E. Plugin behavior changed
F. Target became unreachable
G. Finding is no longer applicable
```

## Task

Investigate the assessment evidence and determine which explanation is supported.

Do not select an explanation merely because it is convenient.

---

# 33. Professional Comparison Record

For important comparisons, maintain a structured record.

```text
Assessment Comparison
=====================

Reference Assessment:
Current Assessment:

Purpose:

Scope Comparison:
- Same targets:
- Added targets:
- Removed targets:
- Changed targets:

Workflow Comparison:
- Objective:
- Assessment type:
- Scanner:
- Network position:

Authentication:
- Reference:
- Current:
- Coverage equivalent:

Configuration:
- Policy:
- Important settings:
- Exclusions:

Plugin / Content:
- Reference:
- Current:
- Relevant differences:

Results:
- New findings:
- Recurring findings:
- Resolved candidates:
- Reappeared findings:
- Severity changes:

Coverage Issues:

Investigation:

Validation:

Interpretation:

Follow-up:

Reviewer:
Date:
```

This creates an auditable reasoning trail.

---

# 34. Common Comparison Mistakes

## Mistake 1 — Comparing Only Finding Counts

### Problem

```text
100 → 50
```

is treated as proof of improvement.

### Better approach

Investigate:

* scope
* coverage
* authentication
* configuration
* findings
* evidence

---

## Mistake 2 — Ignoring Target Changes

A removed host can make findings disappear.

### Better approach

Compare host inventory first.

---

## Mistake 3 — Ignoring Authentication

A failed credential can reduce visibility.

### Better approach

Check authentication evidence before interpreting finding reductions.

---

## Mistake 4 — Ignoring Plugin Changes

Detection logic evolves.

### Better approach

Consider relevant plugin/content state when interpreting important changes.

---

## Mistake 5 — Treating Every Disappearance as Remediation

A finding can disappear for many reasons.

### Better approach

Classify it as a **resolved candidate** until the cause is understood.

---

## Mistake 6 — Treating Every New Finding as a New Vulnerability

A new finding may result from improved detection.

### Better approach

Investigate the underlying target condition and assessment changes.

---

## Mistake 7 — Comparing Different Objectives

Discovery and authenticated vulnerability assessment are not equivalent measurements.

### Better approach

Compare assessments with comparable objectives.

---

## Mistake 8 — Ignoring Incomplete Assessments

A partial assessment can create false trends.

### Better approach

Check scan completion and coverage before comparison.

---

## Mistake 9 — Ignoring Configuration Drift

A recurring scan may gradually stop matching its original design.

### Better approach

Review configuration periodically.

---

## Mistake 10 — Assuming Historical Results Are Automatically Comparable

Historical data does not guarantee equivalent assessment conditions.

### Better approach

Perform a comparability check first.

---

# 35. Decision Rule

When comparing two Nessus assessments:

```text
1. Identify the reference assessment.
        ↓
2. Verify the assessment objective.
        ↓
3. Compare scope.
        ↓
4. Compare target inventory.
        ↓
5. Compare scanner/network perspective.
        ↓
6. Compare authentication.
        ↓
7. Compare configuration.
        ↓
8. Compare relevant plugin/content state.
        ↓
9. Check assessment completeness.
        ↓
10. Compare hosts and services.
        ↓
11. Compare findings.
        ↓
12. Investigate important differences.
        ↓
13. Validate significant conclusions.
        ↓
14. Classify changes.
        ↓
15. Record follow-up actions.
```

The key rule is:

> **Never interpret a result difference until you understand whether the assessment conditions remained comparable.**

---

# 36. Comparison Checklist

## Before Comparison

* [ ] Reference assessment identified
* [ ] Current assessment identified
* [ ] Assessment objective compared
* [ ] Scope compared
* [ ] Target inventory compared
* [ ] Scanner compared
* [ ] Network perspective considered
* [ ] Authentication compared
* [ ] Credential coverage considered
* [ ] Configuration compared
* [ ] Plugin/content state considered
* [ ] Assessment completeness checked

## During Comparison

* [ ] Hosts compared
* [ ] Services compared
* [ ] Findings compared
* [ ] New findings identified
* [ ] Recurring findings identified
* [ ] Resolved candidates identified
* [ ] Reappeared findings identified
* [ ] Severity changes investigated
* [ ] Coverage differences investigated
* [ ] Scope changes separated from remediation

## Before Conclusion

* [ ] Important differences investigated
* [ ] Evidence reviewed
* [ ] Validation performed where necessary
* [ ] Uncertainty documented
* [ ] Limitations documented
* [ ] Follow-up actions defined

---

# 37. Completion Criteria

You have completed this workflow when you can independently:

* establish a meaningful baseline
* determine whether two assessments are comparable
* compare scope and target inventory
* compare hosts and services
* identify new findings
* identify recurring findings
* identify resolved candidates
* identify reappeared findings
* recognize assessment drift
* recognize authentication-related visibility changes
* recognize plugin/content-related differences
* distinguish scope changes from remediation
* track remediation through retesting
* document longitudinal assessment history
* reject an invalid comparison when necessary
* explain why a result changed
* identify what should happen next

---

# 38. Final Mental Model

Remember:

```text
BASELINE
   ↓
COMPARABILITY
   ↓
SCOPE
   ↓
TARGETS
   ↓
SERVICES
   ↓
AUTHENTICATION
   ↓
CONFIGURATION
   ↓
PLUGIN / CONTENT STATE
   ↓
COVERAGE
   ↓
FINDINGS
   ↓
CHANGES
   ↓
CAUSE
   ↓
VALIDATION
   ↓
CLASSIFICATION
   ↓
FOLLOW-UP
```

The professional skill is not simply noticing that two Nessus reports are different.

The skill is explaining **why they are different**, determining which differences are meaningful, and deciding what evidence or action is required next.
