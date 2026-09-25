# Remediation and Retest

## Objective

Learn how to move from a Nessus finding to verified remediation.

A vulnerability assessment does not end when a finding is reported.

The complete lifecycle is:

```text id="7n4m2x"
FINDING
   ↓
INVESTIGATION
   ↓
VALIDATION
   ↓
PRIORITIZATION
   ↓
REMEDIATION
   ↓
RETEST
   ↓
COMPARISON
   ↓
VERIFICATION
   ↓
CLOSE / FOLLOW UP
```

By the end of this workflow, you should be able to:

* distinguish remediation from retesting
* translate findings into remediation actions
* identify root causes
* choose appropriate remediation strategies
* document remediation changes
* design a meaningful retest
* compare pre-remediation and post-remediation assessments
* determine whether a finding is actually resolved
* recognize incomplete remediation
* handle changed conditions and regressions
* document residual risk and exceptions
* close the assessment lifecycle professionally

---

# 1. What Is Remediation?

Remediation is the action taken to address an identified security weakness.

Examples include:

* applying a security update
* upgrading software
* changing configuration
* removing an unnecessary service
* restricting network access
* replacing unsupported software
* correcting permissions
* disabling an insecure protocol
* implementing an approved compensating control

The exact remediation depends on the root cause.

---

# 2. Remediation Is Not the Same as Retesting

These are separate activities.

### Remediation

Changes the target condition.

### Retest

Checks whether the intended change actually resolved the condition.

Conceptually:

```text id="r6q8w3"
Finding
  ↓
Change Target
  ↓
Retest Target
  ↓
Evaluate Evidence
```

Do not treat:

> "The administrator says it was patched."

as equivalent to:

> "The vulnerability was verified as remediated."

The first is a remediation claim.

The second requires assessment evidence.

---

# 3. The Remediation Lifecycle

Use:

```text id="c8m1z7"
IDENTIFY
   ↓
UNDERSTAND
   ↓
PRIORITIZE
   ↓
PLAN
   ↓
REMEDIATE
   ↓
VERIFY
   ↓
RETEST
   ↓
COMPARE
   ↓
CLOSE / REOPEN
```

Every stage should have a clear purpose.

---

# 4. Start With the Root Cause

Before recommending remediation, ask:

```text id="y4q9k2"
What actually causes the finding?
```

For example:

```text id="p3n8v5"
Finding A
Finding B
Finding C
       ↓
Outdated Component
       ↓
Upgrade Component
```

Instead of:

```text id="w7x2m6"
Fix Finding A
Fix Finding B
Fix Finding C
```

The root-cause approach reduces duplicated work and produces more useful remediation guidance.

---

# 5. Common Remediation Categories

## Software Update

Examples:

* operating system patch
* application update
* library update
* firmware update

---

## Configuration Change

Examples:

* disable insecure protocol
* strengthen security setting
* remove unsafe configuration
* correct permissions

---

## Service Removal

If a service is unnecessary:

```text id="q4m7h2"
Unnecessary Service
      ↓
Disable / Remove
      ↓
Reduce Attack Surface
      ↓
Retest
```

---

## Access Restriction

Possible controls include:

* firewall restrictions
* network segmentation
* access-control changes
* management-interface restrictions

These can reduce exposure but may not eliminate the underlying vulnerability.

---

## Platform Replacement

Sometimes remediation requires replacing:

* unsupported operating systems
* obsolete applications
* deprecated components
* unsupported hardware/software combinations

---

## Compensating Control

When immediate remediation is not possible, an organization may implement an approved compensating control.

Document:

* what the control is
* what exposure it reduces
* what it does not address
* who approved it
* how long it remains valid
* when permanent remediation is expected

A compensating control should not automatically be reported as equivalent to removing the vulnerability.

---

# 6. Remediation Planning

A remediation plan should identify:

```text id="s3x8v4"
Finding
   ↓
Root Cause
   ↓
Affected Asset
   ↓
Remediation Action
   ↓
Owner
   ↓
Expected Result
   ↓
Retest Method
   ↓
Target Date
```

Example:

```text id="f8m2q6"
Finding:
Outdated web-server component

Root Cause:
Unsupported component version

Remediation:
Upgrade to approved supported version

Expected Result:
Affected component no longer falls within the
vulnerable version range

Retest:
Authenticated Nessus assessment plus version verification
```

---

# 7. Expected Result

Before remediation begins, define what success looks like.

Examples:

```text id="j7c2m5"
Expected Result:
Affected package is updated to a supported version.
```

or:

```text id="v5n8q1"
Expected Result:
The vulnerable service is no longer exposed from
the unauthorized network segment.
```

or:

```text id="k4p9s3"
Expected Result:
The insecure protocol is disabled and the approved
secure protocol remains operational.
```

This makes the retest objective measurable.

---

# 8. Remediation Evidence

Record evidence of the change where appropriate.

Examples:

* installed package version
* application version
* configuration state
* service state
* firewall rule
* network exposure
* vendor update record
* change-management reference

Do not place secrets in remediation records.

The remediation record and the security assessment should be traceable to one another.

---

# 9. Do Not Assume the Reported Fix Is Correct

A remediation ticket may say:

> "Patch applied."

That is useful information.

It is not the final security conclusion.

Possible problems include:

* wrong package updated
* wrong host updated
* patch failed
* service still running old version
* multiple vulnerable instances exist
* configuration remained unchanged
* another component is affected
* scanner visibility changed

Therefore:

```text id="m1q8z4"
Reported Remediation
        ↓
Independent Verification
        ↓
Retest
```

---

# 10. Retest Design

A retest should reproduce enough of the original assessment conditions to answer:

> "Did the remediation resolve the identified condition?"

Compare:

```text id="e6k3w9"
Original Assessment
        ↓
Remediation
        ↓
Comparable Assessment
        ↓
Result Comparison
```

Try to preserve:

* target
* assessment objective
* scanner perspective
* authentication
* relevant configuration
* network position
* relevant plugin/content coverage

The more these differ, the more carefully the result must be interpreted.

---

# 11. Retest vs New Assessment

A retest is focused.

A new assessment may have a broader objective.

### Retest

```text id="d2w7k5"
Did Finding X get remediated?
```

### New assessment

```text id="m8q4s1"
What vulnerabilities currently exist
across the broader authorized environment?
```

A retest may be performed against a subset of the original scope.

Document the distinction.

---

# 12. Choosing Retest Scope

The smallest useful scope is often preferable.

If one host and one service were remediated:

```text id="a6v3q9"
Retest:
Affected Host
Affected Service
```

If a shared root cause affects 20 hosts:

```text id="y8n4m2"
Retest:
All affected hosts
```

Do not retest only one system when the remediation was supposed to address a fleet-wide condition.

---

# 13. Retest Scope Should Match Remediation Scope

Use:

```text id="p9m5c7"
Remediation Scope
       ↓
Affected Population
       ↓
Retest Scope
```

Example:

```text id="q2w8x6"
Remediation:
Upgrade component on 15 servers

Retest:
Verify the affected condition across the
15 relevant servers
```

A successful retest of one host does not automatically prove all 15 were remediated.

---

# 14. Retest Authentication

If the original finding depended on authenticated visibility, preserve the authentication perspective where appropriate.

Example:

```text id="n5k7r2"
Original:
Authenticated assessment

Retest:
Authenticated assessment
```

If authentication fails during the retest:

```text id="x4q8m1"
Finding disappeared
        ↓
Authentication failed
        ↓
Coverage reduced
        ↓
Cannot conclude remediation
```

A loss of visibility is not remediation evidence.

---

# 15. Retest Network Perspective

Network position can affect what Nessus can observe.

If the original assessment was performed from a specific network location, try to preserve the relevant perspective.

For example:

```text id="z6c2v8"
Original:
Internal network

Retest:
Different network segment
```

A difference in visibility may explain a finding disappearing.

Document meaningful changes.

---

# 16. Retest Configuration

If the finding depended on a specific assessment configuration, preserve the relevant configuration.

Compare:

* policy
* discovery behavior
* assessment settings
* credentials
* exclusions
* plugin/content state
* target definition

A retest should not accidentally become a completely different assessment.

---

# 17. Retest Result Categories

After retesting, classify the outcome.

Useful categories include:

```text id="c5m8q3"
Remediated
Partially Remediated
Still Present
Condition Changed
Not Applicable
Unable to Verify
Reappeared
```

Use terminology appropriate to your organization's process.

---

# 18. Remediated

The evidence supports that the original condition has been addressed.

Example:

```text id="w2n7s4"
Original:
Vulnerable component version detected

Remediation:
Component upgraded

Retest:
Updated version detected
Finding no longer reported

Additional Verification:
Version confirmed independently
```

This provides stronger evidence than simply observing that a finding disappeared.

---

# 19. Partially Remediated

Some but not all affected conditions were addressed.

Example:

```text id="g6q3m8"
Original:
10 affected hosts

Retest:
8 hosts remediated
2 still affected
```

The correct conclusion is not:

> "The vulnerability is fixed."

Instead:

```text id="r4x9c2"
Status:
Partially Remediated

Remaining:
2 affected hosts
```

---

# 20. Still Present

The finding remains under comparable conditions.

Example:

```text id="s8m2v6"
Original:
Finding present

Retest:
Finding present

Evidence:
Same affected component remains vulnerable
```

Document:

* remediation attempted
* observed state
* remaining exposure
* next action

---

# 21. Condition Changed

Sometimes the original condition no longer exists, but not because the intended remediation was applied.

For example:

```text id="t3k7q9"
Original:
Vulnerable service exposed

Retest:
Service removed
```

This may legitimately reduce exposure.

But document that the service was removed rather than claiming the original software vulnerability was patched.

---

# 22. Unable to Verify

Use this when the available evidence is insufficient.

Example:

```text id="m7q2x5"
Retest:
Host unreachable

Status:
Unable to Verify
```

Do not convert:

```text id="9c5v3k"
No Result
```

into:

```text id="1h7m4q"
Remediated
```

---

# 23. Reappeared Finding

A previously remediated condition may return.

Example:

```text id="p5x8n2"
Assessment 1:
Finding Present

Assessment 2:
Not Detected

Assessment 3:
Finding Present
```

Possible causes:

* regression
* rollback
* reinstallation
* configuration drift
* newly deployed vulnerable software
* asset replacement
* assessment changes

Investigate before closing the issue permanently.

---

# 24. Validation vs Retest

These concepts are related but distinct.

### Validation

Determines whether the original finding is accurate.

### Retest

Determines whether a remediation changed the condition.

Conceptually:

```text id="j4n8s6"
Finding
  ↓
Validation
  ↓
Confirmed
  ↓
Remediation
  ↓
Retest
```

You may not need separate validation and retest activities in every case, but understand the difference.

---

# 25. Retest Evidence

A strong retest record should contain:

```text id="v6m3q8"
Original Finding:
Original Asset:
Original Evidence:

Remediation:
Date:
Owner:

Retest Date:
Retest Scope:
Retest Configuration:
Authentication:
Scanner Perspective:

Retest Evidence:

Comparison:

Final Status:

Remaining Risk:

Next Action:
```

This makes closure defensible.

---

# 26. Comparison Is Critical

Do not review only the current assessment.

Compare:

```text id="x7q4m2"
Before
  ↓
Change
  ↓
After
```

Ask:

* Did the same host remain in scope?
* Was the same service assessed?
* Did authentication succeed?
* Was the same perspective used?
* Did the relevant finding disappear?
* Did related findings remain?
* Did another finding appear?
* Did the underlying root cause change?

---

# 27. Secondary Effects

A remediation can introduce other changes.

Examples:

```text id="n2v6c9"
Upgrade Component
       ↓
Original Vulnerability Resolved
       +
Configuration Changed
       +
New Compatibility Issue
```

or:

```text id="y5m8q3"
Disable Service
       ↓
Original Exposure Removed
       +
Application Functionality Affected
```

The retest should consider the intended outcome and relevant side effects.

---

# 28. Remediation Can Change the Attack Surface

Example:

```text id="f3q8m1"
Before:
HTTP + HTTPS

After:
HTTPS only
```

This can be a meaningful security improvement.

But confirm:

* HTTP was intentionally removed
* HTTPS remains functional
* the vulnerable condition is no longer reachable
* no alternate vulnerable exposure was introduced

Do not judge the change solely from one missing finding.

---

# 29. Fleet Remediation

Large environments require population-level verification.

Suppose:

```text id="c7x4p9"
100 servers affected
```

The organization applies the update.

A retest of:

```text id="h2m6w8"
5 servers
```

does not prove all 100 were remediated.

Use appropriate fleet evidence and assessment coverage.

A practical model:

```text id="r8v3q2"
Affected Population
       ↓
Remediation Coverage
       ↓
Retest Coverage
       ↓
Remaining Population
```

---

# 30. Partial Remediation Tracking

Example:

| Host   | Original | Retest      | Status           |
| ------ | -------- | ----------- | ---------------- |
| Host A | Present  | Absent      | Remediated       |
| Host B | Present  | Absent      | Remediated       |
| Host C | Present  | Present     | Still Present    |
| Host D | Present  | Unreachable | Unable to Verify |

The overall issue should not be marked completely closed while unresolved affected systems remain.

---

# 31. Retest Timing

Retest timing should account for:

* remediation completion
* service restart
* deployment propagation
* configuration reload
* reboot requirements
* replication
* change-management windows
* cloud/image deployment timing

A retest performed too early may produce misleading results.

For example:

```text id="s4n7x2"
Patch Applied
   ↓
Restart Required
   ↓
Retest Before Restart
   ↓
Old State Still Running
```

The assessment may correctly observe the old condition.

---

# 32. Remediation Verification Beyond Nessus

Nessus is useful evidence, but it is not always the only evidence needed.

Depending on the finding, verification may include:

* package version
* application version
* configuration state
* service state
* network exposure
* vendor remediation evidence
* change-management record

Use the least-impact verification method that answers the question.

---

# 33. When Nessus No Longer Reports the Finding

A missing finding is useful evidence.

But ask:

```text id="q8m4z1"
Why did it disappear?
```

Possible explanations:

* remediation
* host removed
* service removed
* authentication failure
* target unreachable
* configuration change
* plugin/content change
* finding no longer applicable

Only conclude remediation when the evidence supports that explanation.

---

# 34. Remediation Exceptions

Sometimes remediation cannot occur within the expected timeframe.

An exception should document:

* affected asset
* finding
* reason remediation is delayed
* compensating controls
* residual exposure
* approval
* expiration/review date
* permanent remediation plan

Example:

```text id="k3x7m9"
Exception:
Patch deferred due to application compatibility testing.

Compensating Control:
Service restricted to approved management network.

Review Date:
Documented organizational review date.

Permanent Action:
Upgrade during approved maintenance window.
```

An exception is not the same as remediation.

---

# 35. Residual Risk

After remediation or mitigation, some exposure may remain.

For example:

```text id="m8q5v2"
Original:
Vulnerable service externally reachable

Change:
External access restricted

Result:
External exposure reduced

Remaining:
Service remains vulnerable internally
```

The original exposure changed, but the underlying vulnerability may remain.

Report both facts.

---

# 36. Closing a Finding

A finding should be closed according to defined evidence and organizational process.

A strong closure record can state:

```text id="v4n7c8"
Finding:
Outdated Component

Original State:
Affected version detected on Host A.

Remediation:
Component upgraded.

Retest:
Comparable authenticated assessment performed.

Verification:
Updated component version confirmed.

Result:
Finding no longer detected under comparable conditions.

Status:
Remediated.
```

---

# 37. When Not to Close

Do not close a finding simply because:

* a ticket was marked complete
* an administrator reported a fix
* the host disappeared
* the scan returned no findings after authentication failed
* the target became unreachable
* the service was temporarily stopped
* the finding was not reproduced under different conditions

These may be useful observations, but they do not necessarily establish remediation.

---

# 38. Reopen Conditions

A closed finding may need to be reopened if:

* the vulnerability returns
* the remediation was incomplete
* the affected service is redeployed
* the vulnerable component is reintroduced
* configuration drifts
* the same root cause affects additional assets

Tracking should preserve the history.

---

# 39. Practical Lab 1 — Simple Remediation and Retest

## Objective

Practice the complete lifecycle.

Workflow:

```text id="x5c8m2"
Initial Assessment
      ↓
Finding
      ↓
Controlled Remediation
      ↓
Retest
      ↓
Comparison
      ↓
Verification
```

Record:

```text id="q7n3v6"
Finding:
Original Evidence:
Root Cause:
Remediation:
Expected Result:
Retest:
Observed Result:
Verification:
Final Status:
```

Use only an authorized lab target.

---

# 40. Practical Lab 2 — Partial Remediation

## Scenario

Three authorized lab hosts have the same vulnerable component.

After remediation:

```text id="b4m8x1"
Host A → Updated
Host B → Updated
Host C → Still Vulnerable
```

## Task

Determine:

* overall remediation status
* remaining affected asset
* retest evidence
* next action

Do not close the finding globally.

---

# 41. Practical Lab 3 — Authentication Failure During Retest

## Scenario

Original assessment:

```text id="j2q7m5"
Authenticated
Finding Present
```

Retest:

```text id="c8v4n1"
Authentication Failed
Finding Not Reported
```

## Task

Determine whether remediation has been verif
