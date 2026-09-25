# Independent Assessment Labs

## Objective

This lab is the transition from guided Nessus operation to independent assessment work.

Earlier decision labs provided:

* explicit questions
* known facts
* unknowns
* possible actions
* expected outcomes
* decision structures

This lab deliberately removes much of that guidance.

You receive an assessment objective and a set of constraints.

You must determine:

```text id="9q1n7v"
WHAT TO ASK
      ↓
WHAT TO VERIFY
      ↓
WHAT WORKFLOW TO USE
      ↓
WHAT TO CONFIGURE
      ↓
HOW TO EXECUTE
      ↓
HOW TO MONITOR
      ↓
HOW TO INTERPRET
      ↓
WHAT TO VALIDATE
      ↓
HOW TO PRIORITIZE
      ↓
HOW TO REPORT
      ↓
HOW TO RETEST
      ↓
WHEN TO CLOSE
```

The objective is not to produce the largest number of findings.

The objective is to produce a **defensible assessment**.

---

# Independent Lab Rules

For these labs:

1. Use only authorized systems or intentionally vulnerable lab systems.
2. Do not assume missing information.
3. Do not expand scope without authorization.
4. Treat Nessus results as evidence, not automatically as final conclusions.
5. Record important decisions.
6. Preserve uncertainty where it remains.
7. Prefer low-impact evidence gathering.
8. Do not perform disruptive validation without explicit authorization.
9. Account for scanner position.
10. Account for authentication state.
11. Account for plugin/content state.
12. Account for configuration changes.
13. Distinguish vulnerability severity from environmental priority.
14. Verify remediation rather than accepting claims as proof.
15. Document incomplete coverage.
16. Treat operational impact as part of assessment quality.

---

# Independent Assessment Record

Before starting each lab, create this record.

```text id="5k1r7p"
Assessment Name:

Authorization:

Assessment Objective:

Scope:

Out of Scope:

Assessment Window:

Scanner:

Scanner Position:

Targets:

Expected Hosts:

Expected Services:

Authentication:

Workflow:

Configuration:

Operational Constraints:

Stop Conditions:

Expected Evidence:

Success Criteria:

Known Unknowns:

Initial Assumptions:

Decision Log:

Coverage:

Findings:

Validation:

Prioritization:

Reporting:

Remediation:

Retest:

Final Status:
```

Do not fill unknown fields with guesses.

Use:

```text
Unknown
```

or:

```text
Not yet established
```

when appropriate.

---

# Independent Lab 1 — Basic Vulnerability Assessment

## Scenario

You have an authorized lab server.

The assessment request states:

> Identify vulnerabilities observable from the assigned scanner and provide remediation guidance.

### Constraints

* One target
* No credentials
* Controlled lab
* No disruptive validation
* Normal scan impact

### Your Task

Independently determine:

* assessment objective
* target definition
* workflow
* discovery requirements
* scan configuration
* execution process
* monitoring approach
* result interpretation
* validation requirements
* report structure
* remediation recommendations

### Required Output

Produce:

```text id="n2g3j9"
1. Assessment record
2. Scan configuration rationale
3. Coverage assessment
4. Finding investigation
5. Validation decision
6. Prioritization rationale
7. Remediation recommendation
8. Final conclusion
```

---

# Independent Lab 2 — Authenticated Assessment

## Scenario

The same authorized server must now be assessed from an authenticated perspective.

Credentials are provided through an approved secure mechanism.

### Constraints

* Credentials may not be stored in the repository.
* Least privilege should be used.
* No password guessing.
* No disruptive testing.

### Your Task

Determine:

* whether authentication is required
* what authentication method is appropriate
* what permissions are necessary
* how authentication should be verified
* how authenticated coverage should be assessed
* how results should be compared with the unauthenticated assessment

### Required Question

If the authenticated scan finds more vulnerabilities, determine whether this necessarily means the system became less secure.

---

# Independent Lab 3 — Discovery Before Assessment

## Scenario

You receive an authorized network range but no current asset inventory.

The objective is:

> Identify observable systems and services that should inform subsequent vulnerability assessment.

### Constraints

* Discovery only initially
* Do not automatically perform full vulnerability assessment
* Stay within authorization

### Your Task

Determine:

* discovery scope
* host discovery approach
* service discovery approach
* expected output
* missing-host handling
* unexpected-host handling
* next assessment decisions

### Required Output

Create an attack-surface record:

```text id="5ot1am"
Authorized Scope:

Observed Hosts:

Missing Expected Hosts:

Unexpected Hosts:

Observed Services:

Unexpected Services:

Technology Observations:

Coverage Limitations:

Next Assessment Decisions:
```

---

# Independent Lab 4 — Partial Coverage

## Scenario

An assessment is expected to cover 25 hosts.

After execution:

```text
21 hosts produced results
2 hosts were unreachable
2 hosts were not observed
```

### Task

Determine whether the assessment is complete.

Then determine:

* what caused the missing coverage
* whether the existing results remain useful
* what must be documented
* whether another scan is required

### Required Conclusion

Your conclusion must distinguish:

```text
Assessment results available
```

from:

```text
Complete assessment coverage
```

---

# Independent Lab 5 — Network Troubleshooting

## Scenario

A target is authorized.

The scanner is healthy.

The target does not appear in the expected results.

### Constraints

Do not change target scope.

### Task

Independently troubleshoot:

```text id="m9c5s1"
Target identity
↓
Scanner position
↓
DNS
↓
Routing
↓
Host reachability
↓
Required ports
↓
Services
↓
Firewalls / ACLs
↓
Intermediaries
↓
Target health
↓
Nessus configuration
```

### Required Output

Document the first layer at which the expected path breaks.

---

# Independent Lab 6 — Authentication Failure

## Scenario

A recurring authenticated assessment suddenly reports no authenticated results.

The same configuration worked previously.

### Task

Determine the troubleshooting order.

Your investigation should distinguish:

* network reachability
* authentication service
* authentication method
* account status
* credential validity
* permissions
* target-side restrictions
* Nessus configuration
* environmental changes

### Required Output

Produce:

```text id="0k2q8s"
Observed Change:

Known:

Unknown:

Most Relevant Hypotheses:

Evidence Required:

Verification Steps:

Expected Results:

Actual Results:

Root Cause:

Coverage Impact:

Corrective Action:

Retest:
```

---

# Independent Lab 7 — High-Severity Finding

## Scenario

Nessus reports a high-severity vulnerability on an important internal server.

### Task

Do not immediately assign remediation priority.

Investigate:

* detection basis
* affected component
* applicability
* network exposure
* authentication requirements
* asset importance
* technical impact
* exploitability evidence
* compensating controls
* remediation complexity
* root cause
* validation requirements

### Required Output

Explain why the finding receives its eventual remediation priority.

Your reasoning must be evidence-based rather than severity-only.

---

# Independent Lab 8 — Conflicting Evidence

## Scenario

Nessus reports that a package appears vulnerable.

The system administrator provides evidence indicating the vendor security update was installed.

### Task

Resolve the discrepancy.

Investigate:

* package version
* vendor packaging
* backported fixes
* installed state
* active component
* Nessus detection evidence
* plugin/content state

### Possible Outcomes

```text
Confirmed
Not Applicable
False Positive
Unresolved
Changed Condition
```

Choose only the outcome supported by evidence.

---

# Independent Lab 9 — Finding Disappeared

## Scenario

Assessment A:

```text
Finding present
```

Assessment B:

```text
Finding absent
```

### Available Information

The assessments were performed at different times.

### Task

Determine whether remediation can be established.

Check:

* scope
* target identity
* scanner
* scanner position
* authentication
* configuration
* plugin/content state
* target state
* service state

### Required Output

Create a comparison matrix and explain whether the assessments are comparable.

---

# Independent Lab 10 — Remediation Verification

## Scenario

A system owner states:

> "All affected systems have been remediated."

### Task

Design an independent verification workflow.

Determine:

* population to retest
* evidence required
* appropriate Nessus perspective
* configuration consistency
* comparison method
* handling of remaining findings
* handling of systems that cannot be assessed

### Required Output

Produce a remediation verification plan.

---

# Independent Lab 11 — Fleet Remediation

## Scenario

A vulnerability affects 100 authorized systems.

The owner claims:

> "We patched the fleet."

### Task

Determine how you would establish whether the statement is supported.

Consider:

* asset population
* patch population
* missing systems
* authentication coverage
* system replacements
* exceptions
* inaccessible systems
* partial remediation
* retest sampling versus full verification

### Required Question

What evidence would allow you to distinguish:

```text
Patch claimed
```

from:

```text
Patch verified
```

---

# Independent Lab 12 — Scanner Position Comparison

## Scenario

The same application is assessed from:

* an internal scanner
* an external scanner

The findings differ.

### Task

Determine why.

Consider:

* routing
* firewall rules
* exposed services
* DNS
* load balancing
* source filtering
* authentication
* network segmentation

### Required Output

Separate:

```text
Technical differences
```

from:

```text
Perspective differences
```

---

# Independent Lab 13 — Recurring Assessment Drift

## Scenario

A recurring scan has been running for several months.

Recently:

* finding counts changed
* host count changed
* authentication coverage declined
* scan duration increased

### Task

Determine whether the environment changed, the assessment changed, or both.

Investigate:

* target list
* DNS
* scanner
* scanner capacity
* credentials
* configuration
* plugin/content state
* network architecture
* target environment
* assessment schedule

### Required Output

Create a drift investigation record.

---

# Independent Lab 14 — Production Assessment Planning

## Scenario

You must assess an authorized production environment.

### Constraints

* Business-critical systems exist.
* A maintenance window is available.
* Some systems have restricted scanning requirements.
* Authentication is available for some assets.
* The environment is distributed across multiple networks.

### Task

Create a complete assessment plan covering:

```text id="d4f1nb"
Authorization
Scope
Objective
Asset classification
Scanner placement
Workflow selection
Authentication
Configuration
Scheduling
Operational constraints
Stop conditions
Monitoring
Communication
Results
Validation
Reporting
Remediation
Retest
```

### Required Output

Produce a pre-assessment operational plan.

---

# Independent Lab 15 — Production Impact During Scan

## Scenario

During an authorized production assessment, monitoring indicates increased application latency.

### Task

Determine:

* what evidence to collect
* whether to continue
* whether to reduce activity
* whether to stop
* who should be notified
* how to document the event
* how to determine whether the scan contributed to the impact

### Required Principle

Do not treat completion of the scan as more important than operational safety.

---

# Independent Lab 16 — Unknown Business Context

## Scenario

A technically validated vulnerability exists on a system.

You do not know:

* business owner
* data classification
* criticality
* recovery requirements

### Task

Determine what can still be concluded.

Separate:

```text
Technical validity
```

from:

```text
Business priority
```

### Required Output

Produce a finding record with explicit context limitations.

---

# Independent Lab 17 — Report Under Uncertainty

## Scenario

The assessment is complete but contains:

* one significant unresolved finding
* incomplete business context
* one inaccessible host
* one failed authenticated target

### Task

Write a professional assessment conclusion.

It must communicate:

* what was assessed
* what was found
* what was not established
* coverage limitations
* unresolved items
* recommended next actions

Do not convert uncertainty into certainty.

---

# Independent Lab 18 — Stakeholder Wants a Clean Report

## Scenario

A stakeholder asks you to remove:

* unresolved findings
* incomplete hosts
* limitations

because:

> "The client only wants the important results."

### Task

Determine how to preserve an accurate assessment record while keeping the report useful for its intended audience.

### Required Output

Create:

```text id="pr1q9e"
Executive Summary
Technical Findings
Coverage Limitations
Unresolved Items
Recommended Actions
```

The executive layer may be concise without removing material limitations.

---

# Independent Lab 19 — Old Assessment With Poor Documentation

## Scenario

A previous assessment contains findings but no reliable record of:

* scanner position
* credentials
* target list
* configuration
* plugin state
* objective

### Task

Determine whether the old assessment can be used as a direct baseline.

### Required Output

Classify the historical assessment as:

```text
Comparable
Partially Comparable
Not Reliably Comparable
```

Then explain why.

---

# Independent Lab 20 — Plugin Update Investigation

## Scenario

A plugin update occurs between two recurring assessments.

Several findings change.

### Task

Determine:

* which findings changed
* whether detection logic changed
* whether target state changed
* whether the assessment configuration changed
* whether the results remain comparable

### Required Output

Produce a plugin-change impact record.

---

# Independent Lab 21 — Configuration Drift

## Scenario

A recurring assessment originally used a carefully reviewed configuration.

Months later, results differ significantly.

No configuration review has been performed since the original deployment.

### Task

Determine whether configuration drift may explain the difference.

Review:

* targets
* discovery
* assessment settings
* credentials
* plugins
* performance
* advanced settings
* scheduling
* scanner state

### Required Output

Produce a configuration comparison record.

---

# Independent Lab 22 — Unexpected Asset Discovery

## Scenario

A recurring assessment discovers several systems that were not in the previous inventory.

### Task

Determine:

* whether they are authorized
* whether they are new
* whether they were previously missed
* whether they should become part of the baseline
* whether their discovery changes future assessment planning

Do not automatically add them to recurring scope.

---

# Independent Lab 23 — Missing Asset Discovery

## Scenario

A previously assessed production server no longer appears.

### Task

Investigate whether the system:

* was decommissioned
* changed IP
* changed hostname
* moved networks
* became inaccessible
* was removed from the scan configuration
* is affected by a DNS change

### Required Output

Produce a coverage-loss investigation.

---

# Independent Lab 24 — Shared Root Cause

## Scenario

The scan reports 45 findings across 15 systems.

Investigation suggests many originate from one outdated software component.

### Task

Determine:

* unique root causes
* affected assets
* dependent findings
* remediation action
* retest population

### Required Output

Produce a root-cause-based remediation plan rather than a simple 45-item list.

---

# Independent Lab 25 — Vulnerability vs Exposure

## Scenario

A vulnerability is confirmed on an internal service.

The service is not externally reachable.

### Task

Separate:

```text
Vulnerability existence
```

from:

```text
External exposure
```

Then determine what additional context is needed for prioritization.

---

# Independent Lab 26 — Compliance Assessment

## Scenario

An organization provides a defined configuration baseline.

Your task is to determine whether authorized systems satisfy it.

### Task

Design the assessment around:

* baseline
* controls
* authentication
* evidence
* exceptions
* pass/fail/unknown handling
* remediation
* retest

### Required Principle

Do not transform a compliance result into a general statement that the system is secure.

---

# Independent Lab 27 — Discovery to Vulnerability Assessment

## Scenario

Discovery identifies:

```text id="9kq3m2"
Host A — HTTPS
Host B — SSH
Host C — DNS
Host D — Unknown service
```

### Task

Independently determine which subsequent vulnerability assessment workflows should be considered.

Do not assume every host requires identical configuration.

---

# Independent Lab 28 — Multiple Network Segments

## Scenario

Authorized targets exist across three network segments.

One scanner cannot reach all three consistently.

### Task

Determine whether to:

* reposition the scanner
* use multiple scanners
* modify routing
* modify target scope
* schedule separate assessments

Base the decision on:

* authorization
* network architecture
* operational constraints
* required assessment perspective
* coverage

---

# Independent Lab 29 — Assessment Window Constraint

## Scenario

You have two hours to assess a large authorized environment.

A complete scan may require substantially longer.

### Task

Do not simply increase aggressiveness.

Determine:

* what objective can realistically be answered
* whether scope should be phased
* whether multiple windows are required
* whether discovery should precede assessment
* what coverage must be documented

---

# Independent Lab 30 — Emergency Stop

## Scenario

During a scan, the target owner reports unexpected instability and requests immediate cessation.

### Task

Determine:

* how to stop the assessment
* what evidence to preserve
* what to document
* who to notify
* how to determine partial coverage
* what must happen before any future retest

### Required Principle

An authorized scan can still require immediate cessation when operational conditions change.

---

# Independent Lab 31 — Finding With No Clear Owner

## Scenario

A significant finding is validated.

No team accepts ownership.

### Task

Determine how to progress the assessment without inventing responsibility.

Document:

* technical finding
* affected assets
* ownership gap
* required ownership assignment
* interim risk handling
* escalation path

---

# Independent Lab 32 — Reappearing Vulnerability

## Scenario

A vulnerability was previously remediated.

A later recurring scan detects it again.

### Task

Investigate:

* deployment process
* configuration management
* software installation
* patch management
* asset replacement
* rollback
* exception handling

### Required Output

Treat the event as both:

```text
Current vulnerability
```

and:

```text
Control/process recurrence
```

where supported by evidence.

---

# Independent Lab 33 — Partial Fleet Verification

## Scenario

A vulnerability affects 500 systems.

The organization can provide reliable access to 450.

### Task

Determine whether the fleet can be declared remediated.

### Required Reasoning

Separate:

```text
450 verified systems
```

from:

```text
50 systems not independently verified
```

Do not silently treat inaccessible systems as compliant.

---

# Independent Lab 34 — Assessment With Conflicting Scope

## Scenario

You receive:

* a ticket
* an authorization email
* a target spreadsheet

Each contains a slightly different target list.

### Task

Determine which information must be reconciled before scanning.

### Required Output

Produce:

```text id="0v2k0k"
Conflicting Scope Items:

Authoritative Source:

Confirmed Scope:

Excluded Scope:

Unresolved Items:

Assessment Decision:
```

---

# Independent Lab 35 — Unknown Nessus Environment

## Scenario

You inherit an existing Nessus installation.

You do not know:

* edition
* version
* scanner configuration
* license state
* plugin state
* scanner role

### Task

Perform an environment discovery exercise before performing substantive assessment work.

### Required Output

Produce a Nessus environment profile.

---

# Independent Lab 36 — Different Nessus UI

## Scenario

A colleague's screenshots do not match your Nessus interface.

### Task

Determine how to proceed without assuming the installation is broken.

Investigate:

* edition
* version
* licensing
* permissions
* available features
* current documentation

### Required Principle

Learn capabilities rather than memorizing interface positions.

---

# Independent Lab 37 — Scan Configuration Review

## Scenario

You inherit a scan configuration created by another operator.

The target environment has changed substantially.

### Task

Determine whether the configuration should be reused, modified, or rebuilt.

Review:

* objective
* target scope
* discovery
* assessment settings
* credentials
* plugins
* performance
* exclusions
* scheduling
* scanner

---

# Independent Lab 38 — Assessment With No Findings

## Scenario

A scan completes and reports no vulnerabilities.

### Task

Do not immediately conclude that the environment is secure.

Determine:

* target coverage
* service discovery
* plugin execution
* authentication state
* scanner position
* expected vulnerability presence
* configuration
* scan errors
* assessment limitations

### Required Conclusion

State what the assessment established and what it did not establish.

---

# Independent Lab 39 — Very Large Finding Set

## Scenario

A scan produces hundreds of findings.

### Task

Build a professional remediation approach.

Group by:

* root cause
* asset
* exposure
* technical impact
* remediation action
* dependency
* validation status

Avoid creating an unstructured list of hundreds of individual tasks when common causes exist.

---

# Independent Lab 40 — Final End-to-End Assessment

This is the final independent decision exercise.

## Scenario

You receive an authorized assessment request for a mixed environment containing:

* Linux systems
* Windows systems
* network services
* web-facing services
* internal-only systems
* production systems
* development systems

The request provides a broad target range but incomplete asset inventory.

Credentials are available for some systems.

A maintenance window is defined.

The environment contains network segmentation.

You are expected to produce a professional assessment report and remediation verification plan.

No detailed Nessus configuration is provided.

---

## Your Task

Perform the assessment as if you were receiving the assignment professionally.

You must independently determine:

### 1. Authorization

Establish:

* scope
* exclusions
* permitted activities
* assessment window
* operational constraints
* stop conditions

---

### 2. Assessment Objective

Define what the assessment must establish.

Separate:

* discovery
* vulnerability identification
* authenticated assessment
* configuration assessment
* exposure assessment

where appropriate.

---

### 3. Target Strategy

Determine:

* target lists
* network segments
* scanner placement
* expected hosts
* expected services
* missing inventory handling

---

### 4. Workflow Selection

Determine which assessment workflows are required.

Do not force the entire environment into one identical scan.

---

### 5. Authentication Strategy

Determine:

* where authentication is appropriate
* required permissions
* credential handling
* authentication verification
* partial-authentication handling

---

### 6. Configuration

Build an assessment configuration appropriate to the objective and operational constraints.

Document significant choices.

---

### 7. Preflight

Before launching:

```text id="3kz2p8"
Authorization
Scope
Targets
Scanner
Network Path
Credentials
Configuration
Operational Window
Impact
Stop Conditions
Expected Results
```

---

### 8. Execution

Launch the appropriate assessments.

Monitor:

* scanner health
* progress
* target reachability
* authentication
* errors
* operational impact
* assessment completion

---

### 9. Coverage Analysis

Determine:

* intended scope
* actual scope
* reached hosts
* missing hosts
* authenticated hosts
* unauthenticated hosts
* observed services
* inaccessible services
* incomplete scans

---

### 10. Results Analysis

For significant findings, determine:

* affected asset
* service
* detection basis
* evidence
* applicability
* confidence
* exposure
* technical impact
* environmental context
* root cause

---

### 11. Validation

Determine which findings require validation.

Use the least-impact approach capable of resolving meaningful uncertainty.

---

### 12. Prioritization

Prioritize using:

```text id="q8k2rf"
Validity
↓
Asset Context
↓
Exposure
↓
Technical Impact
↓
Exploitability
↓
Business Context
↓
Compensating Controls
↓
Remediation Complexity
↓
Root Cause
```

Do not substitute severity alone for prioritization.

---

### 13. Reporting

Produce:

* executive summary
* scope
* methodology
* coverage
* limitations
* findings
* validation
* prioritization
* remediation recommendations
* unresolved items
* retest requirements

---

### 14. Remediation

For each important root cause, determine:

* remediation action
* affected population
* expected result
* owner
* dependencies
* exception handling

---

### 15. Retest

Define:

* retest scope
* assessment perspective
* authentication
* configuration
* scanner
* evidence requirements
* acceptable outcomes

---

### 16. Verification

Determine whether remediation is:

```text
Verified
Partially Verified
Still Present
Unable to Verify
Not Applicable
Changed Condition
```

---

### 17. Closure

Before closing the assessment, verify:

```text id="3s1v0v"
Authorization documented
Scope documented
Assessment objective documented
Configuration recorded
Credentials handled securely
Execution recorded
Coverage understood
Findings investigated
Important findings validated
Priorities documented
Report completed
Remediation assigned
Retest defined
Verification completed
Limitations documented
Final status recorded
```

---

# Independent Assessment Scoring Rubric

Do not assign yourself a simple numerical score.

Instead, review whether you independently demonstrated each capability.

| Capability                 | Demonstrated? |
| -------------------------- | ------------- |
| Authorization verification |               |
| Scope definition           |               |
| Objective definition       |               |
| Workflow selection         |               |
| Target planning            |               |
| Scanner-position analysis  |               |
| Authentication planning    |               |
| Credential security        |               |
| Configuration design       |               |
| Preflight review           |               |
| Safe execution             |               |
| Scan monitoring            |               |
| Coverage analysis          |               |
| Result interpretation      |               |
| Finding investigation      |               |
| Validation decisions       |               |
| Root-cause analysis        |               |
| Prioritization             |               |
| Reporting                  |               |
| Remediation planning       |               |
| Retesting                  |               |
| Verification               |               |
| Troubleshooting            |               |
| Documentation              |               |
| Uncertainty handling       |               |
| Operational safety         |               |

The important question is not:

> "How many did I get right?"

It is:

> "Could I perform the assessment without someone telling me every next step?"

---

# Independent Assessment Failure Conditions

Repeat the relevant lab if you:

* scan before confirming authorization
* expand scope without authorization
* guess unknown information
* treat ping failure as proof of host absence
* treat authentication success as complete coverage
* treat no findings as proof of security
* treat severity as business priority
* treat a disappeared finding as automatic remediation
* ignore scanner position
* ignore plugin/content changes
* ignore configuration changes
* ignore incomplete coverage
* perform disruptive validation without authorization
* hide unresolved findings
* invent business criticality
* expose credentials
* continue through serious operational impact without following the stop procedure
* declare remediation without appropriate verification
* compare fundamentally different assessments as though they were equivalent

---

# Independent Assessment Success Conditions

You are ready to move beyond the decision-lab stage when you can independently:

* define the assessment question
* verify authorization
* establish scope
* identify important unknowns
* choose an appropriate workflow
* plan targets
* understand scanner position
* configure discovery
* configure vulnerability assessment
* configure authentication
* select appropriate plugin coverage
* account for operational constraints
* perform preflight checks
* launch safely
* monitor execution
* troubleshoot failures
* evaluate coverage
* investigate findings
* validate important findings
* recognize false positives and unresolved conditions
* identify root causes
* distinguish severity from priority
* produce professional reporting
* design remediation
* perform meaningful retesting
* verify remediation
* document limitations
* explain uncertainty
* make the next decision without step-by-step instruction

---

# Final Independent Operator Test

Before proceeding to the capstone, answer these questions without consulting previous files.

### Question 1

You receive a target.

What do you verify before scanning?

---

### Question 2

You receive credentials.

What do you determine before configuring them?

---

### Question 3

A scan finds nothing.

What do you check before interpreting the result?

---

### Question 4

Authentication fails.

What layer do you investigate first?

---

### Question 5

A finding disappears.

What do you compare?

---

### Question 6

A finding has high severity.

What determines its remediation priority?

---

### Question 7

A host appears that was not expected.

What do you do?

---

### Question 8

A host disappears from a recurring assessment.

What do you investigate?

---

### Question 9

A stakeholder claims remediation is complete.

What establishes verification?

---

### Question 10

A production system becomes unstable.

What takes priority?

---

### Question 11

A Nessus feature is missing from your interface.

What do you verify?

---

### Question 12

Two assessments produce different results.

What must you establish before comparing them?

---

### Question 13

A finding cannot be independently validated.

What should you report?

---

### Question 14

A scan completes but coverage is incomplete.

What can you conclude?

---

### Question 15

A vulnerability affects many hosts because of one common component.

What should remediation focus on?

---

# Final Operator Mental Model

At this stage, the complete Nessus workflow should be internalized as:

```text id="7s8q1m"
AUTHORIZATION
      ↓
SCOPE
      ↓
ASSESSMENT QUESTION
      ↓
KNOWN / UNKNOWN
      ↓
OPERATIONAL CONSTRAINTS
      ↓
WORKFLOW
      ↓
TARGETS
      ↓
SCANNER POSITION
      ↓
DISCOVERY
      ↓
CREDENTIALS
      ↓
CONFIGURATION
      ↓
PREFLIGHT
      ↓
SAFETY CHECK
      ↓
SCAN
      ↓
MONITOR
      ↓
COVERAGE
      ↓
RESULTS
      ↓
INVESTIGATE
      ↓
VALIDATE
      ↓
PRIORITIZE
      ↓
REPORT
      ↓
REMEDIATE
      ↓
RETEST
      ↓
VERIFY
      ↓
DOCUMENT
      ↓
CLOSE
      ↓
CONTINUE / REASSESS
```

At every stage, ask:

```text id="0k9h2e"
What am I trying to determine?

What do I know?

What do I not know?

What am I authorized to do?

What constraints apply?

What evidence do I need?

What is the least-impact way to obtain it?

What do I expect?

What actually happened?

What does the evidence support?

What remains uncertain?

What is the next defensible decision?
```

---

# Completion Criteria

The Decision Labs section is complete when you can take an unfamiliar Nessus assessment request and independently move from:

```text
Request
```

to:

```text
Authorization
```

to:

```text
Scope
```

to:

```text
Assessment Design
```

to:

```text
Execution
```

to:

```text
Evidence
```

to:

```text
Validated Findings
```

to:

```text
Prioritized Remediation
```

to:

```text
Retest
```

to:

```text
Verified Closure
```

without requiring a predefined step-by-step procedure.

The final goal is not memorizing Nessus screens.

The final goal is becoming capable of answering:

> **What should I do next, why should I do it, what evidence should I expect, and what will I do if the result is different?**

That is the foundation of independent vulnerability-assessment work.
