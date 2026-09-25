# Nessus Operator Checklist

## Purpose

This checklist is the final operational reference for the Nessus workflow repository.

It condenses the complete assessment lifecycle into a practical sequence:

```text
AUTHORIZATION
↓
SCOPE
↓
OBJECTIVE
↓
PREPARATION
↓
CONFIGURATION
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
CLOSURE
```

Use this checklist for authorized assessments and controlled labs.

It is not intended to replace assessment judgment.

When a situation does not match the checklist exactly, return to the assessment objective, evidence, authorization, constraints, and next-decision model.

---

# 1. Assessment Intake

## Request

```text
[ ] Assessment request received
[ ] Requester identified
[ ] Assessment objective identified
[ ] Expected deliverables identified
[ ] Assessment window identified
[ ] Required retest identified
[ ] Operational constraints identified
```

### Ask

```text
What is this assessment supposed to determine?
```

Do not begin with:

> Which Nessus scan should I run?

Begin with:

> What question am I trying to answer?

---

# 2. Authorization

```text
[ ] Authorization confirmed
[ ] Authorization source recorded
[ ] Authorized targets identified
[ ] Authorized activities identified
[ ] Exclusions identified
[ ] Assessment window confirmed
[ ] Operational restrictions confirmed
[ ] Stop conditions identified
```

### Verify

```text
Am I authorized to assess this target?

Am I authorized to perform this type of assessment?

Am I authorized to use these credentials?

Am I authorized to perform validation?

Am I authorized to assess during this time window?
```

If authorization is unclear:

```text
STOP
↓
VERIFY
↓
CONTINUE ONLY WHEN AUTHORIZATION IS ESTABLISHED
```

---

# 3. Scope

## Technical Scope

```text
[ ] IP addresses confirmed
[ ] Hostnames confirmed
[ ] CIDR/ranges confirmed
[ ] Target lists confirmed
[ ] Exclusions documented
[ ] Out-of-scope systems documented
```

## Scope Questions

```text
[ ] Does technical scope match authorization?
[ ] Are ranges interpreted correctly?
[ ] Are exclusions technically enforceable?
[ ] Are discovered systems handled according to authorization?
[ ] Are scope changes documented?
```

### Scope Rule

```text
DISCOVERED
≠
AUTHORIZED
```

A discovered asset does not automatically become an assessment target.

---

# 4. Assessment Objective

Write the objective explicitly.

Examples:

```text
[ ] Discovery
[ ] Unauthenticated vulnerability assessment
[ ] Authenticated vulnerability assessment
[ ] Configuration assessment
[ ] Compliance assessment
[ ] Exposure assessment
[ ] Remediation verification
```

### Objective Test

Complete this sentence:

> "This assessment is intended to determine __________."

If the blank cannot be completed clearly, clarify the assessment objective before proceeding.

---

# 5. Known / Unknown Review

Create three categories.

## Known

```text
[ ] Authorization
[ ] Scope
[ ] Objective
[ ] Known targets
[ ] Known constraints
[ ] Known scanner
```

## Unknown

```text
[ ] Asset inventory
[ ] Scanner position
[ ] Network path
[ ] Services
[ ] Authentication coverage
[ ] Business context
[ ] Operational restrictions
```

## Decision-Critical Unknowns

```text
[ ] Authorization uncertainty
[ ] Scope uncertainty
[ ] Safety uncertainty
[ ] Target identity uncertainty
[ ] Network reachability uncertainty
[ ] Authentication uncertainty
[ ] Coverage uncertainty
```

### Rule

Not every unknown blocks the assessment.

Determine whether the unknown changes:

* authorization
* safety
* scope
* workflow
* interpretation
* coverage
* prioritization

---

# 6. Nessus Environment Check

Before substantive assessment work:

```text
[ ] Nessus installation identified
[ ] Nessus edition identified
[ ] Nessus version identified
[ ] Operating environment identified
[ ] License / activation state checked
[ ] Scanner identity confirmed
[ ] Scanner availability confirmed
[ ] Plugin/content readiness checked
[ ] User permissions understood
```

### Remember

Different Nessus installations may differ by:

* edition
* version
* license
* permissions
* available capabilities
* interface
* plugin/content state

Verify the actual environment.

---

# 7. Scanner Position

Document:

```text
Scanner:
-

Scanner Network:
-

Target Network:
-

Routing:
-

Firewall Path:
-

Segmentation:
-

Expected Perspective:
-
```

Check:

```text
[ ] Scanner can reach intended network
[ ] Required routes exist
[ ] Required ports are reachable
[ ] Network restrictions understood
[ ] Scanner perspective matches assessment objective
```

### Core Rule

```text
Different Scanner Position
=
Potentially Different Evidence
```

---

# 8. Discovery

If discovery is required:

```text
[ ] Authorized discovery scope defined
[ ] Host discovery configured
[ ] Port discovery considered
[ ] Service identification considered
[ ] Technology observations considered
[ ] Discovery impact considered
```

After discovery:

```text
[ ] Expected hosts observed
[ ] Expected hosts missing
[ ] Unexpected hosts identified
[ ] Services documented
[ ] Unexpected services investigated
[ ] Coverage limitations documented
```

### Discovery Rule

```text
Discovery answers:
"What can I observe?"

It does not automatically answer:
"What is vulnerable?"
```

---

# 9. Target Validation

Before vulnerability assessment:

```text
[ ] Target identity verified
[ ] Target scope verified
[ ] Expected hosts reviewed
[ ] Missing hosts identified
[ ] Unexpected hosts investigated
[ ] DNS behavior understood
[ ] Network reachability understood
```

### If a Target Is Missing

Investigate:

```text
DNS
↓
Routing
↓
Firewall
↓
Host Availability
↓
Port
↓
Service
↓
Scanner Configuration
```

Do not assume the host is unaffected.

---

# 10. Workflow Selection

Choose the workflow based on the question.

```text
[ ] Discovery
[ ] Unauthenticated vulnerability assessment
[ ] Authenticated vulnerability assessment
[ ] Configuration / compliance
[ ] Specialized / edition-specific workflow
```

### Ask

```text
What evidence does this workflow need to produce?
```

### Avoid

```text
"Scan everything with the biggest template."
```

---

# 11. Target Configuration

```text
[ ] Correct targets entered
[ ] Scope matches authorization
[ ] Exclusions configured where appropriate
[ ] Hostnames verified
[ ] IP addresses verified
[ ] Ranges verified
[ ] Dynamic target behavior considered
```

### Target Principle

Use the smallest technical scope that fully satisfies the authorized assessment objective.

---

# 12. Scan Configuration

Review:

```text
[ ] Discovery
[ ] Assessment
[ ] Credentials
[ ] Plugins
[ ] Performance
[ ] Timeouts
[ ] Advanced settings
[ ] Exclusions
[ ] Scheduling
```

For every significant deviation:

```text
Setting:
-

Reason:
-

Expected Effect:
-

Operational Impact:
-
```

### Configuration Rule

```text
Change only what you can explain.
```

---

# 13. Templates and Policies

```text
[ ] Appropriate template selected
[ ] Template matches objective
[ ] Existing policy reviewed
[ ] Policy matches current environment
[ ] Configuration drift considered
[ ] Plugin coverage reviewed
```

Remember:

```text
Template
=
Starting Point

Policy
=
Reusable Configuration

Plugin
=
Individual Detection / Check
```

---

# 14. Plugin Review

```text
[ ] Required plugin coverage considered
[ ] Relevant plugin families enabled
[ ] Unnecessary coverage considered
[ ] Plugin/content state known
[ ] Plugin updates considered
```

Do not assume:

```text
More plugins
=
Better assessment
```

Coverage should match the assessment question and operational constraints.

---

# 15. Credential Preparation

```text
[ ] Authentication is actually required
[ ] Authentication method selected
[ ] Credentials authorized
[ ] Required permissions identified
[ ] Least privilege considered
[ ] Credential validity checked
[ ] Target-side requirements understood
```

Never place secrets in:

```text
[ ] GitHub
[ ] Markdown
[ ] Public reports
[ ] Screenshots
[ ] Source code
```

---

# 16. Authentication Verification

After configuring credentials:

```text
[ ] Authentication method correct
[ ] Required service reachable
[ ] Account valid
[ ] Required permissions available
[ ] Target restrictions understood
[ ] Nessus authentication evidence reviewed
```

Remember:

```text
Credential supplied
≠
Authentication successful

Authentication successful
≠
Complete assessment coverage
```

---

# 17. Performance and Safety

Before execution:

```text
[ ] Target sensitivity considered
[ ] Scan intensity appropriate
[ ] Concurrency appropriate
[ ] Scanner capacity sufficient
[ ] Assessment window sufficient
[ ] Production restrictions considered
[ ] Expected impact understood
```

### Safety Principle

```text
More aggressive
≠
More professional
```

---

# 18. Preflight

Complete before launching:

```text
[ ] Authorization
[ ] Scope
[ ] Objective
[ ] Targets
[ ] Scanner
[ ] Scanner position
[ ] Network path
[ ] Credentials
[ ] Configuration
[ ] Plugin/content state
[ ] Operational window
[ ] Stop conditions
[ ] Expected result
```

### Preflight Question

> If the assessment behaves differently from expected, do I know what I will investigate first?

---

# 19. Launch

Before clicking the final launch action:

```text
[ ] Correct scan selected
[ ] Correct targets selected
[ ] Correct credentials selected
[ ] Correct configuration selected
[ ] Correct scanner selected
[ ] Scope rechecked
[ ] Operational window confirmed
```

Then launch.

Record:

```text
Start Date / Time:
-

Assessment:
-

Scanner:
-

Targets:
-
```

---

# 20. Monitor

During execution:

```text
[ ] Scan progress monitored
[ ] Scanner health monitored
[ ] Target reachability observed
[ ] Authentication observed
[ ] Errors reviewed
[ ] Resource usage considered
[ ] Operational impact monitored
```

### If the Scan Is Slow

Do not immediately assume it is broken.

Investigate:

```text
Target behavior
Network conditions
Scanner resources
Assessment configuration
Concurrent workload
Service response
Errors
```

---

# 21. Scan Failure

If a scan fails:

```text
[ ] Define exact failure
[ ] Determine when failure occurred
[ ] Identify affected targets
[ ] Review scanner health
[ ] Review network behavior
[ ] Review credentials
[ ] Review configuration
[ ] Review logs / available evidence
[ ] Determine coverage impact
```

Do not immediately rerun with random configuration changes.

---

# 22. Operational Impact

If unexpected impact occurs:

```text
[ ] Record observation
[ ] Assess severity
[ ] Follow stop procedure
[ ] Notify appropriate stakeholders
[ ] Preserve evidence
[ ] Determine whether assessment contributed
[ ] Document affected coverage
```

### Priority

```text
Operational Safety
>
Assessment Completion
```

---

# 23. Coverage Review

After execution:

```text
[ ] Expected hosts identified
[ ] Observed hosts identified
[ ] Assessed hosts identified
[ ] Authenticated hosts identified
[ ] Missing hosts identified
[ ] Inaccessible hosts identified
[ ] Unexpected hosts identified
[ ] Scan interruptions documented
[ ] Coverage limitations documented
```

### Coverage Question

> What did Nessus actually assess?

Not:

> What did I intend to assess?

---

# 24. Result Review

Start with:

```text
[ ] Assessment status
[ ] Coverage
[ ] Hosts
[ ] Services
[ ] Findings
[ ] Errors
[ ] Authentication
[ ] Important informational results
```

Do not begin and end with:

```text
Critical: 3
High: 8
Medium: 17
```

Counts are context, not conclusions.

---

# 25. Finding Investigation

For each important finding:

```text
[ ] Host identified
[ ] Service identified
[ ] Component identified
[ ] Plugin identified
[ ] Detection basis reviewed
[ ] Evidence reviewed
[ ] Applicability assessed
[ ] Exposure assessed
[ ] Authentication state considered
[ ] Technical impact considered
[ ] Confidence assessed
[ ] Root cause considered
[ ] Related findings considered
```

---

# 26. Evidence Review

Ask:

```text
What does Nessus actually show?
```

Then:

```text
What am I interpreting from that evidence?
```

Then:

```text
What remains uncertain?
```

Never silently convert:

```text
Evidence
↓
Assumption
↓
Fact
```

---

# 27. Version-Based Findings

When a finding depends on version evidence:

```text
[ ] Installed version reviewed
[ ] Package state reviewed
[ ] Vendor context considered
[ ] Distribution/backport possibility considered
[ ] Active component considered
[ ] Current target state verified
```

### Rule

A version string may require platform-specific interpretation.

---

# 28. Validation Decision

For each important finding:

```text
[ ] Validation unnecessary
```

or:

```text
[ ] Validation required
```

If validation is required:

```text
[ ] Least-impact method selected
[ ] Authorization confirmed
[ ] Operational impact considered
[ ] Stop conditions defined
[ ] Expected result defined
[ ] Actual result recorded
```

---

# 29. Validation Sequence

Prefer:

```text
Nessus Evidence
↓
Configuration Evidence
↓
Version / Package Evidence
↓
Service Evidence
↓
Non-Destructive Verification
↓
Controlled Testing
```

Stop once sufficient evidence is obtained.

Do not test merely because additional testing is possible.

---

# 30. Validation Outcome

Classify appropriately:

```text
[ ] Confirmed / Supported
[ ] Not Applicable
[ ] False Positive
[ ] Unresolved
[ ] Changed Condition
[ ] Unable to Verify
```

Do not force uncertain findings into a definitive category.

---

# 31. Root Cause

Ask:

```text
Is this finding isolated?
```

or:

```text
Does a shared condition produce multiple findings?
```

Investigate:

```text
[ ] Shared software
[ ] Shared configuration
[ ] Shared credential issue
[ ] Shared network exposure
[ ] Shared platform issue
[ ] Shared deployment problem
```

---

# 32. Prioritization

For significant findings, review:

```text
[ ] Validity
[ ] Asset importance
[ ] Exposure
[ ] Technical impact
[ ] Exploitability
[ ] Business context
[ ] Compensating controls
[ ] Remediation complexity
[ ] Dependencies
[ ] Root cause
```

### Remember

```text
Severity
≠
Priority
```

---

# 33. Remediation Planning

For each important finding:

```text
[ ] Root cause identified
[ ] Remediation defined
[ ] Affected population identified
[ ] Expected result defined
[ ] Dependencies identified
[ ] Owner identified
[ ] Exception process considered
[ ] Verification method defined
```

---

# 34. Reporting

## Executive Section

```text
[ ] Purpose
[ ] Scope
[ ] Major observations
[ ] Significant limitations
[ ] Recommended next actions
```

## Technical Section

```text
[ ] Methodology
[ ] Coverage
[ ] Findings
[ ] Evidence
[ ] Validation
[ ] Priority
[ ] Remediation
```

## Limitations

```text
[ ] Inaccessible targets
[ ] Authentication gaps
[ ] Network restrictions
[ ] Scanner limitations
[ ] Unresolved findings
[ ] Operational restrictions
[ ] Other material uncertainties
```

---

# 35. Report Accuracy

Before finalizing, search for unsupported statements such as:

```text
"No vulnerabilities exist."

"The environment is secure."

"All systems were assessed."

"Everything was fixed."

"The scan found everything."
```

Replace them with evidence-based language that reflects:

* scope
* coverage
* methodology
* limitations
* evidence

---

# 36. Assessment Documentation

Record:

```text
[ ] Authorization
[ ] Scope
[ ] Objective
[ ] Targets
[ ] Scanner
[ ] Scanner position
[ ] Configuration
[ ] Credentials without secrets
[ ] Execution
[ ] Coverage
[ ] Findings
[ ] Validation
[ ] Prioritization
[ ] Reporting
[ ] Remediation
[ ] Retest
[ ] Verification
[ ] Limitations
[ ] Final status
```

### Documentation Test

> Could another qualified operator understand what happened and why?

---

# 37. Retest Planning

Before retesting:

```text
[ ] Original finding identified
[ ] Original target identified
[ ] Original scope reviewed
[ ] Original scanner reviewed
[ ] Original scanner position reviewed
[ ] Original authentication reviewed
[ ] Relevant configuration preserved
[ ] Expected remediation result defined
```

---

# 38. Retest Execution

During retest:

```text
[ ] Correct target
[ ] Correct perspective
[ ] Correct authentication
[ ] Comparable configuration
[ ] Relevant plugin/content coverage
[ ] Assessment completed
[ ] Coverage reviewed
```

Document differences from the original assessment.

---

# 39. Retest Interpretation

If a finding disappears:

```text
[ ] Determine why
[ ] Compare assessment conditions
[ ] Review target state
[ ] Review plugin/content state
[ ] Review authentication
[ ] Review scanner position
```

Do not automatically write:

> "Remediated."

---

# 40. Verification Status

Use an explicit status:

```text
[ ] Verified Remediated
[ ] Partially Remediated
[ ] Still Present
[ ] Unable to Verify
[ ] Not Applicable
[ ] Changed Condition
```

---

# 41. Fleet Verification

For findings affecting multiple systems:

```text
[ ] Total affected population known
[ ] Retested population known
[ ] Verified population known
[ ] Unverified population known
[ ] Exceptions documented
[ ] Replacement systems considered
[ ] Inaccessible systems documented
```

Never silently convert:

```text
450 verified
+
50 inaccessible
```

into:

```text
500 verified
```

---

# 42. Final Assessment Review

Before closing:

```text
[ ] Authorization documented
[ ] Scope documented
[ ] Objective documented
[ ] Scanner documented
[ ] Scanner position documented
[ ] Configuration documented
[ ] Authentication documented
[ ] Coverage understood
[ ] Findings investigated
[ ] Important findings validated
[ ] Priorities explained
[ ] Report completed
[ ] Remediation assigned
[ ] Retest performed where required
[ ] Verification completed
[ ] Limitations documented
```

---

# 43. Final Decision

Determine which state applies:

```text
[ ] Assessment Complete
[ ] Assessment Complete With Limitations
[ ] Assessment Incomplete
```

Use the state supported by the actual evidence.

---

# 44. Closure Questions

Answer all of these:

```text
What was authorized?
-

What was the objective?
-

What was actually assessed?
-

What was not assessed?
-

What vulnerabilities were identified?
-

Which findings were validated?
-

Which findings remain uncertain?
-

What are the important root causes?
-

What requires remediation?
-

What has been remediated?
-

What has been independently verified?
-

What remains unresolved?
-

What should happen next?
-
```

---

# 45. Decision Log

For important decisions, record:

```text
Date / Time:

Decision:

Known Information:

Unknown Information:

Authorization:

Constraints:

Options:

Selected Action:

Reason:

Expected Result:

Actual Result:

Interpretation:

Remaining Uncertainty:

Next Action:
```

Do not record every insignificant UI interaction.

Record decisions that materially affect:

* scope
* configuration
* safety
* coverage
* findings
* validation
* prioritization
* remediation
* retesting

---

# 46. Troubleshooting Quick Path

## Nessus Platform Problem

```text
Service
↓
UI
↓
Resources
↓
Disk
↓
Activation
↓
Plugin / Content State
↓
Version / Edition
↓
Configuration
↓
Logs / Evidence
```

---

## Network Problem

```text
Target
↓
Scanner Position
↓
DNS
↓
Routing
↓
Host Reachability
↓
Port
↓
Service
↓
Firewall / ACL
↓
Intermediary
↓
Target Health
↓
Nessus Configuration
```

---

## Authentication Problem

```text
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

## Result Problem

```text
Scan Status
↓
Coverage
↓
Target
↓
Service
↓
Plugin
↓
Evidence
↓
Applicability
↓
Validation
↓
Interpretation
```

---

# 47. Comparison Quick Path

Before comparing two assessments:

```text
[ ] Same scope?
[ ] Same targets?
[ ] Same scanner?
[ ] Same scanner position?
[ ] Same authentication?
[ ] Same configuration?
[ ] Same plugin/content state?
[ ] Same target environment?
[ ] Similar assessment timing?
```

If several answers are "No", comparison may be limited.

---

# 48. Finding Disappearance Quick Path

```text
Finding disappeared
        ↓
Scope changed?
        ↓
Target changed?
        ↓
Scanner changed?
        ↓
Position changed?
        ↓
Authentication changed?
        ↓
Configuration changed?
        ↓
Plugin/content changed?
        ↓
Service changed?
        ↓
Target state changed?
        ↓
Remediation evidence?
        ↓
Conclusion
```

---

# 49. No-Finding Quick Path

If Nessus reports no vulnerabilities:

```text
[ ] Scope correct?
[ ] Targets reached?
[ ] Services observed?
[ ] Plugins executed?
[ ] Authentication expected?
[ ] Authentication successful?
[ ] Scanner position appropriate?
[ ] Errors present?
[ ] Coverage complete?
[ ] Expected vulnerabilities absent?
```

Then decide what the evidence actually supports.

---

# 50. Unexpected Host Quick Path

```text
Unexpected Host
↓
Is it authorized?
↓
Is it inside the technical boundary?
↓
Who owns it?
↓
Why is it present?
↓
Should it be assessed?
```

Do not automatically scan it.

---

# 51. Missing Host Quick Path

```text
Missing Host
↓
Target Definition
↓
DNS
↓
Routing
↓
Firewall
↓
Reachability
↓
Port
↓
Service
↓
Scanner Position
↓
Nessus Configuration
```

---

# 52. High-Severity Finding Quick Path

```text
High Severity
↓
Detection Evidence
↓
Applicability
↓
Validation
↓
Exposure
↓
Asset Importance
↓
Technical Impact
↓
Exploitability
↓
Business Context
↓
Compensating Controls
↓
Root Cause
↓
Priority
```

---

# 53. Remediation Verification Quick Path

```text
Finding
↓
Root Cause
↓
Remediation
↓
Expected State
↓
Comparable Retest
↓
Evidence
↓
Verification
↓
Final Status
```

---

# 54. Stop Conditions

Stop or pause assessment activity when:

```text
[ ] Authorization becomes unclear
[ ] Scope becomes disputed
[ ] Target becomes unauthorized
[ ] Operational impact exceeds agreed limits
[ ] Target stability becomes questionable
[ ] Required safety controls are unavailable
[ ] Validation may become disruptive
[ ] Credentials are being exposed or misused
[ ] Assessment conditions materially change
[ ] Stakeholder requests cessation
```

Document the reason.

---

# 55. Security of Assessment Artifacts

Before sharing or publishing:

```text
[ ] Credentials removed
[ ] Tokens removed
[ ] Private keys removed
[ ] Sensitive internal addresses reviewed
[ ] Confidential hostnames reviewed
[ ] Client information removed
[ ] Sensitive screenshots reviewed
[ ] Exported reports reviewed
[ ] Evidence sanitized
```

Never publish confidential assessment material simply because the repository is intended to demonstrate learning.

---

# 56. Public Repository Safety

For GitHub publication:

```text
[ ] No passwords
[ ] No API keys
[ ] No private keys
[ ] No session tokens
[ ] No client reports
[ ] No confidential screenshots
[ ] No private assessment exports
[ ] No unauthorized target information
[ ] No sensitive environment details
```

Use synthetic or intentionally vulnerable lab examples.

---

# 57. Professional Assessment Record

At the end of every significant assessment, preserve:

```text
Assessment:

Date:

Operator:

Authorization:

Objective:

Scope:

Exclusions:

Scanner:

Scanner Position:

Targets:

Authentication:

Workflow:

Configuration:

Execution:

Coverage:

Findings:

Validation:

Prioritization:

Remediation:

Retest:

Verification:

Limitations:

Final Status:

Next Action:
```

---

# 58. One-Minute Pre-Scan Checklist

When time is limited:

```text
[ ] Authorized?
[ ] In scope?
[ ] Objective clear?
[ ] Correct target?
[ ] Correct scanner?
[ ] Correct perspective?
[ ] Credentials correct?
[ ] Configuration appropriate?
[ ] Operational window valid?
[ ] Stop conditions known?
```

If any critical answer is unclear:

```text
STOP
↓
VERIFY
```

---

# 59. One-Minute Post-Scan Checklist

After completion:

```text
[ ] Completed successfully?
[ ] Coverage complete?
[ ] Authentication status understood?
[ ] Errors reviewed?
[ ] Important findings investigated?
[ ] Validation performed where needed?
[ ] Limitations documented?
[ ] Next action identified?
```

---

# 60. One-Minute Retest Checklist

```text
[ ] Same target?
[ ] Same scope?
[ ] Same perspective?
[ ] Same authentication?
[ ] Relevant configuration?
[ ] Relevant plugin/content state?
[ ] Finding status verified?
[ ] Remaining uncertainty documented?
```

---

# 61. The Five Questions

When uncertain about what to do next, ask:

```text
1. What am I trying to determine?

2. What do I actually know?

3. What am I authorized to do?

4. What evidence do I need?

5. What is the next lowest-impact action that can produce useful evidence?
```

If you can answer these five questions, the next step is usually clearer.

---

# 62. The Evidence Rule

Before making an important conclusion:

```text
What evidence supports this?
```

Then:

```text
What evidence could contradict it?
```

Then:

```text
Do I have enough evidence to make the claim?
```

If not:

```text
Document the uncertainty.
```

---

# 63. The Coverage Rule

Always distinguish:

```text
INTENDED
```

from:

```text
OBSERVED
```

from:

```text
ASSESSED
```

from:

```text
AUTHENTICATED
```

These are not interchangeable.

---

# 64. The Validation Rule

Use validation when it resolves meaningful uncertainty.

Do not validate merely because you can.

Prefer:

```text
Lowest Impact
+
Sufficient Evidence
```

over:

```text
Maximum Activity
```

---

# 65. The Prioritization Rule

Never reduce prioritization to:

```text
Severity = Priority
```

Instead consider:

```text
Validity
+
Asset
+
Exposure
+
Technical Impact
+
Exploitability
+
Business Context
+
Controls
+
Remediation
+
Root Cause
```

---

# 66. The Retest Rule

A retest should answer:

> Did the relevant condition change as intended?

To answer that, preserve meaningful comparability.

---

# 67. The Documentation Rule

Document enough that another qualified operator can answer:

```text
What happened?
Why?
What evidence supports it?
What remains uncertain?
What should happen next?
```

---

# 68. The Safety Rule

When assessment quality and operational safety conflict:

```text
Follow authorization
+
Follow operational constraints
+
Protect the environment
+
Document the limitation
```

A complete scan is not worth uncontrolled operational impact.

---

# 69. The Scope Rule

Never let technical discovery silently become scope expansion.

```text
Discovery
↓
Observation
↓
Authorization Check
↓
Assessment Decision
```

---

# 70. The Uncertainty Rule

Use:

```text
Unknown
```

when something is not established.

Use:

```text
Unresolved
```

when investigation has occurred but uncertainty remains.

Do not replace either with an unsupported conclusion.

---

# 71. The Independence Rule

When something unexpected happens:

```text
Do not ask:
"What button should I press?"
```

Ask:

```text
What changed?

What does the evidence show?

Which layer failed?

What decision am I trying to make?

What is the safest useful next action?
```

---

# 72. Complete Operator Workflow

The entire repository can now be reduced to this:

```text
AUTHORIZATION
      ↓
SCOPE
      ↓
OBJECTIVE
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
SAFETY
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
```

---

# 73. Final Operator Checklist

Before declaring yourself independently capable of operating Nessus, confirm:

```text
[ ] I can define an assessment objective.

[ ] I can verify authorization.

[ ] I can translate authorization into technical scope.

[ ] I can identify decision-critical unknowns.

[ ] I can select an appropriate workflow.

[ ] I can plan targets.

[ ] I understand scanner position.

[ ] I can perform authorized discovery.

[ ] I can configure unauthenticated assessment.

[ ] I can configure authenticated assessment.

[ ] I can handle credentials securely.

[ ] I can select appropriate plugin coverage.

[ ] I can configure performance responsibly.

[ ] I can perform preflight checks.

[ ] I can execute assessments safely.

[ ] I can monitor running assessments.

[ ] I can troubleshoot scanner problems.

[ ] I can troubleshoot network problems.

[ ] I can troubleshoot authentication problems.

[ ] I can evaluate assessment coverage.

[ ] I can read Nessus results critically.

[ ] I can investigate findings.

[ ] I can distinguish evidence from interpretation.

[ ] I can validate important findings.

[ ] I can recognize unresolved conditions.

[ ] I can identify root causes.

[ ] I can prioritize findings using context.

[ ] I can produce professional reports.

[ ] I can design remediation.

[ ] I can perform meaningful retests.

[ ] I can verify remediation.

[ ] I can compare assessments responsibly.

[ ] I can document limitations.

[ ] I can protect assessment artifacts.

[ ] I can make decisions under incomplete information.

[ ] I can determine the next action without a predefined recipe.
```

---

# Final Nessus Operator Mental Model

When receiving a new assessment, think:

```text
WHAT IS AUTHORIZED?
        ↓
WHAT IS THE QUESTION?
        ↓
WHAT IS IN SCOPE?
        ↓
WHAT DO I KNOW?
        ↓
WHAT DO I NOT KNOW?
        ↓
WHAT MATTERS IMMEDIATELY?
        ↓
WHAT WORKFLOW ANSWERS THE QUESTION?
        ↓
WHAT PERSPECTIVE IS REQUIRED?
        ↓
WHAT CONFIGURATION IS APPROPRIATE?
        ↓
IS IT SAFE TO RUN?
        ↓
WHAT DID I ACTUALLY ASSESS?
        ↓
WHAT EVIDENCE DID I GET?
        ↓
WHAT DOES IT SUPPORT?
        ↓
WHAT REMAINS UNCERTAIN?
        ↓
WHAT NEEDS VALIDATION?
        ↓
WHAT MATTERS MOST?
        ↓
WHAT SHOULD BE FIXED?
        ↓
WAS IT FIXED?
        ↓
CAN I VERIFY IT?
        ↓
CAN I DEFEND THE CONCLUSION?
        ↓
WHAT HAPPENS NEXT?
```

---

# Repository Completion Standard

The Nessus Workflow repository is complete when the operator can move from:

```text
Assessment Request
```

to:

```text
Verified Assessment Outcome
```

without requiring a predefined procedure for every individual situation.

The repository has therefore progressed through:

```text
FOUNDATIONS
        ↓
SETUP
        ↓
FIRST ASSESSMENT
        ↓
CONFIGURATION
        ↓
ASSESSMENT WORKFLOWS
        ↓
RESULTS
        ↓
VALIDATION
        ↓
PRIORITIZATION
        ↓
REPORTING
        ↓
REMEDIATION
        ↓
TROUBLESHOOTING
        ↓
PROFESSIONAL OPERATION
        ↓
DECISION LABS
        ↓
INDEPENDENT ASSESSMENTS
        ↓
CAPSTONE
        ↓
OPERATOR CHECKLIST
```

---

# Final Principle

Nessus is a tool.

Professional vulnerability assessment is the workflow surrounding the tool.

The mature operator does not think:

> "Which Nessus button comes next?"

The mature operator thinks:

> **"What am I trying to determine, what evidence do I have, what am I authorized to do, what constraints apply, and what is the next defensible action?"**

That is the skill this repository is designed to build.
