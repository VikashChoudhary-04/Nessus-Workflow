# Nessus Capstone Brief

## Objective

The capstone is the final practical assessment of the Nessus workflow.

Unlike the earlier labs, this exercise does not provide a step-by-step procedure.

You receive an assessment request, an authorized environment, and operational constraints.

You must independently determine how to conduct the assessment from beginning to end.

The capstone is designed to answer one question:

> **Can you independently operate Nessus as a vulnerability-assessment workflow rather than simply operate its interface?**

---

# Capstone Philosophy

The capstone combines every major capability developed throughout this repository:

```text id="0p4q6v"
AUTHORIZATION
      ↓
SCOPE
      ↓
OBJECTIVE
      ↓
UNKNOWN INFORMATION
      ↓
WORKFLOW DESIGN
      ↓
TARGET PLANNING
      ↓
SCANNER POSITION
      ↓
DISCOVERY
      ↓
CREDENTIALS
      ↓
CONFIGURATION
      ↓
PRE-FLIGHT
      ↓
ASSESSMENT
      ↓
MONITORING
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
REPORTING
      ↓
REMEDIATION
      ↓
RETEST
      ↓
VERIFICATION
      ↓
DOCUMENTATION
      ↓
CLOSURE
```

The capstone intentionally includes uncertainty.

You are not expected to receive perfect information.

You are expected to recognize missing information, determine which gaps matter, and make defensible decisions.

---

# Authorization Requirement

Perform this capstone only against:

* your own systems
* systems for which you have explicit authorization
* intentionally vulnerable laboratory environments
* environments specifically provided for security training

Do not use public third-party infrastructure unless you have explicit authorization to assess it.

The capstone should never be used as justification for scanning an arbitrary internet target.

---

# Capstone Environment

The recommended capstone environment is a controlled lab containing several different systems.

A possible environment is:

```text id="j9q2yw"
                    Nessus
                      |
                Assessment Network
                      |
          +-----------+-----------+
          |           |           |
       Linux       Windows      Network/App
       Host         Host         Host
          |           |           |
       Services    Services    Web / Network
```

The exact operating systems and services are not important.

The important requirement is that the environment provides enough variation to require different assessment decisions.

---

# Recommended Lab Components

A useful capstone environment should contain several of the following:

| Component                   | Purpose                                       |
| --------------------------- | --------------------------------------------- |
| Linux host                  | Authenticated assessment and service analysis |
| Windows host                | Windows authentication and host assessment    |
| Web-facing service          | Network-visible application exposure          |
| Internal-only service       | Exposure comparison                           |
| Multiple services           | Discovery and service identification          |
| Different configurations    | Finding comparison                            |
| Vulnerable software         | Vulnerability investigation                   |
| Misconfiguration            | Configuration analysis                        |
| Authentication-enabled host | Credential workflow                           |
| Multiple network segments   | Scanner-position reasoning                    |
| Remediated system           | Retest and verification                       |

Not every component is mandatory.

The capstone should be adapted to the capabilities available in your authorized lab.

---

# Capstone Scenario

You have been assigned a vulnerability assessment of a controlled environment.

The request contains the following information:

```text id="9p0q4a"
Assessment Type:
Vulnerability Assessment

Environment:
Controlled authorized lab

Objective:
Identify and assess vulnerabilities observable within the authorized environment.

Assessment Perspective:
Internal

Authentication:
Available for selected systems

Operational Requirement:
Avoid unnecessary disruption.

Reporting Requirement:
Produce a professional assessment record and findings summary.

Retest Requirement:
Verify remediation where changes are introduced.
```

The target inventory is intentionally incomplete.

You must determine what additional information is required before execution.

---

# Initial Information

You receive the following target information:

```text id="tq5gq8"
Network Segment A:
10.10.10.0/24

Network Segment B:
10.10.20.0/24

Known Systems:
10.10.10.10
10.10.10.20
10.10.20.10

Potential Additional Systems:
Unknown

Known Services:
Not provided

Credentials:
Available for selected hosts

Scanner Location:
Not yet documented
```

Do not assume that every address in the ranges contains a system.

Do not assume every discovered system is automatically authorized beyond the defined scope.

---

# Capstone Assignment

Your task is to perform a complete vulnerability assessment of the authorized environment.

You must independently determine:

1. What information must be clarified.
2. What is already known.
3. What remains unknown.
4. What must be verified before scanning.
5. What discovery is required.
6. Which assessment workflows are appropriate.
7. Which targets require authentication.
8. How scanner placement affects the assessment.
9. How to configure Nessus.
10. How to execute the assessment safely.
11. How to determine coverage.
12. How to investigate findings.
13. Which findings require validation.
14. How to prioritize findings.
15. How to document the assessment.
16. How to recommend remediation.
17. How to perform retesting.
18. How to verify remediation.
19. When the assessment can reasonably be closed.

---

# Phase 1 — Assessment Intake

Before opening or configuring a scan, establish the assessment context.

Document:

```text id="8b0w4s"
Assessment Name:
-

Requester:
-

Authorization:
-

Objective:
-

Scope:
-

Out of Scope:
-

Assessment Window:
-

Allowed Activities:
-

Restricted Activities:
-

Stop Conditions:
-

Reporting Requirement:
-

Retest Requirement:
-
```

---

# Phase 2 — Identify Unknowns

Review the initial information.

Create three lists.

## Known

Record information that is directly supported.

Examples:

```text id="d5v8lo"
Authorized network ranges
Assessment objective
Known target addresses
Internal assessment perspective
```

---

## Unknown

Record information that has not yet been established.

Examples:

```text id="z8k0t1"
Exact host inventory
Scanner network position
Available services
Authentication coverage
Asset criticality
Operational restrictions
```

---

## Decision-Critical Unknowns

Identify unknowns that could change the assessment decision.

Examples:

```text id="3h8y2s"
Whether the scanner can reach both network segments
Whether selected credentials are authorized
Whether certain systems have special operational restrictions
Whether discovered systems are inside the approved assessment scope
```

Do not attempt to resolve every unknown before beginning useful work.

Resolve the unknowns that materially affect authorization, safety, scope, workflow, or coverage.

---

# Phase 3 — Define Assessment Questions

Do not use:

> "Run Nessus against everything."

Instead define explicit questions.

Examples:

### Discovery

```text id="8q5c9z"
What systems and services are observable within the authorized network ranges?
```

### Unauthenticated Assessment

```text id="7v6k1m"
What vulnerabilities are identifiable from the authorized internal scanner perspective without credentials?
```

### Authenticated Assessment

```text id="n3f7y2"
What additional vulnerabilities or configuration conditions become observable with authorized credentials?
```

### Remediation Verification

```text id="m2j8q0"
Did the implemented remediation remove or materially change the previously identified condition?
```

Your capstone should contain explicit assessment questions.

---

# Phase 4 — Scanner Position

Determine where the Nessus scanner is located relative to:

```text id="u7x4v3"
10.10.10.0/24
```

and:

```text id="e5k8p2"
10.10.20.0/24
```

Document:

* scanner network
* routing
* segmentation
* firewall path
* expected reachable networks
* intended assessment perspective

If the scanner cannot reach a segment, do not silently treat that segment as fully assessed.

---

# Phase 5 — Discovery

Perform appropriate authorized discovery.

Determine:

* observed hosts
* expected hosts
* unexpected hosts
* missing hosts
* ports
* services
* technology observations
* network visibility

Create:

```text id="v0x9n4"
Authorized Targets
        ↓
Observed Hosts
        ↓
Observed Services
        ↓
Technology Observations
        ↓
Assessment Decisions
```

Do not automatically convert every discovered service into a separate assessment objective.

---

# Phase 6 — Target Validation

Before vulnerability assessment, reconcile:

```text id="j6m3r1"
Authorized Scope
        +
Discovered Assets
        +
Expected Inventory
        +
Network Reachability
```

Identify:

* missing hosts
* unexpected hosts
* inaccessible hosts
* duplicate addresses
* changed addresses
* unexpected services

Document unresolved discrepancies.

---

# Phase 7 — Workflow Selection

Determine which workflows are necessary.

Possible workflows include:

```text id="7n4p6c"
Discovery
Unauthenticated Vulnerability Assessment
Authenticated Vulnerability Assessment
Configuration / Compliance Assessment
Edition-Specific Assessment
```

Do not assume every host requires every workflow.

Base the decision on:

* assessment question
* target type
* authentication availability
* network position
* operational constraints
* required evidence

---

# Phase 8 — Authentication Strategy

For hosts where authenticated assessment is appropriate, determine:

```text id="m5s1x8"
Authentication Method
Required Permissions
Credential Scope
Credential Security
Authentication Verification
Expected Coverage
```

Do not store credentials in the repository.

Do not put passwords, private keys, tokens, or secrets into:

* Markdown files
* screenshots
* Git commits
* reports intended for public release

Document credential use without exposing credential material.

---

# Phase 9 — Configuration Design

Build the assessment configuration.

Consider:

* targets
* discovery
* assessment settings
* credentials
* plugin coverage
* performance
* exclusions
* scheduling
* advanced settings where justified

For each meaningful deviation from the baseline configuration, record:

```text id="2v6p1r"
Setting:

Reason for Change:

Expected Effect:

Operational Risk:

Observed Effect:
```

Do not change settings simply because more aggressive values exist.

---

# Phase 10 — Preflight Review

Before launching the assessment, verify:

```text id="r7w3y9"
Authorization
[ ]

Scope
[ ]

Objective
[ ]

Targets
[ ]

Scanner
[ ]

Scanner Position
[ ]

Network Reachability
[ ]

Credentials
[ ]

Configuration
[ ]

Plugin / Content State
[ ]

Operational Window
[ ]

Expected Impact
[ ]

Stop Conditions
[ ]

Expected Results
[ ]
```

Do not launch until the important decision-critical unknowns have been resolved or explicitly accepted as limitations.

---

# Phase 11 — Execute the Assessment

Run the required assessments.

During execution, monitor:

* scan status
* scanner health
* target reachability
* authentication behavior
* errors
* resource usage
* operational impact
* unexpected behavior

Do not repeatedly modify configuration while a scan is running unless there is a documented reason.

---

# Phase 12 — Monitor Coverage

Do not wait until the end to discover that important targets were inaccessible.

Track:

```text id="c8q1m4"
Expected Hosts
↓
Observed Hosts
↓
Assessed Hosts
↓
Authenticated Hosts
↓
Unassessed Hosts
↓
Assessment Errors
```

Document partial coverage immediately when discovered.

---

# Phase 13 — Results Review

After execution:

1. Confirm scan completion state.
2. Review host coverage.
3. Review services.
4. Review findings.
5. Review errors.
6. Review authentication.
7. Review important informational results.
8. Review assessment limitations.

Do not begin by sorting only on severity.

---

# Phase 14 — Finding Investigation

For significant findings, record:

```text id="1x9c5q"
Finding:

Affected Asset:

Service:

Detection Basis:

Evidence:

Applicability:

Network Exposure:

Authentication State:

Technical Impact:

Confidence:

Validation Required:

Root Cause:

Related Findings:

Remediation:
```

Use evidence rather than title or severity as the primary basis for interpretation.

---

# Phase 15 — Validation

Determine which findings require additional validation.

Consider validation when:

* evidence is indirect
* version interpretation is uncertain
* vendor backports may apply
* configuration determines applicability
* findings conflict with target evidence
* remediation impact is significant
* operationally important findings require higher confidence

Use the least-impact validation capable of resolving the uncertainty.

---

# Phase 16 — Prioritization

For each important finding, determine:

```text id="5h7r2n"
Validity
↓
Asset Importance
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

Document why the finding requires its assigned remediation priority.

Do not simply write:

> "Critical severity = highest priority."

---

# Phase 17 — Reporting

Create a professional assessment record.

At minimum include:

## Executive Summary

Explain:

* assessment purpose
* scope
* major observations
* significant limitations
* recommended next actions

---

## Scope

Document:

* authorized ranges
* target lists
* exclusions
* scanner perspective
* assessment window

---

## Methodology

Document:

* discovery
* vulnerability assessment
* authentication
* configuration
* validation
* reporting

---

## Coverage

Document:

* intended targets
* observed targets
* assessed targets
* authenticated targets
* inaccessible targets
* limitations

---

## Findings

For each important finding:

```text id="2w5f8q"
Title
Severity
Affected Assets
Description
Evidence
Technical Impact
Environmental Context
Validation Status
Remediation
Retest Requirement
```

---

## Limitations

Document:

* incomplete coverage
* unavailable authentication
* network restrictions
* scanner-position limitations
* unresolved findings
* operational constraints
* other material uncertainties

---

# Phase 18 — Remediation Plan

For each significant root cause, document:

```text id="9v4p2s"
Root Cause:

Affected Population:

Recommended Remediation:

Expected Result:

Dependencies:

Owner:

Exception Requirement:

Verification Method:
```

Avoid remediation advice that is disconnected from the actual root cause.

---

# Phase 19 — Controlled Remediation

Modify your authorized lab environment to remediate selected findings.

Possible examples:

* install an appropriate security update
* remove an intentionally vulnerable package
* disable an unnecessary service
* change an intentionally insecure configuration
* restrict exposure
* apply an approved compensating control

Document:

```text id="f2x6n8"
Original Condition:

Remediation Performed:

Expected Change:

Actual Change:

Date / Time:

Affected Asset:
```

Do not perform disruptive changes outside the authorized lab.

---

# Phase 20 — Retest

Repeat the relevant assessment.

Preserve comparability where practical:

* same target
* same scanner perspective
* same authentication perspective
* relevant configuration
* appropriate plugin/content coverage

Document any differences.

---

# Phase 21 — Verify

For each remediated finding, determine whether it is:

```text id="6w8p3m"
Verified Remediated
Partially Remediated
Still Present
Unable to Verify
Not Applicable
Changed Condition
```

Do not mark an issue remediated merely because the finding disappeared.

Investigate why it disappeared.

---

# Phase 22 — Final Assessment Status

At the end of the capstone, assign a factual status such as:

```text id="6x2v9j"
Assessment Complete
```

or:

```text id="5p8k3m"
Assessment Complete With Limitations
```

or:

```text id="8r1n6q"
Assessment Incomplete
```

The status must reflect:

* authorization
* coverage
* evidence
* unresolved findings
* operational limitations
* retest status

---

# Required Capstone Deliverables

Complete the following artifacts.

## Deliverable 1 — Assessment Plan

Include:

```text id="5m7q2x"
Authorization
Scope
Objective
Targets
Scanner
Scanner Position
Authentication
Workflow
Configuration
Operational Constraints
Stop Conditions
```

---

## Deliverable 2 — Assessment Record

Include:

```text id="1q9v4b"
Execution Timeline
Scan Status
Coverage
Authentication
Errors
Operational Observations
Important Decisions
```

---

## Deliverable 3 — Finding Register

For each significant finding:

```text id="9f3k7w"
Finding
Asset
Service
Evidence
Applicability
Severity
Priority
Validation
Root Cause
Remediation
Retest
Status
```

---

## Deliverable 4 — Coverage Record

Include:

```text id="4x8m2q"
Authorized Targets
Observed Targets
Assessed Targets
Authenticated Targets
Missing Targets
Inaccessible Targets
Unexpected Targets
Coverage Limitations
```

---

## Deliverable 5 — Professional Report

Include:

```text id="3v7n1p"
Executive Summary
Scope
Methodology
Coverage
Findings
Validation
Prioritization
Remediation
Limitations
Retest Requirements
Conclusion
```

---

## Deliverable 6 — Remediation Verification Record

Include:

```text id="7k5q3m"
Original Finding
Original Evidence
Remediation
Retest
Comparison
Verification Evidence
Final Status
Remaining Risk / Limitation
```

---

# Capstone Decision Log

Maintain a decision log throughout the exercise.

Use:

```text id="q1w7e5"
Date / Time:

Decision:

Information Available:

Information Missing:

Options Considered:

Selected Action:

Reason:

Expected Result:

Actual Result:

Impact:

Next Decision:
```

Record important decisions rather than every trivial UI action.

---

# Capstone Evidence Standard

For every important conclusion, ask:

```text id="8p4r6t"
What evidence supports this?
```

Then ask:

```text id="2m7v9x"
What evidence would contradict it?
```

Then:

```text id="5n3q8w"
Is that contradictory evidence available?
```

This prevents conclusions from becoming stronger than the underlying evidence.

---

# Capstone Uncertainty Standard

When information remains incomplete, classify it.

Use:

```text id="0q6x4m"
Known
```

```text id="4v8n2p"
Reported
```

```text id="6m1r7k"
Inferred
```

```text id="9x3q5w"
Unknown
```

```text id="7p2n8c"
Unresolved
```

Do not silently transform:

```text id="2w5m7q"
Reported
```

into:

```text id="5k8r1n"
Verified
```

---

# Capstone Safety Standard

Immediately reconsider or stop assessment activity when:

* authorization becomes unclear
* scope becomes disputed
* a target is outside approved scope
* operational impact exceeds agreed limits
* target stability becomes questionable
* credentials are being misused or exposed
* a validation action may become disruptive
* assessment conditions materially change
* required safety controls are unavailable

Document the reason for any stop or pause.

---

# Capstone Troubleshooting Standard

When something does not behave as expected, use:

```text id="4j8m2s"
OBSERVE
↓
DEFINE FAILURE
↓
CLASSIFY FAILURE
↓
COLLECT EVIDENCE
↓
FORM HYPOTHESIS
↓
CHANGE ONE RELEVANT VARIABLE
↓
TEST
↓
VERIFY
↓
DOCUMENT
```

Avoid random configuration changes.

---

# Capstone Comparison Standard

Whenever comparing two assessments, record:

```text id="8v3q1m"
Scope:
Same / Different

Targets:
Same / Different

Scanner:
Same / Different

Scanner Position:
Same / Different

Authentication:
Same / Different

Configuration:
Same / Different

Plugin / Content State:
Same / Different

Target Environment:
Same / Different

Assessment Window:
Same / Different

Comparability:
High / Limited / Not Established
```

Do not interpret changes without considering these variables.

---

# Capstone Reporting Standard

Your final report should never imply:

```text id="7m2x5q"
"No vulnerabilities exist."
```

when the assessment merely found none.

Prefer language that accurately describes the evidence and scope.

For example:

```text id="1v6p8n"
"No vulnerabilities meeting the assessment's detection criteria were identified within the successfully assessed scope. Coverage limitations and assessment conditions are documented below."
```

The exact wording should reflect the actual evidence obtained.

---

# Capstone Professional Questions

Before finalizing the report, answer:

### Scope

* What exactly was authorized?
* What was actually assessed?
* What was not assessed?

### Perspective

* Where was the scanner located?
* What could it observe from that position?

### Authentication

* Which systems authenticated?
* Which did not?
* What did authentication enable?

### Coverage

* Which hosts were reached?
* Which were missing?
* Which services were inaccessible?

### Findings

* What evidence supports each important finding?
* Which findings were validated?
* Which remain unresolved?

### Prioritization

* Why does each significant finding have its assigned priority?
* What environmental context influenced the decision?

### Remediation

* What is the root cause?
* What action addresses it?
* What evidence would prove remediation?

### Retest

* Was the retest comparable?
* What changed?
* What remains uncertain?

### Closure

* Is the assessment complete?
* If not, what remains?
* What is the next action?

---

# Capstone Completion Test

The capstone should be considered successful only when you can demonstrate that you independently handled:

```text id="3x7m1q"
[ ] Authorization
[ ] Scope
[ ] Objective
[ ] Unknowns
[ ] Target planning
[ ] Scanner placement
[ ] Discovery
[ ] Workflow selection
[ ] Configuration
[ ] Credentials
[ ] Preflight
[ ] Safe execution
[ ] Monitoring
[ ] Troubleshooting
[ ] Coverage analysis
[ ] Results interpretation
[ ] Finding investigation
[ ] Validation
[ ] Root-cause analysis
[ ] Prioritization
[ ] Reporting
[ ] Remediation
[ ] Retest
[ ] Verification
[ ] Documentation
[ ] Closure
```

---

# What This Capstone Is Testing

The capstone is not primarily testing whether you remember where a Nessus setting is located.

It is testing whether you can reason through an assessment.

A strong operator should be able to receive:

```text id="1q8v6m"
"We need this environment assessed."
```

and transform that request into:

```text id="7n2p4x"
A defined question
        ↓
A verified authorization boundary
        ↓
A defensible scope
        ↓
An appropriate workflow
        ↓
A safe configuration
        ↓
A controlled assessment
        ↓
A known coverage level
        ↓
Evidence-supported findings
        ↓
Validated conclusions
        ↓
Context-aware priorities
        ↓
Actionable remediation
        ↓
Comparable retesting
        ↓
Verified outcomes
        ↓
Defensible documentation
```

---

# Final Capstone Principle

A professional Nessus assessment is not:

```text id="6v2m8p"
Configure
↓
Scan
↓
Export
```

It is:

```text id="9x4q1m"
UNDERSTAND
↓
AUTHORIZE
↓
DEFINE
↓
PREPARE
↓
ASSESS
↓
OBSERVE
↓
INVESTIGATE
↓
VALIDATE
↓
INTERPRET
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
```

The capstone is complete when you can perform that lifecycle independently, explain why you made each important decision, identify the limits of your evidence, and determine the next defensible action.

This is the transition from **learning Nessus** to **operating a vulnerability-assessment workflow with Nessus**.
