# First Scan

## Objective

Perform your first complete Nessus vulnerability assessment against an authorized laboratory target.

By the end of this workflow, you should be able to:

* define the assessment objective
* verify scope
* select an appropriate starting workflow
* configure a target
* review the important scan settings
* launch the assessment
* monitor its progress
* open the results
* identify findings
* inspect basic finding information
* determine whether the scan produced the expected result
* record the assessment outcome

This is your first complete Nessus workflow.

Do not optimize the scan yet.

The goal is to understand the complete lifecycle:

```text
Scope
  ↓
Objective
  ↓
Target
  ↓
Workflow
  ↓
Configuration
  ↓
Launch
  ↓
Monitor
  ↓
Results
  ↓
Review
```

---

# 1. Assessment Scenario

You are performing an authorized vulnerability assessment against a laboratory system.

### Objective

> Identify vulnerabilities detectable by Nessus on the authorized laboratory target.

### Constraints

```text
Only the authorized laboratory target may be scanned.

Do not scan unrelated systems.

Use the default configuration unless a setting
is required for the assessment.

Do not intentionally increase scan aggressiveness.
```

---

# 2. Define the Assessment

Before opening the scan configuration, record:

```text
Assessment Objective:
Identify vulnerabilities detectable by Nessus
on the authorized laboratory target.

Scope:
________________________________

Target:
________________________________

Authentication:
Unauthenticated for this first assessment

Expected Result:
Nessus completes the assessment and produces
results for the target.
```

Replace the blank values with your actual lab target.

Do not invent an address.

---

# 3. Verify Authorization

Before entering the target into Nessus, confirm:

```text
[ ] I own the target or have explicit authorization.
[ ] The target is part of my laboratory.
[ ] The target is within the intended scope.
[ ] I am not scanning an unintended network.
```

If any answer is uncertain:

```text
STOP
↓
Clarify the target/scope
↓
Continue only when confirmed
```

---

# 4. Verify Target Reachability

Before launching Nessus, establish that the target should be reachable from the scanner.

The exact network-testing method depends on your operating system and lab.

You are not performing vulnerability testing yet.

You are answering:

> **Can the scanner reasonably reach the intended target?**

Record:

```text
Target:
____________________________

Expected Reachability:
____________________________

Observed:
____________________________
```

If the target is unreachable, do not immediately create increasingly aggressive Nessus configurations.

First determine whether the problem is:

* target state
* network path
* firewall
* routing
* incorrect target address
* scanner connectivity

---

# 5. Open Scan Creation

In Nessus, navigate to the area used to create a new scan.

Use the UI knowledge from:

```text
01-Setup-and-UI/02-UI-Navigation.md
```

Do not search for a tutorial.

Find the control yourself.

Then inspect the available scan templates.

---

# 6. Choose the Starting Workflow

Your objective is:

> Identify vulnerabilities on an authorized laboratory target.

For this first exercise, select the appropriate basic vulnerability-assessment workflow available in your Nessus edition.

Do not choose a specialized workflow simply because it sounds more advanced.

The decision should be:

```text
Objective:
Vulnerability assessment
        ↓
Need a general vulnerability assessment
        ↓
Choose the appropriate general vulnerability
assessment template
```

The exact template name can vary by Nessus version and edition.

If the available templates differ from an older tutorial:

```text
Check your Nessus version/edition
        ↓
Check current Tenable documentation
        ↓
Choose the currently supported general
vulnerability-assessment workflow
```

---

# 7. Name the Scan

Give the assessment a useful name.

Avoid:

```text
scan1
test
abc
new scan
```

Prefer something that identifies:

```text
Purpose + Target + Assessment Type
```

Example:

```text
Lab-01-Unauthenticated-Vulnerability-Assessment
```

Use a name appropriate for your environment.

Record it:

```text
Scan Name:
____________________________
```

A useful name becomes increasingly important when you have many assessments.

---

# 8. Configure the Target

Enter only the authorized laboratory target.

Verify it before continuing.

```text
Target:
____________________________
```

Then ask:

```text
Is this definitely the system I intend to assess?
```

Do not rely only on memory.

Confirm the target from your lab documentation.

---

# 9. Review the Configuration

Before launching the scan, inspect the configuration.

You are not trying to understand every available setting yet.

At this stage, identify the major configuration areas.

Look for areas related to:

```text
Basic
Discovery
Assessment
Report
Advanced
Credentials
Plugins
```

The exact categories depend on the selected template and Nessus version.

The purpose of this step is to understand:

> **What will Nessus actually do if I launch this scan?**

---

# 10. Do Not Change Advanced Settings Yet

For the first scan:

> **Use the default configuration unless the assessment objective requires a change.**

Do not change settings simply because they exist.

Do not deliberately increase:

* scan aggressiveness
* concurrency
* performance settings
* timeout values
* plugin scope
* discovery behavior

The first scan is a baseline.

Later, you will learn when these settings should be changed.

---

# 11. Authentication Decision

For this first scan:

```text
Authentication:
Unauthenticated
```

Why?

Because we are first learning the basic network-based assessment lifecycle.

We will later perform authenticated assessments and compare their behavior.

The important concept is:

> **The correct authentication choice depends on the assessment objective.**

It is not a permanent rule that all Nessus scans should be authenticated.

---

# 12. Pre-Launch Safety Review

Before starting the scan:

```text
[ ] Correct scan name
[ ] Correct target
[ ] Target is authorized
[ ] Target is in scope
[ ] Correct assessment workflow
[ ] Authentication state is understood
[ ] No unnecessary advanced changes
[ ] Expected scan impact is acceptable
```

Then ask:

> **If I click Launch now, do I understand what Nessus is about to do?**

If the answer is no, stop and inspect the configuration again.

---

# 13. Launch the Scan

Start the scan.

Do not immediately leave the page.

Observe the initial state.

Record:

```text
Start Time:
____________________________

Initial State:
____________________________
```

The exact interface and status terminology can vary by version.

---

# 14. Monitor the Scan

While the scan is running, observe the available information.

Look for information such as:

* scan state
* elapsed time
* progress
* targets
* hosts discovered
* findings
* status messages
* other available execution information

Do not repeatedly interrupt or restart the scan.

The purpose is to learn how Nessus communicates assessment progress.

---

# 15. What Are You Looking For?

During execution, ask:

```text
Is the scan running?
Is the intended target being assessed?
Is the scan progressing?
Are there unexpected errors?
Is the scanner behaving normally?
```

Do not assume that a scan taking longer than expected has failed.

A scan's duration can depend on:

* target responsiveness
* number of checks
* services
* network conditions
* configuration
* scanner resources
* other environmental factors

---

# 16. Decision Point — Is the Scan Behaving Normally?

Use:

```text
Scan Running
    ↓
Is there evidence of abnormal behavior?
       │
   ┌───┴───┐
   │       │
  NO      YES
   │       │
   ↓       ↓
Continue  Investigate
```

### If normal

Allow the assessment to continue.

### If abnormal

Do not immediately change random settings.

Record:

```text
Observed Problem:
____________________________

Expected Behavior:
____________________________

What is different?
____________________________
```

Troubleshooting will be covered more extensively later.

---

# 17. Wait for Completion

Allow the assessment to complete unless there is a clear reason to stop it.

When it finishes, record:

```text
End Time:
____________________________

Final State:
____________________________
```

Determine whether Nessus considers the assessment completed successfully.

Do not assume:

```text
Browser stopped showing activity
```

means:

```text
Assessment completed successfully
```

Verify the actual scan state.

---

# 18. Open the Results

Open the completed scan.

You should now move from:

```text
Assessment Execution
```

to:

```text
Assessment Analysis
```

The question changes from:

> "Is Nessus running?"

to:

> **"What did Nessus discover?"**

---

# 19. Identify the Assessed Host

Locate the target/host information in the results.

Confirm that the expected laboratory target appears.

Record:

```text
Assessed Host:
____________________________
```

If the expected target does not appear, stop and investigate before assuming the scan was successful.

---

# 20. Review the Finding Summary

Inspect the available result summary.

Look for:

* total findings
* severity categories
* affected hosts
* informational findings
* vulnerability findings
* other available result information

Record:

```text
Critical:
________________

High:
________________

Medium:
________________

Low:
________________

Informational:
________________
```

Do not interpret the numbers yet.

This exercise is about locating and reading the results.

---

# 21. Understand What the Summary Does Not Tell You

Suppose the scan reports:

```text
0 Critical
0 High
2 Medium
5 Low
```

Do not conclude:

> "The system is secure."

The summary does not tell you everything.

You still need to determine:

```text
What was actually assessed?
Were credentials used?
Was the target reachable?
Which services were visible?
Were there assessment limitations?
What checks actually ran?
```

A result is meaningful only in the context of how the assessment was performed.

---

# 22. Open an Individual Finding

Choose one finding.

Do not choose based only on severity.

Open its details.

Identify the information Nessus provides.

Look for items such as:

```text
Plugin
Severity
Affected Host
Description
Evidence / Output
Solution / Remediation
References
Other available metadata
```

The exact fields vary by finding and Nessus version.

---

# 23. Read the Finding in the Correct Order

Use:

```text
Finding
  ↓
Affected Asset
  ↓
Description
  ↓
Evidence
  ↓
Detection Information
  ↓
Remediation
  ↓
References
```

Do not immediately copy the finding into a report.

First understand what Nessus is actually telling you.

---

# 24. Finding Investigation Question

For the finding you opened, answer:

```text
What is the finding?
____________________________

Which host is affected?
____________________________

What did Nessus detect?
____________________________

What evidence is available?
____________________________

What remediation does Nessus suggest?
____________________________
```

If a field is not available for that finding, record:

```text
Not provided for this finding
```

Do not invent evidence.

---

# 25. Decision Point — Does the Finding Need Validation?

At this stage, ask:

```text
Does the finding appear sufficiently supported?
```

Possible outcomes:

```text
Clearly supported
      ↓
Continue analysis

Needs additional investigation
      ↓
Validate

Unexpected / suspicious
      ↓
Investigate before reporting
```

You do not need to perform external vulnerability exploitation to complete this exercise.

The purpose is to learn that:

> **A finding can require investigation before it becomes a professional conclusion.**

---

# 26. Check the Assessment Conditions

Return to your original assessment record.

Compare:

```text
Expected
   vs
Observed
```

Check:

```text
Target:
Expected → Actual

Authentication:
Expected → Actual

Assessment:
Expected → Actual

Completion:
Expected → Actual

Results:
Expected → Actual
```

Record any differences.

---

# 27. First-Scan Decision

Now answer:

> **Did Nessus successfully answer the assessment question?**

Use:

```text
Assessment Objective:
Identify vulnerabilities detectable by Nessus
on the authorized laboratory target.

Question:
Did the assessment provide enough information
to support that objective?
```

Possible answers:

```text
YES
```

or:

```text
NO — additional investigation required
```

If your answer is "No," explain why.

Examples:

* target was unreachable
* scan failed
* authentication was unexpectedly required
* results appear incomplete
* scanner encountered an error
* assessment conditions differed from the plan

Do not force a "successful" conclusion.

---

# 28. Record the Assessment

Complete:

```text
Assessment Name:
________________________________

Objective:
________________________________

Scope:
________________________________

Target:
________________________________

Workflow:
________________________________

Authentication:
________________________________

Start Time:
________________________________

End Time:
________________________________

Final State:
________________________________

Findings:
________________________________

Unexpected Conditions:
________________________________

Validation Required:
________________________________

Next Action:
________________________________
```

This is the beginning of your professional assessment record.

---

# 29. Practical Challenge — Repeat Without Instructions

Now close this file.

Open Nessus.

Perform another basic assessment against a different authorized laboratory target, if available.

Do not follow the exact steps above.

Instead use only:

```text
Objective:
Perform a basic vulnerability assessment
against an authorized laboratory target.

Constraints:
Use an appropriate general vulnerability
assessment workflow and avoid unnecessary
configuration changes.

Success Criteria:
The scan completes and the results are
reviewed and documented.
```

You must determine:

* where to start
* which template to use
* how to enter the target
* what configuration to review
* whether authentication is needed
* how to launch
* how to monitor
* how to review results

If you get stuck, return to the relevant earlier module rather than searching for a step-by-step tutorial.

---

# 30. Expected Learning Outcome

After completing the first scan, you should understand this workflow:

```text
Authorized Scope
      ↓
Assessment Objective
      ↓
Target
      ↓
Appropriate Template
      ↓
Review Configuration
      ↓
Authentication Decision
      ↓
Safety Review
      ↓
Launch
      ↓
Monitor
      ↓
Completion
      ↓
Results
      ↓
Finding Investigation
      ↓
Assessment Decision
      ↓
Documentation
```

This is the first complete Nessus operational loop.

---

# 31. Common Beginner Mistakes

Avoid these behaviors:

### Mistake 1 — Scanning before defining scope

```text
Wrong:
Open Nessus → enter random IP → Scan
```

Correct:

```text
Authorization → Scope → Objective → Target → Scan
```

---

### Mistake 2 — Changing everything

Do not change settings simply because advanced options exist.

---

### Mistake 3 — Treating scan completion as success

A completed scan can still produce incomplete or misleading assessment coverage.

---

### Mistake 4 — Treating every finding as equally important

A finding requires context.

---

### Mistake 5 — Reporting without reading evidence

Always inspect the finding details.

---

### Mistake 6 — Blindly rerunning failed scans

Determine why the expected result did not occur.

---

### Mistake 7 — Assuming no findings means no vulnerabilities

The assessment has limitations.

---

# 32. Troubleshooting

## Problem — Target Does Not Appear

Check:

```text
Target address
Target availability
Network path
Firewall
Scan configuration
```

Then determine whether Nessus actually reached the intended host.

---

## Problem — Scan Fails

Check:

```text
Scanner state
Target reachability
Configuration
Nessus status/errors
Network conditions
```

Do not immediately modify multiple settings.

Change one relevant condition at a time when troubleshooting.

---

## Problem — Scan Takes Longer Than Expected

Check:

```text
Target responsiveness
Number of checks
Network conditions
Scan configuration
Scanner resources
```

Do not assume duration alone means failure.

---

## Problem — No Findings Appear

Ask:

```text
Was the target actually assessed?
Did the scan complete?
Was the target reachable?
Were expected services visible?
Was the correct workflow selected?
Were the relevant checks available?
Was authentication required for the question?
```

Then investigate.

---

## Problem — Results Look Unexpected

Use:

```text
Expected
   ↓
Observed
   ↓
Difference
   ↓
Possible Cause
   ↓
Check
   ↓
Decision
```

Do not immediately assume Nessus is wrong.

---

# 33. Completion Criteria

You have completed this workflow when you can independently:

* define a basic assessment objective
* verify authorization and scope
* select a suitable general vulnerability-assessment workflow
* configure a laboratory target
* review important scan configuration
* make an authentication decision
* perform a pre-launch safety check
* launch the scan
* monitor scan execution
* determine whether the scan completed
* open and interpret basic results
* investigate an individual finding
* identify supporting evidence
* recognize when validation may be required
* document the assessment
* determine whether the original assessment objective was answered

Most importantly, you should now understand:

```text
A Nessus scan is not the end of the workflow.

The scan produces evidence.

The operator must determine what that evidence means
and what should happen next.
```

The next module will build on this lifecycle by teaching how to understand **scan states, execution behavior, monitoring, interruption, completion, history, and the transition from scanning to analysis**.
