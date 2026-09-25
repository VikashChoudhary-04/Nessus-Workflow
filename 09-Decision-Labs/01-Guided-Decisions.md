# Guided Decisions

## Objective

Develop practical decision-making skills for Nessus assessments through structured scenarios.

The previous sections taught individual capabilities:

* Choosing workflows
* Defining targets
* Configuring assessments
* Using credentials
* Running scans
* Reading results
* Investigating findings
* Validating findings
* Troubleshooting
* Prioritizing
* Reporting
* Retesting
* Documenting assessments

This section changes the learning method.

Instead of being told exactly what to do, you will be given a situation and asked to determine the correct next action.

The objective is to develop the habit:

```text id="j5q8m2"
OBSERVE
   ↓
DEFINE THE QUESTION
   ↓
IDENTIFY CONSTRAINTS
   ↓
CONSIDER OPTIONS
   ↓
SELECT ACTION
   ↓
PREDICT RESULT
   ↓
EXECUTE
   ↓
VERIFY
   ↓
DOCUMENT
```

The goal is not to memorize the "correct button."

The goal is to understand **why a particular action is appropriate**.

---

# 1. Decision-Making Mental Model

A Nessus operator should continuously ask:

```text id="w7p2k4"
What do I know?
      ↓
What do I not know?
      ↓
What am I trying to determine?
      ↓
What evidence would answer that question?
      ↓
What is the lowest-impact action that can provide it?
      ↓
What could change as a result?
      ↓
How will I verify the outcome?
```

This prevents random configuration changes and unnecessary scanning.

---

# 2. Decision Lab Rules

For every scenario:

1. Read the situation completely.
2. Identify the assessment objective.
3. Identify authorization and scope.
4. Identify known facts.
5. Identify unknowns.
6. Determine the immediate question.
7. List plausible options.
8. Select the most appropriate next action.
9. Explain why.
10. Define the expected result.
11. Define what you would do if the expected result does not occur.
12. Record the decision.

Do not jump directly to execution.

---

# 3. Decision Record Format

Use this format for every lab:

```text id="v8k3m1"
Scenario:
<scenario name>

Objective:
<assessment objective>

Known:
<known facts>

Unknown:
<unknown information>

Constraints:
<scope / safety / operational constraints>

Question:
<what must be determined?>

Options:
1. <option>
2. <option>
3. <option>

Selected action:
<action>

Reason:
<why>

Expected result:
<expected observation>

If unexpected:
<next decision>

Evidence:
<actual evidence>

Final decision:
<final decision>
```

The reasoning is more important than the wording.

---

# 4. Decision Lab 1 — Choosing Discovery vs Vulnerability Assessment

## Scenario

You receive authorization to assess:

```text id="r4n7c2"
10.10.10.0/24
```

The request says:

> "We do not have an accurate inventory of the systems currently active in this subnet."

No vulnerability assessment has been requested yet.

## Question

What should your first workflow focus on?

### Options

```text id="p5m2x8"
A. Authenticated vulnerability assessment
B. Discovery
C. Compliance assessment
D. Retest
```

## Decision

Choose the workflow that answers the immediate question:

> **What systems and services are observable within the authorized scope?**

Therefore, the initial workflow should focus on:

```text id="y7k4m1"
Discovery
```

## Reasoning

The assessment question is inventory-oriented.

You first need to determine:

* Which hosts are present
* Which services are observable
* Which systems may require further assessment

Do not begin by assuming vulnerability assessment is the correct first step.

## Expected Result

You obtain an evidence-based picture of the observable environment.

## Next Decision

Use discovery results to determine which subsequent vulnerability assessment workflows are appropriate.

---

# 5. Decision Lab 2 — No Findings on a Reachable Host

## Scenario

An authorized Nessus assessment completes.

Results:

```text id="c8q2m5"
Target:
10.10.10.20

Host:
Reachable

Service:
443/tcp

Findings:
None
```

The system owner expects the application to have known weaknesses.

## Question

What should you do next?

### Options

```text id="h4m7p2"
A. Immediately declare the host secure
B. Run the same scan repeatedly
C. Investigate coverage and assessment conditions
D. Increase severity
```

## Decision

Select:

```text id="m6x3r8"
Investigate coverage and assessment conditions.
```

## Reasoning

"No findings" does not automatically mean:

```text id="z2k8q4"
No vulnerabilities exist.
```

Investigate:

* Target identity
* Network position
* Service visibility
* Workflow
* Plugin coverage
* Authentication requirements
* Configuration
* Assessment completeness

## Expected Result

You determine whether the no-finding result is meaningful or whether an assessment limitation exists.

---

# 6. Decision Lab 3 — Authentication Failure

## Scenario

An authenticated Linux assessment is configured.

The target is reachable.

SSH is reachable.

Authentication fails.

## Question

What should you investigate first?

### Options

```text id="q7m4x1"
A. Give the account root privileges
B. Increase scan aggressiveness
C. Investigate authentication method/account/credential conditions
D. Ignore the failure
```

## Decision

Select:

```text id="p8k2v5"
Investigate authentication method/account/credential conditions.
```

## Reasoning

The network path has already been shown to work.

The next layer is authentication.

Investigate:

```text id="n4r6c8"
Authentication method
↓
Account
↓
Credential validity
↓
Account restrictions
↓
Target-side configuration
```

Do not immediately grant maximum privilege.

---

# 7. Decision Lab 4 — Authentication Succeeds but Coverage Is Limited

## Scenario

A Nessus authenticated assessment reports successful authentication.

However, expected local software evidence is missing.

## Question

What is the next question?

### Decision

Ask:

> **Does the authenticated account have the permissions required for the intended checks?**

Authentication and authorization are different.

Use:

```text id="t5w8m2"
Authentication
      ↓
Permissions
      ↓
Expected Evidence
      ↓
Coverage
```

## Next Action

Investigate:

* Required privileges
* Account permissions
* Target configuration
* Nessus authentication evidence

Do not describe the assessment as fully authenticated merely because login succeeded.

---

# 8. Decision Lab 5 — Ten Targets, Eight Assessed

## Scenario

The authorized target set contains:

```text id="j3p7n5"
10 hosts
```

The completed assessment contains results for:

```text id="w6k2r8"
8 hosts
```

## Question

What should you conclude?

### Options

```text id="a4m8x2"
A. The other two hosts are secure
B. The scan is useless
C. Investigate why the two hosts were not assessed
D. Remove the two hosts from scope
```

## Decision

Select:

```text id="m7q3k9"
Investigate why the two hosts were not assessed.
```

## Reasoning

You need to determine whether the missing hosts are:

* Offline
* Filtered
* Incorrectly targeted
* Misconfigured
* Excluded
* Unreachable from the scanner
* Changed since scope definition

The two hosts remain an assessment coverage gap until resolved.

---

# 9. Decision Lab 6 — Finding Count Drops

## Scenario

Two assessments are performed one week apart.

First:

```text id="x5m2c8"
45 findings
```

Second:

```text id="k7r4p1"
18 findings
```

The second scan was completed successfully.

## Question

Can you conclude that 27 vulnerabilities were remediated?

### Decision

No.

First compare:

```text id="n8w3q6"
Scope
Target population
Scanner
Network position
Authentication
Permissions
Configuration
Plugin/content state
Nessus version
Target state
```

## Possible Explanations

```text id="z4m7p2"
Actual remediation
```

or:

```text id="v6k1x8"
Reduced assessment visibility
```

or:

```text id="q2r5n9"
Changed detection/content
```

or another evidence-supported explanation.

## Next Action

Perform a structured comparison before assigning remediation meaning to the finding difference.

---

# 10. Decision Lab 7 — Scan Is Taking Too Long

## Scenario

A scan normally takes:

```text id="w3k8m1"
30 minutes
```

Today it has been running for:

```text id="p5q7r4"
2 hours
```

The scanner remains responsive.

## Question

Should you immediately stop the scan?

### Decision

Not automatically.

First determine whether meaningful progress is occurring.

Check:

* Target progress
* Results being generated
* Target responsiveness
* Scanner resource usage
* Network conditions
* Concurrent assessments
* Authentication
* Timeouts

If progress is occurring and operational impact is acceptable, the scan may simply be slow.

---

# 11. Decision Lab 8 — Unexpected Target Appears

## Scenario

Your authorized scope contains:

```text id="h6r2m9"
10.10.10.0/28
```

During discovery, an additional system appears at:

```text id="x4k7p3"
10.10.10.20
```

It appears reachable from the same network.

## Question

Should you automatically add it to the assessment?

### Decision

No.

Discovery does not grant authorization.

First verify:

```text id="n5m8q2"
Is the target authorized?
```

If not confirmed:

```text id="r7c3v1"
Do not expand the assessment solely because
the system was discovered.
```

Document the unexpected asset and obtain the appropriate authorization if further assessment is required.

---

# 12. Decision Lab 9 — Unexpected Service

## Scenario

An authorized server is expected to expose:

```text id="j4p8m2"
443/tcp
```

Discovery identifies:

```text id="q6r1x7"
443/tcp
8080/tcp
```

## Question

What should you do?

### Decision

Investigate the unexpected service.

Determine:

* Is it expected?
* Is it authorized for assessment?
* What service is running?
* Is it part of the intended application?
* Does it change the attack surface?
* Does it require additional assessment?

Do not automatically exploit or aggressively test the service.

---

# 13. Decision Lab 10 — Finding Based on Version Evidence

## Scenario

Nessus reports a vulnerability because it detects:

```text id="c5x8m3"
Software version X.Y.Z
```

The target owner says:

> "The vendor backported the security fix."

## Question

What should you do?

### Decision

Investigate the vendor/distribution patch state and obtain supporting evidence.

Do not automatically:

```text id="m8r2k6"
Mark false positive
```

and do not automatically:

```text id="q4p7n1"
Accept vulnerability as confirmed
```

The relevant question is:

> **Does the installed package actually contain the security fix despite the apparent version?**

---

# 14. Decision Lab 11 — Finding Has High Severity

## Scenario

A Nessus finding is classified as high severity.

The affected system is:

```text id="x7m3q5"
Internal development server
```

It is not externally exposed.

## Question

Should severity automatically determine remediation priority?

### Decision

No.

Consider:

```text id="k8r2p6"
Validity
Asset importance
Exposure
Technical impact
Exploitability
Business context
Compensating controls
Remediation complexity
Dependencies
```

Severity is an important input but not the entire prioritization decision.

---

# 15. Decision Lab 12 — Critical Finding on Shared Infrastructure

## Scenario

A vulnerability affects a shared infrastructure component used by:

```text id="w4c7m1"
50 application servers
```

Nessus reports multiple findings across the population.

## Question

How should you think about remediation?

### Decision

Investigate the shared root cause.

Instead of treating every plugin result as an independent problem:

```text id="q5m8r2"
Shared component
↓
50 affected systems
↓
Multiple related findings
```

The remediation strategy may involve fixing the shared component or common configuration.

Then verify the affected population.

---

# 16. Decision Lab 13 — Finding Disappears

## Scenario

A finding appears in Assessment A.

It is absent in Assessment B.

No remediation record exists.

## Question

What is the correct conclusion?

### Decision

The finding is absent from Assessment B.

The reason is not yet established.

Investigate:

* Scope
* Target state
* Service state
* Authentication
* Configuration
* Plugin/content state
* Scanner position
* Nessus version
* Remediation evidence

Possible final states include:

```text id="z3n7k5"
Remediated
Condition changed
Not applicable
Unable to verify
Assessment visibility changed
```

Do not automatically mark it resolved.

---

# 17. Decision Lab 14 — Manual Authentication Works

## Scenario

An administrator can SSH to the target from their workstation.

Nessus authentication fails from the scanner.

## Question

What should you compare?

### Decision

Compare the two authentication paths.

```text id="g6m2r8"
Administrator workstation
        ↓
Network path
        ↓
Target
```

versus:

```text id="p4x7c1"
Nessus scanner
        ↓
Network path
        ↓
Target
```

Investigate:

* Source IP
* Routing
* Firewall
* Authentication method
* Credential configuration
* Target-side restrictions
* Account policy

Do not assume the credentials are necessarily wrong.

---

# 18. Decision Lab 15 — Production System Becomes Unstable

## Scenario

During an authorized production assessment, the target begins showing unusual response delays.

## Question

What is the priority?

### Decision

Operational safety.

Use:

```text id="k5r8m2"
Observe
↓
Assess impact
↓
Follow stop conditions
↓
Communicate
↓
Preserve evidence
↓
Investigate
↓
Resume only when authorized
```

Do not increase scan intensity.

Do not continue merely to complete the scheduled scan.

---

# 19. Decision Lab 16 — Scanner Resource Pressure

## Scenario

A scanner is running four concurrent assessments.

CPU and memory usage are substantially elevated.

Several scans are slower than their normal baseline.

## Question

What should you investigate?

### Decision

Investigate scanner-side resource contention.

Consider:

* Concurrent workload
* Target population
* Assessment configuration
* Scanner capacity
* Whether workloads can be rescheduled

Do not immediately assume the targets are slow.

---

# 20. Decision Lab 17 — Recurring Scan Suddenly Has Fewer Findings

## Scenario

A recurring authenticated scan normally produces extensive local evidence.

This week's scan produces much less.

The scan itself completed successfully.

## Question

What should you check first?

### Decision

Check authentication and coverage.

Investigate:

```text id="x8q2m5"
Credential validity
↓
Account status
↓
Permissions
↓
Authentication evidence
↓
Coverage
```

A scheduled scan can complete successfully while authenticated visibility has degraded.

---

# 21. Decision Lab 18 — Excluded Asset Appears in Results

## Scenario

A server was explicitly excluded from the assessment.

Results contain data associated with that server.

## Question

What should you do?

### Decision

Investigate why the asset appears.

Possible explanations:

* Target definition error
* Related infrastructure
* Shared IP
* Load balancer
* DNS behavior
* Scope change
* Unexpected target inclusion

Do not assume the asset was intentionally assessed.

Verify the technical path and authorization.

---

# 22. Decision Lab 19 — Scan Configuration Was Changed

## Scenario

A recurring assessment has produced very different results.

You discover that the plugin configuration changed between runs.

## Question

Can you directly compare finding counts?

### Decision

Comparison requires qualification.

Determine:

* What changed
* Why it changed
* Which checks were added/removed
* Whether affected findings depend on those changes
* Whether a new baseline should be established

A configuration change can invalidate simplistic before/after comparisons.

---

# 23. Decision Lab 20 — Nessus Version Changed

## Scenario

Assessment A used one Nessus version.

Assessment B used a newer version.

Finding counts changed significantly.

## Question

What should you do?

### Decision

Include the version difference in the comparison.

Investigate whether:

* Detection logic changed
* Plugins/content changed
* Configuration behavior changed
* Credential behavior changed
* Reporting changed

Do not attribute every difference to remediation.

---

# 24. Decision Lab 21 — Scan Failed After Target Reboot

## Scenario

During an assessment, the target server reboots unexpectedly.

The scan later fails.

## Question

What should you investigate?

### Decision

Separate target instability from scanner failure.

Check:

```text id="r7m4x2"
Target availability
↓
Service state
↓
Network reachability
↓
Scanner health
↓
Assessment state
```

Then determine how much coverage was achieved before the reboot.

---

# 25. Decision Lab 22 — Finding Has Conflicting Evidence

## Scenario

Nessus reports a vulnerability.

Additional evidence suggests the affected package may already be patched.

## Question

What should you do?

### Decision

Investigate and validate.

Use the least-impact method capable of resolving the conflict.

Possible evidence:

* Installed package version
* Vendor advisory
* Distribution patch information
* Configuration
* Plugin evidence
* Target-side package state

Final state may be:

```text id="m2x8p5"
Confirmed
False Positive
Not Applicable
Unresolved
```

depending on evidence.

---

# 26. Decision Lab 23 — Target Is Reachable but Expected Port Is Missing

## Scenario

The application owner says:

> "HTTPS is definitely running."

Nessus discovery does not identify the expected port.

## Question

What should you investigate?

### Decision

Do not immediately conclude the application is down.

Investigate:

```text id="v6r3k9"
Scanner position
↓
DNS
↓
Routing
↓
Firewall
↓
Port
↓
Service binding
↓
Load balancer / proxy
↓
Target state
```

A service can be running while inaccessible from the scanner's network position.

---

# 27. Decision Lab 24 — Scan Completes in Five Minutes

## Scenario

A scan that normally takes an hour completes in five minutes.

No errors are immediately visible.

## Question

Is this automatically a successful scan?

### Decision

No.

Investigate:

* Target count
* Target reachability
* Discovery
* Service visibility
* Authentication
* Plugin coverage
* Configuration
* Scanner state

A dramatic reduction in runtime can indicate a reduced assessment.

---

# 28. Decision Lab 25 — Owner Requests Immediate "Clean" Report

## Scenario

A stakeholder asks:

> "The scan has no critical findings. Can you just report that the system is secure?"

## Question

What should you document?

### Decision

Report what the assessment actually establishes.

For example:

> No critical findings were identified within the assessed scope and coverage achieved.

Do not convert:

```text id="c4m8q2"
No critical findings
```

into:

```text id="x7p3n6"
System is secure
```

The latter claim is much broader than the evidence.

---

# 29. Decision Lab 26 — Assessment Has Unknown Authorization

## Scenario

Someone sends you an IP address and says:

> "Scan this server for vulnerabilities."

No authorization information is provided.

## Question

Should you launch Nessus?

### Decision

No.

The first decision is:

```text id="k8m2r5"
Establish authorization and scope.
```

Technical reachability is irrelevant until authorization is established.

---

# 30. Decision Lab 27 — Newly Discovered Host Is Interesting

## Scenario

During an authorized discovery assessment, you identify an unfamiliar host.

It appears to run an interesting service.

## Question

Should you immediately perform deeper testing?

### Decision

No.

First determine:

```text id="m7x4c9"
Is the host authorized?
Is the service in scope?
What is the assessment objective?
What evidence is required?
```

Discovery creates information.

It does not automatically expand permission.

---

# 31. Decision Lab 28 — Target Owner Wants Credentials Added

## Scenario

An unauthenticated assessment is currently running.

The system owner provides credentials and asks:

> "Can you add these now so we get better results?"

## Question

Should you immediately change the running assessment?

### Decision

Not automatically.

First determine:

* Whether authentication is authorized
* Whether the assessment objective requires it
* Whether changing the configuration mid-assessment is supported
* Whether doing so would make comparison difficult
* Whether a separate authenticated assessment is more appropriate

A separate workflow may provide cleaner evidence and comparability.

---

# 32. Decision Lab 29 — Retest Shows No Finding

## Scenario

A vulnerability was remediated.

The retest no longer reports it.

## Question

Is remediation verified?

### Decision

Not automatically.

Determine why the finding disappeared.

Verify:

* Target scope
* Service
* Configuration
* Authentication
* Plugin/content state
* Relevant evidence

If the underlying condition is confirmed changed, remediation can be considered verified.

---

# 33. Decision Lab 30 — Remediation Changes Only One Host

## Scenario

A vulnerability affects:

```text id="r5k8m2"
20 servers
```

The owner patches one server.

The retest confirms the finding is gone from that server.

## Question

Can the entire finding population be marked remediated?

### Decision

No.

The evidence supports remediation for the verified host.

The remaining population still requires assessment.

Use:

```text id="q3x7n1"
20 affected
↓
1 verified remediated
↓
19 remain to verify
```

---

# 34. Decision Lab 31 — Finding Is Informational

## Scenario

Nessus reports an informational result describing a service.

## Question

Should it be ignored?

### Decision

Not automatically.

Informational results can provide useful context for:

* Attack surface
* Service inventory
* Technology identification
* Finding interpretation
* Validation
* Exposure analysis

Determine whether the information affects the assessment question.

---

# 35. Decision Lab 32 — Finding Severity Is Low but Exposure Is High

## Scenario

A finding has low scanner severity but affects an internet-facing service.

## Question

Should it automatically remain low priority?

### Decision

No.

Consider:

```text id="x8m4p7"
Validity
Exposure
Asset importance
Technical impact
Exploitability
Business context
Compensating controls
```

Severity is one input into prioritization.

Do not automatically override it either.

Use documented reasoning.

---

# 36. Decision Lab 33 — Finding Severity Is Critical but Asset Is Isolated

## Scenario

A critical-severity finding exists on an isolated lab system that has no connection to production.

## Question

Should severity alone determine immediate production remediation?

### Decision

No.

The asset context and exposure matter.

Document the relevant context and determine the appropriate remediation priority based on the actual environment.

---

# 37. Decision Lab 34 — Scanner Moved Networks

## Scenario

A recurring assessment used to run from an internal network.

The scanner was recently moved to a segmented security network.

Results changed significantly.

## Question

What should you investigate?

### Decision

Investigate the scanner-position change.

Compare:

```text id="j5r8x2"
Previous network position
vs
Current network position
```

Determine whether reachability, service visibility, authentication, or exposure changed.

Historical comparisons may require qualification.

---

# 38. Decision Lab 35 — Target IP Changed

## Scenario

A hostname remains the same, but its IP address changed.

The new assessment produces different findings.

## Question

What should you verify?

### Decision

Verify:

* DNS
* Target identity
* Asset ownership
* Actual system behind the hostname
* Scope
* Service state

A hostname is not proof that the underlying system is unchanged.

---

# 39. Decision Lab 36 — Load-Balanced Application

## Scenario

An application is accessed through:

```text id="p6m2r8"
app.example.local
```

Different requests may reach different backend servers.

Nessus results appear inconsistent across assessments.

## Question

What should you consider?

### Decision

Consider the load-balancing architecture.

Investigate:

* Backend population
* Scanner network path
* DNS
* Load balancer behavior
* Session routing
* Target identity
* Assessment timing

A single hostname may represent multiple systems.

---

# 40. Decision Lab 37 — Shared Credential Stops Working

## Scenario

A recurring authenticated assessment suddenly loses authentication across many hosts.

## Question

What should you investigate first?

### Decision

Because multiple hosts changed at once, investigate shared dependencies.

Possible causes:

```text id="r4x7m2"
Credential rotation
Account disablement
Permission change
Centralized authentication policy
Network policy
Shared configuration
```

Do not troubleshoot every host independently before checking the common dependency.

---

# 41. Decision Lab 38 — One Host Fails Authentication

## Scenario

Nine hosts authenticate successfully.

One host fails.

## Question

What is the likely troubleshooting approach?

### Decision

Treat the problem as potentially host-specific.

Compare:

```text id="m7p3x9"
Working host
vs
Failing host
```

Investigate:

* Host configuration
* Service state
* Account policy
* Firewall
* Permissions
* Authentication method
* Network path

Do not immediately replace the credentials globally.

---

# 42. Decision Lab 39 — One Finding Appears on Many Hosts

## Scenario

The same vulnerability appears across 40 servers.

## Question

What should you investigate?

### Decision

Determine whether the hosts share:

* Software package
* Base image
* Configuration
* Deployment pipeline
* Operating system version
* Shared service
* Common management platform

The finding may represent one systemic remediation opportunity rather than 40 unrelated technical problems.

---

# 43. Decision Lab 40 — Security Control Blocks Scanner

## Scenario

A firewall blocks Nessus traffic from the scanner.

The target is otherwise healthy.

## Question

What should you do?

### Decision

Determine:

1. Is the scanner traffic authorized?
2. Is the firewall rule expected?
3. Can the required access be safely and legitimately enabled?
4. Is there another authorized scanner position?
5. What coverage limitation exists if access remains blocked?

Do not disable the firewall without authorization.

---

# 44. Decision Lab 41 — Scan Triggers Rate Limiting

## Scenario

A target begins rate-limiting scanner connections.

## Question

Should you simply increase concurrency?

### Decision

No.

Investigate:

* Target behavior
* Assessment configuration
* Network controls
* Appropriate scan intensity
* Operational constraints

The correct response may involve reducing activity or coordinating with the target owner.

---

# 45. Decision Lab 42 — Result Conflicts With Owner's Evidence

## Scenario

Nessus reports a vulnerable package.

The system owner provides package evidence suggesting it is patched.

## Question

What should you do?

### Decision

Reconcile the evidence.

Check:

```text id="x2m7q4"
Nessus detection
+
Installed package state
+
Vendor/distribution patch information
+
Target configuration
```

Do not automatically choose the Nessus result or the owner's statement.

The evidence determines the conclusion.

---

# 46. Decision Lab 43 — Scan Was Interrupted

## Scenario

A scan was stopped halfway through because of an operational concern.

The result contains findings.

## Question

Can those findings be used?

### Decision

Potentially, but only within the demonstrated coverage.

Document:

* What completed
* What did not
* Which hosts were reached
* Which findings were observed
* Why execution stopped
* What remains unverified

Do not treat the partial result as a complete assessment.

---

# 47. Decision Lab 44 — Scan Failed Before Results

## Scenario

The scan failed shortly after launch and produced almost no useful output.

## Question

Should you create a report stating "no vulnerabilities found"?

### Decision

No.

The assessment did not establish sufficient coverage.

Investigate the failure and determine whether a replacement assessment is required.

---

# 48. Decision Lab 45 — Assessment Window Is Ending

## Scenario

The approved assessment window ends in 15 minutes.

The scan is still running.

## Question

What should you consider?

### Decision

The authorization window and operational constraints matter.

Determine:

* Whether execution may continue
* Whether the scan should stop
* Whether an extension is authorized
* What coverage has been achieved
* Whether a later continuation is appropriate

Do not assume technical completion overrides the assessment window.

---

# 49. Decision Lab 46 — Emergency Stop Request

## Scenario

The authorized system owner requests that the assessment stop immediately.

## Question

What should you do?

### Decision

Follow the applicable authorization and operational process.

If the requester is authorized:

```text id="k6m3r8"
Stop / pause as appropriate
↓
Record time and reason
↓
Preserve evidence
↓
Assess coverage
↓
Determine follow-up
```

Do not prioritize finishing the scan over an authorized stop request.

---

# 50. Decision Lab 47 — Finding Requires Disruptive Validation

## Scenario

A high-impact finding could be validated through a potentially disruptive test.

## Question

Should you perform the test?

### Decision

Only if:

* Explicitly authorized
* Necessary to resolve meaningful uncertainty
* Operationally appropriate
* Performed under defined conditions
* Stop/recovery procedures exist

Otherwise use lower-impact evidence or report the finding as unresolved if appropriate.

---

# 51. Decision Lab 48 — Business Context Is Missing

## Scenario

You need to prioritize a vulnerability.

Technical information is available.

Business criticality is not.

## Question

Should you guess the business importance?

### Decision

No.

Record:

```text id="m5x8q2"
Business criticality:
Unknown / Not provided
```

Then obtain the information from the appropriate owner or decision-maker if it is required for prioritization.

---

# 52. Decision Lab 49 — Owner Wants Every Finding Marked Critical

## Scenario

A stakeholder says:

> "These are all important. Mark everything critical so the team fixes it."

## Question

What should you do?

### Decision

Preserve evidence-based classification and prioritization.

Do not arbitrarily change severity or priority.

Instead:

* Explain the technical severity
* Explain environmental context
* Identify exposure
* Identify asset importance
* Identify remediation dependencies
* Document the actual prioritization rationale

---

# 53. Decision Lab 50 — Assessment Has No Obvious Owner

## Scenario

A recurring scan produces findings, but nobody appears responsible for remediation.

## Question

What should you do?

### Decision

Identify ownership before assuming the security team owns every fix.

Determine:

```text id="r8m3x7"
Asset owner
Application owner
Infrastructure owner
Security owner
Remediation owner
```

An assessment is operationally incomplete if findings cannot be routed to actionable owners.

---

# 54. Decision Lab 51 — Credential Is About to Expire

## Scenario

A recurring authenticated assessment is scheduled tomorrow.

The assessment credential expires today.

## Question

What should you do?

### Decision

Treat the credential lifecycle as part of assessment readiness.

Plan:

```text id="q6x2m9"
Credential renewal/rotation
↓
Update authorized configuration
↓
Verify authentication
↓
Confirm expected coverage
↓
Run assessment
```

Do not wait for tomorrow's failed assessment if the credential lifecycle is already known.

---

# 55. Decision Lab 52 — Plugin Update Before Comparison

## Scenario

You need to compare today's assessment with last month's baseline.

Plugin/content updates occurred between the two assessments.

## Question

What should you do?

### Decision

Document the plugin/content difference.

Determine:

* What changed
* Which findings may be affected
* Whether direct comparison remains meaningful
* Whether a new baseline is appropriate

Do not hide the change.

---

# 56. Decision Lab 53 — Configuration Drift

## Scenario

A recurring assessment has gradually accumulated configuration changes.

Nobody knows which changes were intentional.

## Question

What should you do?

### Decision

Treat the configuration as requiring review.

Compare against the intended baseline.

Determine:

```text id="v5m8r2"
What changed?
Why?
Who approved it?
What effect does it have?
Should it remain?
```

Do not blindly preserve configuration drift.

---

# 57. Decision Lab 54 — Unexpected Finding After Upgrade

## Scenario

Nessus is upgraded.

A finding appears that was not present before.

## Question

What should you investigate?

### Decision

Consider both:

```text id="p2x7m4"
Actual target condition
```

and:

```text id="m8r3q6"
Changed detection/content behavior
```

Review the plugin/content and version differences before concluding that the vulnerability is newly introduced.

---

# 58. Decision Lab 55 — Finding Is Not Reproducible

## Scenario

A finding appears in one assessment but cannot be reproduced under apparently similar conditions.

## Question

Should you immediately classify it as false positive?

### Decision

No.

Investigate:

* Target state
* Timing
* Load balancing
* Scanner position
* Authentication
* Configuration
* Plugin/content state
* Network behavior

If uncertainty remains:

```text id="r7m2x8"
Unresolved
```

may be more accurate than a forced conclusion.

---

# 59. Decision Lab 56 — Same Finding, Different Assets

## Scenario

The same vulnerability appears on:

```text id="c8m4q1"
Internet-facing server
```

and:

```text id="x5r7n2"
Internal isolated server
```

## Question

Should they automatically receive identical priority?

### Decision

Not necessarily.

Compare:

* Exposure
* Asset importance
* Technical impact
* Exploitability
* Business context
* Compensating controls

The technical finding can be the same while environmental priority differs.

---

# 60. Decision Lab 57 — Report Deadline Arrives

## Scenario

A report deadline is approaching.

One important finding remains unresolved because validation evidence is incomplete.

## Question

Should you remove it from the report?

### Decision

No.

Document:

```text id="m4x8r2"
Finding:
<finding>

Evidence:
<available evidence>

Validation:
Incomplete

Status:
Unresolved

Limitation:
<limitation>

Required follow-up:
<action>
```

A documented uncertainty is better than silently omitting a material issue.

---

# 61. Decision Lab 58 — Assessment Has Conflicting Scope Records

## Scenario

Two documents contain different target lists.

One includes a server.

The other excludes it.

## Question

Should you scan it?

### Decision

Resolve the authorization conflict before assessment.

Do not select the broader scope simply because it produces more coverage.

Establish the current authoritative scope.

---

# 62. Decision Lab 59 — Scanner Replacement

## Scenario

The original scanner is unavailable.

A replacement scanner is available in a different network segment.

## Question

Can you simply rerun the same assessment?

### Decision

Not without evaluating the difference.

Compare:

```text id="p8m2r5"
Scanner
Version
Edition
Network position
Routing
Credentials
Plugin/content state
Configuration
Target reachability
```

The replacement may provide different visibility.

Document the change.

---

# 63. Decision Lab 60 — Assessment Owner Requests "Maximum Coverage"

## Scenario

The owner says:

> "Enable everything so we don't miss anything."

## Question

Should you enable every available option?

### Decision

Not automatically.

Determine:

* Assessment objective
* Relevant plugin coverage
* Target capacity
* Scanner capacity
* Operational constraints
* Potential noise
* Potential target impact
* Required evidence

The objective is:

> **Sufficient relevant coverage, not maximum configuration complexity.**

---

# 64. Decision Lab 61 — Finding Count Is High Because of One Root Cause

## Scenario

Nessus produces 80 findings.

Investigation shows that most are related to one outdated shared component.

## Question

How should the report be structured?

### Decision

Group the findings by root cause while preserving affected assets and relevant technical evidence.

The report should help the remediation team understand:

```text id="q4m7x2"
Root Cause
↓
Affected Population
↓
Technical Findings
↓
Remediation
↓
Verification
```

Avoid creating unnecessary remediation work items for every duplicated detection.

---

# 65. Decision Lab 62 — One Host Has a Different Baseline

## Scenario

Nine servers run the same approved configuration.

One server differs.

The vulnerability appears only on that server.

## Question

What should you investigate?

### Decision

Investigate the configuration drift.

Compare:

```text id="x6m2r8"
Known-good population
vs
Outlier
```

Determine whether:

* The finding is valid
* The configuration is unauthorized
* The host has a legitimate exception
* The difference explains the finding

---

# 66. Decision Lab 63 — Assessment Is Technically Complete but Documentation Is Missing

## Scenario

The scan completed successfully.

Findings are available.

However, there is no reliable record of:

* Scope
* Scanner position
* Authentication
* Configuration
* Limitations

## Question

Is the assessment ready for final reporting?

### Decision

Not necessarily.

The missing context can materially affect interpretation.

Reconstruct what can be verified, identify what cannot, and document the limitations.

Do not invent missing details.

---

# 67. Decision Lab 64 — Stakeholder Disagrees With Finding

## Scenario

A system owner says:

> "This finding is wrong."

## Question

What should determine the outcome?

### Decision

Evidence.

Use:

```text id="r3m8x5"
Nessus evidence
+
Target evidence
+
Configuration
+
Vendor/package evidence
+
Validation
```

The owner's disagreement is important context but is not by itself proof of either correctness or incorrectness.

---

# 68. Decision Lab 65 — Finding Is Important but Validation Is Impossible

## Scenario

A finding has meaningful potential impact.

The target cannot currently be validated because the required maintenance window is unavailable.

## Question

What should you do?

### Decision

Document the finding and limitation.

Use:

```text id="n5x2m8"
Observed evidence
+
Validation limitation
+
Current confidence
+
Required follow-up
```

Do not manufacture certainty.

Schedule appropriate validation when authorized.

---

# 69. Decision Lab 66 — Remediation Was Applied Before Retest

## Scenario

The owner says:

> "We patched it yesterday."

No retest has occurred.

## Question

Can the finding be marked verified?

### Decision

No.

The remediation claim can be recorded as an action reported by the owner, but verification requires appropriate evidence.

Retest when possible.

---

# 70. Decision Lab 67 — Retest Uses Different Perspective

## Scenario

Original finding:

```text id="k7m3x1"
Authenticated assessment
```

Retest:

```text id="q4r8p2"
Unauthenticated assessment
```

The finding is no longer visible.

## Question

Can you conclude remediation?

### Decision

Not automatically.

The assessment perspectives differ.

The retest should provide evidence appropriate to the original condition.

Investigate the underlying state.

---

# 71. Decision Lab 68 — Retest Uses Different Scanner Position

## Scenario

Original finding was identified from an internal scanner.

Retest is performed from an external scanner.

The finding disappears.

## Question

What should you consider?

### Decision

The network perspective changed.

Compare:

```text id="m8x4r2"
Original exposure
vs
Current exposure
```

The result may represent changed visibility rather than remediation.

---

# 72. Decision Lab 69 — Target Was Patched but Finding Remains

## Scenario

The owner says the vulnerable software was patched.

Nessus still reports the finding.

## Question

What should you investigate?

### Decision

Reconcile the evidence.

Check:

* Installed version/package
* Vendor/distribution patch state
* Running service
* Restart requirement
* Multiple installed versions
* Target identity
* Nessus evidence
* Plugin/content state

Do not automatically reject either side.

---

# 73. Decision Lab 70 — Target Was Rebuilt

## Scenario

A vulnerable server was destroyed and rebuilt.

The new server has the same hostname.

## Question

How should you treat the old finding?

### Decision

Verify asset identity and current target state.

The hostname alone does not establish continuity.

Document:

```text id="x5m7r3"
Original asset
Current asset
Rebuild event
Current configuration
Retest evidence
```

The finding may have been remediated through asset replacement, but this should be supported by evidence.

---

# 74. Decision Lab 71 — Assessment Is Out of Scope but Business Owner Requests It

## Scenario

An asset is excluded from the approved scope.

The owner personally asks you to scan it anyway.

## Question

Should you add it?

### Decision

Not until authorization and scope are updated through the appropriate process.

The request may be legitimate, but the existing scope does not authorize the additional target.

---

# 75. Decision Lab 72 — Assessment Request Is Too Broad

## Scenario

The request says:

> "Scan the whole company network."

No target range, exclusions, window, or assessment objective is provided.

## Question

What should you do?

### Decision

Do not translate the vague request directly into a large scan.

Clarify:

* Scope
* Authorization
* Objective
* Scanner position
* Exclusions
* Operational constraints
* Assessment window
* Expected evidence
* Stop conditions

A broad request requires precise assessment definition.

---

# 76. Decision Lab 73 — Finding Has No Evidence

## Scenario

A finding appears in an exported summary.

The detailed evidence is unavailable.

## Question

Should it be reported as confirmed?

### Decision

Not automatically.

Determine whether supporting evidence can be recovered.

If not:

```text id="p8x4m2"
Evidence limitation
↓
Confidence limitation
↓
Potentially unresolved
```

Do not convert a summary label into a fully supported technical conclusion.

---

# 77. Decision Lab 74 — Scan Report Contains Sensitive Data

## Scenario

You export a Nessus report containing:

* Internal hostnames
* Internal IP addresses
* Vulnerability details
* Service information

A colleague asks you to upload it to a public repository for convenience.

## Question

Should you do so?

### Decision

No.

Use an approved secure sharing mechanism.

Public training repositories should contain sanitized examples, not real assessment data.

---

# 78. Decision Lab 75 — You Do Not Know the Nessus Edition

## Scenario

You receive a Nessus environment but are unsure which edition is installed.

## Question

Should you assume the features shown in an online tutorial are available?

### Decision

No.

First determine:

```text id="m6r2x8"
Edition
Version
License/subscription state
Scanner
Operating environment
Available capabilities
```

Then adapt the workflow to the actual environment.

---

# 79. Decision Lab 76 — UI Looks Different From Training Material

## Scenario

A tutorial shows a setting in one location.

Your Nessus interface does not match.

## Question

What should you do?

### Decision

Do not assume your installation is broken.

Check:

* Nessus version
* Edition
* Current UI structure
* Available capability
* Official documentation
* User permissions

Focus on the required capability rather than memorizing screen positions.

---

# 80. Decision Lab 77 — Assessment Uses an Old Template

## Scenario

A recurring assessment was created several years ago using an older template.

The environment has changed substantially.

## Question

Should you continue using it unchanged?

### Decision

No automatic answer.

Review:

```text id="q8m4x1"
Objective
Scope
Targets
Workflow
Credentials
Plugins
Configuration
Operational constraints
Version
```

Determine whether the existing configuration still answers the current assessment question.

---

# 81. Decision Lab 78 — Finding Appears Only After Authentication

## Scenario

Unauthenticated assessment:

```text id="x5r8m2"
No finding
```

Authenticated assessment:

```text id="k3p7q1"
Finding identified
```

## Question

Does that mean the vulnerability was created by authentication?

### Decision

No.

Authentication changed the assessment perspective and evidence available.

The finding may have existed previously but was not observable from the unauthenticated perspective.

---

# 82. Decision Lab 79 — Finding Appears Only From External Scanner

## Scenario

Internal assessment:

```text id="m7x2r5"
No finding
```

External assessment:

```text id="q4p8n1"
Finding
```

## Question

What should you investigate?

### Decision

Investigate exposure and network perspective.

The difference may reflect:

* External exposure
* Firewall behavior
* Network segmentation
* Scanner position
* Service visibility

The two assessments answer different questions.

---

# 83. Decision Lab 80 — Finding Appears Only From Internal Scanner

## Scenario

External scanner cannot identify a vulnerability.

Internal scanner can.

## Question

What might explain this?

### Decision

Potentially:

* Internal-only service
* Internal exposure
* Network segmentation
* Authentication differences
* Additional service visibility
* Scanner position

Investigate the evidence before interpreting the difference.

---

# 84. Decision Lab 81 — Assessment Has a Tight Deadline

## Scenario

You have one hour to complete an authorized assessment.

The scope is large.

## Question

Should you simply increase scanner aggressiveness?

### Decision

Not automatically.

First determine:

* What is the highest-value assessment question?
* Which targets are required?
* Which workflow is appropriate?
* What coverage is achievable?
* What operational constraints exist?
* Whether scope can be prioritized or clarified

Do not trade assessment quality and safety for an arbitrary completion time without authorization.

---

# 85. Decision Lab 82 — Target Owner Wants Exclusion Without Reason

## Scenario

An asset owner asks you to exclude a vulnerable system.

No reason is provided.

## Question

Should you silently exclude it?

### Decision

No.

Determine:

* Whether the owner is authorized to request the change
* Why the exclusion is required
* Whether the scope authorization permits it
* How the exclusion affects coverage
* Whether the exclusion needs approval

Document the result.

---

# 86. Decision Lab 83 — Scan Is Producing Repeated Connection Errors

## Scenario

A target shows many connection failures.

The scanner itself is healthy.

## Question

Where should troubleshooting begin?

### Decision

Follow the network/target path:

```text id="c6m2r8"
Scanner
↓
Network
↓
Routing
↓
Firewall/ACL
↓
Target
↓
Port
↓
Service
```

Do not immediately change plugin settings.

---

# 87. Decision Lab 84 — Target Is Intermittently Reachable

## Scenario

A host responds sometimes but not consistently.

## Question

What should you investigate?

### Decision

Investigate:

* Network stability
* Routing
* Firewall behavior
* Load balancing
* Target resource state
* Service stability
* Security controls

Intermittent reachability can create inconsistent scan results.

---

# 88. Decision Lab 85 — Finding Count Increased After Authentication

## Scenario

Unauthenticated:

```text id="m8r2x5"
12 findings
```

Authenticated:

```text id="q4p7n1"
35 findings
```

## Question

Does this mean the system became less secure?

### Decision

Not necessarily.

Authentication increased assessment visibility.

Investigate which findings depend on authenticated evidence.

The difference may demonstrate broader coverage rather than a change in the target's security state.

---

# 89. Decision Lab 86 — Report Contains Raw Severity Counts

## Scenario

A draft report says:

```text id="r5x8m2"
Critical: 2
High: 18
Medium: 43
Low: 91
```

No interpretation is provided.

## Question

Is this sufficient professional reporting?

### Decision

No.

Add:

* Scope
* Coverage
* Important findings
* Root causes
* Validation
* Environmental context
* Prioritization rationale
* Limitations
* Remediation actions

Counts alone do not explain what the organization should do.

---

# 90. Decision Lab 87 — Finding Is High but Unvalidated

## Scenario

A high-severity finding is based on indirect version evidence.

## Question

What should happen?

### Decision

Investigate and validate where appropriate.

Do not automatically:

```text id="x6m3q8"
Close it
```

or:

```text id="p7r2k5"
Treat it as confirmed exploitation
```

The correct state depends on evidence.

---

# 91. Decision Lab 88 — Finding Is Low but Root Cause Is Broad

## Scenario

A low-severity configuration issue affects:

```text id="m4x8r2"
500 systems
```

## Question

Should its scope be ignored because severity is low?

### Decision

No.

Population size and systemic exposure can matter.

Investigate:

* Shared root cause
* Affected population
* Exposure
* Technical impact
* Remediation feasibility
* Business context

---

# 92. Decision Lab 89 — Remediation Is Complete but Verification Is Not

## Scenario

The system owner confirms:

> "The change was deployed everywhere."

No retest has occurred.

## Question

What is the status?

### Decision

Remediation is **reported by the owner**, but verification is not yet complete.

Document the distinction.

Then perform appropriate retesting.

---

# 93. Decision Lab 90 — Final Assessment Conclusion

## Scenario

You completed an authorized assessment.

Coverage:

```text id="k7m3x8"
95% of authorized targets
```

Five systems were unreachable.

Important findings were investigated and validated where possible.

## Question

Can the report say:

> "The environment is secure"?

### Decision

No.

A defensible conclusion should describe:

* What was assessed
* What was identified
* What coverage was achieved
* What limitations remain
* What remediation is required
* What follow-up is necessary

Do not make broader claims than the evidence supports.

---

# 94. Guided Decision Pattern

Across all of these labs, the same pattern appears:

```text id="q5m8x2"
ASSESSMENT QUESTION
       ↓
KNOWN FACTS
       ↓
UNKNOWN FACTS
       ↓
CONSTRAINTS
       ↓
POSSIBLE ACTIONS
       ↓
LOWEST-IMPACT USEFUL ACTION
       ↓
EXPECTED EVIDENCE
       ↓
OBSERVED EVIDENCE
       ↓
DECISION
       ↓
NEXT ACTION
```

This is the foundation of independent Nessus operation.

---

# 95. Decision Quality Test

A good Nessus decision should answer:

### Why?

Why is this action necessary?

### What?

What exactly will be done?

### Where?

Which authorized target is affected?

### How?

Which workflow or capability will be used?

### Risk?

What operational impact could occur?

### Evidence?

What result would support the decision?

### Next?

What will happen depending on the result?

If you cannot answer these questions, the decision may be premature.

---

# 96. Common Decision-Making Mistakes

## Mistake 1 — Acting before defining the question

```text id="w4m7x2"
"Let's scan it."
```

Better:

> What are we trying to determine?

---

## Mistake 2 — Treating every problem as a credential problem

Better:

```text id="x8r3m5"
Network
↓
Service
↓
Method
↓
Account
↓
Credential
↓
Permissions
```

---

## Mistake 3 — Treating every problem as a scanner problem

Better:

> Determine which layer actually failed.

---

## Mistake 4 — Using maximum settings by default

Better:

> Use the configuration required to answer the assessment question safely.

---

## Mistake 5 — Assuming missing findings mean remediation

Better:

> Compare assessment conditions and verify the underlying state.

---

## Mistake 6 — Treating severity as priority

Better:

> Add environmental context.

---

## Mistake 7 — Expanding scope because discovery found something interesting

Better:

> Verify authorization first.

---

## Mistake 8 — Hiding uncertainty

Better:

> Record "unresolved" when evidence is insufficient.

---

## Mistake 9 — Guessing missing business context

Better:

> Mark it unknown and obtain the information.

---

## Mistake 10 — Changing several variables simultaneously

Better:

```text id="m7x4q2"
One meaningful change
↓
Retest
↓
Observe
```

---

# 97. Decision Lab Completion Criteria

You have completed the guided decision phase when you can independently:

* Identify the immediate assessment question.
* Separate known facts from unknowns.
* Respect authorization boundaries.
* Choose discovery vs assessment appropriately.
* Distinguish network problems from authentication problems.
* Distinguish authentication from authorization.
* Recognize incomplete coverage.
* Interpret no-finding results cautiously.
* Investigate unexpected finding changes.
* Compare assessment conditions.
* Account for scanner position.
* Account for plugin/content changes.
* Account for version changes.
* Recognize configuration drift.
* Identify shared root causes.
* Handle production-impact scenarios.
* Preserve uncertainty.
* Avoid unsupported conclusions.
* Select a low-impact evidence-gathering action.
* Define expected results before acting.
* Decide what to do when the expected result does not occur.

---

# Final Mental Model

Keep this model throughout the remaining decision labs:

```text id="v2m8r5"
WHAT AM I TRYING TO DETERMINE?
              ↓
WHAT DO I KNOW?
              ↓
WHAT DO I NOT KNOW?
              ↓
WHAT AM I AUTHORIZED TO DO?
              ↓
WHAT CONSTRAINTS APPLY?
              ↓
WHAT OPTIONS EXIST?
              ↓
WHICH ACTION PROVIDES USEFUL EVIDENCE
WITH THE LEAST UNNECESSARY IMPACT?
              ↓
WHAT DO I EXPECT TO OBSERVE?
              ↓
WHAT DID I ACTUALLY OBSERVE?
              ↓
WHAT DOES THAT EVIDENCE SUPPORT?
              ↓
WHAT REMAINS UNCERTAIN?
              ↓
WHAT IS THE NEXT DECISION?
```

The professional skill being developed is not:

> **"I know what Nessus setting to use."**

It is:

> **"I can look at an assessment situation, identify the actual question and constraints, choose an evidence-driven action, evaluate the result, and make the next decision without guessing."**
