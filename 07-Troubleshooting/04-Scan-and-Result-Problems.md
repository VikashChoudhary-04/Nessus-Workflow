# Scan and Result Problems

## Objective

Learn how to troubleshoot Nessus assessments that:

* Fail to start
* Stop unexpectedly
* Remain running longer than expected
* Complete with incomplete coverage
* Produce unexpected results
* Produce fewer findings than expected
* Produce substantially more findings than expected
* Show inconsistent results between assessments
* Produce results that are difficult to interpret
* Appear to have changed after a configuration, plugin, credential, or environment change

The objective is not simply to make a scan complete.

The objective is to determine:

> **What happened during the assessment, what evidence supports that conclusion, whether the resulting data is usable, and what should happen next?**

---

# 1. Scan Troubleshooting Mental Model

Use this model:

```text
ASSESSMENT DEFINITION
        ↓
CONFIGURATION
        ↓
PRE-LAUNCH CHECKS
        ↓
LAUNCH
        ↓
TARGET REACHABILITY
        ↓
DISCOVERY
        ↓
ASSESSMENT
        ↓
PLUGIN EXECUTION
        ↓
RESULT COLLECTION
        ↓
ASSESSMENT COMPLETION
        ↓
RESULT REVIEW
        ↓
COVERAGE DECISION
        ↓
VALIDATION
        ↓
INTERPRETATION
```

A problem at an earlier stage can affect everything afterward.

For example:

```text
Target unreachable
        ↓
No service discovery
        ↓
No vulnerability checks
        ↓
Few or no findings
```

The final result may look clean while actually representing incomplete coverage.

---

# 2. First Question: What Exactly Is Wrong?

Do not begin by rerunning the scan.

First classify the problem.

Common categories include:

| Problem                  | Initial question                                                                     |
| ------------------------ | ------------------------------------------------------------------------------------ |
| Scan will not start      | Is the scanner ready and is the configuration valid?                                 |
| Scan starts then stops   | Why did execution stop?                                                              |
| Scan remains running     | Is meaningful progress occurring?                                                    |
| Scan completes quickly   | Was the target actually reached?                                                     |
| No findings              | Was coverage sufficient?                                                             |
| Too many findings        | Are they real findings, duplicates, informational results, or configuration effects? |
| Findings changed         | What changed between assessments?                                                    |
| Results are inconsistent | Were the two assessments actually comparable?                                        |
| Findings disappeared     | Was the issue remediated or did assessment visibility change?                        |
| Results are incomplete   | Which hosts/services/checks were not assessed?                                       |
| Scan failed              | What evidence identifies the failure cause?                                          |

The correct troubleshooting path depends on the problem category.

---

# 3. Scan State Comes Before Result Interpretation

Before interpreting findings, determine the assessment state.

Possible states vary by Nessus version and interface, but conceptually include:

```text
Draft
  ↓
Configured
  ↓
Queued
  ↓
Running
  ↓
Completed
```

and potentially:

```text
Stopped
Failed
Interrupted
Partially completed
```

The exact labels may differ.

The important distinction is:

> **A result produced by an assessment is not automatically a complete result.**

---

# 4. Scan Will Not Start

If an assessment cannot start, investigate the execution path.

Use:

```text
Configuration
     ↓
Scanner
     ↓
Target
     ↓
Scheduling / Queue
     ↓
Launch
```

Potential causes include:

* Scanner unavailable
* Scanner service problem
* Invalid or incomplete configuration
* Unsupported configuration
* Scheduling problem
* Scanner resource constraints
* Plugin/content readiness issue
* Target configuration problem
* User permissions
* Product/license/edition limitations
* Internal scanner state
* Competing workloads

Do not immediately recreate the assessment.

First determine why the existing configuration could not execute.

---

# 5. Scan Start Troubleshooting

Use this sequence:

```text
Is Nessus healthy?
      ↓
Is the scanner available?
      ↓
Is the assessment configuration valid?
      ↓
Is the target valid?
      ↓
Are required credentials/configuration available?
      ↓
Is the scan allowed to run now?
      ↓
Are scanner resources available?
      ↓
Does the assessment enter execution?
```

If the scan never enters execution, the target may not yet be the primary problem.

---

# 6. Scan Starts and Stops Unexpectedly

A scan that begins but stops unexpectedly requires a different investigation.

Possible causes include:

* Scanner service interruption
* System restart
* Resource exhaustion
* Network interruption
* Target instability
* Scanner crash
* Administrative stop
* Scheduling or operational change
* Product-level failure
* Unexpected environmental change

Determine:

1. When did it stop?
2. What stage had been reached?
3. Was the scanner healthy afterward?
4. Did other scans continue?
5. Did the target remain reachable?
6. Were there system or network changes?
7. Does the scan history provide useful evidence?
8. Is partial result data available?

---

# 7. Do Not Automatically Rerun a Stopped Scan

A stopped assessment may contain useful evidence.

Before rerunning, determine:

```text
What was completed?
What was not completed?
Which hosts were reached?
Which services were assessed?
Which findings were produced?
Why did execution stop?
```

Then decide:

```text
Continue/recover if appropriate
        OR
Investigate before rerun
        OR
Run a controlled replacement assessment
```

The correct action depends on the cause and required coverage.

---

# 8. Scan Remains Running

Long duration does not automatically mean failure.

A scan may take longer because of:

* Large target scope
* Many open services
* Extensive plugin coverage
* Authenticated checks
* Slow targets
* Network latency
* Timeouts
* Rate limiting
* Security controls
* Scanner resource constraints
* Concurrent workloads
* Specialized checks
* Unresponsive hosts

The key question is:

> **Is meaningful progress occurring?**

---

# 9. Slow vs Stuck

Use this distinction:

### Slow

```text
Scan is progressing
+
Targets are being assessed
+
Results are appearing
+
Scanner remains responsive
```

### Potentially stuck

```text
No meaningful progress
+
No new target activity
+
No new results
+
No expected state changes
+
Scanner or target shows abnormal behavior
```

Do not stop a scan simply because it takes longer than expected.

Investigate evidence of lack of progress first.

---

# 10. Long-Running Scan Investigation

Check:

* Current assessment state
* Target count
* Completed targets
* Remaining targets
* Findings being generated
* Scanner CPU
* Scanner memory
* Disk availability
* Network behavior
* Target responsiveness
* Authentication state
* Plugin execution behavior where visible
* Other concurrent assessments
* Timeouts
* Recent infrastructure changes

Compare actual progress with what the assessment is expected to do.

---

# 11. Resource Constraints

Scanner resources can affect execution.

Potential constraints include:

* CPU
* RAM
* Disk
* Network throughput
* Concurrent scan load
* System limits
* Virtual machine resource allocation

Do not immediately increase concurrency or aggressiveness.

First determine:

> **Is the scanner resource-constrained, and is changing the configuration justified?**

---

# 12. Target Resource Constraints

The scanner may be healthy while the target is struggling.

Potential symptoms:

* Slow responses
* Connection resets
* Intermittent availability
* Service restarts
* High target resource usage
* Security controls triggering
* Rate limiting
* Temporary blocking

This matters because increasing scanner aggressiveness can make the problem worse.

Use:

```text
Scanner behavior
      ↓
Target response
      ↓
Impact
      ↓
Assessment configuration
```

not:

```text
Scan is slow
      ↓
Make it more aggressive
```

---

# 13. Scan Completes Too Quickly

A very fast scan is not automatically good or bad.

Investigate whether:

* Target was reachable
* Expected hosts were discovered
* Expected ports were assessed
* Expected services were identified
* Authentication was attempted/successful where required
* Relevant plugins executed
* The target list was correct
* Scope was unintentionally reduced

A fast scan can indicate:

```text
Small/simple target
```

or:

```text
Incomplete reachability
```

or:

```text
Incorrect target
```

or:

```text
Reduced workflow coverage
```

Time alone is not evidence of quality.

---

# 14. No Findings

A scan with no findings requires a coverage investigation.

Do not immediately conclude:

> "The target has no vulnerabilities."

Ask:

```text
Was the intended target reached?
        ↓
Were expected ports/services discovered?
        ↓
Was the intended workflow used?
        ↓
Were relevant plugins enabled?
        ↓
Was authentication required?
        ↓
Did authentication succeed?
        ↓
Was sufficient evidence obtained?
        ↓
Was the assessment complete?
```

Only after answering these questions should you interpret the result.

---

# 15. No Findings Decision Tree

```text
No findings
    ↓
Was target reached?
    ├── No → Investigate network/target problem
    │
    └── Yes
         ↓
Were expected services found?
    ├── No → Investigate discovery/service visibility
    │
    └── Yes
         ↓
Was the correct workflow used?
    ├── No → Reassess workflow selection
    │
    └── Yes
         ↓
Were relevant plugins available/enabled?
    ├── No → Investigate plugin coverage
    │
    └── Yes
         ↓
Was authentication required?
    ├── Yes → Verify authentication and permissions
    │
    └── No
         ↓
Was assessment coverage sufficient?
    ├── No → Qualify result / reassess
    │
    └── Yes
         ↓
"No findings identified within the assessed scope
and available coverage."
```

That final statement is intentionally narrower than:

> "No vulnerabilities exist."

---

# 16. Too Many Findings

An unusually large finding set also requires investigation.

Possible causes include:

* Broad target scope
* Large plugin coverage
* Authenticated visibility
* Multiple hosts/services
* Duplicate or related findings
* Shared root causes
* Informational results
* Configuration findings
* Multiple instances of the same issue
* Newly updated plugin/content coverage
* Previously hidden visibility becoming available

Do not automatically assume the scanner is producing false positives.

First understand what changed.

---

# 17. Finding Volume vs Finding Quality

A large number of findings does not automatically mean:

```text
Large number of unique security problems
```

For example:

```text
20 findings
```

might represent:

```text
1 root cause
+
multiple affected hosts
+
multiple plugin detections
```

Group findings by:

* Root cause
* Affected technology
* Affected asset
* Remediation action
* Evidence source

This helps avoid double counting.

---

# 18. Unexpected Finding Changes

Suppose:

```text
Assessment A:
42 findings

Assessment B:
17 findings
```

Do not immediately conclude:

```text
25 vulnerabilities were fixed
```

Possible explanations include:

* Actual remediation
* Target scope change
* Host removal
* Service change
* Authentication failure
* Credential permission change
* Plugin/content update
* Nessus version change
* Scanner location change
* Configuration change
* Network filtering
* Target behavior change
* Finding validation state change

Use:

```text
Difference in findings
        ↓
What else changed?
        ↓
Compare assessment conditions
        ↓
Identify plausible cause
        ↓
Validate
        ↓
Interpret
```

---

# 19. The Comparability Rule

Two assessments can only be meaningfully compared when the relevant conditions are sufficiently similar.

Compare:

| Dimension             | Assessment A | Assessment B |
| --------------------- | ------------ | ------------ |
| Scope                 |              |              |
| Target set            |              |              |
| Scanner               |              |              |
| Scanner location      |              |              |
| Workflow              |              |              |
| Credentials           |              |              |
| Authentication result |              |              |
| Configuration         |              |              |
| Plugin/content state  |              |              |
| Nessus version        |              |              |
| Network path          |              |              |
| Target state          |              |              |

The more dimensions that changed, the more carefully the difference must be interpreted.

---

# 20. Finding Disappeared

A finding that disappears from a later assessment has several possible explanations.

Potential explanations include:

```text
Actual remediation
```

```text
Finding no longer applicable
```

```text
Target/service changed
```

```text
Authentication visibility changed
```

```text
Plugin/content changed
```

```text
Network visibility changed
```

```text
Scanner position changed
```

```text
Finding detection conditions changed
```

Therefore:

> **Missing from the latest result is not automatically equivalent to remediated.**

Investigate the reason for disappearance.

---

# 21. Finding Reappeared

A previously resolved finding can return.

Possible explanations include:

* Regression
* Reinstallation
* Configuration drift
* New affected host
* Service re-enabled
* Patch removed
* Credential visibility changed
* Plugin/content change
* Target replacement
* Environment migration

Track the lifecycle:

```text
Detected
   ↓
Investigated
   ↓
Validated
   ↓
Remediated
   ↓
Retested
   ↓
Resolved
   ↓
Reappeared?
   ↓
Investigate
```

A reappearing finding should not simply be marked as a new unrelated issue without investigation.

---

# 22. Incomplete Results

A scan may complete while still producing incomplete coverage.

Examples:

* Some hosts unreachable
* Some ports filtered
* Authentication failed on some targets
* Targets changed during execution
* Scanner resource constraints
* Service interruptions
* Assessment stopped early
* Some checks could not execute
* Network path changed

Separate:

```text
Scan completed
```

from:

```text
Assessment coverage was complete
```

These are not the same statement.

---

# 23. Coverage Review

Before accepting results, ask:

```text
Target Coverage
    ↓
Host Coverage
    ↓
Service Coverage
    ↓
Authentication Coverage
    ↓
Plugin Coverage
    ↓
Evidence Coverage
```

For each layer:

* What was intended?
* What was actually assessed?
* What was missed?
* Why was it missed?
* Does the limitation affect the assessment conclusion?

---

# 24. Unexpected Host Count

Suppose the assessment was intended for:

```text
10 hosts
```

but results show:

```text
7 hosts
```

Do not immediately assume the missing three are secure.

Investigate:

* Target definition
* DNS
* Routing
* Host availability
* Firewall
* Scanner position
* Discovery configuration
* Scope changes
* Dynamic infrastructure

Document the coverage limitation if the hosts could not be assessed.

---

# 25. Unexpected Service Count

Suppose the target normally exposes:

```text
22/tcp
80/tcp
443/tcp
```

but Nessus observes only:

```text
443/tcp
```

Possible causes include:

* Services actually changed
* Network filtering
* Scanner position
* Firewall rules
* Service binding
* Port configuration
* Discovery configuration
* Temporary service failure

Do not interpret missing services as proof that the services do not exist.

Investigate.

---

# 26. Authentication-Dependent Result Changes

Authenticated scans can reveal significantly more information than unauthenticated scans.

If an authenticated assessment produces more findings, that does not necessarily mean:

```text
The system became less secure.
```

It may mean:

```text
Assessment visibility increased.
```

Likewise, fewer findings can mean:

```text
Assessment visibility decreased.
```

Always compare authentication state.

---

# 27. Plugin and Content Changes

Plugin/content updates can affect result sets.

A later assessment may detect something that an earlier assessment did not because:

* Detection logic changed
* New checks became available
* Existing checks improved
* Plugin families changed
* New vulnerability intelligence became available

Likewise, a finding may change because the detection logic changed.

Therefore, when comparing results:

```text
Finding Difference
        ↓
Check Target Changes
        ↓
Check Configuration Changes
        ↓
Check Authentication Changes
        ↓
Check Plugin/Content Changes
        ↓
Check Nessus Version
        ↓
Interpret
```

Do not attribute every difference to remediation.

---

# 28. Version Changes

A Nessus upgrade can change behavior.

Potential effects include:

* UI changes
* Available settings
* Plugin/content behavior
* Detection logic
* Credential capabilities
* Reporting behavior
* Scheduling behavior
* Scanner behavior

When results change across a version boundary, record the version difference.

Do not assume that two scans performed with different Nessus versions are perfectly equivalent.

---

# 29. Configuration Changes

A configuration change can affect both coverage and findings.

Examples:

* Target change
* Port range change
* Discovery change
* Plugin selection change
* Credential change
* Performance change
* Advanced setting change
* Exclusion change
* Schedule change
* Scanner selection change

Use configuration history when available.

If configuration changed, ask:

> **Could this change explain the observed result difference?**

---

# 30. Scan Failed

When a scan fails, collect evidence before recreating it.

Record:

* Assessment name
* Target
* Scanner
* Start time
* Failure time
* Assessment state
* Error message where available
* Configuration
* Authentication state
* Network conditions
* Scanner health
* Resource state
* Other concurrent assessments
* Recent environment changes

Then classify the failure:

```text
Scanner
Network
Target
Authentication
Configuration
Plugin/content
Resource
Scheduling
Product/edition
Unknown
```

If the cause remains unknown:

> Record it as unresolved rather than inventing an explanation.

---

# 31. Scan Failure Decision Tree

```text
Scan failed
     ↓
Did Nessus remain healthy?
     ├── No → Scanner/platform troubleshooting
     │
     └── Yes
          ↓
Did the scan reach execution?
     ├── No → Configuration/queue/scheduling investigation
     │
     └── Yes
          ↓
Did target communication work?
     ├── No → Network/target troubleshooting
     │
     └── Yes
          ↓
Did authentication work where required?
     ├── No → Credential/authentication troubleshooting
     │
     └── Yes
          ↓
Did assessment execution progress?
     ├── No → Scanner/resource/plugin investigation
     │
     └── Yes
          ↓
Were results produced?
     ├── No → Result/plugin/execution investigation
     │
     └── Yes
          ↓
Is coverage sufficient?
     ├── No → Investigate limitations
     │
     └── Yes
          ↓
Review results
```

---

# 32. Scan Timeout or Very Long Duration

A timeout or unusually long assessment should be investigated before simply increasing the timeout.

Possible causes include:

* Unreachable targets
* Filtered ports
* Slow services
* Repeated connection attempts
* Network latency
* Security controls
* Target resource constraints
* Excessive target scope
* Excessive plugin coverage
* Scanner resource constraints
* Concurrent assessments

Ask:

> **What part of the workflow is consuming the time?**

Do not treat timeout extension as the first solution.

---

# 33. Performance Troubleshooting

Use a controlled approach.

### Step 1

Establish baseline:

```text
Target count:
Typical duration:
Current duration:
Scanner resources:
Network conditions:
```

### Step 2

Identify what changed.

### Step 3

Determine whether the delay is:

```text
Scanner-side
Network-side
Target-side
Configuration-side
Plugin/check-side
```

### Step 4

Change one meaningful variable.

### Step 5

Retest.

### Step 6

Compare.

This produces a useful troubleshooting record.

---

# 34. Do Not Increase Aggressiveness Blindly

A common reaction to a slow scan is:

```text
Scan slow
↓
Increase concurrency
↓
Increase intensity
↓
Reduce timeouts
```

This can:

* Increase target load
* Increase network load
* Trigger security controls
* Increase instability
* Produce less reliable results
* Make troubleshooting harder

Instead determine why the scan is slow.

Performance tuning should be evidence-driven.

---

# 35. Results Appear Inconsistent

Suppose two assessments of the same host produce different findings.

First verify whether they are actually comparable.

Check:

```text
Same target?
Same target state?
Same scanner?
Same network position?
Same workflow?
Same credentials?
Same authentication state?
Same configuration?
Same plugin/content state?
Same Nessus version?
Same relevant service state?
```

If several answers are "no," the result difference may be expected.

---

# 36. Reproducibility Test

When an unexpected result appears:

```text
Unexpected Result
      ↓
Document Conditions
      ↓
Repeat Under Same Conditions
      ↓
Compare Evidence
      ↓
Determine Reproducibility
```

If the result is reproducible, investigate the detection.

If it is not reproducible, investigate environmental or execution differences.

Do not immediately classify an inconsistent finding as false positive.

---

# 37. False Positive vs Incomplete Assessment

These are different.

### Possible false positive

```text
Assessment reached target
+
Relevant evidence collected
+
Evidence conflicts with finding
```

### Incomplete assessment

```text
Assessment could not obtain required evidence
```

For example:

```text
Authentication failed
↓
Local package information unavailable
↓
Version-based finding not assessed as intended
```

This is not automatically a false positive.

It may be an assessment coverage limitation.

---

# 38. Finding Evidence Changed

A finding can appear different between scans because the evidence changed.

Compare:

* Host
* Port
* Service
* Version
* Configuration
* Authentication state
* Plugin output
* Target state
* Network position

Then determine whether the underlying condition changed.

---

# 39. Practical Lab 1 — No Findings

## Objective

Learn to distinguish a clean result from incomplete coverage.

## Scenario

Run an authorized assessment against a lab target expected to expose at least one assessable service.

## Procedure

1. Define the target.
2. Select an appropriate workflow.
3. Run the assessment.
4. Review the result.
5. If no findings appear, do not immediately conclude the target is secure.
6. Verify target reachability.
7. Verify expected services.
8. Review plugin coverage.
9. Check authentication requirements.
10. Review assessment completeness.
11. Determine whether "no findings" is meaningful.

## Success Condition

You can explain:

> Whether the assessment produced no identified findings under sufficient coverage, or whether the result is limited by incomplete assessment visibility.

---

# 40. Practical Lab 2 — Interrupted Assessment

## Objective

Learn how to determine whether a stopped assessment can be used.

## Procedure

1. Start an authorized lab assessment.
2. Allow meaningful progress.
3. Stop it using an authorized control.
4. Review the resulting state.
5. Determine which targets were assessed.
6. Review available findings.
7. Identify missing coverage.
8. Decide whether to:

   * Continue if supported and appropriate
   * Run a replacement assessment
   * Investigate configuration/environment first
9. Document the decision.

## Success Condition

You can explain:

> What evidence from the interrupted assessment is usable and what coverage remains unverified.

---

# 41. Practical Lab 3 — Finding Count Changed

## Objective

Learn to investigate finding-count changes without assuming remediation.

## Scenario

Assessment A:

```text
30 findings
```

Assessment B:

```text
12 findings
```

## Procedure

Compare:

* Scope
* Targets
* Scanner
* Network position
* Authentication
* Permissions
* Configuration
* Plugin/content state
* Nessus version
* Target state

Determine the most evidence-supported explanation.

## Success Condition

You can distinguish between:

```text
Actual remediation
```

and:

```text
Changed assessment visibility
```

---

# 42. Practical Lab 4 — Scan Takes Much Longer

## Objective

Diagnose a performance problem without blindly increasing aggressiveness.

## Scenario

A scan normally takes:

```text
20 minutes
```

but now takes:

```text
90 minutes
```

## Procedure

Investigate:

1. Target count.
2. Target reachability.
3. Service response time.
4. Scanner CPU.
5. Scanner memory.
6. Network behavior.
7. Authentication.
8. Plugin/content state.
9. Concurrent scans.
10. Target-side changes.
11. Configuration changes.

Identify the changed layer.

## Success Condition

You can explain why the scan became slower and identify an evidence-based corrective action.

---

# 43. Practical Lab 5 — Finding Disappears

## Objective

Determine whether a disappeared finding represents remediation.

## Procedure

1. Identify the original finding.
2. Record affected host/service.
3. Record original evidence.
4. Compare the later assessment.
5. Verify target scope.
6. Verify service state.
7. Verify authentication.
8. Compare plugin/content state.
9. Investigate remediation evidence.
10. Perform controlled validation where appropriate.
11. Determine the most accurate state.

Possible outcomes:

```text
Remediated
Still present
Not applicable
Condition changed
Unable to verify
Assessment visibility changed
```

Do not select "remediated" solely because the finding is absent.

---

# 44. Practical Lab 6 — Inconsistent Results

## Objective

Learn to identify why repeated assessments produce different results.

## Procedure

Run two authorized assessments under deliberately controlled conditions.

Then change one variable, such as:

* Authentication state
* Target service state
* Scanner
* Configuration
* Target scope
* Plugin/content state

Run the assessment again.

Compare results.

## Success Condition

You can connect the observed result change to the changed assessment condition.

---

# 45. Practical Lab 7 — Partial Target Coverage

## Objective

Learn how incomplete host coverage affects conclusions.

## Scenario

Authorized scope:

```text
10 hosts
```

Actual assessment:

```text
8 reachable
2 unreachable
```

## Procedure

1. Identify the missing hosts.
2. Determine why they were unreachable.
3. Review whether they were included in the intended scope.
4. Determine whether the missing hosts affect the assessment objective.
5. Document the limitation.
6. Troubleshoot the missing hosts.
7. Retest where appropriate.
8. Update the assessment conclusion.

## Success Condition

You can produce a clear coverage statement rather than treating the eight assessed hosts as representative of all ten without qualification.

---

# 46. Result Troubleshooting Record

For significant scan or result problems, record:

```text
Assessment:
Date:
Operator:
Objective:
Target:
Scanner:
Scanner position:

Expected behavior:
Observed behavior:

Assessment state:
Start time:
End time:
Duration:

Target coverage:
Host coverage:
Service coverage:
Authentication coverage:
Plugin/content state:

Observed problem:
Evidence:
Initial hypothesis:

Investigation performed:
Change made:
Retest:

Result:
Coverage impact:
Confidence:
Remaining limitation:

Final interpretation:
Next action:
```

Do not fabricate missing information.

Use:

```text
Unknown
```

or:

```text
Not verified
```

when appropriate.

---

# 47. Result Quality Gate

Before using Nessus results for reporting or remediation decisions, perform this quality gate.

## Scope

* [ ] Intended target scope confirmed
* [ ] Actual target coverage reviewed
* [ ] Unexpected targets investigated

## Execution

* [ ] Assessment completed or incomplete state understood
* [ ] Scanner remained healthy
* [ ] Major execution errors investigated
* [ ] Duration is understood where unusual

## Discovery

* [ ] Expected hosts considered
* [ ] Expected services considered
* [ ] Missing hosts/services investigated

## Authentication

* [ ] Authentication requirements identified
* [ ] Authentication state verified
* [ ] Partial authentication identified
* [ ] Permission limitations identified

## Coverage

* [ ] Relevant workflow selected
* [ ] Relevant plugins/content considered
* [ ] Important coverage limitations documented

## Findings

* [ ] Findings reviewed beyond severity counts
* [ ] Important findings investigated
* [ ] Duplicate/root-cause relationships considered
* [ ] Significant findings validated where appropriate

## Comparison

* [ ] Previous assessment conditions reviewed where applicable
* [ ] Scope changes checked
* [ ] Authentication changes checked
* [ ] Configuration changes checked
* [ ] Plugin/content changes checked
* [ ] Nessus version changes checked
* [ ] Scanner changes checked

## Reporting

* [ ] Evidence separated from interpretation
* [ ] Limitations documented
* [ ] No unsupported conclusions made
* [ ] Secrets excluded
* [ ] Next action identified

---

# 48. Troubleshooting Decision Rule

When a scan or result looks wrong, use:

```text
OBSERVE
   ↓
DEFINE THE PROBLEM
   ↓
CHECK ASSESSMENT STATE
   ↓
CHECK SCOPE
   ↓
CHECK TARGET COVERAGE
   ↓
CHECK NETWORK
   ↓
CHECK AUTHENTICATION
   ↓
CHECK CONFIGURATION
   ↓
CHECK PLUGIN/CONTENT STATE
   ↓
CHECK SCANNER HEALTH
   ↓
CHECK TARGET HEALTH
   ↓
COMPARE WITH BASELINE
   ↓
FORM HYPOTHESIS
   ↓
CHANGE ONE VARIABLE
   ↓
RETEST
   ↓
VERIFY
   ↓
DOCUMENT
```

This sequence prevents random troubleshooting.

---

# 49. Common Mistakes

## Mistake 1 — Rerunning immediately

Why it fails:

The original cause may disappear temporarily, leaving no useful explanation.

Better:

> Capture evidence before rerunning.

---

## Mistake 2 — Treating completion as complete coverage

Why it fails:

A scan can complete while missing targets, services, authentication, or checks.

Better:

> Review coverage independently of execution status.

---

## Mistake 3 — Treating no findings as secure

Why it fails:

The target may not have been adequately assessed.

Better:

> Verify coverage before interpreting the result.

---

## Mistake 4 — Treating fewer findings as remediation

Why it fails:

Visibility or assessment conditions may have changed.

Better:

> Compare assessment conditions.

---

## Mistake 5 — Treating more findings as deterioration

Why it fails:

Authentication, plugin updates, or improved visibility can reveal previously unseen issues.

Better:

> Identify what changed before interpreting finding volume.

---

## Mistake 6 — Increasing scan aggressiveness immediately

Why it fails:

It can increase load and obscure the actual cause.

Better:

> Diagnose first.

---

## Mistake 7 — Ignoring scanner health

Why it fails:

A target may be healthy while the scanner is resource-constrained or unstable.

Better:

> Check both sides.

---

## Mistake 8 — Ignoring target health

Why it fails:

A slow or unstable target can make a healthy scanner appear problematic.

Better:

> Correlate scanner and target behavior.

---

## Mistake 9 — Calling an unexplained finding a false positive

Why it fails:

Insufficient evidence is not the same as contradictory evidence.

Better:

> Distinguish false positive from unresolved or incomplete assessment.

---

## Mistake 10 — Ignoring configuration drift

Why it fails:

Small configuration changes can materially alter assessment coverage.

Better:

> Compare relevant settings between runs.

---

# 50. Professional Interpretation Standard

Before making a conclusion from Nessus results, be able to answer:

```text
What was assessed?
        ↓
How was it assessed?
        ↓
What evidence was obtained?
        ↓
What was not assessed?
        ↓
What findings were identified?
        ↓
Which findings were investigated?
        ↓
Which findings were validated?
        ↓
What changed from previous assessments?
        ↓
What remains uncertain?
        ↓
What should happen next?
```

A professional assessment is not simply:

```text
Scan completed
+
Export report
```

It is:

```text
Assessment
+
Coverage understanding
+
Evidence
+
Investigation
+
Validation
+
Interpretation
+
Action
```

---

# 51. Completion Criteria

You have completed this troubleshooting workflow when you can independently:

* Diagnose scans that fail to start.
* Investigate scans that stop unexpectedly.
* Distinguish slow from potentially stuck assessments.
* Investigate unusually long scans.
* Investigate scans that complete suspiciously quickly.
* Explain why a no-finding result may be incomplete.
* Investigate unexpectedly large finding sets.
* Compare finding changes across assessments.
* Distinguish remediation from reduced assessment visibility.
* Recognize partial target coverage.
* Investigate authentication-dependent result changes.
* Account for plugin/content changes.
* Account for Nessus version changes.
* Identify configuration-driven result differences.
* Investigate inconsistent results.
* Distinguish false positives from incomplete evidence.
* Decide whether a result is sufficiently reliable for reporting.
* Document unresolved uncertainty accurately.
* Choose an appropriate next action based on evidence.

The final skill is not:

> "I know how to rerun a Nessus scan."

It is:

> **"I can determine why an assessment or result behaves unexpectedly, establish whether the resulting evidence is trustworthy and sufficiently complete, identify the cause of meaningful differences, and choose the next action without guessing."**

---

# Final Troubleshooting Mental Model

The complete troubleshooting model for this section is:

```text
AUTHORIZATION
      ↓
TARGET
      ↓
SCANNER POSITION
      ↓
SCANNER HEALTH
      ↓
NETWORK
      ↓
TARGET HEALTH
      ↓
SERVICE
      ↓
AUTHENTICATION
      ↓
CONFIGURATION
      ↓
PLUGIN / CONTENT STATE
      ↓
ASSESSMENT EXECUTION
      ↓
RESULT COLLECTION
      ↓
COVERAGE
      ↓
EVIDENCE
      ↓
VALIDATION
      ↓
COMPARISON
      ↓
INTERPRETATION
      ↓
NEXT ACTION
```

When a Nessus assessment behaves unexpectedly, do not ask only:

> **"How do I make the scan work?"**

Ask:

> **"Where did the expected assessment workflow diverge, what evidence proves the divergence, how does it affect coverage and interpretation, and what is the safest evidence-based next action?"**
