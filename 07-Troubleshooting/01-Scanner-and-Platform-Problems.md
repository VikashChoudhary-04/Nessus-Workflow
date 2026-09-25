# Scanner and Platform Problems

## Objective

Learn how to troubleshoot Nessus when the scanner itself or the underlying platform is preventing normal operation.

A professional Nessus operator must be able to distinguish between:

```text
Nessus Problem
      ↓
Platform Problem
      ↓
Network Problem
      ↓
Target Problem
      ↓
Configuration Problem
      ↓
Authentication Problem
```

This workflow focuses on problems originating from the Nessus installation, scanner service, platform, licensing/activation state, plugin/content state, local resources, and scanner configuration.

By the end of this workflow, you should be able to:

* determine whether Nessus itself is operational
* distinguish service problems from UI problems
* verify scanner readiness
* investigate startup failures
* recognize plugin/content readiness problems
* investigate activation or licensing issues
* identify resource-related problems
* recognize version and compatibility issues
* troubleshoot scanner configuration problems systematically
* collect useful evidence before changing settings
* avoid destructive troubleshooting
* determine when escalation or vendor documentation is required

---

# 1. Troubleshooting Mental Model

Do not immediately change settings when something fails.

Start with:

```text id="c7m2x8"
OBSERVE
   ↓
DEFINE THE FAILURE
   ↓
CLASSIFY THE LAYER
   ↓
COLLECT EVIDENCE
   ↓
FORM A HYPOTHESIS
   ↓
MAKE ONE CONTROLLED CHANGE
   ↓
TEST
   ↓
COMPARE
   ↓
DOCUMENT
```

The objective is not merely:

> "Make Nessus work."

The objective is:

> "Determine why Nessus is not operating as expected and restore the required capability without introducing unnecessary changes."

---

# 2. The Scanner Readiness Model

Before troubleshooting a scan, establish:

```text id="q4n8v1"
Nessus Installed?
       ↓
Service Running?
       ↓
Web Interface Accessible?
       ↓
Initialized?
       ↓
Activated / Licensed?
       ↓
Plugins / Content Ready?
       ↓
Scanner Available?
       ↓
READY
```

If one layer fails, later troubleshooting may be misleading.

For example:

```text id="h8m3q6"
Plugin Content Not Ready
        ↓
Scan Behavior Abnormal
        ↓
Target Troubleshooting
        ↓
Wrong Layer
```

Always check the scanner first.

---

# 3. Define the Exact Failure

Avoid vague descriptions such as:

> "Nessus is not working."

Replace it with:

```text id="v2k7m4"
Observed:
Web interface loads, but scan creation fails.

Observed:
Scan starts but immediately stops.

Observed:
Nessus service is not running.

Observed:
Scanner is available, but plugins are not ready.

Observed:
UI is accessible, but assessment remains queued.
```

The more precise the failure description, the smaller the troubleshooting search space.

---

# 4. First Five Minutes

When Nessus behaves unexpectedly, perform a basic readiness check.

## Step 1 — Confirm the Service

Determine whether the Nessus service/process is running.

The exact service-management command depends on the operating system and installation method.

Examples on Linux may use the system service manager.

Do not assume the same command applies to every platform.

---

## Step 2 — Confirm the Web Interface

Check whether the Nessus interface is accessible.

If it is unavailable:

```text id="n9x3k5"
Service
 ↓
Listener
 ↓
Local Firewall
 ↓
Port Binding
 ↓
Browser / Network Path
```

Investigate from the lowest relevant layer.

---

## Step 3 — Check Scanner State

Within the Nessus interface, inspect the scanner/product state using the controls available in your edition and version.

Look for indicators related to:

* scanner availability
* activation
* license/subscription state
* plugin/content readiness
* errors
* update status

Exact labels vary by Nessus version and edition.

---

## Step 4 — Check Recent Changes

Ask:

```text id="m6q2w8"
What changed immediately before the problem?
```

Examples:

* Nessus upgrade
* operating-system update
* reboot
* configuration change
* plugin update
* license change
* certificate change
* firewall change
* storage exhaustion
* network change

Recent changes often provide the strongest troubleshooting clue.

---

# 5. Separate UI Problems From Scanner Problems

A browser error does not automatically mean Nessus itself is broken.

Use:

```text id="p7m4x2"
Browser/UI
   ↓
Can interface load?
   ↓
Can user authenticate?
   ↓
Can scanner state be viewed?
   ↓
Can assessment be created?
   ↓
Can assessment execute?
```

This helps isolate the failure.

### Example

```text
UI inaccessible
```

is different from:

```text
UI accessible
but scan execution fails
```

Treat them as different problems.

---

# 6. Service Not Running

## Symptoms

Possible symptoms include:

* web interface unavailable
* scanner unavailable
* connection refused
* service exits after startup
* service repeatedly restarts

## Workflow

```text id="z5c8m1"
Confirm Service State
       ↓
Check Service Status
       ↓
Check Recent Logs
       ↓
Check Recent System Changes
       ↓
Check Dependencies
       ↓
Check Resources
       ↓
Restart Only When Appropriate
       ↓
Verify
```

Do not repeatedly restart without collecting evidence.

---

# 7. Service Starts and Immediately Stops

Possible causes include:

* configuration error
* permission problem
* corrupted state
* dependency problem
* incompatible update
* insufficient resources
* storage problem
* certificate/configuration issue
* another process occupying a required resource

Do not assume the cause.

Collect:

```text id="y4n7p2"
Service Status
Relevant Logs
Recent Changes
System Resource State
Configuration Changes
```

Then form a hypothesis.

---

# 8. Check System Resources

Nessus assessments can consume significant system resources depending on:

* number of targets
* scan configuration
* plugin coverage
* concurrency
* assessment type
* target responsiveness
* scanner hardware
* other processes

Relevant resources include:

```text id="x6q3m8"
CPU
Memory
Disk
Network
File Descriptors / System Limits
```

Exact resource requirements vary by workload and environment.

Do not apply arbitrary tuning values simply because a scan is slow.

---

# 9. Disk Space Problems

Insufficient disk space can cause unusual behavior.

Potential symptoms include:

* failed updates
* failed scans
* incomplete results
* service instability
* inability to write logs
* unexpected application errors

Check:

```text id="h2m8q5"
Available Disk Space
       ↓
Relevant Filesystems
       ↓
Logs / Data Growth
       ↓
Recent Error Messages
```

Do not delete Nessus data blindly.

First determine what is consuming space and what data is required.

---

# 10. Memory Pressure

Memory pressure may produce:

* slow UI
* slow scans
* service instability
* operating-system swapping
* scan failures
* processes being terminated

Check system-level evidence.

Do not assume that increasing concurrency will improve performance.

In some environments:

```text id="v5k2m9"
More Concurrency
      ↓
More Resource Pressure
      ↓
Worse Performance
```

---

# 11. CPU Saturation

High CPU usage can occur during intensive assessment activity.

Ask:

```text id="m7x3q4"
Is CPU usage:
- expected during an active scan?
- caused by Nessus?
- caused by another process?
- sustained after the scan?
```

A temporary increase during assessment is different from sustained system-level saturation.

---

# 12. Network Resource Problems

The scanner depends on network connectivity for:

* target assessment
* plugin/content updates
* license/activation-related communication where applicable
* administrative access
* external services where required by the product

A scanner may be locally healthy but operationally constrained by network controls.

Do not immediately classify this as a Nessus software failure.

---

# 13. Port Binding Problems

If the Nessus web interface cannot start, determine whether the required listening resource is available.

Conceptually:

```text id="n4q8v6"
Nessus
 ↓
Attempts to Listen
 ↓
Required Port Available?
 ↓
YES → Continue
NO  → Investigate Conflict
```

Possible causes include:

* another process using the port
* local firewall
* binding to an unexpected interface
* configuration problem

Use operating-system tools appropriate to your platform to inspect listeners.

---

# 14. Local Firewall Problems

A local firewall may prevent access to the Nessus interface.

Distinguish:

```text id="q8m3x1"
Nessus Not Listening
```

from:

```text id="c5v7n2"
Nessus Listening
but Access Blocked
```

This distinction saves time.

Check:

* listener state
* local firewall
* network path
* interface binding
* browser location

Do not disable the firewall blindly.

---

# 15. Interface Binding Problems

Nessus may be running but not reachable from the expected network interface.

Possible states:

```text id="r6x2m8"
Listening on localhost
```

versus:

```text id="t4n7q3"
Listening on expected interface
```

Determine what the installation is configured to expose.

If the product is intended for local administration, do not unnecessarily expose the interface to broader networks.

---

# 16. Browser and UI Problems

Sometimes the scanner is healthy while the browser experience is not.

Possible causes include:

* stale browser state
* cached content
* browser compatibility issue
* local proxy
* TLS/certificate warning
* extension interference
* incorrect URL/port
* network path problem

Basic isolation:

```text id="w5m9c2"
Try Known-Correct URL
        ↓
Check Browser Error
        ↓
Try Clean Session / Alternate Browser
        ↓
Check Local Connectivity
        ↓
Check Nessus Listener
```

Do not modify Nessus configuration merely because one browser behaves unexpectedly.

---

# 17. TLS / Certificate Symptoms

A Nessus interface may present TLS/certificate-related warnings depending on how it is configured.

Distinguish:

```text id="p3x7k9"
Certificate Warning
```

from:

```text id="z8m2q5"
TLS Service Failure
```

A certificate warning does not necessarily mean the scanner is broken.

If the environment uses custom certificates, verify:

* certificate validity
* hostname expectations
* expiration
* trust configuration
* recent certificate changes

Follow the product's current documentation for certificate management.

---

# 18. Initialization Problems

After installation or certain configuration changes, Nessus may not yet be ready for normal assessment activity.

Verify:

```text id="g5n8v2"
Initialization Complete?
Activation Complete?
Plugin Content Ready?
User Account Ready?
Scanner Available?
```

Do not begin assessment configuration until the platform is actually ready.

---

# 19. Activation and Licensing Problems

Activation or licensing state can affect available functionality.

Possible symptoms:

* activation prompts
* unavailable features
* scanner not ready
* license/subscription errors
* update-related limitations

Troubleshooting workflow:

```text id="m4q7x2"
Identify Edition
      ↓
Check License / Activation State
      ↓
Check Expiration / Status
      ↓
Check Network Requirements
      ↓
Check Product Messages
      ↓
Use Current Tenable Documentation
```

Do not assume a missing capability is a software failure.

It may be an edition or licensing difference.

---

# 20. Edition Differences

Before troubleshooting a missing feature, ask:

```text id="v8m3c6"
Does this Nessus edition provide the capability?
```

Possible explanations:

```text id="q2x7n5"
Feature Missing
      ↓
Edition Limitation?
Version Difference?
User Permission?
Configuration?
Plugin State?
Actual Failure?
```

This is especially important when following guides written for a different Nessus edition.

---

# 21. User Permission Problems

A Nessus user may be able to access the platform but lack permission for a specific action.

Symptoms may include:

* unavailable controls
* inability to edit settings
* inability to create certain assessments
* inability to manage users
* inability to access specific resources

Do not immediately modify the scanner.

First determine:

```text id="y6m2q9"
What action failed?
        ↓
Is the action permitted for this user?
        ↓
Is the feature available in this edition?
        ↓
Is the resource accessible?
```

---

# 22. Plugin / Content Readiness

Nessus depends heavily on its vulnerability and assessment content.

A scanner may be operational while its content is:

* updating
* incomplete
* unavailable
* stale
* inconsistent

Potential symptoms:

* assessment cannot start
* unexpected plugin behavior
* missing expected checks
* content update errors
* long initialization/update periods

Check product status and update indicators before troubleshooting individual findings.

---

# 23. Plugin Update Problems

If plugin/content updates fail, investigate:

```text id="n7x4m2"
Can Nessus Reach Required Services?
        ↓
Is DNS Working?
        ↓
Is Outbound Access Allowed?
        ↓
Is Proxy Configuration Correct?
        ↓
Is Activation Valid?
        ↓
Is Storage Available?
        ↓
Are Product Errors Present?
```

The exact update mechanism and requirements vary by product/version.

Use current Tenable documentation for environment-specific requirements.

---

# 24. Do Not Manually Manipulate Plugin Data

Avoid arbitrary:

* deleting plugin directories
* copying plugin databases from another installation
* replacing internal files
* modifying proprietary data structures
* applying undocumented patches

These actions can make diagnosis harder and may damage the installation.

Prefer supported product mechanisms.

---

# 25. Stale or Incomplete Plugin State

If the scanner appears to have inconsistent plugin state:

```text id="m5c8q3"
Record Current State
        ↓
Check Product Messages
        ↓
Check Update Status
        ↓
Check Disk / Network
        ↓
Follow Supported Recovery Process
        ↓
Verify Plugin Readiness
```

Do not immediately reinstall.

Reinstallation should generally be considered only after simpler supported recovery paths have been evaluated.

---

# 26. Version Problems

Record the exact version before troubleshooting:

```text id="r8m4x6"
Product:
Edition:
Version:
OS:
Architecture:
Scanner:
Plugin / Content State:
```

Then ask:

```text id="w2q7n9"
Did the problem begin after an upgrade?
```

If yes, investigate:

* release-specific behavior
* compatibility
* changed configuration
* deprecated settings
* changed feature availability
* documented upgrade requirements

Use official Tenable release notes and documentation for version-specific conclusions.

---

# 27. Upgrade-Related Problems

After an upgrade, do not assume the upgrade itself is defective.

Check:

```text id="f4m8c2"
Before Upgrade
      ↓
Upgrade
      ↓
Observed Change
      ↓
Configuration
      ↓
Service State
      ↓
Content State
      ↓
Permissions
      ↓
Platform Dependencies
```

Record what changed.

This is especially important when the previous installation worked normally.

---

# 28. Operating-System Compatibility

Nessus behavior depends on the supported operating system and installation method.

When troubleshooting:

* identify OS version
* identify architecture
* identify Nessus version
* confirm support status
* review installation method
* review recent OS changes

Do not assume a generic Linux command or Windows procedure applies universally.

---

# 29. Service Logs

Logs are among the most useful troubleshooting evidence.

Use them to answer:

```text id="j6x3m8"
What failed?
When did it fail?
What component reported the failure?
What happened immediately before it?
```

Do not paste enormous logs into a report.

Extract the relevant:

* timestamp
* error
* warning
* component
* sequence of events

Preserve enough context to understand the error.

---

# 30. Log Interpretation

Suppose you observe:

```text id="z5m8q2"
ERROR:
Unable to initialize component X
```

Do not immediately conclude:

> "Component X is broken."

Ask:

```text id="p4x7n1"
Why?
 ↓
Missing dependency?
Permission?
Configuration?
Resource?
Corrupt state?
Version mismatch?
```

The log message is evidence, not necessarily the root cause.

---

# 31. Recent Changes Are High-Value Evidence

Create a change timeline:

```text id="q8m3v6"
10:00  Nessus Working
10:15  OS Updated
10:30  Nessus Restarted
10:31  UI Unavailable
```

This is more useful than:

> "Nessus suddenly stopped."

Another example:

```text id="c6x2m9"
09:00  Plugin Update Started
09:20  Update Failed
09:25  Scan Cannot Start
```

The relationship may identify the relevant layer.

---

# 32. Configuration Changes

Configuration changes can cause scanner behavior to change.

When investigating:

```text id="n4m7x2"
Current Configuration
        ↓
Recent Configuration Change
        ↓
Expected Behavior
        ↓
Observed Behavior
```

If possible, compare with the last known-good configuration.

Do not change multiple settings simultaneously.

---

# 33. One Change at a Time

A fundamental troubleshooting rule:

```text id="x7q3m5"
Problem
 ↓
Hypothesis
 ↓
One Change
 ↓
Test
 ↓
Result
```

Avoid:

```text id="w8n2c6"
Change 7 Settings
 ↓
Restart
 ↓
Problem Changes
 ↓
Unknown Cause
```

You may restore functionality but lose the ability to explain why.

---

# 34. Restarting Nessus

Restarting can be appropriate when:

* a supported troubleshooting procedure requires it
* the service is stuck
* a configuration change requires restart
* product documentation recommends it

But:

```text id="r5m9q2"
Restart
≠
Diagnosis
```

Before restarting, capture useful state when possible.

After restarting, verify:

* service state
* UI
* scanner availability
* plugin readiness
* assessment functionality

---

# 35. Rebooting the Host

A full reboot is more disruptive than restarting the Nessus service.

Consider:

* other workloads
* active assessments
* maintenance windows
* system availability
* operational impact

Do not reboot simply because Nessus is slow.

First identify whether the problem actually requires it.

---

# 36. Scanner Stuck in an Unexpected State

Possible symptoms:

* assessment remains queued
* assessment appears active but produces no progress
* scanner reports unavailable
* UI state does not update

Use:

```text id="m3q8v5"
Check Scanner State
       ↓
Check Active Assessment
       ↓
Check Service Health
       ↓
Check Resource Usage
       ↓
Check Logs
       ↓
Check Network / Target Conditions
```

Do not immediately terminate every assessment.

Determine whether it is actually stuck.

---

# 37. Queued Assessment Problems

A queued assessment can have several explanations.

Possible causes include:

* scanner capacity
* another assessment consuming resources
* scheduling
* scanner availability
* configuration issue
* product state
* resource exhaustion

Ask:

```text id="j8x4m2"
Is the scanner busy?
Is the assessment scheduled?
Is the scanner available?
Are resources sufficient?
Did the platform report an error?
```

Do not assume queueing means failure.

---

# 38. Slow Scanner vs Broken Scanner

A slow scan may be normal.

Potential reasons:

* large scope
* many plugins
* slow targets
* network latency
* rate limiting
* service timeouts
* authentication delays
* intentionally conservative configuration
* resource contention

The useful question is:

> "Is there meaningful progress?"

Monitor:

* target progression
* host completion
* service activity
* finding generation
* scanner resource state
* error messages

---

# 39. No Progress

If there is no meaningful progress, investigate systematically.

```text id="y4m7q2"
No Progress
   ↓
Scanner Healthy?
   ↓
Target Reachable?
   ↓
Network Responsive?
   ↓
Resources Available?
   ↓
Errors?
   ↓
Authentication Delay?
   ↓
Timeout / Retry Behavior?
```

Avoid increasing concurrency simply because progress appears slow.

---

# 40. Scanner Resource Contention

If multiple assessments run simultaneously:

```text id="q6x3m8"
Assessment A ─┐
Assessment B ─┼── Scanner Resources
Assessment C ─┘
```

Possible effects include:

* longer scan duration
* higher CPU usage
* memory pressure
* network saturation
* slower target response
* increased timeout behavior

Consider workload management before changing scan settings.

---

# 41. Scanner Capacity

Scanner capacity depends on the environment.

Factors include:

* hardware
* target count
* plugin workload
* network conditions
* concurrency
* target responsiveness
* authenticated checks
* simultaneous assessments

Do not rely on a single universal "maximum hosts per scan" number.

Measure the actual environment.

---

# 42. Unexpected Scan Termination

If an assessment terminates unexpectedly:

```text id="f9m3x7"
Record Status
      ↓
Check Error Message
      ↓
Check Scanner Service
      ↓
Check System Resources
      ↓
Check Logs
      ↓
Check Recent Changes
      ↓
Determine Cause
```

Possible causes include:

* service failure
* resource exhaustion
* network interruption
* target behavior
* configuration
* platform restart
* scanner update
* unexpected system event

Do not simply rerun without understanding the failure.

---

# 43. Scanner Restart During Assessment

If the scanner restarts during an assessment, treat the result carefully.

Determine:

* whether the assessment resumed
* whether it restarted
* which targets were assessed
* whether results are complete
* whether the final status reflects full coverage

A completed-looking result does not automatically prove complete coverage after an interruption.

---

# 44. Database / Internal State Problems

Nessus internally maintains application state and assessment data.

If you observe symptoms suggesting internal state problems:

* collect logs
* record version
* record recent changes
* verify storage
* follow supported recovery procedures

Do not manually modify internal databases unless an official procedure explicitly requires it.

---

# 45. Backup and Recovery Awareness

Before making major changes, understand what can be restored.

For important deployments, maintain appropriate:

* configuration backups
* change records
* license/activation information
* deployment documentation
* recovery procedures

The exact backup mechanisms depend on the product/version and deployment model.

---

# 46. Installation Corruption

Possible indicators include:

* repeated startup failures
* missing required components
* unexpected internal errors
* failed updates
* damaged installation state

Do not immediately conclude:

> "Reinstall Nessus."

First collect:

```text id="v8q2m5"
Version
OS
Installation Method
Logs
Recent Changes
Disk State
Configuration
Update State
```

Then determine whether repair, supported recovery, upgrade, or reinstall is appropriate.

---

# 47. Reinstallation

Reinstallation can be disruptive.

Before considering it:

* determine whether configuration needs preservation
* determine whether activation must be restored
* understand what assessment data may be affected
* record the current version
* preserve relevant evidence
* consult current Tenable documentation

Never delete the installation simply to "start fresh" before understanding the consequences.

---

# 48. Proxy Problems

Some Nessus environments use proxies for outbound communication.

If updates or required connections fail:

```text id="m7x4q2"
Nessus
 ↓
Proxy
 ↓
Network
 ↓
Required Destination
```

Check:

* proxy configuration
* proxy reachability
* authentication requirements
* DNS
* firewall
* TLS inspection
* recent proxy changes

The exact supported configuration depends on the Nessus version and environment.

---

# 49. DNS Problems

DNS failures can affect:

* plugin/content updates
* target hostname resolution
* external connectivity
* administrative workflows

Test:

```text id="x5n8c3"
Hostname
 ↓
DNS Resolution
 ↓
Network Connectivity
 ↓
Destination Reachability
```

Do not assume that because the internet works in a browser, Nessus has equivalent network access.

Different services can use different network paths or proxy settings.

---

# 50. Time and Clock Problems

System time can affect:

* TLS validation
* certificates
* logs
* scheduled assessments
* update processes

Check:

```text id="q3m7v9"
System Time
Timezone
Time Synchronization
```

If timestamps appear inconsistent, establish whether the problem is actual system time or merely display/timezone interpretation.

---

# 51. Certificate Expiration

If a certificate-related failure occurs:

```text id="h6x2m8"
Certificate:
Issuer
Validity
Hostname
Trust
Recent Changes
```

Do not simply disable certificate validation as a workaround.

Determine the actual certificate problem.

---

# 52. Scheduled Assessment Problems

If a scheduled assessment does not execute:

Check:

```text id="p8q4m1"
Schedule
   ↓
Scanner Available?
   ↓
Target Available?
   ↓
Credentials Valid?
   ↓
Platform Ready?
   ↓
Assessment Created Correctly?
   ↓
Execution History
```

A schedule can be configured correctly while the underlying scanner or target is unavailable.

---

# 53. Scanner Problem Classification

Use this classification:

| Symptom                       | First Layer                             |
| ----------------------------- | --------------------------------------- |
| UI unavailable                | Service / listener / local network      |
| Service stops                 | Logs / configuration / platform         |
| Scanner unavailable           | Product state / service / licensing     |
| Plugin update fails           | Network / activation / storage          |
| Scan cannot start             | Scanner readiness / configuration       |
| Scan queued                   | Scheduler / capacity / scanner state    |
| Scan slow                     | Scope / resources / target behavior     |
| Scan stops                    | Logs / resources / service / network    |
| Findings unexpectedly missing | Coverage / plugins / configuration      |
| Feature unavailable           | Edition / version / permissions         |
| Upgrade caused issue          | Version / configuration / compatibility |

This table is a starting point, not a substitute for evidence.

---

# 54. Practical Lab 1 — Service Failure

## Objective

Learn to identify whether the Nessus service is actually running.

## Tasks

1. Stop the Nessus service in your authorized lab.
2. Observe the resulting behavior.
3. Identify the visible symptom.
4. Check service state.
5. Check relevant logs.
6. Restore the service.
7. Verify scanner readiness.

Record:

```text id="y2m7q4"
Initial State:
Failure:
Observed Symptom:
Service State:
Evidence:
Root Cause:
Recovery:
Verification:
```

Do not perform this on an important production scanner without authorization.

---

# 55. Practical Lab 2 — Resource Observation

## Objective

Understand scanner resource behavior.

Run an appropriately sized authorized lab assessment.

Observe:

* CPU
* memory
* disk
* network
* assessment progress

Record:

```text id="v8q3m1"
Target Count:
Assessment Type:
Observed CPU:
Observed Memory:
Observed Disk:
Observed Network:
Scan Progress:
Errors:
Conclusion:
```

Do not deliberately exhaust resources on an important system.

---

# 56. Practical Lab 3 — Port / Listener Troubleshooting

## Scenario

The Nessus interface is inaccessible.

## Task

Determine:

1. Is the Nessus service running?
2. Is it listening?
3. What interface is it bound to?
4. Is a local firewall involved?
5. Is the browser using the correct endpoint?
6. Can the issue be fixed without changing unrelated settings?

Record:

```text id="m4x8q2"
Service:
Listener:
Binding:
Firewall:
Client:
Root Cause:
Fix:
Verification:
```

---

# 57. Practical Lab 4 — Plugin Readiness

## Objective

Understand the difference between:

```text id="c5m7x2"
Scanner Running
```

and:

```text id="n8q3v6"
Scanner Ready for Assessment
```

## Tasks

Inspect the platform state before creating an assessment.

Record:

```text id="h2q6m9"
Edition:
Version:
Activation:
Plugin State:
Scanner State:
Assessment Ready:
```

Do not manually manipulate plugin files.

---

# 58. Practical Lab 5 — Edition / Version Investigation

## Scenario

A tutorial tells you to use a capability that is not visible.

## Task

Determine whether the difference is caused by:

* edition
* version
* permissions
* configuration
* feature availability
* actual failure

Do not assume the feature is broken.

Record:

```text id="z7m4c1"
Expected Capability:
Actual Environment:
Edition:
Version:
User Permissions:
Evidence:
Conclusion:
```

---

# 59. Practical Lab 6 — Scan That Appears Stuck

## Scenario

An authorized lab assessment appears to make no progress.

## Task

Investigate in this order:

```text id="f3q8m5"
Scanner Health
      ↓
Assessment Status
      ↓
Target Reachability
      ↓
Resources
      ↓
Network
      ↓
Authentication
      ↓
Logs
```

Record:

```text id="x6n2v8"
Observed Progress:
Scanner State:
Target State:
Resource State:
Network State:
Authentication:
Relevant Errors:
Hypothesis:
Action:
Result:
```

Do not immediately cancel the assessment.

---

# 60. Practical Lab 7 — Upgrade Investigation

## Scenario

A controlled lab scanner worked before an upgrade and behaves differently afterward.

## Task

Create a timeline:

```text id="m9x4q2"
Known-Good State
      ↓
Upgrade
      ↓
First Failure
      ↓
Observed Symptoms
      ↓
Evidence
      ↓
Hypothesis
      ↓
Recovery
      ↓
Verification
```

Determine whether the upgrade is actually supported by evidence as the cause.

---

# 61. Troubleshooting Record

For important scanner problems, maintain:

```text id="r5q8m3"
Nessus Troubleshooting Record
=============================

Date:

Product:
Edition:
Version:
OS:
Architecture:

Observed Problem:

Expected Behavior:

Actual Behavior:

Impact:

Recent Changes:

Service State:

UI State:

Scanner State:

Activation / License State:

Plugin / Content State:

CPU:

Memory:

Disk:

Network:

Relevant Logs:

Configuration Changes:

Hypothesis:

Action Taken:

Result:

Root Cause:

Recovery:

Verification:

Follow-Up:

Documentation / Reference:
```

This makes troubleshooting repeatable.

---

# 62. Common Troubleshooting Mistakes

## Mistake 1 — Restarting Everything Immediately

### Problem

The service is restarted before evidence is collected.

### Better approach

Capture the current state first.

---

## Mistake 2 — Reinstalling Too Early

### Problem

Potentially useful evidence and configuration are lost.

### Better approach

Investigate service, logs, resources, configuration and supported recovery first.

---

## Mistake 3 — Changing Multiple Settings

### Problem

You cannot identify which change affected the result.

### Better approach

Change one variable at a time.

---

## Mistake 4 — Assuming Every Problem Is a Nessus Problem

### Problem

The actual issue is DNS, firewall, storage, CPU, or the operating system.

### Better approach

Troubleshoot by layer.

---

## Mistake 5 — Ignoring Version and Edition

### Problem

A feature from another edition/version is treated as broken.

### Better approach

Identify the actual product context first.

---

## Mistake 6 — Deleting Internal Data

### Problem

Internal application state is modified without understanding the consequences.

### Better approach

Use supported recovery procedures.

---

## Mistake 7 — Increasing Scan Aggressiveness

### Problem

Performance problems are "solved" by increasing concurrency or other aggressive settings.

### Better approach

Determine whether the bottleneck is scanner resources, network, targets, configuration, or workload.

---

## Mistake 8 — Ignoring Logs

### Problem

Troubleshooting becomes guesswork.

### Better approach

Use timestamps and relevant errors to build a timeline.

---

## Mistake 9 — Disabling Security Controls as a First Step

### Problem

Firewalls, TLS validation, or other controls are disabled without proving they are the cause.

### Better approach

Identify the blocked path first.

---

## Mistake 10 — Repeating the Same Failed Action

### Problem

The same restart or scan is performed repeatedly.

### Better approach

Update the hypothesis based on new evidence.

---

# 63. Decision Tree

Use this decision tree:

```text id="c2x7m5"
Nessus Problem
      ↓
What exactly failed?
      ↓
─────────────────────────────────
│               │               │
UI              Service         Assessment
│               │               │
↓               ↓               ↓
Listener?       Running?        Scanner Ready?
Network?        Logs?            Target?
Browser?        Resources?       Configuration?
│               │               │
↓               ↓               ↓
Platform        Platform        Assessment
Investigation  Investigation    Investigation
```

Then:

```text id="q8m4v2"
If the problem is not at the scanner/platform layer:
        ↓
Move to the appropriate troubleshooting workflow.
```

Do not continue changing scanner settings when evidence points to a target or authentication problem.

---

# 64. Escalation Criteria

Consider escalation when:

* supported troubleshooting does not resolve the issue
* the failure is reproducible
* product behavior appears inconsistent with documentation
* an upgrade introduced unexplained behavior
* internal state appears corrupted
* licensing/activation requires vendor assistance
* the issue affects production operations
* recovery could risk assessment data
* the problem requires product-specific undocumented knowledge

Before escalation, collect:

```text id="m6q3x8"
Product / Edition
Version
OS
Architecture
Exact Symptom
Expected Behavior
Observed Behavior
Timeline
Relevant Logs
Recent Changes
Reproduction Steps
Actions Already Tried
Current State
```

Good evidence reduces resolution time.

---

# 65. Completion Criteria

You have completed this workflow when you can independently:

* determine whether the Nessus service is running
* distinguish UI problems from scanner problems
* verify scanner readiness
* investigate startup failures
* use logs effectively
* investigate CPU, memory, disk and network constraints
* investigate listener and firewall problems
* recognize plugin/content readiness issues
* investigate activation/licensing problems
* distinguish edition/version limitations from actual failures
* recognize permission problems
* investigate upgrade-related behavior
* troubleshoot queued or apparently stuck assessments
* avoid destructive troubleshooting
* change one variable at a time
* document troubleshooting evidence
* recover a controlled lab scanner
* determine when escalation is appropriate

---

# 66. Final Mental Model

Remember:

```text id="w4n8m2"
OBSERVE
   ↓
DEFINE
   ↓
CLASSIFY
   ↓
COLLECT EVIDENCE
   ↓
HYPOTHESIZE
   ↓
CHANGE ONE THING
   ↓
TEST
   ↓
VERIFY
   ↓
DOCUMENT
```

And troubleshoot from the bottom up:

```text id="p7m3q9"
HOST / PLATFORM
      ↓
NESSUS SERVICE
      ↓
NESSUS UI
      ↓
SCANNER STATE
      ↓
PLUGIN / CONTENT STATE
      ↓
CONFIGURATION
      ↓
ASSESSMENT
      ↓
TARGET
```

The professional skill is not memorizing every Nessus error.

It is being able to answer:

> **What failed, which layer owns the failure, what evidence supports that conclusion, what is the least disruptive corrective action, and how will I verify that the scanner is healthy again?**
