# Credentials

## Objective

Learn how credentials change a Nessus assessment, how to configure authenticated assessment safely, and how to determine whether authentication actually succeeded.

By the end of this file, you should be able to:

* Explain why credentials can increase assessment visibility.
* Distinguish authenticated and unauthenticated assessment objectives.
* Select an appropriate credential method for the target.
* Understand credential scope and least privilege.
* Configure credentials without exposing secrets unnecessarily.
* Verify whether authentication actually worked.
* Troubleshoot authentication failures systematically.
* Recognize partial authentication.
* Understand how authentication affects result interpretation.
* Decide when an authenticated assessment should be rerun.

---

# 1. Credentials Change the Assessment Perspective

Credentials are not simply another scan option.

They can fundamentally change what Nessus is able to observe.

Without credentials:

```text id="6y5j0k"
Nessus
   ↓
Network Visibility
   ↓
Externally Observable Information
```

With authorized credentials:

```text id="w2n9q4"
Nessus
   ↓
Network Visibility
   +
Authenticated Access
   ↓
Additional Host-Level Information
```

Therefore:

> **Adding credentials can change both assessment coverage and the meaning of the results.**

---

# 2. Authentication Is a Capability, Not a Goal

Do not begin with:

> "How do I make Nessus authenticate?"

Begin with:

> **"Does the assessment objective require authenticated visibility?"**

Examples:

### Objective

> "What can an unauthenticated network observer identify?"

Credentials may be inappropriate because they change the perspective.

### Objective

> "Which host-level vulnerabilities can be identified with authorized system access?"

Credentials may be essential to the assessment.

The objective determines whether authentication belongs in the workflow.

---

# 3. Authentication Mental Model

Use:

```text id="d9h1ps"
ASSESSMENT OBJECTIVE
       ↓
DOES AUTHENTICATION ADD REQUIRED VISIBILITY?
       ↓
     YES
       ↓
SELECT AUTH METHOD
       ↓
SELECT AUTHORIZED ACCOUNT
       ↓
CONFIGURE
       ↓
VERIFY
       ↓
ASSESS
       ↓
INTERPRET RESULTS IN AUTHENTICATED CONTEXT
```

Authentication should be deliberate from beginning to end.

---

# 4. Why Authenticated Assessment Can Find More

Some security information is difficult or impossible to determine reliably from the network alone.

Host-level access can potentially provide information such as:

* Installed software.
* Package information.
* Local configuration.
* Patch state.
* Registry/configuration information on supported systems.
* Local security settings.
* Service configuration.
* Other host-level evidence supported by Nessus.

The exact checks depend on:

* Operating system.
* Authentication method.
* Account privileges.
* Nessus version.
* Plugin support.
* Target configuration.

Do not assume every authenticated scan provides identical visibility.

---

# 5. Authenticated Does Not Mean Unlimited

A credential can authenticate successfully while still providing insufficient permissions for some checks.

Think:

```text id="p5x9sa"
Credential
   ↓
Authentication succeeds
   ↓
What can the account actually access?
   ↓
Available evidence
```

Therefore:

```text
Authentication Success
        ≠
Complete Host Visibility
```

This distinction is extremely important.

---

# 6. Authentication Methods

Nessus supports different credential mechanisms depending on the target and installed product.

Common categories can include:

* Windows authentication.
* SSH-based authentication.
* Database authentication.
* Web/application-related authentication in supported workflows.
* Other technology-specific authentication mechanisms.

The exact available methods depend on your Nessus version, edition, plugins, and target technology.

Always select the method appropriate for the actual target.

---

# 7. Start With the Target

Before configuring credentials, identify:

```text id="0s6d3c"
Target OS / Technology
        ↓
Supported Authentication Method
        ↓
Authorized Account
        ↓
Required Permissions
```

Do not start by creating random credentials and testing them against the target.

---

# 8. Credential Scope

Credentials should be scoped appropriately.

Ask:

* Which targets require these credentials?
* Who owns the account?
* What permissions does it have?
* Is the account authorized for vulnerability assessment?
* Is the credential temporary or persistent?
* How will the credential be protected?
* When should it stop being used?

Use the least privilege that still provides the required assessment visibility where practical.

---

# 9. Least Privilege

The principle is:

> **Give Nessus enough access to answer the assessment question, but avoid unnecessary privileges.**

For example:

```text id="k8g3w1"
Assessment Requirement
        ↓
Required Evidence
        ↓
Minimum Practical Permission
        ↓
Credential
```

Do not automatically use a highly privileged account if a lower-privileged account can provide the necessary checks.

However, if specific assessment coverage genuinely requires elevated permissions, the account must have the required authorized access.

---

# 10. Lab vs Production Credentials

In a lab:

* You can deliberately create test accounts.
* Credentials can be rotated frequently.
* Systems can be reset.
* Failure has limited consequences.

In production:

* Account ownership matters.
* Credential handling is more sensitive.
* Password rotation matters.
* Access logging may matter.
* Privilege boundaries matter.
* Change-control requirements may apply.

Treat credentials as sensitive operational data in both environments.

---

# 11. Never Put Passwords in Documentation

Do not write:

```text id="m4s9y8"
Username: scanner
Password: MyPassword123
```

inside a GitHub repository.

Instead record:

```text id="v7q2ad"
Credential:
Authorized assessment account
Storage:
Nessus credential store
Owner:
[Team / role]
Purpose:
Authenticated vulnerability assessment
```

Never commit real passwords, private keys, API tokens, or other authentication secrets to the repository.

---

# 12. Credential Configuration

The exact Nessus UI varies.

The conceptual process is:

```text id="g1k4b8"
Choose Assessment
      ↓
Open Credential Configuration
      ↓
Choose Appropriate Authentication Type
      ↓
Enter Authorized Account Information
      ↓
Configure Required Options
      ↓
Save Securely
      ↓
Run Assessment
      ↓
Verify Authentication
```

Do not assume the exact field names from an older tutorial will match your version.

---

# 13. Credential Configuration Checklist

Before saving credentials:

```text id="w4p6c2"
[ ] Correct authentication method
[ ] Correct target
[ ] Correct account
[ ] Account authorized
[ ] Required permissions understood
[ ] Authentication prerequisites satisfied
[ ] No unnecessary privileges
[ ] Secret stored only in appropriate credential storage
```

---

# 14. Windows Authentication

For an authorized Windows assessment, authentication may provide additional host-level visibility.

Before configuring it, understand:

* Target Windows version.
* Intended account.
* Account privileges.
* Required Windows configuration.
* Network accessibility.
* Firewall behavior.
* Authentication method supported by your Nessus version.

Do not assume that a valid Windows username/password automatically guarantees authenticated assessment success.

---

# 15. SSH Authentication

For an authorized Linux/Unix assessment, SSH-based authentication is commonly relevant.

Conceptually:

```text id="d4s8k3"
Nessus
  ↓
SSH
  ↓
Authorized Account
  ↓
Host-Level Checks
```

Possible authentication mechanisms can include:

* Password-based authentication.
* SSH key-based authentication.
* Other supported mechanisms.

The exact options depend on your Nessus version and target environment.

---

# 16. SSH Key Authentication

When key-based authentication is used, think about:

```text id="j7n5c4"
Private Key
   ↓
Protected Credential Storage
   ↓
Authorized Target
```

Do not commit private keys to GitHub.

If a lab private key is accidentally exposed:

1. Treat it as compromised.
2. Remove its authorization from the target.
3. Generate a replacement if needed.
4. Update the Nessus credential.
5. Check whether the key was exposed elsewhere.

---

# 17. Database Credentials

Some database assessments may require database-specific credentials.

The reasoning remains the same:

```text id="m0g6fd"
Database Assessment Objective
        ↓
Required Database Visibility
        ↓
Supported Authentication Method
        ↓
Authorized Database Account
        ↓
Assessment
```

Do not use database administrator credentials simply because they are available unless the assessment actually requires that level of access.

---

# 18. Credential Preconditions

Authentication can fail before Nessus even gets a useful opportunity to perform host-level checks.

Potential prerequisites include:

* Network reachability.
* Correct service enabled.
* Correct port accessible.
* Valid account.
* Correct permissions.
* Appropriate authentication protocol.
* Firewall rules.
* SSH configuration.
* Windows policy/configuration.
* Database access rules.
* Target-side security controls.

Therefore:

> **Credential troubleshooting starts before the credential field.**

---

# 19. Authentication Verification

One of the most important habits:

> **Never assume authentication succeeded because the scan completed.**

After the assessment, look for evidence indicating whether authenticated checks actually worked.

Depending on the workflow and Nessus version, useful evidence may include:

* Authentication status.
* Credential-related messages.
* Host-level findings.
* Local checks.
* Plugin output.
* Scan warnings/errors.
* Evidence showing access to host-level information.

The exact indicators vary.

---

# 20. Authentication Success vs Scan Success

These are separate states.

```text id="8z4r2q"
Scan Completed
      ↓
Did Authentication Work?
      │
 ┌────┴────┐
 YES       NO
 │          │
Analyze    Analyze as
authenticated unauthenticated/
results     limited assessment
```

A completed scan with failed credentials may still produce useful network-level results.

But it may not answer the original authenticated assessment objective.

---

# 21. Partial Authentication

Authentication may not always be simply:

```text
SUCCESS
```

or:

```text
FAILURE
```

There can be partial coverage.

For example:

```text id="y1m7v5"
Host A → authenticated checks succeeded
Host B → authentication failed
Host C → target unreachable
```

The overall assessment therefore has different coverage levels.

Do not describe the entire assessment simply as "authenticated" without qualification.

---

# 22. Authentication Coverage Matrix

For multi-host assessments, create a matrix:

| Target | Reachable | Authentication | Host-Level Coverage |
| ------ | --------- | -------------- | ------------------- |
| Host A | Yes       | Success        | Available           |
| Host B | Yes       | Failed         | Limited             |
| Host C | No        | Unknown        | None                |

This makes the result interpretation much clearer.

---

# 23. Authentication Failure Troubleshooting

Use a structured process.

```text id="q5k2d1"
Authentication Failed
       ↓
Is Target Reachable?
       ↓
Is Authentication Service Accessible?
       ↓
Is Username Correct?
       ↓
Is Credential Correct?
       ↓
Does Account Have Required Access?
       ↓
Is Authentication Method Supported?
       ↓
Are Target Policies Blocking Access?
       ↓
Review Nessus Evidence
```

Do not immediately change several variables.

---

# 24. Step 1 — Verify Reachability

Before troubleshooting credentials:

```text id="h2f4r8"
Can Nessus reach the target?
```

If not:

```text
Credential troubleshooting is premature.
```

Fix network reachability first.

---

# 25. Step 2 — Verify the Authentication Service

Examples:

### SSH

Determine whether the intended SSH service is reachable.

### Windows

Determine whether the relevant Windows authentication mechanism is accessible from the scanner.

### Database

Determine whether the database service is reachable and accepting connections from the scanner environment.

The exact test depends on the environment.

---

# 26. Step 3 — Verify the Account

Confirm:

* Username.
* Domain/workgroup context where applicable.
* Account existence.
* Account status.
* Password/key validity.
* Required permissions.
* Authentication restrictions.

Avoid repeated login attempts if the environment has account lockout controls.

---

# 27. Step 4 — Verify Permissions

A credential can be valid but insufficient.

Ask:

> "What is this account actually allowed to read or execute?"

This determines what Nessus can observe.

For example:

```text id="h6p4q9"
Authentication
    ↓
Account Permissions
    ↓
Available Host Information
    ↓
Possible Plugin Coverage
```

---

# 28. Step 5 — Review Target-Side Controls

Potential blockers include:

* Firewall.
* SSH configuration.
* Windows security policies.
* Endpoint security controls.
* Network segmentation.
* Account restrictions.
* Login restrictions.
* Database access controls.

Do not immediately conclude:

> "Nessus credentials are broken."

The target environment may be preventing authentication.

---

# 29. Step 6 — Review Nessus Evidence

Look for:

* Authentication-related errors.
* Credential warnings.
* Host-level evidence.
* Plugin output.
* Scan messages.
* Relevant result details.

Use the evidence to determine exactly what failed.

---

# 30. Avoid Blind Credential Guessing

Do not repeatedly try credentials just to see whether one works.

Risks can include:

* Account lockout.
* Security alerts.
* Audit events.
* Operational disruption.
* Unnecessary traffic.
* Potential credential misuse.

Use credentials that are explicitly authorized for the assessment.

---

# 31. Credential Rotation

Credentials can expire or rotate.

A recurring assessment can therefore change from:

```text id="w3v6x2"
Authenticated
```

to:

```text id="r8m5j1"
Authentication Failed
```

without any change to Nessus itself.

When a previously successful authenticated scan suddenly loses host-level coverage, investigate credential validity and account status.

---

# 32. Authentication and Recurring Assessments

For recurring assessments, document:

```text id="t8q4x7"
Credential Owner:
Credential Purpose:
Credential Rotation Process:
Expiration:
Update Responsibility:
Last Verified:
```

Never store the actual secret in the repository.

---

# 33. Authentication and Result Interpretation

Suppose an unauthenticated assessment reports:

```text id="m5v7s2"
10 findings
```

Then an authenticated assessment reports:

```text id="c2x9n8"
25 findings
```

Do not conclude:

> "The authenticated scan found 15 extra vulnerabilities because it is better."

Instead:

```text id="7d2p0x"
Different Perspective
       ↓
Different Available Evidence
       ↓
Different Plugin Coverage
       ↓
Different Result Set
```

The assessments answer different questions.

---

# 34. Authenticated Assessment Is Not Proof of Completeness

Even with successful authentication:

* Some checks may not apply.
* Some permissions may be insufficient.
* Some services may be unreachable.
* Some plugins may not run.
* Some vulnerabilities may require separate validation.
* Environmental conditions can still limit coverage.

Therefore:

> **Authentication increases visibility; it does not eliminate assessment limitations.**

---

# 35. Credential Selection Decision Tree

Use:

```text id="k2m7q4"
Does the objective require host-level visibility?
        │
   ┌────┴────┐
  NO        YES
  │           │
No auth    Identify target
needed         ↓
          Select auth method
               ↓
          Authorized account?
               │
          ┌────┴────┐
         NO        YES
         │           │
       Stop      Verify permissions
                     ↓
                Configure
                     ↓
                  Scan
                     ↓
             Verify authentication
```

---

# 36. Practical Exercise 1 — Unauthenticated Baseline

Choose an authorized lab VM.

Run a vulnerability assessment without credentials.

Record:

```text id="m9h2r4"
Target:
Workflow:
Authentication:
Findings:
Host-Level Evidence:
Limitations:
```

This is your baseline.

---

# 37. Practical Exercise 2 — Authenticated Assessment

Use a deliberately created authorized lab account.

Configure the appropriate authentication method.

Before scanning, record:

```text id="x5d8m2"
Target:
Authentication Method:
Account Type:
Required Permissions:
Expected Additional Visibility:
```

Run the assessment.

Then record:

```text id="p4r6t8"
Authentication Result:
Additional Evidence:
Additional Findings:
Errors:
Coverage Limitations:
```

Never put the actual password or private key in your notes or GitHub repository.

---

# 38. Practical Exercise 3 — Compare Authentication States

Compare:

```text id="b7n1x5"
Run A:
Unauthenticated

Run B:
Authenticated
```

Create:

| Category              | Unauthenticated | Authenticated |
| --------------------- | --------------- | ------------- |
| Network visibility    |                 |               |
| Host-level evidence   |                 |               |
| Authentication status |                 |               |
| Findings              |                 |               |
| Coverage limitations  |                 |               |

Then explain why the results differ.

---

# 39. Practical Exercise 4 — Authentication Failure

In an isolated lab, deliberately create a safe authentication failure.

For example, use a test account with an intentionally invalid credential.

Do not repeatedly attempt login.

Observe the Nessus result.

Record:

```text id="u4q6w8"
Expected Authentication:
Actual Authentication:
Observed Error:
Target Reachability:
Likely Cause:
Corrective Action:
```

Then restore the correct credential and rerun the assessment.

---

# 40. Practical Exercise 5 — Partial Authentication

If your lab has multiple authorized targets:

* Configure authentication for one target correctly.
* Deliberately make authentication unavailable for another authorized target.
* Keep the targets otherwise reachable.

Run the assessment.

Determine:

```text id="d7f3m9"
Target A:
Authentication:
Coverage:

Target B:
Authentication:
Coverage:
```

The objective is to understand why a multi-target assessment can have mixed authentication coverage.

---

# 41. Common Mistakes

## Mistake 1 — Using the Highest-Privilege Account Automatically

Bad:

> "Administrator/root will find everything."

Better:

> "Use the minimum practical privilege required by the assessment objective and supported checks."

---

## Mistake 2 — Assuming Valid Credentials Guarantee Success

Bad:

> "The username and password work, so Nessus must be authenticated."

Better:

> "Verify authentication through assessment evidence."

---

## Mistake 3 — Storing Credentials in the Repository

Bad:

```text id="r1f5h7"
username/password/private key
```

Better:

```text id="w8c3k2"
Credential type:
Purpose:
Owner:
Storage:
```

---

## Mistake 4 — Repeatedly Testing Invalid Credentials

Bad:

> "I'll keep trying until it works."

Better:

> "Verify the authorized credential once and troubleshoot systematically."

---

## Mistake 5 — Calling a Scan Authenticated Without Evidence

Bad:

> "I configured credentials, therefore this was an authenticated scan."

Better:

> "I configured credentials and verified whether authenticated checks actually succeeded."

---

## Mistake 6 — Ignoring Partial Authentication

Bad:

> "The scan was authenticated."

Better:

> "Authentication succeeded for these targets and failed or was unavailable for these others."

---

# 42. Credential Troubleshooting Checklist

```text id="a8k6s3"
[ ] Target reachable
[ ] Correct authentication method
[ ] Correct username/account
[ ] Credential valid
[ ] Account enabled
[ ] Required permissions available
[ ] Authentication service reachable
[ ] Network controls checked
[ ] Target-side policy checked
[ ] Nessus authentication evidence reviewed
[ ] No repeated unsafe login attempts
[ ] Authentication result documented
```

---

# 43. Professional Credential Record

For an important assessment, document:

```text id="f4n8q1"
## Authentication

### Target
[Target]

### Authentication Method
[Method]

### Account Type
[Account type / privilege level]

### Purpose
[Why authentication is required]

### Required Visibility
[What the account needs to allow Nessus to assess]

### Credential Owner
[Team / role]

### Storage
[Nessus credential storage / approved secret management]

### Verification
[How authentication success was established]

### Coverage
[Which targets successfully authenticated]

### Limitations
[Known limitations]

### Last Verified
[Date]
```

Never include the actual secret.

---

# 44. Professional Decision Rule

Before using credentials, complete:

> **"I am using this credential because the assessment requires __________ visibility. The account provides __________ access. I will verify success by checking __________."**

Example:

> "I am using this credential because the assessment requires host-level patch and configuration visibility. The account provides the permissions required for the intended checks. I will verify success using the authentication evidence and host-level assessment results."

---

# 45. Completion Criteria

You have completed this file when you can independently:

* Explain why authentication changes an assessment.
* Decide whether credentials are appropriate for an objective.
* Identify an appropriate authentication method.
* Select an authorized account.
* Apply least-privilege reasoning.
* Configure credentials safely.
* Protect passwords and private keys.
* Verify authentication instead of assuming success.
* Troubleshoot authentication failures systematically.
* Recognize partial authentication.
* Interpret authenticated and unauthenticated results correctly.
* Handle credential rotation and recurring assessments.
* Document authentication without exposing secrets.

The final test is:

> **Given an authorized target and an assessment objective, can you determine whether authentication is required, select and configure an appropriate credential safely, verify that it actually worked, identify its coverage limitations, and explain how authentication affects the interpretation of the results?**

If yes, you are ready to move into **policies, templates, and plugin selection**, where you will learn how Nessus determines what checks it performs.
