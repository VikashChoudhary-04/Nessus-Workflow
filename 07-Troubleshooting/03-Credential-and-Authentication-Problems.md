# Credential and Authentication Problems

## Objective

Learn how to troubleshoot Nessus assessments where credentials are configured but authentication does not work as expected.

By the end of this workflow, you should be able to determine whether an authentication problem is caused by:

* Incorrect credentials
* Incorrect authentication method
* Insufficient permissions
* Target-side configuration
* Network or service reachability
* Account restrictions
* Credential policy or security controls
* Nessus configuration
* Partial authentication across a target set
* An authentication method that is valid in theory but unavailable in the actual environment

The goal is not simply to make authentication succeed.

The goal is to determine:

> **What authentication capability was expected, what actually happened, what evidence proves it, and how does that affect assessment coverage?**

---

## 1. Authentication Troubleshooting Mental Model

Authentication problems should be investigated as a layered problem.

Use this model:

```text
AUTHORIZED TARGET
       ↓
TARGET REACHABLE
       ↓
REQUIRED SERVICE REACHABLE
       ↓
AUTHENTICATION METHOD AVAILABLE
       ↓
ACCOUNT ACCEPTED
       ↓
AUTHENTICATION SUCCESSFUL
       ↓
REQUIRED PERMISSIONS AVAILABLE
       ↓
NESSUS CAN USE AUTHENTICATED EVIDENCE
       ↓
EXPECTED COVERAGE ACHIEVED
```

A failure at one layer can make the next layer impossible to evaluate.

For example:

```text
Target unreachable
        ↓
Authentication cannot be tested
```

or:

```text
Target reachable
        ↓
SSH reachable
        ↓
Credentials rejected
        ↓
Authenticated assessment unavailable
```

or:

```text
Credentials accepted
        ↓
Account has insufficient privileges
        ↓
Some authenticated checks unavailable
```

These are different problems and require different responses.

---

# 2. First Question: What Exactly Failed?

Do not begin by changing credentials.

First define the failure.

Possible observations include:

| Observation                                         | What it may mean                                                 |
| --------------------------------------------------- | ---------------------------------------------------------------- |
| Target cannot be reached                            | Network or target problem                                        |
| Required port unavailable                           | Service, firewall, routing, or target problem                    |
| Authentication method unavailable                   | Target-side or configuration problem                             |
| Credentials rejected                                | Account or credential problem                                    |
| Authentication succeeds but coverage is limited     | Permission or configuration problem                              |
| Some hosts authenticate and others do not           | Host-specific or environmental difference                        |
| Scan completes but authenticated checks are missing | Authentication may not have been successful or usable            |
| Authentication works manually but not in Nessus     | Nessus configuration, method, service, or target-side difference |
| Results differ from unauthenticated scan            | May be expected due to additional visibility                     |

Start with the actual observation.

---

# 3. Authentication Troubleshooting Workflow

Use this workflow:

```text
Confirm Authorization
↓
Confirm Target
↓
Confirm Scanner Position
↓
Check Target Reachability
↓
Check Required Authentication Service
↓
Confirm Authentication Method
↓
Confirm Account
↓
Confirm Credentials
↓
Check Account Restrictions
↓
Check Required Permissions
↓
Check Target-Side Configuration
↓
Check Nessus Credential Configuration
↓
Check Authentication Evidence
↓
Assess Coverage
↓
Retest
↓
Document
```

Do not skip directly to the credential field.

---

# 4. Step 1 — Confirm Authorization

Before troubleshooting authentication, confirm that the target and account are authorized for the assessment.

Verify:

* Target is inside the approved scope.
* Authentication is permitted.
* The account is authorized for security assessment use.
* The assessment window is valid.
* Testing will not violate account-use restrictions.
* Any required production safeguards remain in place.

Authentication credentials can provide significant access.

Treat them as sensitive security material.

---

# 5. Step 2 — Confirm the Target

Verify that Nessus is attempting authentication against the intended system.

Check:

* IP address
* Hostname
* FQDN
* Target list
* Target range
* DNS resolution
* NAT or address translation
* Load balancer behavior
* Dynamic addressing
* Duplicate or stale hostnames

A common mistake is troubleshooting credentials when Nessus is actually connecting to the wrong host.

### Example

You intended:

```text
server01.example.local
```

but DNS resolves it to:

```text
10.10.20.41
```

while the expected system is currently:

```text
10.10.20.57
```

The authentication failure may not be a credential problem at all.

Verify target identity first.

---

# 6. Step 3 — Confirm Scanner Position

The scanner's network location affects authentication.

Ask:

> **From where is Nessus connecting to the target?**

Consider:

* Internal vs external scanner
* VPN connectivity
* Routing
* Firewall rules
* ACLs
* Network segmentation
* Security groups
* Source IP restrictions
* Jump hosts or intermediaries
* NAT

A credential that works from an administrator's workstation may fail from the Nessus scanner because the scanner originates from a different network location.

Therefore:

```text
Works from workstation
        ≠
Works from Nessus scanner
```

Do not treat those as equivalent tests.

---

# 7. Step 4 — Check Target Reachability

Before investigating authentication, establish that the required path exists.

Use the network troubleshooting workflow from:

`07-Troubleshooting/02-Network-and-Target-Problems.md`

Determine:

```text
Scanner
   ↓
Target
   ↓
Required Service
```

For example:

```text
Scanner → Target IP → TCP 22 → SSH
```

or:

```text
Scanner → Target IP → Required Windows management/authentication service
```

The exact services and ports depend on the authentication method and target environment.

### Important distinction

```text
Host reachable
```

does not necessarily mean:

```text
Authentication service reachable
```

Similarly:

```text
Authentication service reachable
```

does not mean:

```text
Credentials accepted
```

---

# 8. Step 5 — Confirm the Authentication Method

Determine exactly which authentication mechanism you intended to use.

Examples include:

* SSH-based authentication
* Windows-based authentication
* Database authentication
* Other supported credential mechanisms

The exact methods available depend on:

* Nessus version
* Nessus edition
* Target operating system
* Target service
* Credential type
* Environment configuration

Do not assume that a method available in one Nessus environment is available in another.

---

# 9. Authentication Method Decision

Use this reasoning model:

```text
What target am I assessing?
        ↓
What authenticated evidence do I need?
        ↓
Which authentication method can provide it?
        ↓
Is that method supported?
        ↓
Is the target configured for it?
        ↓
Is the service reachable?
        ↓
Can the account authenticate?
```

This prevents a common mistake:

> Selecting a credential type simply because it sounds appropriate.

The authentication method must match the target and the evidence required.

---

# 10. Step 6 — Confirm the Account

Verify the intended account.

Check:

* Username
* Domain or authentication context where applicable
* Local vs centralized account
* Account status
* Account expiration
* Account lockout state
* Required group membership
* Required permissions
* Login restrictions
* Source restrictions
* Authentication policy

Do not assume:

```text
Correct username
+
Correct password
=
Successful Nessus authentication
```

Account policy can prevent authentication even when the credentials themselves are correct.

---

# 11. Step 7 — Confirm Credentials

Now verify the actual credentials.

Possible problems include:

* Incorrect password
* Expired password
* Rotated password
* Typographical error
* Wrong account
* Incorrect domain/context
* Incorrect key
* Incorrect credential format
* Credential stored incorrectly in Nessus

Never place real credentials in:

* GitHub repositories
* Markdown files
* Screenshots
* Public issue trackers
* Lab notes that may be published
* Shell history unnecessarily
* Chat messages that become part of documentation

Use placeholders in documentation:

```text
USERNAME=<authorized-assessment-account>
PASSWORD=<stored-securely>
```

Never:

```text
USERNAME=admin
PASSWORD=MyRealPassword123!
```

---

# 12. Step 8 — Check Account Restrictions

A valid account can still fail because of account policy.

Investigate whether the account is affected by:

* Login restrictions
* Source-IP restrictions
* Time-based restrictions
* Account expiration
* Account lockout
* Password expiration
* Interactive-login requirements
* Remote-login restrictions
* MFA requirements
* Security policies
* Conditional access controls
* Network restrictions

The exact controls depend on the target platform.

The important question is:

> **Can this account perform the required authentication from the Nessus scanner's network position using the intended authentication method?**

---

# 13. Step 9 — Check Required Permissions

Authentication and authorization are different.

An account may successfully authenticate but lack the permissions required for the intended assessment.

Use:

```text
Authentication
    ↓
Who are you?
    ↓
Authorization
    ↓
What are you allowed to access?
```

Example:

```text
SSH login succeeds
        ↓
Account has limited privileges
        ↓
Some local checks unavailable
        ↓
Authenticated coverage is incomplete
```

Do not report:

> "Authenticated scan succeeded."

if only basic login succeeded but the required assessment privileges were unavailable.

A more accurate statement may be:

> Authentication succeeded, but the account did not provide the permissions required for complete authenticated coverage.

---

# 14. Step 10 — Check Target-Side Configuration

The target itself may prevent authenticated assessment.

Possible causes include:

* Required service disabled
* Remote management disabled
* SSH configuration restrictions
* Authentication method disabled
* Firewall restrictions
* Host-based access controls
* Security policy restrictions
* Account restrictions
* Service binding limitations
* Network segmentation
* Security software blocking or limiting access

Do not immediately disable security controls.

Instead determine:

1. Which control is preventing the connection?
2. Is changing it authorized?
3. Is the change necessary?
4. Can the assessment be performed another way?
5. What impact would the change have?
6. How will the original state be restored?

---

# 15. Step 11 — Check Nessus Credential Configuration

Once target-side conditions are understood, review the Nessus configuration.

Verify:

* Correct credential category
* Correct authentication method
* Correct username
* Correct authentication context
* Correct key or secret
* Correct target scope
* Appropriate privilege expectations
* Relevant credential options
* Any required supporting settings

The exact UI and fields vary by Nessus version and edition.

Do not rely on screenshots from another version.

Use the capability and field names actually available in your installation.

---

# 16. Step 12 — Look for Authentication Evidence

Do not assume authentication succeeded because:

* The scan completed.
* Findings appeared.
* The credential was saved.
* The scan took a long time.
* The target was reachable.

Look for evidence that authenticated checks actually worked.

Depending on the assessment and Nessus version, evidence may include:

* Authentication status
* Plugin output
* Host-level authentication information
* Local configuration evidence
* Installed software/package information
* Patch information
* Registry or system information
* Other authenticated data

The exact evidence available varies by workflow and target.

The question is:

> **What evidence demonstrates that Nessus obtained the intended authenticated visibility?**

---

# 17. Authentication Success vs Assessment Coverage

These are separate questions.

### Question 1

```text
Did authentication succeed?
```

### Question 2

```text
Did authentication provide the required assessment coverage?
```

A successful login does not automatically answer Question 2.

Use:

```text
Authentication Success
        ↓
Required Permissions?
        ↓
Required Evidence Available?
        ↓
Expected Checks Executed?
        ↓
Coverage Sufficient?
```

This distinction is critical in professional vulnerability assessment.

---

# 18. Partial Authentication

A scan can contain mixed authentication results.

Example:

```text
Target Group
├── Server A → authenticated
├── Server B → authenticated
├── Server C → authentication failed
├── Server D → authenticated
└── Server E → insufficient privileges
```

Do not summarize this as:

> "Authenticated scan completed successfully."

That hides important coverage differences.

Instead separate the populations.

Example:

| Target   | Authentication | Coverage                |
| -------- | -------------- | ----------------------- |
| Server A | Successful     | Expected                |
| Server B | Successful     | Expected                |
| Server C | Failed         | Unauthenticated/limited |
| Server D | Successful     | Expected                |
| Server E | Successful     | Limited by permissions  |

The exact status categories should reflect the evidence available from the assessment.

---

# 19. Authentication Troubleshooting Matrix

Use this matrix as a starting point.

| Observation                                      | Investigate                                           |
| ------------------------------------------------ | ----------------------------------------------------- |
| Target unreachable                               | Network path                                          |
| Required port closed/filtered                    | Firewall/service/routing                              |
| Service unavailable                              | Target configuration                                  |
| Authentication method rejected                   | Method compatibility/configuration                    |
| Username rejected                                | Account/context                                       |
| Password rejected                                | Credential validity                                   |
| Account locked                                   | Account policy                                        |
| Authentication works manually but not in Nessus  | Scanner position/configuration/method                 |
| Authentication succeeds but local checks missing | Permissions/configuration                             |
| Some hosts work and others fail                  | Host-specific configuration/account policy            |
| Results are limited                              | Authentication evidence and privileges                |
| Authentication worked previously but stopped     | Password rotation/account changes/configuration drift |
| Scheduled scans fail but manual scans work       | Credential lifecycle/scheduling/environment changes   |

This is a troubleshooting aid, not a substitute for evidence.

---

# 20. Authentication Failure Decision Tree

Use this decision tree:

```text
Authentication expected?
        │
        ├── No
        │    ↓
        │  Investigate why authenticated
        │  workflow was selected/configured
        │
        └── Yes
             ↓
        Target reachable?
             │
        ├── No → Network/target troubleshooting
        │
        └── Yes
             ↓
        Required service reachable?
             │
        ├── No → Service/firewall/routing investigation
        │
        └── Yes
             ↓
        Authentication method supported?
             │
        ├── No → Select supported method
        │
        └── Yes
             ↓
        Account accepted?
             │
        ├── No → Account/credential/policy investigation
        │
        └── Yes
             ↓
        Required permissions available?
             │
        ├── No → Permission/authorization investigation
        │
        └── Yes
             ↓
        Authenticated evidence available?
             │
        ├── No → Nessus/configuration/target investigation
        │
        └── Yes
             ↓
        Expected coverage achieved?
             │
        ├── No → Investigate coverage limitations
        │
        └── Yes
             ↓
        Authenticated assessment usable
```

---

# 21. Do Not Blindly Increase Privileges

A common troubleshooting mistake is:

```text
Authentication failed
        ↓
Give account administrator/root privileges
```

This can create unnecessary security risk.

Instead determine what capability is actually required.

Use:

```text
Required Evidence
       ↓
Required Permission
       ↓
Least-Privileged Account
       ↓
Verify Coverage
```

If elevated privileges are genuinely required and authorized, document why.

Do not assume maximum privilege is automatically necessary.

---

# 22. Do Not Blindly Change Multiple Variables

Suppose authentication fails.

Avoid changing all of these simultaneously:

```text
Username
Password
Authentication method
Permissions
Firewall
Target configuration
Nessus settings
```

If authentication then succeeds, you do not know what fixed it.

Instead:

```text
Observe
↓
Form hypothesis
↓
Change one relevant variable
↓
Retest
↓
Observe
↓
Confirm or reject hypothesis
```

This produces useful troubleshooting evidence.

---

# 23. Manual Authentication vs Nessus Authentication

A useful diagnostic comparison is:

```text
Manual connection
        vs
Nessus connection
```

For example:

```text
Administrator workstation
        ↓
Target
        ↓
SSH login succeeds
```

but:

```text
Nessus scanner
        ↓
Target
        ↓
SSH authentication fails
```

This does not prove that Nessus is broken.

Differences may include:

* Source IP
* Routing
* DNS
* Authentication method
* SSH configuration
* Security policy
* Account restrictions
* Key format
* Credential configuration
* Target-side access controls

The comparison should identify what differs between the two paths.

---

# 24. Authentication Problems in Recurring Assessments

Recurring scans introduce an additional problem:

> **Authentication state can change after the original assessment was configured.**

Possible changes include:

* Password rotation
* Account expiration
* Account removal
* Group membership changes
* Permission changes
* MFA changes
* Security policy changes
* SSH configuration changes
* Windows policy changes
* Network segmentation changes
* Credential replacement
* Target replacement

Therefore:

```text
Recurring Scan
      ↓
Credential Health
      ↓
Authentication Evidence
      ↓
Coverage Verification
```

A recurring scan should not be considered healthy simply because it continues to execute.

---

# 25. Scheduled Authentication Failure Example

Suppose:

```text
Monday:
Authenticated scan works.

Tuesday:
Password rotates.

Wednesday:
Scheduled scan completes.

Results:
Fewer authenticated findings.
```

Do not immediately conclude:

> "The environment improved."

Possible explanation:

```text
Credential changed
↓
Authentication failed
↓
Authenticated visibility decreased
↓
Finding set changed
```

Compare authentication evidence before interpreting the finding change.

This is why authentication status is part of result comparison.

---

# 26. Practical Lab 1 — Deliberate Credential Failure

## Objective

Learn to distinguish credential failure from network failure.

## Environment

Use an authorized lab target.

## Procedure

1. Confirm the target is reachable.
2. Confirm the intended authentication service is reachable.
3. Configure an authorized authentication workflow.
4. Deliberately use an invalid lab credential.
5. Run the assessment.
6. Observe authentication-related evidence.
7. Do not repeatedly guess credentials.
8. Correct the credential.
9. Retest.
10. Compare the two results.

## Record

```text
Target:
Authentication method:
Expected account:
Initial credential state:
Target reachable:
Required service reachable:
Authentication result:
Evidence:
Corrective change:
Retest result:
Coverage difference:
```

## Success Condition

You can explain:

> "The target and service were reachable. The authentication attempt failed because the supplied credential was invalid. After correcting the credential, authenticated evidence became available."

---

# 27. Practical Lab 2 — Authentication vs Permission

## Objective

Understand why successful authentication does not guarantee complete authenticated coverage.

## Procedure

1. Use an authorized lab target.
2. Create or use an assessment account with intentionally limited permissions.
3. Configure the corresponding Nessus authentication method.
4. Run the assessment.
5. Determine whether authentication succeeds.
6. Identify what authenticated evidence is available.
7. Identify what expected evidence is missing.
8. Determine whether the limitation is caused by permissions.
9. If authorized, adjust permissions.
10. Retest.
11. Compare coverage.

## Record

| Question                          | Result |
| --------------------------------- | ------ |
| Target reachable?                 |        |
| Authentication successful?        |        |
| Required permissions available?   |        |
| Authenticated evidence available? |        |
| Expected checks available?        |        |
| Coverage complete?                |        |
| Corrective action?                |        |
| Retest result?                    |        |

## Success Condition

You can explain the difference between:

```text
Authentication Success
```

and:

```text
Assessment Coverage
```

---

# 28. Practical Lab 3 — Partial Authentication

## Objective

Learn to recognize mixed authentication results across multiple targets.

## Environment

Use several authorized lab systems where authentication conditions differ.

Example:

```text
Host A → valid credentials
Host B → invalid account
Host C → valid account but insufficient permissions
Host D → service unavailable
```

## Procedure

1. Define the authorized target set.
2. Configure the intended authentication workflow.
3. Run the assessment.
4. Separate targets by authentication outcome.
5. Review evidence for each group.
6. Identify coverage limitations.
7. Troubleshoot one failed category at a time.
8. Retest.
9. Compare the populations.

## Success Condition

You can produce a coverage statement such as:

```text
Authenticated:
A

Authentication failed:
B

Authenticated but insufficient privileges:
C

Authentication not testable because service was unavailable:
D
```

The exact wording should match the evidence you actually observed.

---

# 29. Practical Lab 4 — Manual Login Works, Nessus Fails

## Objective

Determine why an account works manually but does not authenticate through Nessus.

## Procedure

1. Confirm the account is authorized.
2. Confirm manual authentication from the administrative workstation.
3. Record the source network.
4. Identify the Nessus scanner network position.
5. Compare source paths.
6. Confirm target reachability from the scanner.
7. Confirm the required authentication service.
8. Compare authentication method and credential configuration.
9. Review target-side restrictions.
10. Retest from Nessus.
11. Document the actual cause.

## Do Not Assume

```text
Manual login works
        ↓
Nessus must be broken
```

Instead ask:

> **What differs between the two authentication paths?**

---

# 30. Practical Lab 5 — Recurring Credential Failure

## Objective

Understand how credential lifecycle changes can alter recurring scan results.

## Scenario

A recurring authenticated assessment previously produced strong authenticated coverage.

A later run produces substantially less authenticated evidence.

## Procedure

Investigate:

1. Authentication status.
2. Credential age.
3. Account status.
4. Password rotation.
5. Permission changes.
6. Target configuration changes.
7. Network changes.
8. Nessus configuration changes.
9. Plugin/content changes.
10. Scanner changes.

Determine whether the reduced finding set is caused by:

```text
Actual remediation
```

or:

```text
Reduced assessment visibility
```

Do not treat the finding count alone as the answer.

---

# 31. Authentication Troubleshooting Evidence

When investigating authentication, collect evidence appropriate to the problem.

Useful evidence may include:

### Nessus-side

* Assessment configuration
* Credential configuration category
* Authentication status/evidence
* Assessment logs where available
* Plugin output
* Scan history
* Result differences

### Target-side

* Authentication logs
* Service status
* Account status
* Permission/group information
* Security policy
* Firewall rules
* Relevant configuration

### Network-side

* DNS resolution
* Routing
* Required port reachability
* Source IP
* Firewall/ACL behavior
* VPN state

Do not collect unnecessary sensitive information.

---

# 32. Troubleshooting Without Exposing Secrets

A professional troubleshooting record should never require storing the actual password or private key.

Use:

```text
Account:
assessment-user

Credential:
Configured / rotated / expired / invalid

Secret value:
Not recorded

Authentication method:
[method]

Result:
Failed

Evidence:
[non-secret evidence]
```

For a private key:

```text
Key:
Configured

Key material:
Not recorded

Result:
Authentication failed

Next investigation:
Key validity / format / target authorization
```

The objective is to document the state, not the secret.

---

# 33. Common Authentication Troubleshooting Mistakes

## Mistake 1 — Re-entering credentials repeatedly

Why it fails:

You are not determining the actual failure layer.

Better:

```text
Reachability
→ Service
→ Method
→ Account
→ Credential
→ Permission
→ Evidence
```

---

## Mistake 2 — Giving maximum privileges immediately

Why it fails:

It introduces unnecessary risk and hides the real permission requirement.

Better:

> Determine the minimum required privilege for the intended assessment.

---

## Mistake 3 — Assuming scan completion means authentication succeeded

Why it fails:

A scan can complete without obtaining the intended authenticated visibility.

Better:

> Verify authentication evidence and coverage.

---

## Mistake 4 — Treating fewer findings as improvement

Why it fails:

Authentication failure can reduce visibility.

Better:

> Compare authentication state before interpreting result changes.

---

## Mistake 5 — Ignoring scanner location

Why it fails:

The Nessus scanner may have a different network path than an administrator's workstation.

Better:

> Troubleshoot from the scanner's actual network position.

---

## Mistake 6 — Changing firewall rules immediately

Why it fails:

You may introduce unnecessary exposure.

Better:

> Identify the exact blocked path and determine whether the change is authorized and necessary.

---

## Mistake 7 — Guessing credentials

Why it fails:

It can trigger lockouts and violates responsible assessment practices.

Better:

> Use authorized assessment credentials and controlled testing.

---

## Mistake 8 — Ignoring partial authentication

Why it fails:

A multi-host assessment can contain different authentication states.

Better:

> Evaluate coverage per host or logical target group.

---

## Mistake 9 — Treating authentication as binary

Why it fails:

Authentication can succeed while required privileges or evidence remain unavailable.

Better:

```text
Authentication
+
Permissions
+
Evidence
+
Coverage
```

---

# 34. Professional Authentication Troubleshooting Record

For each significant authentication problem, record:

```text
Assessment:
Date:
Operator:
Target:
Authorization:
Scanner:
Scanner network position:
Objective:
Authentication method:
Expected account:
Target service:
Observed problem:

Network reachability:
Service reachability:
Authentication result:
Permission result:
Authenticated evidence:
Coverage impact:

Evidence collected:
Hypothesis:
Change made:
Retest:
Retest result:
Remaining limitation:

Final assessment impact:
Next action:
```

Do not record secrets.

---

# 35. Authentication Coverage Statement

When authentication is incomplete, describe the limitation precisely.

Avoid:

> "The scan was authenticated."

if only some targets authenticated.

Avoid:

> "Credentials failed."

if the account authenticated but lacked required privileges.

Prefer statements such as:

> Authentication succeeded for the assessed account, but some hosts did not provide the expected authenticated evidence.

Or:

> The assessment reached the target service, but the supplied account did not provide the permissions required for the intended authenticated checks.

Or:

> Authentication could not be established from the scanner network position, so the assessment should be interpreted as having limited authenticated coverage.

The statement must match the evidence.

---

# 36. Decision Rule

When authentication fails, ask these questions in order:

```text
1. Is the target authorized?
2. Is the intended target correct?
3. Is the scanner in the expected network position?
4. Is the target reachable?
5. Is the required authentication service reachable?
6. Is the authentication method appropriate?
7. Is the account valid and permitted?
8. Are the credentials valid?
9. Are account restrictions blocking access?
10. Are sufficient permissions available?
11. Is the target configured for the required method?
12. Is Nessus configured correctly?
13. Is authenticated evidence actually available?
14. Does the resulting coverage answer the assessment question?
```

If you cannot answer one of these questions, investigate that layer before moving deeper.

---

# 37. Authentication Troubleshooting Checklist

## Authorization

* [ ] Target is authorized
* [ ] Credential use is authorized
* [ ] Assessment window is valid

## Target

* [ ] Correct target identified
* [ ] DNS verified
* [ ] Target address verified
* [ ] Scope confirmed
* [ ] Scanner position known

## Network

* [ ] Target reachable
* [ ] Required service reachable
* [ ] Routing verified
* [ ] Relevant firewall/ACL behavior understood

## Authentication Method

* [ ] Correct method selected
* [ ] Method supported in actual environment
* [ ] Target supports required method
* [ ] Nessus configuration matches intended method

## Account

* [ ] Correct account
* [ ] Account active
* [ ] Account permitted for remote/authenticated use
* [ ] Account restrictions understood
* [ ] Credentials current

## Permissions

* [ ] Required privileges identified
* [ ] Least privilege considered
* [ ] Permissions verified
* [ ] Expected authenticated checks identified

## Evidence

* [ ] Authentication result reviewed
* [ ] Authenticated evidence reviewed
* [ ] Missing evidence investigated
* [ ] Coverage assessed per target

## Retest

* [ ] One meaningful change made at a time
* [ ] Retest performed
* [ ] Before/after evidence compared
* [ ] Remaining limitations documented

## Security

* [ ] Secrets not recorded
* [ ] Credentials not committed to GitHub
* [ ] Security controls not disabled unnecessarily
* [ ] Account lockout risk avoided
* [ ] Assessment impact considered

---

# 38. Completion Criteria

You have completed this workflow when you can independently:

* Identify the exact authentication failure layer.
* Distinguish network failure from authentication failure.
* Distinguish authentication from authorization.
* Determine whether the intended authentication method is appropriate.
* Verify target-side authentication requirements.
* Troubleshoot account restrictions.
* Troubleshoot credential problems without blind guessing.
* Recognize insufficient permissions.
* Identify partial authentication.
* Determine whether authenticated evidence is actually available.
* Explain how authentication affects assessment coverage.
* Troubleshoot differences between manual authentication and Nessus authentication.
* Investigate recurring credential failures.
* Document authentication problems without exposing secrets.
* Decide whether to fix, retest, accept a limitation, or change the assessment approach.

The final skill is not:

> "I know how to enter Nessus credentials."

It is:

> **"I can determine whether Nessus obtained the authenticated visibility required to answer the assessment question, identify why it did not when it failed, and document the resulting coverage accurately."**

---

# Final Mental Model

Keep this model in mind:

```text
AUTHORIZATION
      ↓
TARGET IDENTITY
      ↓
SCANNER POSITION
      ↓
NETWORK REACHABILITY
      ↓
AUTHENTICATION SERVICE
      ↓
AUTHENTICATION METHOD
      ↓
ACCOUNT
      ↓
CREDENTIAL
      ↓
ACCOUNT RESTRICTIONS
      ↓
PERMISSIONS
      ↓
TARGET CONFIGURATION
      ↓
NESSUS CONFIGURATION
      ↓
AUTHENTICATION EVIDENCE
      ↓
ASSESSMENT COVERAGE
      ↓
RETEST
      ↓
DOCUMENT
```

When authentication fails, do not ask only:

> "What credential should I enter?"

Ask:

> **"At which layer did the expected authentication path break, what evidence proves it, and what does that mean for assessment coverage?"**
