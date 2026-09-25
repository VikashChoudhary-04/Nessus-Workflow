# Capstone Success Criteria

## Objective

The capstone is intended to demonstrate that the operator can independently conduct an authorized Nessus assessment from initial request through remediation verification.

This file defines what **successful independent operation** looks like.

The capstone is not passed because:

* a scan completed
* Nessus produced findings
* a report was exported
* every plugin was enabled
* a particular number of vulnerabilities was found
* the operator memorized the Nessus interface

The capstone is successful when the operator can make sound assessment decisions, produce defensible evidence, understand limitations, and complete the assessment lifecycle.

---

# Success Model

Use this model:

```text id="7k3p1w"
AUTHORIZED
     ↓
DEFINED
     ↓
PREPARED
     ↓
EXECUTED
     ↓
COVERAGE UNDERSTOOD
     ↓
EVIDENCE INTERPRETED
     ↓
FINDINGS INVESTIGATED
     ↓
IMPORTANT FINDINGS VALIDATED
     ↓
PRIORITIES EXPLAINED
     ↓
REMEDIATION DEFINED
     ↓
RETEST PERFORMED
     ↓
REMEDIATION VERIFIED
     ↓
DOCUMENTED
     ↓
CLOSED OR CLEARLY QUALIFIED
```

Every stage does not need to produce a perfect result.

Every stage must produce an understandable and defensible decision.

---

# Criterion 1 — Authorization

## Successful Behavior

The operator verifies that the assessment is authorized before performing technical assessment activity.

The operator can identify:

* authorized targets
* authorized assessment activities
* exclusions
* assessment window
* operational restrictions
* stop conditions

## Evidence of Success

The assessment record contains:

```text id="m8q4v2"
Authorization Source:
-

Scope:
-

Exclusions:
-

Permitted Activities:
-

Assessment Window:
-

Operational Restrictions:
-

Stop Conditions:
-
```

## Failure Conditions

The capstone is not considered successful if the operator:

* scans an unverified target
* treats a target request as authorization
* expands scope without approval
* ignores an explicit exclusion
* continues after authorization becomes unclear

---

# Criterion 2 — Scope Definition

## Successful Behavior

The operator translates authorization into a concrete technical scope.

The operator can distinguish:

```text id="2v7n5x"
Authorization Boundary
```

from:

```text id="5q1m8p"
Technical Target Definition
```

## Evidence of Success

The operator can explain:

* why each target is included
* what is excluded
* how ranges are interpreted
* how discovered hosts are handled
* how unexpected assets are handled

## Failure Conditions

The operator:

* scans beyond the approved scope
* assumes every discovered system is authorized
* ignores scope ambiguity
* fails to document target changes

---

# Criterion 3 — Assessment Objective

## Successful Behavior

The operator can state exactly what the assessment is intended to determine.

Examples:

```text id="3p8w6k"
What vulnerabilities are observable from this scanner perspective?
```

or:

```text id="6m2q9v"
What additional conditions become visible through authenticated assessment?
```

## Evidence of Success

The assessment record contains explicit objectives.

## Failure Conditions

The assessment is effectively:

> "Run Nessus and see what happens."

---

# Criterion 4 — Identification of Unknowns

## Successful Behavior

The operator recognizes missing information before it causes an incorrect decision.

Examples:

* unknown scanner position
* incomplete asset inventory
* unknown authentication coverage
* missing business context
* unclear target identity
* uncertain network path
* unknown operational restrictions

## Evidence of Success

Important unknowns are documented as:

```text id="9x4r2m"
Known
Unknown
Reported
Inferred
Unresolved
```

rather than being silently assumed.

---

# Criterion 5 — Workflow Selection

## Successful Behavior

The operator chooses the assessment workflow based on the question.

The operator understands the difference between:

* discovery
* unauthenticated vulnerability assessment
* authenticated vulnerability assessment
* configuration/compliance assessment
* specialized or edition-specific workflows

## Evidence of Success

The operator can explain:

> Why this workflow answers the assessment question.

## Failure Conditions

The operator selects a workflow solely because it is the most familiar or appears to provide the largest number of findings.

---

# Criterion 6 — Target Planning

## Successful Behavior

The operator defines technically appropriate targets while respecting authorization.

The operator can handle:

* IP addresses
* hostnames
* ranges
* CIDR notation
* target lists
* dynamic environments
* exclusions

## Evidence of Success

The operator understands:

```text id="5n8q3v"
Intended Scope
↓
Technical Targets
↓
Observed Targets
↓
Assessed Targets
```

and can identify differences between them.

---

# Criterion 7 — Scanner Position

## Successful Behavior

The operator understands where the scanner is located relative to the targets.

The operator can reason about:

* routing
* segmentation
* firewall rules
* source filtering
* internal versus external perspective
* scanner reachability

## Evidence of Success

The operator can explain why the same target may produce different observations from different scanner positions.

## Failure Condition

The operator interprets missing services as target absence without checking network perspective.

---

# Criterion 8 — Discovery

## Successful Behavior

The operator can perform or plan authorized discovery and interpret:

* host observations
* ports
* services
* technology observations
* missing hosts
* unexpected hosts

## Evidence of Success

The operator produces a discovery record.

## Failure Conditions

The operator:

* treats discovery as proof of security
* automatically expands scope
* treats missing hosts as nonexistent
* treats service identification as absolute truth

---

# Criterion 9 — Credential Planning

## Successful Behavior

The operator understands when authentication is useful and how it changes assessment visibility.

The operator can determine:

* appropriate authentication method
* required permissions
* least-privilege requirements
* authentication verification
* partial authentication handling

## Evidence of Success

The operator can explain:

```text id="8v2m6q"
Credential supplied
        ↓
Authentication successful
        ↓
Required permissions available
        ↓
Relevant assessment evidence collected
```

as separate conditions.

---

# Criterion 10 — Credential Security

## Successful Behavior

Credentials are handled securely.

The operator does not place secrets in:

* GitHub repositories
* Markdown files
* public reports
* screenshots
* source code
* command history when avoidable

## Evidence of Success

Documentation describes the authentication method without exposing secret material.

## Failure Condition

Sensitive credential material is committed to the repository.

---

# Criterion 11 — Configuration Design

## Successful Behavior

The operator can configure Nessus according to:

* objective
* target
* workflow
* credentials
* plugin coverage
* performance requirements
* operational constraints

## Evidence of Success

Significant configuration decisions have documented reasons.

Example:

```text id="7q3m1x"
Setting:
Assessment performance

Reason:
Target environment is operationally sensitive.

Expected Effect:
Reduce unnecessary assessment load.

Observed Effect:
Assessment completed within approved window.
```

---

# Criterion 12 — Configuration Minimality

## Successful Behavior

The operator changes only what is necessary.

The operator understands:

```text id="2x9v5k"
More aggressive
≠
Better assessment
```

## Failure Conditions

The operator:

* enables everything without justification
* changes many variables simultaneously
* increases intensity to compensate for uncertainty
* modifies settings without understanding the impact

---

# Criterion 13 — Preflight

## Successful Behavior

Before launching, the operator verifies:

```text id="4p8n2w"
Authorization
Scope
Objective
Targets
Scanner
Network
Credentials
Configuration
Plugin State
Operational Window
Stop Conditions
```

## Evidence of Success

A preflight record exists.

---

# Criterion 14 — Safe Execution

## Successful Behavior

The operator launches assessments within the approved scope and operational conditions.

The operator understands:

* scan impact
* assessment windows
* target sensitivity
* concurrency
* resource constraints
* stop procedures

## Failure Conditions

The operator continues simply because:

> "The scan needs to finish."

when operational safety requires stopping.

---

# Criterion 15 — Monitoring

## Successful Behavior

The operator monitors meaningful execution signals.

Examples:

* scan progress
* target behavior
* authentication
* scanner health
* errors
* resource pressure
* operational impact

## Failure Condition

The operator launches the scan and ignores it until completion.

---

# Criterion 16 — Coverage Analysis

## Successful Behavior

The operator can answer:

> What was actually assessed?

The operator can identify:

* expected hosts
* observed hosts
* assessed hosts
* authenticated hosts
* inaccessible hosts
* missing targets
* unexpected targets
* scan interruptions
* coverage limitations

## Evidence of Success

A coverage record exists.

---

# Criterion 17 — Result Interpretation

## Successful Behavior

The operator reads results through:

```text id="8m4q2v"
Assessment
↓
Host
↓
Service
↓
Finding
↓
Plugin
↓
Evidence
↓
Interpretation
```

The operator does not rely solely on:

* dashboard counts
* severity labels
* finding titles

---

# Criterion 18 — Finding Investigation

## Successful Behavior

For important findings, the operator determines:

* what was detected
* where
* how
* why it matters
* what evidence supports it
* whether it applies
* what remains uncertain

## Evidence of Success

Each important finding has an investigation record.

---

# Criterion 19 — Evidence Quality

## Successful Behavior

The operator distinguishes:

```text id="6v1q8m"
Evidence
```

from:

```text id="9p4x2w"
Interpretation
```

and:

```text id="2m7n5k"
Assumption
```

## Failure Condition

An unsupported interpretation is presented as established fact.

---

# Criterion 20 — Validation

## Successful Behavior

The operator determines which findings require additional validation.

The operator uses a least-impact sequence:

```text id="5x8q1m"
Existing Nessus Evidence
↓
Configuration / Version Evidence
↓
Service Evidence
↓
Non-Destructive Verification
↓
Controlled Testing
```

when appropriate.

## Failure Conditions

The operator:

* validates everything unnecessarily
* skips validation for materially uncertain important findings
* performs disruptive testing without authorization

---

# Criterion 21 — Uncertainty Handling

## Successful Behavior

The operator can confidently say:

* confirmed
* supported
* unresolved
* not applicable
* false positive
* changed condition
* unable to verify

when supported by evidence.

## Failure Condition

The operator forces every finding into:

```text id="4q7n2x"
True
```

or:

```text id="9m1v5k"
False
```

when the evidence does not justify that certainty.

---

# Criterion 22 — Root-Cause Analysis

## Successful Behavior

The operator identifies shared causes when multiple findings arise from the same condition.

Example:

```text id="3v8q2m"
One outdated component
        ↓
Multiple plugin findings
        ↓
One primary remediation path
```

## Failure Condition

The operator treats every plugin result as an independent remediation project without investigation.

---

# Criterion 23 — Severity Interpretation

## Successful Behavior

The operator understands that severity describes the technical significance assigned by the assessment mechanism, while remediation priority requires environmental context.

The operator considers:

* asset importance
* exposure
* technical impact
* exploitability
* business context
* compensating controls
* remediation complexity

---

# Criterion 24 — Prioritization

## Successful Behavior

The operator can explain why one issue requires attention before another without relying solely on severity.

## Evidence of Success

Every significant priority has a rationale.

Example:

```text id="6m3q8v"
Priority Rationale:

The issue affects an externally reachable service,
has meaningful technical impact,
and affects a shared component across multiple systems.
The remediation can address several related findings simultaneously.
```

---

# Criterion 25 — Reporting

## Successful Behavior

The final report contains:

* purpose
* scope
* methodology
* coverage
* limitations
* findings
* evidence
* validation
* priority
* remediation
* retest requirements
* conclusion

## Failure Condition

The report is simply an exported Nessus result file with no professional interpretation.

---

# Criterion 26 — Reporting Accuracy

## Successful Behavior

The report never claims more than the assessment established.

For example:

```text id="4n8q1m"
Observed:
No finding identified.
```

does not automatically become:

```text id="7x3m5v"
The system is secure.
```

---

# Criterion 27 — Remediation Quality

## Successful Behavior

Remediation addresses the underlying condition.

The operator can connect:

```text id="2q7v9m"
Finding
↓
Root Cause
↓
Remediation
↓
Expected State
↓
Verification
```

---

# Criterion 28 — Retest Design

## Successful Behavior

The operator understands that a retest should be meaningfully comparable to the original assessment.

The operator considers:

* target
* scope
* scanner
* scanner position
* authentication
* configuration
* plugin/content state
* target changes

---

# Criterion 29 — Remediation Verification

## Successful Behavior

The operator distinguishes:

```text id="5m8q2x"
Remediation Claimed
```

from:

```text id="9v3n7p"
Remediation Verified
```

## Failure Condition

The operator closes a finding merely because:

* the administrator said it was fixed
* the plugin no longer reports it
* the system was rebuilt
* the target became inaccessible

without understanding the cause.

---

# Criterion 30 — Documentation

## Successful Behavior

The assessment documentation allows another qualified operator to understand:

* what happened
* why decisions were made
* what evidence was collected
* what limitations existed
* what remains unresolved
* what should happen next

## Final Documentation Test

Ask:

> Could another qualified operator continue this assessment without asking me what I meant?

If not, the documentation is incomplete.

---

# Criterion 31 — Troubleshooting

## Successful Behavior

The operator troubleshoots systematically rather than randomly.

The model is:

```text id="8q2m6v"
Observe
↓
Define
↓
Classify
↓
Collect Evidence
↓
Hypothesize
↓
Change One Variable
↓
Test
↓
Verify
↓
Document
```

---

# Criterion 32 — Network Troubleshooting

The operator can distinguish:

* DNS problems
* routing problems
* firewall problems
* ACL restrictions
* host availability
* port filtering
* service problems
* intermediary behavior
* target health
* Nessus configuration problems

---

# Criterion 33 — Authentication Troubleshooting

The operator can distinguish:

```text id="6x4m9q"
Network
↓
Required Service
↓
Authentication Method
↓
Account
↓
Credential
↓
Permissions
↓
Target Restrictions
↓
Nessus Configuration
```

---

# Criterion 34 — Scanner Troubleshooting

The operator can investigate:

* service state
* UI availability
* resource pressure
* disk capacity
* activation/licensing
* plugin/content readiness
* version/edition
* upgrade issues
* scanner availability

without blindly modifying internal product state.

---

# Criterion 35 — Operational Awareness

The operator understands that a technically valid scan can still be operationally inappropriate.

The operator considers:

* production sensitivity
* assessment windows
* system criticality
* maintenance periods
* scanner capacity
* target load
* security monitoring
* incident response
* communication
* change management

---

# Criterion 36 — Version and Edition Awareness

The operator does not assume that every Nessus installation has:

* the same interface
* the same features
* the same templates
* the same policies
* the same capabilities

The operator verifies the actual environment.

---

# Criterion 37 — Comparison Discipline

When comparing assessments, the operator checks:

```text id="7n3q8m"
Scope
Targets
Scanner
Position
Authentication
Configuration
Plugin State
Target State
Timing
```

before interpreting changes.

---

# Criterion 38 — Professional Decision-Making

For significant decisions, the operator can explain:

```text id="3m9v1q"
What I knew
What I did not know
What I was authorized to do
What constraints applied
What options existed
What I chose
Why I chose it
What I expected
What actually happened
What the evidence means
What remains uncertain
What happens next
```

This is one of the most important capstone criteria.

---

# Criterion 39 — Reproducibility

Another operator should be able to reproduce the meaningful parts of the assessment using the documentation.

This does not require recording every mouse click.

It requires recording the decisions and conditions that materially affect results.

---

# Criterion 40 — Security of Assessment Artifacts

The operator protects:

* credentials
* private keys
* tokens
* internal IP information where sensitive
* confidential reports
* exported assessment data
* screenshots
* evidence
* client information

Do not publish confidential assessment material in a public repository.

Use sanitized examples when necessary.

---

# Minimum Success Standard

The capstone is successful only if the operator demonstrates all of the following:

```text id="2q6m8v"
[✓] Authorized assessment
[✓] Defined scope
[✓] Defined objective
[✓] Appropriate workflow
[✓] Safe configuration
[✓] Appropriate authentication
[✓] Safe execution
[✓] Monitoring
[✓] Coverage analysis
[✓] Finding investigation
[✓] Evidence-based interpretation
[✓] Appropriate validation
[✓] Context-aware prioritization
[✓] Professional reporting
[✓] Remediation planning
[✓] Meaningful retest
[✓] Verification
[✓] Documentation
```

A weakness in one area should trigger additional practice in that area rather than being hidden by strengths elsewhere.

---

# Evidence Required for Capstone Completion

Maintain the following artifacts:

```text id="9x4m7p"
01-assessment-plan
02-preflight-record
03-discovery-record
04-assessment-record
05-coverage-record
06-finding-register
07-validation-record
08-prioritization-record
09-report
10-remediation-record
11-retest-record
12-verification-record
13-final-assessment-record
```

These can be Markdown files, sanitized screenshots, exported results, or other appropriate evidence.

Never include secrets.

---

# Capstone Self-Review

Before declaring success, answer each question.

## Authorization

* Did I verify the assessment was authorized?
* Did I document the scope?
* Did I identify exclusions?

## Objective

* Could I explain exactly what the assessment was intended to determine?

## Configuration

* Can I explain why I selected the workflow and major configuration choices?

## Authentication

* Do I know which targets authenticated?
* Do I understand what authentication changed?

## Coverage

* Do I know which targets were actually assessed?
* Do I know which targets were inaccessible?

## Results

* Did I investigate important findings?
* Did I distinguish evidence from interpretation?

## Validation

* Did I validate important uncertain findings?
* Did I avoid unnecessary disruptive testing?

## Prioritization

* Can I explain why important findings received their priorities?

## Reporting

* Does the report accurately communicate scope and limitations?

## Remediation

* Does each significant finding have a meaningful remediation path?

## Retest

* Was the retest comparable enough to answer the remediation question?

## Verification

* Do I have evidence supporting remediation status?

## Documentation

* Could another operator understand and continue the work?

---

# Failure Recovery

If the capstone does not meet the success criteria, do not restart the entire repository.

Identify the failed capability.

Examples:

| Failure                         | Practice Area                     |
| ------------------------------- | --------------------------------- |
| Scope mistake                   | Targets and Discovery             |
| Wrong workflow                  | Choosing the Workflow             |
| Authentication misunderstanding | Credentials                       |
| Missing coverage                | Results / Network Troubleshooting |
| Weak finding interpretation     | Investigating Findings            |
| Poor validation decision        | Validating Findings               |
| Severity-only prioritization    | Prioritizing Findings             |
| Weak report                     | Professional Reporting            |
| Poor retest                     | Remediation and Retest            |
| Operational mistake             | Operational Considerations        |
| Missing evidence                | Assessment Documentation          |
| Weak decisions                  | Decision Labs                     |

Return to the relevant workflow and repeat the capability before repeating the complete capstone.

---

# Independence Test

The operator should be able to receive a new assessment request containing unfamiliar details and independently determine:

```text id="5v8q2m"
What is authorized?
        ↓
What is the actual question?
        ↓
What information is missing?
        ↓
What matters immediately?
        ↓
What workflow answers the question?
        ↓
What targets should be assessed?
        ↓
What perspective is required?
        ↓
Is authentication useful?
        ↓
How should Nessus be configured?
        ↓
What can go wrong?
        ↓
How will coverage be measured?
        ↓
How will findings be interpreted?
        ↓
What requires validation?
        ↓
How should findings be prioritized?
        ↓
What should be reported?
        ↓
How will remediation be verified?
        ↓
When can the assessment close?
```

If these questions can be answered without relying on a predefined recipe, the operator has reached the intended independence level.

---

# Professional Independence Standard

The final standard is:

> **Do not require someone else to tell you the next Nessus step. Determine the next step from the assessment objective, evidence, authorization, constraints, and observed results.**

This means that when an assessment behaves differently from the expected path, the operator does not freeze.

Instead:

```text id="1q7m4x"
Unexpected Result
        ↓
Investigate
        ↓
Understand
        ↓
Adapt
        ↓
Verify
        ↓
Document
```

---

# Final Success Definition

The capstone is successful when the operator can independently produce a defensible answer to all five questions:

### 1. What did we intend to assess?

Supported by:

* authorization
* scope
* objective
* target definition

### 2. What did we actually assess?

Supported by:

* scanner perspective
* network reachability
* target coverage
* authentication coverage
* execution records

### 3. What did we find?

Supported by:

* Nessus evidence
* investigation
* validation
* technical interpretation

### 4. What matters?

Supported by:

* validity
* exposure
* technical impact
* exploitability
* asset context
* business context
* remediation considerations

### 5. What should happen next?

Supported by:

* root cause
* remediation
* retest
* verification
* remaining uncertainty

---

# Final Mental Model

The complete success standard can be reduced to:

```text id="8m2q6v"
AUTHORIZED?
    ↓
IN SCOPE?
    ↓
RIGHT QUESTION?
    ↓
RIGHT WORKFLOW?
    ↓
RIGHT PERSPECTIVE?
    ↓
RIGHT CONFIGURATION?
    ↓
SAFE TO RUN?
    ↓
WHAT WAS ACTUALLY COVERED?
    ↓
WHAT EVIDENCE EXISTS?
    ↓
WHAT DOES IT SUPPORT?
    ↓
WHAT REMAINS UNCERTAIN?
    ↓
WHAT MATTERS?
    ↓
WHAT SHOULD BE FIXED?
    ↓
WAS IT FIXED?
    ↓
CAN I PROVE IT?
    ↓
CAN ANOTHER OPERATOR UNDERSTAND IT?
```

If the answer to these questions is consistently defensible, the capstone has achieved its purpose.

---

# Transition to the Final Operator Checklist

Once the capstone has been completed and the success criteria have been reviewed, the final repository file is:

```text
10-Capstone/03-Operator-Checklist.md
```

That file converts the entire repository into a compact operational checklist that can be used before, during, and after a real authorized Nessus assessment.
