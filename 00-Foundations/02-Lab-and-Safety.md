# Lab and Safety

## Objective

Establish a safe, repeatable environment for learning Nessus.

Before scanning anything, you must be able to answer:

```text
What am I allowed to scan?
What is in scope?
What is the target?
Could the scan affect the target?
What should I do if the scan behaves unexpectedly?
```

The professional habit is:

> **Authorization and scope come before scanning.**

---

## 1. Authorization Comes First

Only scan systems that are:

* owned by you
* explicitly authorized for assessment
* intentionally vulnerable laboratory systems

Do not treat technical accessibility as authorization.

This is unsafe reasoning:

```text
I can reach it
    ↓
I can scan it
```

Use:

```text
Authorization
    ↓
Defined Scope
    ↓
Target Validation
    ↓
Assessment
```

If authorization is unclear:

```text
STOP
↓
Clarify authorization and scope
↓
Continue only when authorized
```

---

# 2. Recommended Learning Environment

The safest environment for learning Nessus is an isolated laboratory.

A practical lab can contain:

```text
Nessus Scanner
      │
      ├──────── Linux Target
      │
      ├──────── Windows Target
      │
      └──────── Vulnerable Application
```

The targets should be systems you control or have explicit permission to assess.

Examples of suitable controlled environments include:

* virtual machines
* intentionally vulnerable applications
* local test servers
* isolated networks
* authorized cloud test instances

The repository does not require a specific virtualization platform.

Use whatever environment you can safely reset and reproduce.

---

# 3. Keep the Lab Isolated Where Practical

A laboratory should minimize the chance that a scan reaches unintended systems.

A simple conceptual layout is:

```text
Host Machine
     │
     └── Lab Network
           │
           ├── Nessus
           ├── Linux Target
           ├── Windows Target
           └── Vulnerable Application
```

Before scanning, confirm:

```text
Nessus → Target
```

is possible.

Also confirm that unintended networks are not included in the target scope.

---

# 4. Scope

Scope defines what the assessment is allowed to include.

For example:

```text
Authorized scope:

192.168.56.10
192.168.56.20
192.168.56.30
```

or:

```text
192.168.56.0/24
```

Do not confuse:

```text
Network range
```

with:

```text
Authorized assessment scope
```

A technically valid IP range can still contain systems that should not be scanned.

---

# 5. Validate Targets Before Scanning

Before creating a scan, confirm:

```text
1. Is the target authorized?
2. Is the target correctly identified?
3. Is the target inside the intended scope?
4. Is the target reachable from the scanner?
5. Is the target the system I actually intend to assess?
```

Do not immediately launch a vulnerability scan just because you were given an IP address.

First establish what that address represents.

---

# 6. Scan Impact

Nessus performs active assessment activities.

Depending on the workflow and configuration, scanning can generate:

* network traffic
* connection attempts
* service requests
* authentication attempts
* increased resource usage
* application activity

Most laboratory targets can tolerate this well.

Production systems may not.

Therefore:

> **The correct scan configuration depends partly on the target's sensitivity and the assessment objective.**

---

# 7. Never Assume "More Aggressive" Means "Better"

A common beginner mistake is changing settings to make a scan appear more powerful.

For example:

```text
More checks
+
More concurrency
+
More aggressive behavior
=
Better scan
```

This is not necessarily true.

A better model is:

```text
Assessment Objective
        +
Target Characteristics
        +
Required Coverage
        +
Acceptable Impact
        ↓
Appropriate Configuration
```

The objective is **sufficient coverage with appropriate impact**, not maximum aggressiveness.

---

# 8. Production vs Laboratory Targets

### Laboratory

You normally have more freedom to:

* test different configurations
* intentionally create failures
* repeat scans
* modify targets
* reset systems
* compare results

### Production

You must consider:

* authorization
* business impact
* maintenance windows
* fragile systems
* sensitive services
* authentication risks
* resource consumption
* change-control requirements
* monitoring/incident-response implications

The repository primarily uses controlled laboratory environments for learning.

Professional scenarios will introduce production-style constraints without requiring you to actually scan production systems.

---

# 9. Credentials Require Additional Care

Credentialed scanning introduces another class of risk.

Before using credentials, confirm:

```text
Is the account authorized for this assessment?
Is the authentication method correct?
Does the account have the required permissions?
Could repeated failures cause account lockout?
Is the account appropriate for the target?
```

Never experiment with credentials against systems you are not authorized to assess.

For laboratory work, use dedicated test accounts where practical.

---

# 10. Authentication Failure Is Not a Reason to Guess

Suppose you configure an authenticated assessment and authentication fails.

Do not respond by repeatedly trying random credentials.

Use:

```text
Authentication Failure
        ↓
Check Configuration
        ↓
Check Target
        ↓
Check Credential Type
        ↓
Check Permissions
        ↓
Check Target-Side Configuration
        ↓
Verify
        ↓
Retest
```

This reduces unnecessary authentication attempts and teaches the correct diagnostic process.

---

# 11. Define a Scan Objective

Before every practical assessment, write one sentence:

```text
Objective:
Determine whether __________________________.
```

Examples:

```text
Determine whether the lab Linux host has detectable vulnerabilities.
```

```text
Identify reachable systems in the authorized laboratory subnet.
```

```text
Assess the authorized Windows host using valid credentials.
```

```text
Verify whether a previously reported vulnerability remains present.
```

This prevents configuration from becoming random.

---

# 12. Define the Scope

For each lab exercise, record:

```text
Scope:
____________________________
```

Example:

```text
Scope:
192.168.56.10
```

Or:

```text
Scope:
192.168.56.0/24
```

Then explicitly identify exclusions if necessary:

```text
Excluded:
192.168.56.1
192.168.56.254
```

The actual values depend on your laboratory.

---

# 13. Record the Assessment Conditions

Before launching an important scan, record:

```text
Assessment Objective:
Target:
Scope:
Authentication:
Expected Access:
Scan Type / Workflow:
Important Configuration Changes:
Expected Result:
Potential Impact:
```

You do not need to document every Nessus setting.

Record the settings that materially affect the assessment.

---

# 14. Safe Failure Practice

Professional users need to understand failure conditions.

The laboratory should eventually allow you to safely practice scenarios such as:

```text
Target unreachable
       ↓
Diagnose

Credential incorrect
       ↓
Diagnose

Insufficient privileges
       ↓
Diagnose

Expected service unavailable
       ↓
Diagnose

Unexpectedly incomplete results
       ↓
Investigate
```

Only introduce these failures in systems you control.

Do not intentionally disrupt systems outside your laboratory.

---

# 15. Resettable Lab

A useful Nessus learning environment should be easy to restore.

Prefer targets that can be:

```text
Snapshot
   ↓
Modify
   ↓
Scan
   ↓
Break / Change
   ↓
Investigate
   ↓
Reset
```

This allows repeated experimentation without permanently damaging the environment.

---

# 16. Baseline Before Experimentation

Before changing a target, establish a known baseline.

For example:

```text
Baseline Target
      ↓
Initial Assessment
      ↓
Record Results
      ↓
Make Controlled Change
      ↓
Rescan
      ↓
Compare
```

This is particularly useful when learning:

* configuration changes
* remediation
* authentication
* plugin behavior
* scan configuration
* result comparison

---

# 17. Scope Verification Checklist

Before launching a scan, verify:

```text
[ ] Authorization is established
[ ] Assessment objective is defined
[ ] Scope is documented
[ ] Targets are identified
[ ] Exclusions are known
[ ] Target is reachable
[ ] Correct scanner is being used
[ ] Credentials are authorized
[ ] Scan configuration matches the objective
[ ] Potential scan impact is understood
```

Do not proceed merely because the checklist looks familiar.

Actually verify the relevant items.

---

# 18. Stop Conditions

A professional operator should know when **not** to continue.

Stop and investigate if:

```text
Unexpected target appears
```

or:

```text
The target is outside the authorized scope
```

or:

```text
The scan is causing unexpected impact
```

or:

```text
Authentication behavior is unexpected
```

or:

```text
The assessment objective is no longer clear
```

or:

```text
The scanner is behaving unexpectedly
```

The correct response is:

```text
STOP
 ↓
Understand the condition
 ↓
Determine whether the assessment can safely continue
 ↓
Resume only when appropriate
```

---

# 19. Practical Exercise — Build Your Lab Definition

Before moving to Nessus installation, define your intended learning environment.

Create a small record containing:

```text
Lab Name:
____________________________

Nessus Host:
____________________________

Linux Target:
____________________________

Windows Target:
____________________________

Vulnerable Application:
____________________________

Authorized Network:
____________________________

Excluded Systems:
____________________________
```

If you do not have all of these targets yet, write:

```text
Not configured yet
```

Do not invent systems that do not exist.

---

# 20. Practical Exercise — Scope Decision

Consider:

```text
Authorized:
192.168.56.10
192.168.56.20
192.168.56.30

Not authorized:
192.168.56.1
```

You are given:

```text
192.168.56.0/24
```

Question:

Should you scan the entire `/24`?

### Correct reasoning

No.

The network range is larger than the explicitly authorized scope.

The assessment should remain limited to the authorized targets unless authorization is expanded.

The important lesson is:

> **A network range is a technical boundary; authorization defines the assessment boundary.**

---

# 21. Practical Exercise — Scan Impact

Situation:

```text
You have a fragile laboratory application.
Your objective is to determine whether a known vulnerability
is detectable.
```

You discover a configuration that can increase scan intensity.

Before changing it, ask:

```text
What does this change accomplish?
Is it necessary for the objective?
Could it increase target impact?
Do I have a reason to change it?
```

If the default workflow can answer the question, leave the setting unchanged.

---

# 22. Practical Exercise — Authentication

Situation:

```text
You have an authorized Linux laboratory host.

Objective:
Perform an authenticated vulnerability assessment.

You have:
- a test account
- the correct authentication method
- authorized access
```

Before launching the scan, determine:

```text
What credentials will Nessus use?
Does the account have the required permissions?
How will I determine whether authentication succeeded?
What will I investigate if authentication fails?
```

Do not treat:

```text
Credential entered
```

as equivalent to:

```text
Authentication successful
```

The assessment must provide evidence that the intended authentication condition was achieved.

---

# 23. Practical Exercise — Unexpected Result

Situation:

```text
You scan an authorized lab host.

Expected:
Authenticated assessment results.

Observed:
Only limited remote findings appear.
```

Do not immediately conclude:

> "The host has no additional vulnerabilities."

Use:

```text
Observed Result
      ↓
Compare With Expected Result
      ↓
Did authentication succeed?
      ↓
Did the scan use the intended configuration?
      ↓
Was the target reachable as expected?
      ↓
Were required services accessible?
      ↓
Were relevant checks available?
      ↓
Investigate
      ↓
Retest
```

The objective is to determine whether the result represents:

```text
Actual absence of a finding
```

or:

```text
Insufficient assessment coverage
```

---

# 24. Practical Exercise — Stop or Continue?

For each situation, decide whether you should continue immediately or stop and investigate.

### Situation A

The target is inside the authorized scope and behaves normally.

```text
Decision: __________________
```

### Situation B

The scan unexpectedly reaches a system that was not part of the intended scope.

```text
Decision: __________________
```

### Situation C

Credential authentication repeatedly fails.

```text
Decision: __________________
```

### Situation D

The scan causes unexpected behavior on a fragile laboratory application.

```text
Decision: __________________
```

### Situation E

The scan completes normally and the results match the expected assessment conditions.

```text
Decision: __________________
```

The goal is not to memorize "stop" rules.

The goal is to recognize when the assessment conditions no longer match the plan.

---

# 25. Assessment Record

For each substantial lab exercise, maintain a lightweight record:

```text
Assessment Objective:
________________________________

Authorization:
________________________________

Scope:
________________________________

Targets:
________________________________

Exclusions:
________________________________

Authentication:
________________________________

Workflow:
________________________________

Important Configuration:
________________________________

Expected Result:
________________________________

Actual Result:
________________________________

Unexpected Conditions:
________________________________

Findings:
________________________________

Validation:
________________________________

Next Action:
________________________________
```

This habit will become useful later when producing professional vulnerability-assessment documentation.

---

# 26. Completion Criteria

Before continuing to the next module, you should be able to:

* explain why authorization comes before scanning
* distinguish technical reachability from authorized scope
* define a Nessus assessment objective
* define assessment scope
* identify targets and exclusions
* recognize potential scan impact
* explain why more aggressive settings are not automatically better
* handle credentials responsibly
* recognize authentication failure as an assessment condition
* define when to stop a scan
* establish a reproducible laboratory workflow
* record important assessment conditions
* reason about unexpected results without blindly rerunning scans

Most importantly, you should naturally think:

```text
Authorization
    ↓
Scope
    ↓
Objective
    ↓
Target
    ↓
Appropriate Workflow
    ↓
Safe Configuration
    ↓
Assessment
```

Only after these conditions are established should you move into actual Nessus installation and operation.
