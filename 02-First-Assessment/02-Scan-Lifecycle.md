# Scan Lifecycle

## Objective

Understand what happens before, during, and after a Nessus scan, and learn how to operate an assessment safely throughout its entire lifecycle.

By the end of this file, you should be able to:

* Recognize the major stages of a Nessus assessment.
* Perform a useful pre-launch check.
* Monitor a running assessment without blindly waiting.
* Recognize normal and abnormal scan behavior.
* Decide when to continue, stop, investigate, or rerun an assessment.
* Understand the difference between a completed scan and a useful assessment.
* Determine whether results are sufficiently complete for analysis.
* Use scan history to understand what happened.
* Record important lifecycle observations.
* Explain what should happen next after a scan finishes.

---

## Why the Scan Lifecycle Matters

Launching a scan is not the end of the assessment.

A professional vulnerability assessment follows a lifecycle:

```text
SCOPE
   ↓
OBJECTIVE
   ↓
TARGET
   ↓
CONFIGURATION
   ↓
PRE-LAUNCH CHECK
   ↓
LAUNCH
   ↓
MONITOR
   ↓
COMPLETE / STOP / FAIL
   ↓
CHECK RESULT QUALITY
   ↓
ANALYZE
   ↓
VALIDATE
   ↓
PRIORITIZE
   ↓
REPORT
```

A scan can technically finish while still producing incomplete or misleading results.

Examples:

* The target was unreachable.
* Credentials failed.
* Important hosts were excluded accidentally.
* The scan stopped before completion.
* Network controls interfered with assessment traffic.
* The selected workflow did not answer the original question.
* Only part of the intended scope was assessed.
* Findings were produced but their evidence was insufficient for the intended conclusion.

Therefore:

> **Scan completion is an operational event, not automatically an assessment conclusion.**

---

## 1. The Lifecycle Mental Model

Think of every assessment as moving through several operational stages.

```text
DRAFT
  ↓
CONFIGURED
  ↓
PRE-LAUNCH REVIEW
  ↓
RUNNING
  ↓
MONITORING
  ↓
COMPLETED / STOPPED / FAILED
  ↓
RESULT REVIEW
  ↓
ANALYSIS
```

The exact labels and available controls can differ between Nessus versions and editions.

Do not memorize the labels.

Instead, ask:

1. What state is the assessment currently in?
2. What has already happened?
3. What has not happened yet?
4. Can I safely change anything?
5. Do I need to continue?
6. Do I need to stop?
7. Is another run required?

---

# 2. Stage 1 — Draft

A draft assessment is an assessment that has been created but is not yet ready or has not yet been launched.

At this stage, confirm:

* Assessment name.
* Target.
* Scope.
* Objective.
* Selected workflow/template.
* Major assessment settings.
* Credentials, if applicable.
* Scheduling, if applicable.
* Any exclusions.
* Any special safety considerations.

### Decision

Ask:

> "If I launched this assessment right now, would I understand exactly what Nessus is going to assess and why?"

If the answer is no, do not launch yet.

---

# 3. Stage 2 — Configured

A configured assessment has enough information to execute.

At minimum, understand:

```text
WHO?
  → What system am I authorized to assess?

WHAT?
  → What target is being assessed?

WHY?
  → What question is this assessment answering?

HOW?
  → What workflow/configuration will Nessus use?

WITH WHAT ACCESS?
  → Is authentication configured?

WHEN?
  → Is this running now or scheduled?

WHAT IMPACT?
  → Could this configuration create unnecessary load or disruption?
```

Do not confuse "configuration is accepted by the interface" with "configuration is correct."

Nessus can accept a configuration that does not match your assessment objective.

---

# 4. Stage 3 — Pre-Launch Review

Before launching, perform a deliberate safety check.

## Scope

Confirm:

* Target is authorized.
* Target is the intended system.
* No unintended production systems are included.
* Exclusions are correct where applicable.
* The target format is valid for the workflow.

## Objective

Confirm:

* You know what you are trying to discover.
* The selected workflow can reasonably answer that question.

## Authentication

If authentication is being used:

* Confirm the intended account.
* Confirm the credential method.
* Confirm the target supports the required authentication mechanism.
* Avoid repeated guessing or unnecessary authentication attempts.

## Configuration

Check the settings that materially affect:

* Target coverage.
* Discovery.
* Assessment depth.
* Credentials.
* Plugins.
* Performance.
* Scheduling.
* Safety.

## Impact

Ask:

> "Could this assessment create more traffic, load, or disruption than my target environment can safely tolerate?"

If yes, stop and adjust the plan before launching.

---

# 5. Pre-Launch Checklist

Use this short checklist before every important assessment:

```text
[ ] Authorization confirmed
[ ] Scope confirmed
[ ] Target confirmed
[ ] Objective defined
[ ] Workflow selected intentionally
[ ] Credentials reviewed
[ ] Important settings reviewed
[ ] Exclusions reviewed
[ ] Potential impact considered
[ ] Expected result defined
[ ] Stop conditions understood
```

For a simple lab assessment, this should take only a short amount of time.

For production assessments, the review should be more deliberate.

---

# 6. Stage 4 — Launch

When the assessment is launched, Nessus begins executing the configured workflow against the target.

The important transition is:

```text
Configuration
      ↓
Execution
```

At this point, your job changes.

Before launch:

> "Is the configuration correct?"

After launch:

> "Is the assessment behaving as expected?"

Do not immediately change settings just because the scan has not produced interesting findings yet.

Give the assessment enough time to establish useful progress.

---

# 7. Stage 5 — Running

While a scan is running, observe its behavior.

Useful questions include:

* Has the scan actually started?
* Is the target being reached?
* Is activity progressing?
* Are hosts being discovered?
* Are findings appearing?
* Are authenticated checks succeeding or failing?
* Is the assessment unusually slow?
* Is the target responding normally?
* Has the scan encountered errors?
* Is the observed behavior consistent with the objective?

You are not simply waiting for a percentage counter to reach 100%.

You are evaluating whether the assessment is producing meaningful evidence.

---

# 8. What to Monitor

Depending on your Nessus version and edition, available monitoring information may include some combination of:

* Scan state.
* Progress.
* Hosts discovered.
* Hosts assessed.
* Findings.
* Errors or warnings.
* Authentication status.
* Timing information.
* Activity/history.
* Completion state.

Exact information varies.

Therefore:

> **Use the information available in your installed version rather than assuming every Nessus interface exposes identical monitoring data.**

---

# 9. Normal Slow vs Abnormal Slow

A slow assessment is not automatically broken.

Possible legitimate causes include:

* Large target scope.
* Slow target response.
* Network latency.
* Many services.
* Extensive plugin coverage.
* Authentication attempts.
* Rate limiting.
* Network controls.
* Resource constraints.
* Scanner load.
* Target-side behavior.

Ask:

```text
Is progress still occurring?
        ↓
      YES
        ↓
Is the behavior expected for this target?
        ↓
   YES → Continue monitoring
   NO  → Investigate
```

If there is no meaningful progress for an unusual amount of time, investigate instead of blindly waiting.

---

# 10. The First Monitoring Decision

During a running scan, use this decision process:

```text
Is the scan running?
        │
        ├── NO → Determine why
        │
        └── YES
             ↓
     Is meaningful progress occurring?
             │
       ┌─────┴─────┐
       │           │
      YES          NO
       │           │
   Continue      Investigate
   monitoring
```

If progress is occurring:

* Continue monitoring.
* Avoid unnecessary changes.
* Watch for errors or unexpected behavior.

If progress is not occurring:

* Check target reachability.
* Check scanner state.
* Check errors/warnings.
* Check authentication if applicable.
* Check network conditions.
* Check whether the target itself is responding.

---

# 11. Do Not Chase Findings During Execution

A common beginner mistake is seeing the first interesting vulnerability and immediately treating the scan as successful.

Finding:

```text
Critical vulnerability
```

does not automatically mean:

```text
Assessment complete
```

You still need to know:

* Was the entire scope assessed?
* Did the scan complete?
* Were important hosts reachable?
* Did authentication work?
* Were there scan errors?
* Is the finding supported by evidence?
* Are other hosts still being assessed?

The first finding is only an observation.

---

# 12. Stage 6 — Completion

When the scan reaches a completed state, do not immediately jump to remediation.

First ask:

```text
Did the assessment complete normally?
        ↓
Was the intended scope assessed?
        ↓
Were there important errors?
        ↓
Did authentication behave as expected?
        ↓
Are the results sufficiently complete?
```

A completed scan is the beginning of analysis.

---

# 13. Completed Does Not Mean Validated

There are several separate concepts:

```text
Scan completed
      ↓
Results available
      ↓
Finding investigated
      ↓
Finding validated
      ↓
Finding prioritized
      ↓
Finding reported
```

Do not collapse these into one step.

For example:

```text
Nessus reports vulnerability
        ↓
You inspect the finding
        ↓
You examine evidence
        ↓
You determine whether validation is necessary
        ↓
You validate safely if required
```

---

# 14. Stage 7 — Stopped

A scan may be intentionally stopped.

Possible reasons include:

* Incorrect target discovered.
* Unexpected production impact.
* Wrong scope.
* Target instability.
* Excessive load.
* Incorrect configuration.
* Authorization issue.
* Emergency change in testing conditions.
* Need to correct the assessment configuration.

Stopping a scan is not automatically a failure.

The important question is:

> "Why was it stopped, and what does that mean for the completeness of the results?"

---

# 15. Safe Stopping

If you discover a serious problem while scanning:

```text
Unexpected impact
       ↓
Assess immediate risk
       ↓
Stop assessment if appropriate
       ↓
Record what happened
       ↓
Determine affected scope
       ↓
Correct the cause
       ↓
Decide whether a new assessment is required
```

Do not continue simply because the scan is already in progress.

Safety takes priority over completion.

---

# 16. Stopped Results Are Not Automatically Useless

A stopped assessment may still contain useful evidence.

For example:

```text
Target A
  → assessed successfully

Target B
  → partially assessed

Target C
  → not reached
```

The correct conclusion is not:

> "The scan is useless."

Instead:

> "The scan provides evidence for the portion that was successfully assessed, but coverage must be understood before drawing conclusions."

Record the limitation.

---

# 17. Stage 8 — Failed

A failed assessment requires investigation.

Possible causes include:

* Scanner problem.
* Target problem.
* Network problem.
* Authentication problem.
* Configuration problem.
* Resource problem.
* Plugin/update issue.
* Permission problem.
* Product/version-specific issue.

Use this troubleshooting sequence:

```text
What failed?
     ↓
When did it fail?
     ↓
Which target(s) were affected?
     ↓
What errors were reported?
     ↓
Was the scanner healthy?
     ↓
Was the target reachable?
     ↓
Were credentials involved?
     ↓
Was configuration appropriate?
     ↓
Can the cause be corrected safely?
```

Do not immediately rerun the exact same scan without understanding the failure.

---

# 18. Rerun vs Continue vs Investigate

When something goes wrong, distinguish these actions.

## Continue Monitoring

Use when:

* The scan is active.
* Progress is occurring.
* No serious problem is observed.

## Investigate

Use when:

* Progress stops.
* Errors appear.
* Authentication behaves unexpectedly.
* Target behavior is abnormal.
* Results look inconsistent with expectations.

## Stop

Use when:

* Scope is wrong.
* Safety is compromised.
* Unexpected impact occurs.
* Authorization is uncertain.
* The assessment should not continue.

## Rerun

Use when:

* The previous assessment cannot answer the objective.
* Configuration was incorrect.
* Important targets were missed.
* Authentication was corrected.
* A failure was understood and fixed.

---

# 19. Do Not Confuse Rerunning With Resuming

These are different operational ideas.

A resume-like behavior, where available, continues an interrupted operation.

A rerun starts another assessment execution.

The correct choice depends on:

* Nessus version/edition.
* Why the scan stopped.
* Whether configuration changed.
* Whether the original results are usable.
* Whether the assessment needs a clean execution.

Do not assume that restarting an assessment automatically produces an equivalent result.

---

# 20. Result Completeness

After completion, determine whether the result set is sufficiently complete.

Use this model:

```text
INTENDED SCOPE
      ↓
ACTUALLY REACHED
      ↓
ACTUALLY ASSESSED
      ↓
RESULTS PRODUCED
      ↓
RESULTS SUFFICIENT?
```

Questions:

### Scope

Did Nessus assess every intended target?

### Reachability

Could the scanner communicate with the target?

### Authentication

If authentication was expected, did it succeed?

### Coverage

Did the chosen workflow perform the checks needed for the objective?

### Errors

Were important errors reported?

### Timing

Did the scan finish normally?

### Evidence

Do important findings contain useful supporting evidence?

---

# 21. Coverage Is More Important Than the Number of Findings

Suppose two scans produce:

```text
Scan A → 35 findings
Scan B → 8 findings
```

You cannot conclude that Scan A was better simply because it found more.

The meaningful questions are:

* Were the same targets assessed?
* Was authentication equivalent?
* Were the workflows equivalent?
* Were the same plugin families involved?
* Were there environmental differences?
* Did either scan experience errors?
* Was the objective the same?

A smaller result set can be perfectly reasonable.

---

# 22. Scan History

Use scan history when you need to understand how an assessment changed over time.

Useful questions include:

* When was the assessment executed?
* Was it run more than once?
* Did the target set change?
* Did configuration change?
* Did findings change?
* Did authentication change?
* Was a previous run incomplete?
* Was a later run performed after remediation?

Think of history as an assessment timeline:

```text
Run 1
  ↓
Initial Findings
  ↓
Remediation
  ↓
Run 2
  ↓
Remaining Findings
  ↓
Retest
  ↓
Verification
```

Do not interpret differences between runs without checking what changed between them.

---

# 23. Why Results Can Change

Two scans of the same target can produce different results.

Possible reasons include:

* Target changed.
* Software changed.
* Configuration changed.
* Credentials changed.
* Authentication succeeded or failed.
* Network conditions changed.
* Scanner configuration changed.
* Plugin coverage changed.
* Vulnerability state changed.
* Temporary service behavior changed.
* Previous remediation changed the environment.

Therefore:

> **Different result ≠ automatically new vulnerability or removed vulnerability.**

Investigate the reason for the difference.

---

# 24. The Assessment Record

For every meaningful assessment, record enough information to reconstruct what happened.

At minimum:

```text
Assessment:
Date:
Objective:
Authorized Scope:
Targets:
Exclusions:
Workflow:
Authentication:
Important Settings:
Start:
End:
Final State:
Major Errors:
Major Findings:
Coverage Limitations:
Validation Required:
Next Action:
```

For repeated assessments, also record:

```text
Previous Assessment:
What Changed:
Why It Changed:
Observed Difference:
Interpretation:
```

This becomes especially important when an assessment is part of remediation verification.

---

# 25. A Practical Lifecycle Decision Tree

Use this decision tree when operating Nessus:

```text
ASSESSMENT CREATED
       ↓
Is scope correct?
       │
   ┌───┴───┐
   NO     YES
   │       │
Fix      Continue
   │       ↓
   │   Is configuration appropriate?
   │       │
   │   ┌───┴───┐
   │   NO     YES
   │   │       │
   │  Fix     Launch
   │           ↓
   │     Is scan progressing?
   │           │
   │      ┌────┴────┐
   │     YES       NO
   │      │         │
   │  Monitor    Investigate
   │      │         │
   │      └────┬────┘
   │           ↓
   │      Scan finishes?
   │           │
   │      ┌────┴────┐
   │   COMPLETE    FAIL/STOP
   │      │           │
   │      │       Investigate
   │      │           │
   │      ↓           ↓
   │   Check       Correct cause
   │   coverage        │
   │      │            ↓
   │      └──────→ Rerun if needed
   │
   ↓
RESULT ANALYSIS
```

---

# 26. Practical Exercise 1 — Observe a Normal Lifecycle

Use your authorized Nessus lab.

Perform:

1. Create a simple assessment.
2. Select an appropriate general vulnerability-assessment workflow.
3. Use one authorized lab target.
4. Keep the configuration simple.
5. Launch the assessment.
6. Observe its running state.
7. Record any visible progress information.
8. Wait for completion.
9. Record the final state.
10. Open the results.

Record:

```text
Target:
Workflow:
Start:
End:
Final State:
Hosts Assessed:
Major Findings:
Errors/Warnings:
Coverage Notes:
```

### Goal

Become comfortable watching a scan without interfering unnecessarily.

---

# 27. Practical Exercise 2 — Identify the Difference Between Progress and Completion

Run another authorized assessment.

While it is running, answer:

1. Is the assessment active?
2. Is there evidence of progress?
3. Are findings appearing?
4. Are there errors?
5. Has the intended target been reached?
6. What would make you investigate rather than continue waiting?

Do not stop the scan simply to create an artificial failure.

The objective is to learn observation.

---

# 28. Practical Exercise 3 — Inspect an Incomplete Result

If your lab allows it safely:

1. Start an assessment.
2. Stop it intentionally after meaningful activity has occurred.
3. Open the resulting information.
4. Determine what information remains available.
5. Identify what portion of the assessment was actually completed.
6. Record what cannot safely be concluded.

Use:

```text
Assessment State:
Targets Intended:
Targets Reached:
Evidence Available:
Known Limitations:
Can This Answer the Original Objective?
Why / Why Not?
```

Do this only in an isolated, authorized environment.

---

# 29. Practical Exercise 4 — Compare Two Runs

Perform two assessments against the same authorized lab target.

For the second run, make one controlled change, such as:

* Different authentication state.
* A documented target configuration change.
* A remediation performed between runs.
* Another deliberate assessment configuration change.

Then compare:

```text
Run 1
↓
Observed Results
↓
Environmental / Configuration Change
↓
Run 2
↓
Observed Results
↓
Explain the Difference
```

The goal is not to memorize the result.

The goal is to explain why the result changed.

---

# 30. Practical Exercise 5 — Decide What Happens Next

For each scenario, determine the next operational action.

### Scenario A

The scan is running and making steady progress.

**Your decision:**

Continue monitoring.

### Scenario B

The scan finishes normally and covers the intended target.

**Your decision:**

Begin result analysis.

### Scenario C

The scan fails and reports a credential problem.

**Your decision:**

Investigate authentication before rerunning.

### Scenario D

The scan is targeting a system that turns out to be outside the authorized scope.

**Your decision:**

Stop the assessment and correct the scope.

### Scenario E

The scan finishes, but an important target was unreachable.

**Your decision:**

Treat coverage as incomplete and determine whether another assessment is required.

### Scenario F

The scan reports a severe finding.

**Your decision:**

Investigate the finding and its evidence; do not treat severity alone as proof that the entire assessment is complete.

---

# 31. Troubleshooting Guide

## Scan Does Not Start

Check:

```text
Scanner availability
       ↓
Configuration validity
       ↓
Target validity
       ↓
Scheduling
       ↓
Product/license state
       ↓
Reported errors
```

---

## Scan Appears Stuck

Check:

* Is there actual progress?
* Are targets responding?
* Are there errors?
* Is the scanner under heavy load?
* Is the target rate-limiting?
* Are network controls interfering?
* Is authentication causing delays?

Do not repeatedly restart without identifying the cause.

---

## Scan Completes Too Quickly

Ask:

* Was the intended target actually reachable?
* Was the correct target entered?
* Was the scope correct?
* Did the scan encounter an immediate error?
* Was the selected workflow appropriate?
* Were important targets excluded?

A very fast scan is not automatically good or bad.

Interpret it in context.

---

## Scan Produces No Findings

Do not immediately conclude:

> "The system is secure."

Instead check:

```text
Was target reachable?
        ↓
Was scope correct?
        ↓
Did assessment execute normally?
        ↓
Were relevant checks performed?
        ↓
Did authentication matter?
        ↓
Were there errors?
        ↓
Is further validation required?
```

---

## Authentication Fails

Determine:

* Which authentication method was used?
* Was the account correct?
* Was the credential valid?
* Is the target configured for that authentication method?
* Are required permissions available?
* Is network access permitted?
* Did the scan provide authentication-related evidence?

Then decide whether another run is necessary.

---

# 32. Professional Habit — Always Ask "What Happened?"

After every assessment, be able to answer:

```text
What did I intend to assess?
What did I actually assess?
How did I assess it?
Did the assessment execute normally?
What did Nessus observe?
What evidence supports the important findings?
What limitations remain?
What needs validation?
What happens next?
```

If you cannot answer these questions, the assessment is not yet fully understood.

---

# 33. Operator Checklist

Before launch:

```text
[ ] Scope confirmed
[ ] Target confirmed
[ ] Objective defined
[ ] Workflow selected
[ ] Configuration reviewed
[ ] Authentication reviewed
[ ] Impact considered
```

During execution:

```text
[ ] Scan actually started
[ ] Progress observed
[ ] Target behavior monitored
[ ] Errors reviewed
[ ] Authentication behavior checked where applicable
[ ] Unexpected impact monitored
```

After completion:

```text
[ ] Final state confirmed
[ ] Intended scope assessed
[ ] Coverage checked
[ ] Errors reviewed
[ ] Important findings identified
[ ] Evidence inspected
[ ] Limitations recorded
[ ] Next action defined
```

---

# 34. Completion Criteria

You have completed this file when you can independently:

* Explain the Nessus scan lifecycle.
* Perform a pre-launch review.
* Distinguish configuration problems from execution problems.
* Monitor a running assessment.
* Recognize meaningful progress.
* Investigate an apparently stalled assessment.
* Decide when stopping is appropriate.
* Explain what an incomplete scan means.
* Distinguish scan completion from finding validation.
* Interpret changes between assessment runs.
* Use scan history as part of an assessment timeline.
* Determine whether results are sufficiently complete for the original objective.
* Decide whether to continue, investigate, stop, or rerun.
* Record a useful assessment lifecycle summary.

The final test is simple:

> **Given an authorized Nessus assessment that is currently running, stopped, failed, or completed, can you determine what happened, whether the results are trustworthy enough for the next step, and what that next step should be?**

If yes, you understand the scan lifecycle.
