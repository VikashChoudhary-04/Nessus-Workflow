# End-to-End Assessment

## Objective

Learn how to independently conduct a complete Nessus vulnerability assessment from initial request through final interpretation and next action.

This workflow combines the capabilities developed throughout the repository:

* Authorization and scope
* Assessment objectives
* Workflow selection
* Target definition
* Discovery
* Scan configuration
* Credentials
* Plugin selection
* Scan execution
* Monitoring
* Troubleshooting
* Result interpretation
* Finding investigation
* Validation
* Prioritization
* Reporting
* Remediation
* Retesting
* Verification

The goal is no longer to learn an individual Nessus feature.

The goal is to operate the complete assessment lifecycle.

---

# 1. The Professional Assessment Mental Model

A professional Nessus assessment is not:

```text
Create Scan
↓
Click Launch
↓
Export Results
```

Use this model instead:

```text
AUTHORIZATION
      ↓
SCOPE
      ↓
ASSESSMENT QUESTION
      ↓
WORKFLOW SELECTION
      ↓
TARGET DEFINITION
      ↓
PRE-ASSESSMENT REVIEW
      ↓
CONFIGURATION
      ↓
CREDENTIALS
      ↓
SAFETY / IMPACT CHECK
      ↓
EXECUTION
      ↓
MONITORING
      ↓
COVERAGE REVIEW
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
CLOSE / CONTINUE
```

Every arrow represents a decision.

---

# 2. Start With the Assessment Request

Do not start by opening Nessus.

Start by understanding what the assessment is supposed to accomplish.

A request might say:

> "Assess the internal application servers for vulnerabilities."

That is not yet a complete assessment definition.

You need to determine:

* Who authorized the assessment?
* What systems are in scope?
* What systems are excluded?
* What is the assessment objective?
* What network position should the scanner use?
* Is authentication required?
* What evidence is expected?
* When may scanning occur?
* Are there operational restrictions?
* What level of impact is acceptable?
* What output is required?
* Who owns remediation?
* What happens after remediation?

---

# 3. Convert the Request Into an Assessment Question

Transform a broad request into a measurable question.

Example:

```text
Request:
"Assess internal application servers."
```

Possible assessment question:

```text
Which known vulnerabilities are identifiable
on the authorized application servers from
the internal scanner position, and what
evidence supports each finding?
```

For an authenticated assessment:

```text
Which vulnerabilities and configuration
conditions are identifiable using the
authorized authenticated perspective?
```

For a configuration assessment:

```text
Which systems fail the defined security
baseline or compliance requirements?
```

The assessment question determines the workflow.

---

# 4. Define Authorization

Before touching the target, establish authorization.

Record:

```text
Authorization source:
Authorized target scope:
Excluded targets:
Assessment window:
Approved assessment type:
Credential authorization:
Permitted testing intensity:
Stop conditions:
Emergency contact:
```

Authorization should be specific enough to determine whether an action is allowed.

---

# 5. Define the Scope

Scope is more than an IP range.

Consider:

```text
Hosts
Networks
Applications
Services
Cloud assets
Management interfaces
Authentication boundaries
Excluded systems
Network positions
Assessment windows
```

Use the smallest scope that answers the assessment question.

Avoid:

> "Scan the entire network."

unless that is explicitly authorized and operationally appropriate.

---

# 6. Build a Scope Record

Create a working record such as:

| Item               | Value                                           |
| ------------------ | ----------------------------------------------- |
| Assessment         | Internal Vulnerability Assessment               |
| Objective          | Identify vulnerabilities on application servers |
| Authorized targets | `<approved targets>`                            |
| Exclusions         | `<approved exclusions>`                         |
| Scanner            | `<scanner>`                                     |
| Scanner position   | Internal network                                |
| Assessment type    | Vulnerability assessment                        |
| Authentication     | Authorized / Not required / Partial             |
| Window             | `<approved window>`                             |
| Impact constraints | `<constraints>`                                 |
| Stop conditions    | `<conditions>`                                  |
| Owner              | `<owner>`                                       |

Never invent missing values.

Use:

```text
Unknown / Not yet confirmed
```

until verified.

---

# 7. Understand the Environment

Before selecting a scan configuration, determine what you know about the targets.

Questions include:

* What operating systems are expected?
* What applications are expected?
* What services are expected?
* Are systems internal or externally reachable?
* Are there load balancers?
* Are there firewalls?
* Are there network segments?
* Are credentials available?
* Are there sensitive or fragile systems?
* Are systems production?
* Are there maintenance windows?
* Are there known exclusions?

This information influences the workflow.

---

# 8. Select the Assessment Perspective

Choose the perspective that answers the question.

Possible perspectives include:

```text
Discovery
```

```text
Unauthenticated vulnerability assessment
```

```text
Authenticated vulnerability assessment
```

```text
Configuration assessment
```

```text
Compliance assessment
```

```text
Specialized / edition-specific workflow
```

Do not select a workflow merely because it is available.

Ask:

> **What evidence do I need to answer the assessment question?**

---

# 9. Decide Whether Multiple Assessments Are Required

One scan does not necessarily answer every question.

For example:

```text
Assessment 1
Discovery
    ↓
What systems/services exist?

Assessment 2
Unauthenticated assessment
    ↓
What is externally/network visibly identifiable?

Assessment 3
Authenticated assessment
    ↓
What additional local evidence is available?

Assessment 4
Configuration assessment
    ↓
Which controls match the defined baseline?
```

Separate workflows when their objectives, evidence requirements, or operational constraints differ.

---

# 10. Define the Target Set

Translate authorized scope into Nessus targets.

Possible forms include:

* Individual IP addresses
* Hostnames
* FQDNs
* Ranges
* CIDR networks
* Target lists

Verify that the technical target matches the authorized target.

Use:

```text
Authorization
      ↓
Approved Asset Set
      ↓
Nessus Target Definition
```

not:

```text
Nessus Target Definition
      ↓
Assumed Authorization
```

---

# 11. Validate Target Identity

Before scanning, verify:

* DNS resolution
* IP addresses
* Hostnames
* Expected systems
* Scope boundaries
* Exclusions
* Dynamic addresses
* Load-balanced endpoints
* NAT where relevant

If the target identity is uncertain, resolve that uncertainty before proceeding.

---

# 12. Determine Scanner Position

Document:

```text
Scanner:
Network:
Source position:
VPN:
Routing path:
Relevant segmentation:
```

Scanner position affects:

* Reachability
* Visible services
* Firewall behavior
* Authentication
* Exposure
* Finding evidence

A scan from an internal network is not equivalent to a scan from an external network.

---

# 13. Prepare Credentials

If authenticated assessment is required:

1. Confirm credential authorization.
2. Select the appropriate authentication method.
3. Confirm target compatibility.
4. Use least privilege where practical.
5. Protect the credentials.
6. Confirm required permissions.
7. Verify authentication before interpreting authenticated results.

Do not store actual secrets in this repository.

---

# 14. Determine Expected Coverage

Before launching, define what successful coverage means.

Example:

```text
Expected:
10 authorized hosts
↓
Expected:
SSH / HTTP / HTTPS visibility
↓
Expected:
Authenticated checks on Linux servers
↓
Expected:
Relevant vulnerability plugin coverage
```

This gives you a baseline for evaluating the result.

Without expected coverage, it is difficult to recognize incomplete assessment.

---

# 15. Build the Scan Configuration

Configure only what is needed to answer the assessment question.

Review:

```text
Targets
Discovery
Assessment
Credentials
Plugins
Performance
Advanced settings
Scheduling
```

The exact settings available depend on Nessus version and edition.

Do not copy settings from another environment without understanding why they were used.

---

# 16. Configuration Review

Before launch, ask:

### Target

* Is the target authorized?
* Is the target correct?
* Are exclusions respected?

### Discovery

* Will expected hosts be discovered?
* Will expected services be identified?

### Assessment

* Does the selected workflow answer the question?
* Is the required coverage enabled?

### Credentials

* Are credentials required?
* Are they configured correctly?
* Is the authentication method appropriate?

### Plugins

* Is relevant coverage available?
* Are exclusions intentional?

### Performance

* Is the configuration appropriate for the environment?
* Could the target be stressed unnecessarily?

### Advanced settings

* Are changes justified?

---

# 17. Pre-Launch Safety Review

Before launching, perform a final safety review.

Confirm:

```text
Authorization
✓

Scope
✓

Target identity
✓

Assessment objective
✓

Workflow
✓

Credentials
✓

Configuration
✓

Scanner position
✓

Impact constraints
✓

Stop conditions
✓

Assessment window
✓
```

If a critical item is unknown, resolve it before scanning.

---

# 18. Launch the Assessment

Launch only after the preflight review is complete.

Record:

```text
Launch date:
Launch time:
Operator:
Scanner:
Assessment:
Configuration:
Target set:
Credential state:
```

The assessment record should allow another operator to understand what was executed.

---

# 19. Monitor Execution

Monitoring is not watching a progress indicator.

Monitor:

* Assessment state
* Target progress
* Scanner health
* Target behavior
* Authentication state
* Errors
* Unexpected load
* Network behavior
* Execution duration

Ask:

> **Is the assessment progressing as expected without violating operational constraints?**

---

# 20. Respond to Unexpected Behavior

If something unexpected happens:

```text
Observe
↓
Classify
↓
Assess impact
↓
Investigate
↓
Decide
```

Possible actions:

```text
Continue
Investigate
Pause/stop
Modify
Rerun
Escalate
```

Do not automatically stop every slow scan.

Do not automatically continue every unstable scan.

Use evidence.

---

# 21. Know When to Stop

Stop or pause an assessment when justified by:

* Unexpected operational impact
* Target instability
* Authorization boundary problem
* Wrong target
* Unexpected production impact
* Scanner instability
* Critical configuration error
* Emergency request
* Defined stop condition

When stopping:

1. Record the reason.
2. Record the current state.
3. Preserve useful evidence.
4. Determine what coverage was achieved.
5. Decide whether another assessment is appropriate.

---

# 22. Review Assessment Completeness

After execution, do not immediately start prioritizing findings.

First answer:

> **Did the assessment achieve the intended coverage?**

Review:

```text
Target coverage
↓
Host coverage
↓
Service coverage
↓
Authentication coverage
↓
Plugin coverage
↓
Result completeness
```

Document important gaps.

---

# 23. Build the Actual Coverage Picture

Create a coverage record.

| Layer          | Intended | Observed | Gap |
| -------------- | -------- | -------- | --- |
| Targets        |          |          |     |
| Hosts          |          |          |     |
| Services       |          |          |     |
| Authentication |          |          |     |
| Plugins        |          |          |     |
| Findings       |          |          |     |

This converts vague confidence into evidence.

---

# 24. Review Results

Now examine the findings.

Start with:

```text
Assessment
↓
Hosts
↓
Services
↓
Findings
↓
Plugin
↓
Evidence
```

Do not begin with:

```text
Critical: 5
High: 18
Medium: 42
```

Severity counts are a summary, not an assessment conclusion.

---

# 25. Investigate Significant Findings

For important findings, ask:

```text
What is affected?
Where is it?
How was it detected?
What evidence supports it?
Does it apply?
What is the technical impact?
What is the environmental context?
Does it require validation?
What is the root cause?
```

Use evidence from Nessus and appropriate authorized supporting sources.

---

# 26. Validate Where Necessary

Not every finding requires the same validation effort.

Prioritize validation when:

* Impact is significant.
* Evidence is indirect.
* Version ambiguity exists.
* Backported patches may be involved.
* Configuration affects applicability.
* Evidence conflicts.
* Remediation is disruptive.
* The finding materially affects risk decisions.

Use the least-impact method capable of resolving the uncertainty.

---

# 27. Determine Finding State

Each important finding should have an evidence-based state.

Possible states include:

```text
Supported / Confirmed
Not Applicable
False Positive
Unresolved
Condition Changed
```

Use only the state supported by evidence.

"Unresolved" is preferable to an unsupported conclusion.

---

# 28. Identify Root Causes

Do not automatically treat every plugin result as an independent remediation item.

Group related findings.

Example:

```text
Root Cause:
Outdated software component

Affected:
Server A
Server B
Server C

Observed Findings:
Multiple related vulnerability detections
```

The remediation may be:

```text
Update the shared software component
```

rather than:

```text
Fix every plugin finding independently
```

---

# 29. Prioritize Findings

Priority should consider:

```text
Validity / confidence
        ↓
Asset importance
        ↓
Exposure
        ↓
Technical impact
        ↓
Exploitability
        ↓
Business context
        ↓
Compensating controls
        ↓
Remediation complexity
        ↓
Dependencies
```

Do not invent business criticality.

If business context is unknown:

```text
Business criticality:
Not provided
```

rather than guessing.

---

# 30. Build the Remediation Queue

Convert findings into actionable work.

Example:

| Priority | Root Cause | Affected Assets | Action | Owner | Retest |
| -------- | ---------- | --------------- | ------ | ----- | ------ |
|          |            |                 |        |       |        |
|          |            |                 |        |       |        |
|          |            |                 |        |       |        |

A remediation item should answer:

> **What should change, where, and how will we know it worked?**

---

# 31. Define Expected Remediation Results

Before remediation occurs, define what success looks like.

Examples:

```text
Software update
→ Vulnerable version no longer installed

Configuration change
→ Required secure configuration observed

Service removal
→ Service no longer exposed

Access restriction
→ Previously exposed path no longer reachable

Compensating control
→ Documented control reduces the relevant exposure
```

Without an expected result, retesting becomes ambiguous.

---

# 32. Retest

Retest the remediated condition.

Where appropriate, preserve:

* Relevant target
* Scanner
* Scanner position
* Workflow
* Credentials
* Configuration
* Plugin/content coverage

Compare the result against the original evidence.

---

# 33. Verify the Retest

Possible outcomes include:

```text
Remediated
Partially remediated
Still present
Condition changed
Not applicable
Unable to verify
Reappeared
```

Do not mark a finding resolved merely because a later scan does not display it.

Determine why it disappeared.

---

# 34. Assessment Lifecycle Example

A complete assessment might look like:

```text
Request
  ↓
Authorization confirmed
  ↓
Scope defined
  ↓
Question established
  ↓
Authenticated vulnerability assessment selected
  ↓
Targets verified
  ↓
Credentials prepared
  ↓
Configuration reviewed
  ↓
Preflight completed
  ↓
Assessment launched
  ↓
Execution monitored
  ↓
Coverage reviewed
  ↓
Findings investigated
  ↓
Important findings validated
  ↓
Findings prioritized
  ↓
Report prepared
  ↓
Remediation performed
  ↓
Retest executed
  ↓
Results compared
  ↓
Remediation verified
  ↓
Assessment closed / continued
```

This is the operational lifecycle you should eventually be able to perform without a step-by-step guide.

---

# 35. End-to-End Assessment Record

Use this as a working template.

```text
Assessment:
Assessment ID:
Date:
Operator:

AUTHORIZATION
Authorization source:
Approved scope:
Exclusions:
Assessment window:
Permitted assessment type:
Stop conditions:

OBJECTIVE
Assessment question:
Expected evidence:
Success criteria:

TARGETS
Target definition:
Expected hosts:
Expected services:
Actual hosts:
Actual services:

SCANNER
Scanner:
Version:
Edition:
Network position:
Relevant network path:

WORKFLOW
Assessment type:
Reason selected:

CREDENTIALS
Authentication required:
Authentication method:
Account:
Permission level:
Authentication result:
Coverage impact:

CONFIGURATION
Template/policy:
Discovery:
Assessment:
Plugins/content:
Performance:
Advanced settings:
Scheduling:

PRE-LAUNCH
Authorization verified:
Scope verified:
Target verified:
Credentials verified:
Impact reviewed:
Stop conditions reviewed:

EXECUTION
Launch time:
Completion/stop time:
Duration:
Assessment state:
Execution observations:

COVERAGE
Target coverage:
Host coverage:
Service coverage:
Authentication coverage:
Plugin coverage:
Known limitations:

RESULTS
Major findings:
Informational observations:
Root causes:
Validation status:

PRIORITIZATION
Priority findings:
Business context:
Technical context:
Compensating controls:

REPORTING
Report prepared:
Audience:
Limitations documented:

REMEDIATION
Remediation owner:
Actions:
Expected result:

RETEST
Retest date:
Retest configuration:
Retest result:

VERIFICATION
Remediation status:
Residual findings:
Remaining uncertainty:

FINAL ACTION
Close:
Continue monitoring:
Additional assessment:
Exception:
Escalation:
```

---

# 36. Handling an Incomplete Assessment

An incomplete assessment is not automatically a failed assessment.

It may still provide useful evidence.

The correct response is:

```text
Determine completed coverage
        ↓
Determine missing coverage
        ↓
Determine why coverage was missing
        ↓
Determine impact on conclusions
        ↓
Decide whether additional assessment is required
```

Possible final statement:

> The assessment identified findings on the successfully assessed targets, but two authorized systems were unreachable during execution. Results for those systems remain unverified.

This is more useful than declaring the entire assessment successful or unsuccessful without qualification.

---

# 37. Handling an Unexpected Result

Suppose Nessus reports:

```text
No findings
```

but the assessment owner expects vulnerabilities.

Do not immediately conclude the scanner failed.

Investigate:

```text
Scope
↓
Target
↓
Network
↓
Discovery
↓
Authentication
↓
Plugins
↓
Configuration
↓
Target state
↓
Evidence
```

Possible explanations include:

* Target was wrong.
* Service was unavailable.
* Network position differed.
* Authentication failed.
* Relevant plugin coverage was missing.
* Vulnerability was genuinely not identified.
* Target condition changed.

The evidence determines the next action.

---

# 38. Handling an Unexpectedly Large Result

Suppose the assessment produces hundreds of findings.

Do not immediately assume:

> "The environment is extremely insecure."

First determine:

```text
How many unique assets?
How many unique root causes?
How many related findings?
How many informational findings?
How many configuration findings?
How much authenticated visibility was added?
Did plugin/content state change?
```

Then prioritize based on evidence and context.

---

# 39. Handling a Production Assessment

Production assessments require additional operational discipline.

Before execution, confirm:

* Maintenance window if required
* Target ownership
* Impact expectations
* Rate/performance constraints
* Stop conditions
* Emergency contact
* Scanner position
* Monitoring
* Exclusions
* Fragile systems
* Authentication impact

During execution:

```text
Monitor
↓
Observe target behavior
↓
Respect impact thresholds
↓
Stop if authorized conditions require it
```

Do not assume that because Nessus is designed for vulnerability assessment, every configuration is appropriate for every production system.

---

# 40. Handling Multiple Scanner Locations

A large environment may require different scanner positions.

Example:

```text
External Scanner
      ↓
Internet-facing exposure

Internal Scanner
      ↓
Internal exposure

Segmented Scanner
      ↓
Restricted network segment
```

The results are not interchangeable.

Document scanner position for every assessment.

---

# 41. Handling Large Target Populations

For large environments, think in populations.

Instead of:

```text
1,000 individual findings
```

consider:

```text
Root Cause
    ↓
Affected Asset Population
    ↓
Exposure
    ↓
Priority
    ↓
Remediation
    ↓
Verification
```

Population-level remediation requires population-level verification.

Do not retest only one representative host unless the assessment objective and evidence justify that approach.

---

# 42. Recurring End-to-End Assessments

For recurring assessments, the workflow becomes:

```text
Previous Baseline
      ↓
Review Changes
      ↓
Confirm Scope
      ↓
Confirm Credentials
      ↓
Confirm Configuration
      ↓
Execute
      ↓
Review Coverage
      ↓
Compare Results
      ↓
Investigate Changes
      ↓
Prioritize
      ↓
Remediate
      ↓
Retest
      ↓
Update Baseline
```

A recurring scan is not:

```text
Schedule once
↓
Forget it
```

Review the assessment design periodically.

---

# 43. Assessment Handoff

Another qualified operator should be able to understand:

* Why the assessment was performed
* What was authorized
* What was assessed
* How it was assessed
* What credentials were used
* What scanner was used
* What limitations existed
* What findings were identified
* What was validated
* What was prioritized
* What remediation was requested
* What remains open

If another operator cannot reconstruct the assessment from the documentation, the assessment record is incomplete.

---

# 44. Professional Assessment Checklist

## Authorization

* [ ] Authorization confirmed
* [ ] Scope documented
* [ ] Exclusions documented
* [ ] Assessment window confirmed
* [ ] Stop conditions defined

## Objective

* [ ] Assessment question defined
* [ ] Required evidence identified
* [ ] Success criteria defined

## Targets

* [ ] Target identity verified
* [ ] Target list verified
* [ ] Expected hosts known
* [ ] Expected services considered
* [ ] Scanner position documented

## Workflow

* [ ] Appropriate workflow selected
* [ ] Multiple workflows considered where necessary
* [ ] Workflow rationale documented

## Credentials

* [ ] Authentication requirement established
* [ ] Authorized credentials available
* [ ] Least privilege considered
* [ ] Authentication method verified
* [ ] Secrets protected

## Configuration

* [ ] Targets reviewed
* [ ] Discovery reviewed
* [ ] Assessment settings reviewed
* [ ] Plugins/content reviewed
* [ ] Performance reviewed
* [ ] Advanced settings justified

## Safety

* [ ] Operational impact considered
* [ ] Production constraints reviewed
* [ ] Emergency contact known
* [ ] Stop conditions understood

## Execution

* [ ] Launch recorded
* [ ] Assessment monitored
* [ ] Unexpected behavior investigated
* [ ] Scanner health monitored
* [ ] Target behavior monitored

## Coverage

* [ ] Target coverage reviewed
* [ ] Host coverage reviewed
* [ ] Service coverage reviewed
* [ ] Authentication coverage reviewed
* [ ] Plugin coverage reviewed
* [ ] Limitations documented

## Results

* [ ] Findings reviewed
* [ ] Important findings investigated
* [ ] Validation performed where necessary
* [ ] Root causes identified
* [ ] False positives distinguished from unresolved findings

## Prioritization

* [ ] Asset context considered
* [ ] Exposure considered
* [ ] Technical impact considered
* [ ] Exploitability considered
* [ ] Business context obtained where available
* [ ] Compensating controls considered

## Reporting

* [ ] Scope documented
* [ ] Methodology documented
* [ ] Findings documented
* [ ] Evidence documented
* [ ] Limitations documented
* [ ] Remediation actions documented

## Retest

* [ ] Expected remediation result defined
* [ ] Retest performed
* [ ] Before/after evidence compared
* [ ] Residual findings documented
* [ ] Verification completed

## Closure

* [ ] Final status recorded
* [ ] Open items identified
* [ ] Follow-up defined
* [ ] Assessment record preserved

---

# 45. Decision Rule

When receiving a new Nessus assessment request, use:

```text
1. What am I authorized to assess?
        ↓
2. What question must the assessment answer?
        ↓
3. What evidence is required?
        ↓
4. Which workflow provides that evidence?
        ↓
5. What targets are actually in scope?
        ↓
6. What scanner position is required?
        ↓
7. Is authentication required?
        ↓
8. What configuration is necessary?
        ↓
9. What operational impact is acceptable?
        ↓
10. What does successful coverage look like?
        ↓
11. How will execution be monitored?
        ↓
12. How will findings be investigated?
        ↓
13. Which findings require validation?
        ↓
14. How will findings be prioritized?
        ↓
15. What remediation is required?
        ↓
16. How will remediation be retested?
        ↓
17. How will the assessment be closed?
```

This sequence should eventually become automatic.

---

# 46. What Professional Independence Looks Like

You are becoming operationally independent when you can receive a request such as:

> "Assess these authorized Linux servers for vulnerabilities."

and independently determine:

```text
Authorization
      ↓
Scope
      ↓
Assessment question
      ↓
Scanner position
      ↓
Discovery requirements
      ↓
Authenticated vs unauthenticated perspective
      ↓
Credential requirements
      ↓
Configuration
      ↓
Execution plan
      ↓
Monitoring
      ↓
Coverage
      ↓
Investigation
      ↓
Validation
      ↓
Prioritization
      ↓
Reporting
      ↓
Remediation
      ↓
Retest
```

without needing someone to tell you which button to press at every stage.

---

# 47. End-to-End Practical Exercise

## Scenario

You receive this authorized request:

```text
Assess the provided lab application servers for
network-visible and authenticated vulnerabilities.

Use the approved Nessus scanner.

Do not assess excluded systems.

The assessment must be performed during the
approved testing window.

Provide findings, evidence, limitations,
prioritized remediation actions, and retest results.
```

No step-by-step instructions are provided.

Your task is to operate the assessment.

---

## Phase 1 — Plan

Determine:

* Authorization
* Scope
* Exclusions
* Assessment question
* Scanner position
* Network expectations
* Authentication requirements
* Expected evidence
* Operational constraints

Record your assumptions.

Do not silently invent missing information.

---

## Phase 2 — Design

Determine:

* Whether discovery is required
* Whether unauthenticated assessment is required
* Whether authenticated assessment is required
* Whether configuration assessment is required
* Whether multiple workflows are necessary

Document why.

---

## Phase 3 — Prepare

Verify:

* Scanner health
* Nessus version/edition
* Targets
* DNS
* Network reachability
* Authentication
* Permissions
* Configuration
* Plugin/content readiness

Do not launch until the preflight review is complete.

---

## Phase 4 — Execute

Launch the assessment.

Monitor:

* State
* Progress
* Scanner health
* Target behavior
* Authentication
* Errors
* Duration
* Operational impact

Document significant observations.

---

## Phase 5 — Assess Coverage

Determine:

```text
Expected
vs
Observed
```

for:

* Targets
* Hosts
* Services
* Authentication
* Plugins
* Results

Document limitations.

---

## Phase 6 — Analyze

For significant findings:

```text
Identify
↓
Investigate
↓
Validate where necessary
↓
Determine root cause
↓
Assess impact
↓
Prioritize
```

Avoid treating raw severity counts as the final answer.

---

## Phase 7 — Report

Produce:

* Executive summary
* Scope
* Objective
* Methodology
* Coverage
* Limitations
* Findings
* Evidence
* Impact
* Prioritization
* Remediation
* Next actions

Separate evidence from interpretation.

---

## Phase 8 — Remediate

For each selected remediation item:

```text
Finding
↓
Root Cause
↓
Remediation
↓
Expected Result
```

Record ownership and status.

---

## Phase 9 — Retest

After remediation:

1. Confirm the remediation occurred.
2. Run the appropriate retest.
3. Compare evidence.
4. Determine whether the condition changed.
5. Record the result.

---

## Phase 10 — Close

Determine:

```text
Resolved
Partially Resolved
Still Present
Unable to Verify
Not Applicable
Open / Follow-up Required
```

Then document the final assessment state.

---

# 48. Final End-to-End Assessment Checklist

Before considering an assessment complete:

```text
AUTHORIZATION
[ ] Authorized
[ ] Scope confirmed
[ ] Exclusions confirmed
[ ] Window confirmed
[ ] Stop conditions known

OBJECTIVE
[ ] Question defined
[ ] Evidence requirements defined
[ ] Success criteria defined

TARGETS
[ ] Targets verified
[ ] Scanner position documented
[ ] Expected hosts/services considered

WORKFLOW
[ ] Appropriate workflow selected
[ ] Multiple perspectives considered
[ ] Rationale documented

CREDENTIALS
[ ] Authentication requirement determined
[ ] Credentials authorized
[ ] Authentication verified
[ ] Permissions understood
[ ] Secrets protected

CONFIGURATION
[ ] Targets configured
[ ] Discovery configured
[ ] Assessment configured
[ ] Plugins/content reviewed
[ ] Performance reviewed
[ ] Advanced settings justified

EXECUTION
[ ] Preflight completed
[ ] Assessment launched
[ ] Assessment monitored
[ ] Unexpected behavior investigated
[ ] Scanner health verified

COVERAGE
[ ] Target coverage verified
[ ] Host coverage verified
[ ] Service coverage verified
[ ] Authentication coverage verified
[ ] Plugin coverage considered
[ ] Limitations documented

RESULTS
[ ] Findings reviewed
[ ] Important findings investigated
[ ] Validation performed where needed
[ ] Root causes identified
[ ] False positives distinguished from uncertainty

PRIORITIZATION
[ ] Asset context considered
[ ] Exposure considered
[ ] Technical impact considered
[ ] Exploitability considered
[ ] Business context obtained where possible
[ ] Compensating controls considered

REPORTING
[ ] Scope documented
[ ] Methodology documented
[ ] Coverage documented
[ ] Findings documented
[ ] Evidence documented
[ ] Limitations documented
[ ] Remediation documented

RETEST
[ ] Expected result defined
[ ] Remediation verified
[ ] Retest performed
[ ] Results compared
[ ] Residual risk documented

CLOSURE
[ ] Final state recorded
[ ] Open issues identified
[ ] Follow-up defined
[ ] Assessment record preserved
```

---

# Final Mental Model

The complete professional Nessus assessment is:

```text
AUTHORIZATION
      ↓
SCOPE
      ↓
QUESTION
      ↓
EVIDENCE REQUIREMENTS
      ↓
WORKFLOW
      ↓
TARGET
      ↓
SCANNER POSITION
      ↓
CREDENTIALS
      ↓
CONFIGURATION
      ↓
SAFETY CHECK
      ↓
EXECUTION
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
CLOSURE / NEXT ASSESSMENT
```

The professional skill is not:

> **"I can run a Nessus scan."**

It is:

> **"I can take an authorized assessment request, translate it into a defensible assessment workflow, execute it safely, determine what was actually covered, interpret the evidence, investigate and validate important findings, communicate limitations, drive remediation, and verify the result."**
