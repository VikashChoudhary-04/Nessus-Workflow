# Professional Reporting

## Objective

Learn how to turn Nessus assessment results into a clear, evidence-based professional report.

A vulnerability assessment is not complete when the scan finishes.

The assessment becomes useful when another person can understand:

* what was assessed
* why it was assessed
* what was discovered
* how reliable the results are
* which findings matter
* what evidence supports them
* what should be done
* what remains uncertain
* how remediation will be verified

By the end of this workflow, you should be able to:

* structure a professional Nessus assessment report
* separate executive information from technical evidence
* describe scope and methodology accurately
* communicate limitations and coverage
* write useful finding descriptions
* connect evidence to impact
* provide actionable remediation guidance
* avoid misleading severity-only reporting
* document validation and uncertainty
* create remediation-oriented reporting
* prepare reports for technical and non-technical audiences

---

# 1. Why Reporting Matters

A scanner produces technical data.

A report turns that data into a decision-support artifact.

The workflow is:

```text id="0e4j3s"
Assessment
    ↓
Results
    ↓
Investigation
    ↓
Validation
    ↓
Prioritization
    ↓
Interpretation
    ↓
Report
    ↓
Decision / Remediation
```

A raw finding list is not automatically a professional report.

A useful report explains the meaning of the findings.

---

# 2. The Reporting Mental Model

Use:

```text id="h4z7xq"
WHY
 ↓
WHAT WAS ASSESSED
 ↓
HOW IT WAS ASSESSED
 ↓
WHAT WAS FOUND
 ↓
HOW CONFIDENT ARE WE
 ↓
WHY IT MATTERS
 ↓
WHAT SHOULD HAPPEN
 ↓
HOW WILL IT BE VERIFIED
```

Every major section should answer a question.

| Report Section    | Question                          |
| ----------------- | --------------------------------- |
| Executive Summary | What should decision-makers know? |
| Scope             | What was assessed?                |
| Methodology       | How was it assessed?              |
| Coverage          | What was actually assessed?       |
| Findings          | What was discovered?              |
| Evidence          | Why do we believe it?             |
| Impact            | Why does it matter?               |
| Remediation       | What should be done?              |
| Limitations       | What could not be determined?     |
| Retest            | How will remediation be verified? |

---

# 3. Know Your Audience

Different readers need different levels of detail.

## Executive Audience

Usually needs:

* assessment purpose
* overall themes
* major exposure areas
* significant findings
* business-relevant impact
* remediation themes
* important limitations
* recommended next actions

They usually do not need:

* raw plugin output
* every technical parameter
* command-by-command investigation notes
* scanner configuration details unrelated to decisions

---

## Technical Audience

Needs more detail:

* affected hosts
* ports/services
* components
* finding references
* evidence
* configuration details
* authentication context
* validation results
* remediation steps
* retest requirements

---

## Security / Risk Team

May need:

* severity
* exploitability
* exposure
* asset importance
* business context
* remediation status
* exceptions
* trends
* residual risk
* ownership

A single report can support multiple audiences if it is structured correctly.

---

# 4. Executive Summary

The executive summary should answer:

```text id="g3j7dx"
What did we assess?
What did we find?
What matters most?
What should happen next?
```

Avoid:

> "Nessus found 247 vulnerabilities."

That statement lacks context.

A better structure is:

```text id="j0a5sc"
Assessment Purpose:
Assess the authorized production web-server environment.

Scope:
12 production hosts.

Major Observations:
- Several systems require patching.
- One externally reachable service requires immediate investigation.
- Multiple findings share a common software-update root cause.

Important Limitation:
One host was not successfully assessed with the intended authenticated perspective.

Next Actions:
- Investigate the externally reachable finding.
- Remediate the shared software issue.
- Repeat the assessment for the incompletely assessed host.
```

The exact conclusions must come from the assessment evidence.

---

# 5. Do Not Overstate the Results

Avoid statements such as:

> "The network is secure."

A vulnerability assessment cannot establish universal security.

Likewise avoid:

> "No vulnerabilities exist."

A scan only provides evidence under the conditions under which it operated.

Prefer:

> "No findings matching the configured assessment coverage were identified on the successfully assessed targets."

This preserves the distinction between:

```text id="q1v3az"
No Finding
```

and:

```text id="8j0z3h"
No Vulnerability Exists
```

---

# 6. Scope

Document exactly what was assessed.

Include where appropriate:

* target ranges
* hostnames
* systems
* applications
* environments
* assessment window
* exclusions
* authorized boundaries

Example:

```text id="p9v7r2"
Scope:
- 10.10.10.10
- 10.10.10.11
- 10.10.10.12

Environment:
Authorized laboratory web infrastructure.

Assessment Window:
2026-09-25

Excluded:
10.10.10.20
```

Never silently expand the scope in the report.

---

# 7. Scope Is More Than an IP Range

Scope should capture the authorization boundary and the technical target.

Consider:

```text id="4u5k2p"
Authorization
     ↓
Environment
     ↓
Target Systems
     ↓
Services
     ↓
Assessment Objective
```

If a host was discovered outside the approved scope, document it appropriately but do not imply that it was assessed merely because it was visible.

---

# 8. Assessment Objective

State why the assessment was performed.

Examples:

```text id="n5w8s3"
Objective:
Identify network-visible vulnerabilities affecting the authorized web-server environment.
```

or:

```text id="c3x1q7"
Objective:
Evaluate the configuration state of authorized Linux systems against the approved baseline.
```

The objective should determine the interpretation of results.

---

# 9. Methodology

Describe the workflow at a level appropriate for the audience.

Include:

* Nessus edition/version where relevant
* scanner used
* assessment type
* discovery approach
* authenticated/unauthenticated perspective
* relevant policy/template
* plugin/content state where useful
* assessment dates
* validation approach

Example:

```text id="f7k2vd"
Methodology:

An authorized vulnerability assessment was performed using
Nessus against the defined target set.

The assessment used an authenticated perspective where
credentials were successfully established.

Findings were reviewed individually and significant or
ambiguous findings were investigated using additional
non-destructive evidence where appropriate.
```

Do not claim actions that were not performed.

---

# 10. Assessment Coverage

Coverage is one of the most important reporting elements.

Document:

* intended targets
* reachable targets
* successfully assessed targets
* authentication status
* incomplete targets
* excluded targets
* important limitations

Example:

```text id="q4k1s8"
Intended Targets: 10
Reachable: 9
Successfully Assessed: 8
Authentication Successful: 7
Incomplete: 2
```

The numbers should come from the actual assessment.

---

# 11. Coverage Limitations

Examples:

* host unreachable
* authentication failed
* service unavailable
* network filtering
* incomplete scan
* credential permissions insufficient
* unsupported target technology
* scanner configuration limitation
* assessment window ended early

Document limitations clearly.

A limitation is not a failure of the report.

Hiding a limitation is a failure of the report.

---

# 12. Findings Overview

A findings overview can provide a high-level picture.

For example:

| Severity      | Count |
| ------------- | ----: |
| Critical      |     2 |
| High          |     8 |
| Medium        |    21 |
| Low           |    34 |
| Informational |    76 |

These numbers are descriptive.

They should not be presented as the complete risk conclusion.

Always consider:

* scope
* coverage
* duplicate/related findings
* root causes
* asset context
* exposure
* validation status

---

# 13. Finding Counts Need Context

Suppose:

```text id="f5a2r8"
Critical: 0
High: 0
Medium: 2
```

That does not automatically mean the environment is low risk.

The two medium findings could affect:

* a critical service
* a shared infrastructure component
* an internet-facing system

Likewise:

```text id="x8q3km"
Critical: 4
```

does not automatically tell you the remediation sequence.

Counts are useful descriptive information.

They are not the entire assessment conclusion.

---

# 14. Finding Structure

A professional technical finding should answer:

```text id="k5c9z0"
WHAT?
WHERE?
EVIDENCE?
WHY DOES IT MATTER?
HOW CAN IT BE REMEDIATED?
HOW WILL IT BE VERIFIED?
```

A useful structure is:

```text id="t8m4ny"
Finding Title
Finding Reference
Severity

Affected Assets

Description

Evidence

Technical Impact

Environmental Context

Validation

Remediation

Retest / Verification

References
```

---

# 15. Finding Title

The title should communicate the actual issue.

Weak:

> "Security Vulnerability"

Better:

> "Outdated Web Server Component"

Better still, where supported by evidence:

> "Outdated Web Server Component on HTTPS Service"

Avoid sensational language.

---

# 16. Finding Reference

Include an identifier where useful.

Examples may include:

* Nessus plugin reference
* CVE
* vendor advisory
* internal finding ID
* policy/control reference

Do not assume every finding has a CVE.

Some findings represent:

* configuration weaknesses
* information disclosure
* policy violations
* unsupported software
* weak protocols
* local configuration conditions

Use the identifier appropriate to the finding.

---

# 17. Description

Explain what the condition is.

A good description answers:

> "What did the assessment identify?"

Example:

```text id="8q1m4c"
The assessment identified an outdated version of the affected
web-server component on the HTTPS service. Nessus reported
version evidence indicating that the installed component may
be affected by the referenced vulnerability.
```

Avoid adding claims that were not established.

---

# 18. Evidence

Evidence supports the finding.

Possible evidence includes:

* detected version
* package information
* configuration state
* service response
* plugin output
* authentication evidence
* network exposure
* validated target condition

Example:

```text id="c7f2mz"
Evidence:
Host: 10.10.10.10
Port: 443/tcp
Service: HTTPS
Detected Component: Example Web Server
Detected Version: X.Y.Z
Nessus Finding: Plugin reference
```

Do not expose credentials or secrets.

---

# 19. Evidence vs Interpretation

Keep them separate.

### Evidence

```text id="7v5q1h"
Detected Version: X.Y.Z
Port: 443/tcp
```

### Interpretation

```text id="d3x9wk"
The detected version falls within the affected range described
by the relevant vendor advisory.
```

This distinction makes the report easier to review.

---

# 20. Validation Status

Where validation was performed, document the outcome.

Possible statuses include:

```text id="8s2r7v"
Confirmed
Not Applicable
False Positive
Unresolved
Condition Changed
```

Use the terminology appropriate to your assessment process.

If a finding could not be fully validated:

```text id="j4n6xc"
Validation Status:
Unresolved

Reason:
The available evidence was insufficient to establish the target's
exact component state.
```

Do not convert uncertainty into certainty merely to make the report cleaner.

---

# 21. Technical Impact

Explain what the vulnerability could enable.

Example:

```text id="a2m8pw"
Technical Impact:

If successfully exploited under the documented prerequisites,
the vulnerability could permit unauthorized actions against the
affected service.
```

Avoid claiming exploitation occurred unless it was actually established.

Distinguish:

```text id="0h8m1x"
Potential Impact
```

from:

```text id="6d4n7k"
Observed Impact
```

---

# 22. Environmental Context

Add context specific to the target.

Examples:

```text id="z5w2cb"
The affected service is externally reachable.
```

or:

```text id="r8f3yk"
The affected host is restricted to an internal management segment.
```

or:

```text id="u6k9qm"
The affected system provides services used by multiple internal applications.
```

Only include context supported by authorized evidence or documented asset information.

---

# 23. Remediation

A remediation recommendation should be actionable.

Weak:

> "Fix this vulnerability."

Better:

```text id="6h9q2v"
Upgrade the affected component to a vendor-supported version
that addresses the identified vulnerability. Confirm application
compatibility before deployment and perform a Nessus retest after
the change.
```

Where appropriate, remediation can include:

* patching
* upgrading
* configuration change
* service removal
* access restriction
* segmentation
* disabling unnecessary functionality
* replacing unsupported software

---

# 24. Avoid Over-Specific Remediation

Do not prescribe a technical change you have not verified.

For example, do not write:

> "Disable setting X immediately."

unless you have established that:

* setting X exists
* it is relevant
* disabling it is appropriate
* the change will not break required functionality

Use evidence-based guidance.

---

# 25. Retest Requirement

Important findings should have a clear verification method.

Example:

```text id="g8m2sd"
Verification:

After remediation, perform a comparable Nessus assessment
against the affected host and verify that the finding is no
longer detected.

Where necessary, validate the updated component or configuration
independently of the Nessus result.
```

This connects reporting to the remediation lifecycle.

---

# 26. Root Cause Reporting

When several findings share a cause, report the underlying issue.

Example:

```text id="v2k5rm"
Root Cause:
Multiple findings are associated with the same outdated software
component.

Recommended Remediation:
Update the component to a supported version and retest all
affected services.
```

This is more useful than creating five disconnected descriptions of the same remediation problem.

---

# 27. Prioritization in the Report

A finding can include a priority rationale.

Example:

```text id="f4q7sx"
Priority Rationale:

The finding affects an externally reachable service and requires
limited privileges under the documented exploitation conditions.
The affected system supports an important business function.
Remediation should therefore be scheduled promptly.
```

This is stronger than:

> "High severity = fix immediately."

---

# 28. Unresolved Findings

An unresolved finding should remain clearly marked.

Example:

```text id="m9w4qc"
Finding Status:
Unresolved

Reason:
Nessus identified evidence consistent with the vulnerable
condition, but the exact installed component state could not
be independently confirmed during the assessment window.

Recommended Action:
Verify the installed component version and repeat the assessment.
```

Unresolved does not mean false positive.

It means the evidence is insufficient for a stronger conclusion.

---

# 29. Limitations Section

Every professional report should state meaningful limitations.

Examples:

* limited target scope
* limited assessment window
* unavailable credentials
* incomplete authentication
* network restrictions
* unreachable hosts
* unsupported technologies
* exclusions
* incomplete scan
* non-destructive validation only

Example:

```text id="s3k8mz"
Limitations:

One authorized host was unreachable during the assessment window.
Authenticated coverage was therefore incomplete for that system.

The results should not be interpreted as evidence that the host
is free of vulnerabilities.
```

---

# 30. Methodology Limitations vs Findings

Keep these separate.

### Methodology limitation

> One host could not be authenticated.

### Finding

> The assessed host exposes an outdated service.

The limitation affects confidence and coverage.

It should not be mixed into the technical description of an unrelated finding.

---

# 31. Executive vs Technical Detail

A professional report can use layers.

```text id="0z8g1k"
Executive Summary
        ↓
Assessment Overview
        ↓
Key Findings
        ↓
Detailed Findings
        ↓
Evidence
        ↓
Appendices
```

This allows different readers to stop at the level relevant to them.

---

# 32. Example Report Structure

A practical report can follow:

```text id="m6w4cs"
1. Executive Summary
2. Assessment Objective
3. Scope
4. Assessment Window
5. Methodology
6. Coverage
7. Limitations
8. Findings Overview
9. Priority Findings
10. Detailed Findings
11. Remediation Recommendations
12. Retest / Verification Plan
13. Conclusion
14. Appendices
```

The exact structure can be adapted to organizational requirements.

---

# 33. Conclusion Section

The conclusion should summarize what the evidence supports.

Avoid:

> "The environment is secure."

Prefer:

```text id="j8q4nl"
The assessment identified several vulnerabilities and configuration
conditions within the authorized scope. The most significant
observations involve exposed services and outdated components.

One target could not be fully assessed using the intended
authenticated perspective. Additional assessment is recommended
after authentication is restored.

Remediation should focus on the documented findings and their
associated root causes, followed by comparable retesting.
```

---

# 34. Appendix

The appendix can contain technical detail that would otherwise make the main report difficult to read.

Possible contents:

* complete finding inventory
* host inventory
* port/service observations
* plugin references
* detailed evidence
* assessment configuration
* exclusions
* validation notes
* remediation tracking
* glossary

Do not place secrets in the appendix.

---

# 35. Nessus Exports and Reports

Nessus provides reporting/export capabilities that can vary by product edition and version.

Treat exported scanner output as source data rather than automatically assuming it is the finished professional report.

Before distributing an export, review:

* target information
* finding content
* sensitive information
* scope
* assessment date
* completeness
* audience
* formatting
* remediation context

The exact available export formats and options depend on the installed Nessus product/version.

Use current Tenable documentation when a specific export capability needs to be confirmed.

---

# 36. Protect the Report

Assessment reports can contain sensitive security information.

Potentially sensitive content includes:

* internal IP addresses
* hostnames
* software versions
* network architecture
* vulnerability details
* configuration information
* security-control information
* authentication information
* internal asset names

Apply appropriate organizational handling requirements.

Never commit confidential assessment reports to a public GitHub repository.

---

# 37. Reporting Workflow

Use this process:

```text id="4r2q6p"
ASSESSMENT COMPLETE
        ↓
CHECK COVERAGE
        ↓
REVIEW RESULTS
        ↓
INVESTIGATE FINDINGS
        ↓
VALIDATE IMPORTANT FINDINGS
        ↓
GROUP ROOT CAUSES
        ↓
PRIORITIZE
        ↓
WRITE EXECUTIVE SUMMARY
        ↓
DOCUMENT SCOPE / METHOD
        ↓
WRITE DETAILED FINDINGS
        ↓
DOCUMENT LIMITATIONS
        ↓
DEFINE REMEDIATION
        ↓
DEFINE RETEST
        ↓
QUALITY REVIEW
        ↓
FINAL REPORT
```

Do not write the executive conclusion before understanding coverage and findings.

---

# 38. Quality Review

Before delivering a report, perform a final review.

## Scope

* [ ] Scope is accurate
* [ ] Exclusions are documented
* [ ] Assessment dates are correct
* [ ] Unauthorized targets are not presented as assessed

## Methodology

* [ ] Assessment type is accurate
* [ ] Authentication state is accurate
* [ ] Relevant scanner/configuration details are accurate
* [ ] No unperformed activity is claimed

## Findings

* [ ] Finding titles are accurate
* [ ] Evidence is included
* [ ] Affected assets are correct
* [ ] Validation status is clear
* [ ] Impact is evidence-based
* [ ] Root causes are identified where appropriate
* [ ] Related findings are not unnecessarily duplicated

## Remediation

* [ ] Recommendations are actionable
* [ ] Recommendations do not assume unverified facts
* [ ] Retest requirements are defined
* [ ] Owners/status are included where required

## Limitations

* [ ] Coverage limitations are documented
* [ ] Authentication limitations are documented
* [ ] Incomplete assessments are disclosed
* [ ] Uncertainty is preserved

## Security

* [ ] Credentials are not included
* [ ] Tokens/secrets are not included
* [ ] Sensitive report handling is appropriate
* [ ] Distribution audience is appropriate

---

# 39. Practical Lab 1 — Write a Technical Finding

Take one authorized lab finding and produce:

```text id="h2f9kv"
Finding Title:
Reference:
Severity:

Affected Asset:
Affected Service:

Description:

Evidence:

Technical Impact:

Environmental Context:

Validation:

Remediation:

Retest:

References:
```

Do not copy the Nessus plugin description blindly.

Rewrite it so another technical person can understand the actual condition.

---

# 40. Practical Lab 2 — Write an Executive Summary

Using several lab findings, write a short executive summary containing:

1. assessment purpose
2. scope
3. major observations
4. important limitations
5. major remediation themes
6. next steps

Keep technical plugin details out of the executive section unless they materially affect the decision.

---

# 41. Practical Lab 3 — Report an Incomplete Assessment

## Scenario

You intended to assess five hosts.

Observed:

```text id="5h8q1m"
5 Authorized
4 Reachable
3 Successfully Assessed
1 Authentication Failed
1 Unreachable
```

Write:

* coverage statement
* limitation statement
* executive impact
* recommended follow-up

Do not describe the entire environment as assessed successfully.

---

# 42. Practical Lab 4 — Group Findings by Root Cause

Take multiple findings from one host.

Determine:

```text id="z2q7pv"
Finding A ─┐
Finding B ─┼── Shared Root Cause
Finding C ─┘
```

Write one remediation recommendation that addresses the root cause.

Then identify whether each individual finding still requires separate validation.

---

# 43. Practical Lab 5 — Build a Complete Mini Report

Use an authorized lab assessment.

Create:

```text id="k6n3aw"
1. Executive Summary
2. Objective
3. Scope
4. Methodology
5. Coverage
6. Limitations
7. Findings Overview
8. Detailed Findings
9. Remediation
10. Retest Plan
11. Conclusion
```

Do not attempt to make it visually elaborate.

Focus on:

* accuracy
* evidence
* clarity
* reproducibility
* actionable remediation

---

# 44. Practical Lab 6 — Report a False Positive or Unresolved Finding

Take a finding where validation does not support the original conclusion.

Write:

```text id="q4m7dc"
Finding:
Original Evidence:
Additional Investigation:
Validation Result:
Final Status:
Reason:
Recommended Action:
```

Practice reporting uncertainty honestly.

---

# 45. Practical Lab 7 — Audience Transformation

Take the same technical finding and describe it at three levels.

### Executive

```text id="8v1m3s"
Short business-relevant explanation.
```

### Security Team

```text id="r6k4pq"
Risk, exposure, validation, remediation and follow-up.
```

### Technical Team

```text id="2x9d7h"
Host, port, component, evidence, remediation and verification.
```

The underlying facts must remain consistent.

Only the presentation changes.

---

# 46. Professional Report Record

For a major assessment, maintain a source record:

```text id="n7c2xq"
Assessment:
Assessment Date:

Objective:

Authorized Scope:

Scanner:
Nessus Version / Edition:

Assessment Type:

Authentication:

Policy / Configuration:

Plugin / Content State:

Coverage:

Limitations:

Major Findings:

Validated Findings:

Unresolved Findings:

Root Causes:

Priorities:

Remediation Recommendations:

Retest Requirements:

Report Version:

Reviewer:

Distribution:
```

This makes the report traceable to the actual assessment.

---

# 47. Common Reporting Mistakes

## Mistake 1 — Reporting the Dashboard Instead of the Assessment

### Problem

The report becomes a screenshot or count dump.

### Better approach

Explain what the results mean.

---

## Mistake 2 — Hiding Coverage Problems

### Problem

The report says "10 hosts assessed" when only 8 were successfully assessed.

### Better approach

State the actual coverage.

---

## Mistake 3 — Treating Severity as Business Priority

### Problem

Every Critical finding is presented as automatically the most urgent business issue.

### Better approach

Add exposure, asset context, exploitability, evidence and remediation context.

---

## Mistake 4 — Copying Plugin Text Without Interpretation

### Problem

Readers receive scanner output but not useful analysis.

### Better approach

Translate evidence into a clear finding description.

---

## Mistake 5 — Overstating Certainty

### Problem

Potential impact is written as observed compromise.

### Better approach

Distinguish potential, validated and observed conditions.

---

## Mistake 6 — Missing Remediation

### Problem

The report identifies problems but does not explain what should happen.

### Better approach

Provide actionable, evidence-based remediation.

---

## Mistake 7 — Missing Retest

### Problem

The report ends with remediation.

### Better approach

Define how remediation will be verified.

---

## Mistake 8 — Ignoring Root Cause

### Problem

The report creates dozens of disconnected remediation items.

### Better approach

Group related findings when they share a remediation cause.

---

## Mistake 9 — Including Secrets

### Problem

Credentials or tokens appear in evidence or screenshots.

### Better approach

Redact secrets and never include them in GitHub artifacts.

---

## Mistake 10 — Writing for Only One Audience

### Problem

Executives receive raw technical output or engineers receive only vague summaries.

### Better approach

Layer the report.

---

# 48. Decision Rule

When writing a professional Nessus report:

```text id="x5w8m1"
What was authorized?
        ↓
What was assessed?
        ↓
What was actually covered?
        ↓
What was found?
        ↓
What evidence supports it?
        ↓
What remains uncertain?
        ↓
Why does it matter?
        ↓
What is the root cause?
        ↓
What should be done?
        ↓
How will it be verified?
```

The key rule is:

> **A professional report should allow another person to understand the assessment, evaluate the evidence, act on the findings, and verify the outcome without relying on the assessor's memory.**

---

# 49. Reporting Checklist

## Assessment Context

* [ ] Objective documented
* [ ] Scope documented
* [ ] Assessment window documented
* [ ] Methodology documented
* [ ] Scanner context documented

## Coverage

* [ ] Intended targets documented
* [ ] Successfully assessed targets identified
* [ ] Authentication coverage documented
* [ ] Incomplete targets identified
* [ ] Limitations documented

## Findings

* [ ] Important findings investigated
* [ ] Evidence included
* [ ] Validation status documented
* [ ] Technical impact explained
* [ ] Environmental context included
* [ ] Root causes identified
* [ ] Related findings grouped where appropriate

## Remediation

* [ ] Recommendations actionable
* [ ] Priority rationale documented where needed
* [ ] Remediation dependencies identified
* [ ] Retest requirements defined

## Quality

* [ ] No unsupported claims
* [ ] No secrets
* [ ] No unexplained severity-only conclusions
* [ ] Audience appropriate
* [ ] Report reviewed before distribution

---

# 50. Completion Criteria

You have completed this workflow when you can independently:

* create a professional Nessus report structure
* write an accurate executive summary
* document scope and methodology
* document actual assessment coverage
* communicate limitations
* write technically useful findings
* separate evidence from interpretation
* explain technical impact
* document validation status
* provide actionable remediation
* group related findings by root cause
* define retest requirements
* adapt the same evidence for different audiences
* protect sensitive assessment information
* review a report for unsupported claims
* produce a report another professional can act on

---

# 51. Final Mental Model

Remember:

```text id="p4x7z2"
AUTHORIZED OBJECTIVE
        ↓
SCOPE
        ↓
METHODOLOGY
        ↓
COVERAGE
        ↓
FINDINGS
        ↓
EVIDENCE
        ↓
VALIDATION
        ↓
IMPACT
        ↓
ROOT CAUSE
        ↓
PRIORITY
        ↓
REMEDIATION
        ↓
RETEST
        ↓
REPORT
```

The report is not simply a record of what Nessus displayed.

It is the documented reasoning that connects:

```text id="j3v8nq"
Assessment Data
      ↓
Evidence
      ↓
Interpretation
      ↓
Decision
      ↓
Action
      ↓
Verification
```

A strong report tells the reader **what happened, what the evidence supports, what remains uncertain, what should happen next, and how the result will be verified.**
