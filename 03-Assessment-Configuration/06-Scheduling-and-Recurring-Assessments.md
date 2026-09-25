# Scheduling and Recurring Assessments

## Objective

Learn how to schedule Nessus assessments responsibly, build recurring assessment workflows, and prevent old configurations from continuing indefinitely without review.

By the end of this file, you should be able to:

* Explain when scheduling is useful.
* Distinguish one-time assessments from recurring assessments.
* Configure scheduling based on an operational window.
* Understand the difference between scheduling and automation.
* Identify risks associated with recurring assessments.
* Maintain recurring targets, credentials, and configurations.
* Review recurring assessment results over time.
* Recognize when a recurring assessment should be modified, paused, or retired.
* Document scheduling decisions.
* Build a simple recurring vulnerability-assessment workflow.

---

# 1. Scheduling Is Part of Assessment Design

Scheduling is not simply:

> "Tell Nessus to run every week."

A scheduled assessment is still an assessment.

The complete model is:

```text id="m4k8p2"
AUTHORIZATION
     ↓
OBJECTIVE
     ↓
SCOPE
     ↓
WORKFLOW
     ↓
CONFIGURATION
     ↓
SCHEDULE
     ↓
EXECUTION
     ↓
RESULTS
     ↓
REVIEW
```

The schedule must remain consistent with the authorization, objective, and operational constraints.

---

# 2. One-Time vs Recurring Assessments

## One-Time Assessment

Used when the assessment is tied to a specific event.

Examples:

* Initial vulnerability assessment.
* New server assessment.
* Pre-deployment assessment.
* Post-remediation retest.
* Investigation of a specific issue.

Conceptually:

```text id="h7n2q9"
Configure
   ↓
Run
   ↓
Analyze
   ↓
Finish
```

---

## Recurring Assessment

Used when the same assessment needs to execute repeatedly.

Examples:

* Weekly vulnerability monitoring.
* Monthly infrastructure assessment.
* Regular assessment of a stable lab environment.
* Periodic compliance/configuration checks.

Conceptually:

```text id="z5v8m3"
Configure
   ↓
Run
   ↓
Analyze
   ↓
Wait
   ↓
Run Again
   ↓
Compare
   ↓
Repeat
```

---

# 3. Recurring Does Not Mean "Set and Forget"

This is one of the most important principles in this file.

A recurring scan can continue executing even after the environment changes.

For example:

```text id="r6q3w1"
Month 1
Authorized Server A
      ↓
Month 2
Server A Reconfigured
      ↓
Month 3
Server A Replaced
      ↓
Month 4
New System Added
      ↓
Recurring Assessment Still Uses Old Configuration
```

Therefore:

> **Recurring assessments require periodic human review.**

---

# 4. When Scheduling Makes Sense

Scheduling is useful when:

* The assessment objective repeats.
* The target scope is stable.
* The assessment is authorized on an ongoing basis.
* The operational window is known.
* Results need periodic review.
* Credentials can be maintained.
* The scanner can safely execute at the selected frequency.

---

# 5. When Scheduling May Not Make Sense

Avoid blindly scheduling an assessment when:

* Authorization is temporary.
* Targets change frequently.
* The assessment is a one-time investigation.
* The assessment could disrupt the target.
* Credentials are unstable.
* The assessment window is unclear.
* No one owns the resulting findings.
* The recurring result will not be reviewed.

A recurring scan without a review process can create large amounts of unattended security data.

---

# 6. Schedule Frequency Should Follow the Objective

Do not choose frequency simply because:

> "Weekly sounds professional."

Instead ask:

> "How often does the information need to be refreshed?"

For example:

```text id="u3n7k5"
Fast-changing environment
        ↓
Potentially more frequent assessment
```

while:

```text id="b8r2m6"
Stable environment
        ↓
Potentially less frequent assessment
```

The correct frequency depends on the environment, risk, operational constraints, and assessment purpose.

---

# 7. Scheduling and Authorization

Before creating a recurring assessment, verify:

```text id="n5c8x4"
Is the target continuously authorized?
        ↓
YES
        ↓
Is recurring assessment permitted?
        ↓
YES
        ↓
Is the schedule within the approved window?
        ↓
YES
        ↓
Schedule
```

If authorization expires, the recurring assessment must not continue simply because it remains configured.

---

# 8. Operational Windows

Assessment traffic can affect systems.

Therefore, scheduling may need to account for:

* Business hours.
* Maintenance windows.
* Peak traffic.
* Production sensitivity.
* Network capacity.
* System load.
* Change windows.
* Backup operations.
* Other scheduled security tools.

A schedule should be compatible with the environment.

---

# 9. Avoid Scheduling by Convenience Alone

Bad reasoning:

> "I'll run it at midnight because nobody is working."

That may still conflict with:

* Backups.
* Batch jobs.
* Monitoring.
* Maintenance.
* Network operations.
* Other scans.

Better:

> "I will choose a window that is explicitly appropriate for the target environment."

---

# 10. Scheduling Model

Think of scheduling as:

```text id="c2j6x9"
WHAT?
Assessment configuration

WHEN?
Execution window

HOW OFTEN?
Frequency

UNDER WHAT AUTHORIZATION?
Approved scope and period

WHO REVIEWS IT?
Owner

WHAT HAPPENS AFTERWARD?
Analysis / remediation workflow
```

If any of these are unclear, the recurring assessment needs more planning.

---

# 11. Recurring Assessment Ownership

Every recurring assessment should have an owner.

Document:

```text id="k7m3v1"
Assessment:
Owner:
Purpose:
Targets:
Schedule:
Review Frequency:
Credential Owner:
Escalation Contact:
Retirement Condition:
```

The owner does not necessarily have to be the person who created the scan.

The important point is accountability.

---

# 12. Credential Maintenance

Recurring authenticated assessments have an additional dependency:

```text id="x4p8n2"
Recurring Scan
      ↓
Credential
      ↓
Credential Valid?
      │
 ┌────┴────┐
YES       NO
 │          │
Continue   Authentication
           failure
```

When credentials rotate:

* Update the approved credential store.
* Verify authentication.
* Review whether coverage changed.
* Record the change.

Do not silently let recurring assessments become unauthenticated.

---

# 13. Credential Expiration

Suppose a recurring assessment normally produces:

```text id="s6v2q4"
Host-level findings
```

Then suddenly produces:

```text id="d9m3k7"
Only network-level findings
```

One possible explanation is authentication failure.

Investigate:

* Credential expiration.
* Password rotation.
* Account lockout.
* Permission changes.
* Target-side policy changes.

Do not automatically interpret the reduced findings as improved security.

---

# 14. Target Maintenance

Recurring targets also require review.

A target may:

* Be decommissioned.
* Change IP address.
* Move networks.
* Change hostname.
* Become cloud-hosted.
* Be replaced.
* Become temporarily unavailable.
* Fall outside the original authorization.

Therefore periodically review:

```text id="p4k9r3"
Target List
    ↓
Still Exists?
    ↓
Still Relevant?
    ↓
Still Authorized?
    ↓
Still Appropriate?
```

---

# 15. Configuration Maintenance

Review:

* Workflow.
* Targets.
* Credentials.
* Plugin coverage.
* Exclusions.
* Performance settings.
* Advanced options.
* Schedule.

An assessment configuration that was correct six months ago may no longer be correct.

---

# 16. Recurring Assessment Lifecycle

Use:

```text id="m8v5c2"
CREATE
  ↓
REVIEW
  ↓
SCHEDULE
  ↓
EXECUTE
  ↓
MONITOR
  ↓
ANALYZE
  ↓
REMEDIATE
  ↓
RETEST
  ↓
COMPARE
  ↓
REVIEW CONFIGURATION
  ↓
CONTINUE / MODIFY / RETIRE
```

The final decision is important.

A recurring assessment should not continue indefinitely without evaluation.

---

# 17. Scheduling vs Continuous Monitoring

These are not identical.

### Scheduled Assessment

Runs at defined times.

```text id="a3q7n8"
Monday
↓
Scan

Next Monday
↓
Scan
```

### Continuous Monitoring

May involve continuously updated telemetry or other mechanisms.

Nessus scheduled scanning should not automatically be described as continuous monitoring.

Use accurate terminology.

---

# 18. Scheduling and Result Comparison

Recurring assessments become much more useful when you compare runs.

Example:

```text id="g5n2m8"
Run 1
20 findings
      ↓
Remediation
      ↓
Run 2
12 findings
      ↓
Remediation
      ↓
Run 3
7 findings
```

Do not judge the numbers alone.

Ask:

* Did the same targets run?
* Was authentication equivalent?
* Was plugin content comparable?
* Did the environment change?
* Were findings actually remediated?
* Did new findings appear?

---

# 19. Result Trend Interpretation

A recurring assessment creates a timeline:

```text id="x8c4m7"
Time
 ↓
Findings
 ↓
Changes
 ↓
Remediation
 ↓
Retest
```

Useful trend questions include:

* Which findings persist?
* Which findings disappeared?
* Which findings are new?
* Which assets repeatedly generate findings?
* Did remediation produce the expected change?
* Did assessment coverage remain consistent?

---

# 20. Do Not Compare Incompatible Runs

Consider:

```text id="w3k7p2"
Run 1:
Unauthenticated
```

and:

```text id="j9m4c6"
Run 2:
Authenticated
```

A simple finding-count comparison is misleading because the assessment perspective changed.

Similarly:

```text id="b5x8n3"
Run 1:
100 hosts

Run 2:
50 hosts
```

The raw finding counts are not directly comparable without accounting for scope.

---

# 21. Schedule Changes

If the schedule changes, document:

```text id="r2v6m9"
Old Schedule:
New Schedule:
Reason:
Approved By:
Effective Date:
```

The exact approval process depends on your environment.

The important point is traceability.

---

# 22. Pausing a Recurring Assessment

A recurring assessment may need to be paused when:

* Maintenance is underway.
* The target is unstable.
* Authorization is temporarily suspended.
* The assessment creates unexpected impact.
* Credentials are being changed.
* The configuration requires review.

Pausing is preferable to allowing a known-problematic assessment to continue blindly.

---

# 23. Retiring a Recurring Assessment

Retire or disable a recurring assessment when:

* The target no longer exists.
* The assessment objective is complete.
* Authorization ended.
* A replacement workflow exists.
* The assessment is no longer useful.
* The configuration is obsolete.

Do not leave abandoned recurring scans indefinitely.

---

# 24. Recurring Assessment Review Checklist

Periodically verify:

```text id="e8r5n2"
[ ] Objective still valid
[ ] Authorization still valid
[ ] Targets still valid
[ ] Exclusions still valid
[ ] Credentials still valid
[ ] Plugin coverage still appropriate
[ ] Performance still appropriate
[ ] Schedule still appropriate
[ ] Assessment owner still responsible
[ ] Results are being reviewed
[ ] Findings are being tracked
```

---

# 25. Scheduling and Safety

Before scheduling a potentially disruptive assessment:

```text id="k4p8v3"
Understand target
      ↓
Understand expected impact
      ↓
Choose appropriate window
      ↓
Start with conservative configuration
      ↓
Monitor first executions
      ↓
Adjust only when justified
```

Do not assume that because a scan worked safely once, every future execution will behave identically.

The environment can change.

---

# 26. First Scheduled Execution

For a new recurring assessment, treat the first execution as a validation run.

After it finishes, confirm:

* Correct targets.
* Correct workflow.
* Correct authentication.
* Expected duration.
* Expected findings.
* No unexpected impact.
* Appropriate schedule.

Only then should the recurring configuration be considered operationally mature.

---

# 27. Recurring Scan Naming

Use names that identify purpose.

Example:

```text id="u7x3m1"
Weekly-External-Web-Assessment
```

or:

```text id="q8n4v6"
Monthly-Authenticated-Linux-Assessment
```

Avoid:

```text id="c3k9r5"
Scan-123
```

Good names make recurring schedules easier to manage.

---

# 28. Scheduling Record

For an important recurring assessment:

```text id="p6v2m8"
## Recurring Assessment

### Purpose
[Assessment objective]

### Scope
[Authorized scope]

### Targets
[Targets]

### Workflow
[Workflow]

### Authentication
[Authenticated / Unauthenticated]

### Schedule
[Frequency / window]

### Owner
[Role/team]

### Review Process
[How results are reviewed]

### Credential Maintenance
[How credentials are maintained]

### Configuration Review
[Review frequency]

### Retirement Condition
[When assessment should be disabled]
```

Do not include secrets.

---

# 29. Practical Exercise 1 — Create a One-Time Schedule

Use your authorized Nessus lab.

Create a simple assessment and schedule it for a future time within your safe lab environment.

Record:

```text id="m3k7p1"
Assessment:
Target:
Workflow:
Schedule:
Reason:
Expected Duration:
Expected Result:
```

After it runs, compare actual and expected behavior.

---

# 30. Practical Exercise 2 — Build a Recurring Assessment

Create a recurring assessment for an authorized lab target.

Choose a reasonable frequency.

Record:

```text id="v8q2c5"
Purpose:
Target:
Workflow:
Authentication:
Frequency:
Execution Window:
Owner:
Review Method:
```

Do not use a production target for this exercise unless you have explicit authorization for recurring scanning.

---

# 31. Practical Exercise 3 — Credential Rotation Simulation

In your lab:

1. Create an authorized assessment account.
2. Configure Nessus to use it.
3. Confirm authenticated results.
4. Rotate/change the credential safely.
5. Run the assessment again.
6. Observe the authentication result.
7. Update the credential.
8. Run again.
9. Compare the results.

Record:

```text id="n4x7r2"
Before Rotation:
Authentication:
Coverage:

After Rotation:
Authentication:
Coverage:

After Credential Update:
Authentication:
Coverage:
```

---

# 32. Practical Exercise 4 — Target Change

For a lab recurring assessment:

1. Configure a known target.
2. Run the assessment.
3. Change the target environment safely.
4. Determine whether the recurring configuration still represents the intended assessment.
5. Update the configuration if necessary.
6. Document the change.

Record:

```text id="j6p3v8"
Original Target:
Environment Change:
Does Original Configuration Still Apply?
Required Change:
Reason:
```

---

# 33. Practical Exercise 5 — Compare Recurring Results

Run the same authorized assessment more than once.

Create:

| Run | Date | Targets | Authentication | Findings | Major Changes |
| --- | ---- | ------- | -------------- | -------- | ------------- |
| 1   |      |         |                |          |               |
| 2   |      |         |                |          |               |
| 3   |      |         |                |          |               |

Then explain:

* What changed?
* What stayed consistent?
* Were the runs directly comparable?
* Did authentication remain consistent?
* Did target scope remain consistent?
* Were findings remediated or merely changed?

---

# 34. Practical Exercise 6 — Retire a Recurring Assessment

Create a lab recurring assessment.

Then simulate the end of its purpose.

Document:

```text id="s5m8q1"
Assessment:
Original Purpose:
Why It Is No Longer Needed:
Retirement Decision:
Final State:
Replacement:
```

The goal is to learn that assessment lifecycle includes retirement.

---

# 35. Common Mistakes

## Mistake 1 — Scheduling Without Authorization Review

Bad:

> "It worked once, so I can schedule it permanently."

Better:

> "Recurring execution must remain within valid authorization."

---

## Mistake 2 — Ignoring Credential Expiration

Bad:

> "The scan is scheduled, so authentication will always work."

Better:

> "Credential lifecycle is part of recurring assessment maintenance."

---

## Mistake 3 — Never Reviewing Targets

Bad:

> "The target list is already saved."

Better:

> "Target ownership, identity, and authorization must remain valid."

---

## Mistake 4 — Comparing Raw Finding Counts

Bad:

> "Findings dropped from 30 to 20, so security improved."

Better:

> "First verify scope, authentication, configuration, plugin context, and environmental changes."

---

## Mistake 5 — Running at the Most Convenient Time

Bad:

> "Midnight is always safe."

Better:

> "Use an approved operational window appropriate for the environment."

---

## Mistake 6 — Leaving Abandoned Schedules Active

Bad:

> "I'll just leave the old scan there."

Better:

> "Disable or retire obsolete recurring assessments."

---

# 36. Troubleshooting Scheduled Assessments

If a scheduled assessment does not execute as expected, investigate:

```text id="q7m2x8"
Schedule Correct?
      ↓
Assessment Enabled?
      ↓
Scanner Available?
      ↓
Target Reachable?
      ↓
Credentials Valid?
      ↓
Configuration Valid?
      ↓
Any Reported Errors?
```

Do not immediately recreate the assessment.

Understand what failed first.

---

# 37. Recurring Assessment Failure

If repeated runs fail:

```text id="x4n7c3"
Run 1 → Failure
Run 2 → Failure
Run 3 → Failure
```

Ask:

> "What changed or remained broken between runs?"

Potential causes:

* Credentials.
* Target availability.
* Network conditions.
* Scanner availability.
* Configuration.
* Authorization.
* Scheduling.
* Target-side controls.

A repeated failure is evidence that the underlying problem has not been resolved.

---

# 38. Professional Decision Rule

Before enabling a recurring assessment, complete:

> **"This assessment should run on this schedule because __________. The scope remains authorized because __________. The expected operational impact is __________. The owner will review __________. The assessment should be modified or retired when __________."**

If you cannot complete this clearly, the recurring assessment is not ready.

---

# 39. Recurring Assessment Checklist

Before activation:

```text id="b6q1r8"
[ ] Objective defined
[ ] Authorization confirmed
[ ] Targets verified
[ ] Workflow verified
[ ] Credentials verified
[ ] Configuration reviewed
[ ] Impact considered
[ ] Schedule selected
[ ] Owner assigned
[ ] Review process defined
[ ] Retirement condition defined
```

After first execution:

```text id="w2m8x4"
[ ] Assessment executed
[ ] Targets correct
[ ] Authentication correct
[ ] Results expected
[ ] No unexpected impact
[ ] Schedule appropriate
[ ] Configuration documented
```

During ongoing operation:

```text id="p9v3k7"
[ ] Authorization remains valid
[ ] Targets remain valid
[ ] Credentials remain valid
[ ] Results are reviewed
[ ] Findings are tracked
[ ] Configuration remains appropriate
[ ] Schedule remains appropriate
```

---

# 40. Completion Criteria

You have completed this file when you can independently:

* Explain when a Nessus assessment should be scheduled.
* Distinguish one-time and recurring assessments.
* Choose a schedule based on the assessment objective and environment.
* Account for operational windows.
* Maintain authorization over recurring assessments.
* Maintain target and credential validity.
* Understand how recurring results can be compared.
* Recognize when two runs are not directly comparable.
* Pause, modify, or retire recurring assessments when appropriate.
* Assign ownership to recurring assessments.
* Document scheduling and maintenance decisions.
* Troubleshoot scheduled assessment failures.

The final test is:

> **Given a recurring vulnerability-assessment requirement, can you design the schedule, verify authorization and operational constraints, maintain the targets and credentials, interpret results across runs, and determine when the assessment should continue, change, pause, or retire?**

If yes, the assessment-configuration section is complete.

You are now ready to move into the core operational workflows: **discovery, unauthenticated assessment, authenticated assessment, configuration/compliance assessment, and edition-specific workflows.**
