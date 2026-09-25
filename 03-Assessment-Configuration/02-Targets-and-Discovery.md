# Targets and Discovery

## Objective

Learn how to define Nessus targets correctly and understand how discovery affects what is actually assessed.

By the end of this file, you should be able to:

* Translate an authorized scope into appropriate Nessus targets.
* Distinguish authorization scope from technical target syntax.
* Choose between hosts, ranges, subnets, and other supported target forms.
* Understand the difference between target definition and host discovery.
* Avoid accidentally expanding an assessment beyond its intended scope.
* Determine whether discovered hosts are expected.
* Recognize when discovery results should change the next assessment step.
* Handle exclusions deliberately.
* Understand why target accuracy is more important than target quantity.

---

# 1. Target Selection Comes Before Scanning

A Nessus assessment cannot be better than the scope you give it.

The basic chain is:

```text
AUTHORIZATION
     ↓
SCOPE
     ↓
TARGET DEFINITION
     ↓
DISCOVERY
     ↓
ASSESSMENT
```

A technical target such as:

```text
192.168.56.20
```

does not automatically prove that you are authorized to assess it.

Authorization comes first.

Target syntax comes second.

---

# 2. Authorization Scope vs Technical Scope

These are related but different.

## Authorization Scope

Defines what you are allowed to assess.

Examples:

```text
192.168.56.0/24
```

or:

```text
192.168.56.20
192.168.56.21
```

or a documented hostname list.

## Technical Target

Defines what you actually enter into Nessus.

For example:

```text
192.168.56.20
```

The important rule is:

> **Never use technical convenience to expand beyond authorization.**

If your authorization covers three hosts, entering the entire subnet because it is easier is not acceptable.

---

# 3. The Target Mental Model

Think of the target as a boundary:

```text id="2j7a2u"
AUTHORIZED ENVIRONMENT
┌─────────────────────────────┐
│                             │
│   Target A       Target B   │
│                             │
│             Target C        │
│                             │
└─────────────────────────────┘
```

Nessus should operate inside that boundary.

A good target definition should be:

* Authorized.
* Intentional.
* Understandable.
* Technically valid.
* Appropriate for the workflow.

---

# 4. Common Target Forms

Depending on the Nessus version and workflow, targets can commonly be represented using forms such as:

* Individual IP addresses.
* Hostnames.
* IP ranges.
* CIDR network ranges.
* Lists of targets.
* Other target formats supported by the installed version.

Examples:

```text id="f6pk2e"
192.168.56.20
```

```text id="2m8e8a"
server01.lab.local
```

```text id="7n6jyn"
192.168.56.20-192.168.56.30
```

```text id="g9m7j3"
192.168.56.0/24
```

Use the smallest target representation that accurately expresses the authorized assessment.

---

# 5. Individual Host

An individual host is useful when the assessment concerns one specific system.

Example:

```text id="q4a9dp"
192.168.56.20
```

Advantages:

* Easy to verify.
* Easy to document.
* Low risk of accidental scope expansion.
* Useful for learning and focused assessments.

For your early Nessus labs, individual hosts are often the easiest way to understand what Nessus is doing.

---

# 6. Multiple Hosts

If several specific systems are authorized, a target list may be appropriate.

Conceptually:

```text id="2c4z4x"
192.168.56.20
192.168.56.21
192.168.56.25
```

The exact input format depends on the Nessus interface and supported syntax.

Before launching, verify that every target belongs to the authorized scope.

---

# 7. IP Range

An IP range can represent a contiguous group of addresses.

Example:

```text id="w3jv6c"
192.168.56.20-192.168.56.30
```

This is useful when the authorized scope explicitly covers that range.

But ask:

> "Do I actually need to assess every address in this range?"

If only two systems matter, two explicit hosts may be clearer and safer.

---

# 8. CIDR Network

A CIDR target can represent an entire network.

Example:

```text id="z3cbp6"
192.168.56.0/24
```

This represents a broad scope.

Use it only when:

* The entire range is authorized.
* The assessment objective requires it.
* The expected target count is understood.
* Operational impact is acceptable.

Do not use a large subnet simply because it is convenient.

---

# 9. Target Minimization

A useful professional principle is:

> **Use the smallest target definition that fully satisfies the assessment objective.**

Suppose your authorization says:

```text
192.168.56.0/24
```

but the request says:

> "Assess the web server at 192.168.56.20."

There are two technically possible choices:

```text
192.168.56.0/24
```

or:

```text
192.168.56.20
```

The second is more directly aligned with the stated objective.

The authorization allows it, but the objective determines what should actually be assessed.

---

# 10. Discovery vs Target Definition

These concepts are easy to confuse.

## Target Definition

Answers:

> "Where should Nessus operate?"

## Discovery

Answers:

> "What is actually present or reachable within that defined area?"

For example:

```text id="4r5fef"
Target:
192.168.56.0/24

        ↓

Discovery

        ↓

192.168.56.10  → reachable
192.168.56.20  → reachable
192.168.56.35  → reachable
```

Discovery does not automatically change authorization.

It provides information about the defined environment.

---

# 11. Why Discovery Matters

Suppose you are authorized to assess:

```text
192.168.56.0/24
```

Discovery identifies:

```text
192.168.56.10
192.168.56.20
192.168.56.35
```

You now know something about the environment that was not obvious from the original request.

This can help answer:

* Which hosts are alive?
* Which systems should be assessed next?
* Which expected hosts are missing?
* Are unexpected systems present?
* Is the scope larger or smaller than expected?
* Should the assessment be narrowed or expanded within the authorized boundary?

---

# 12. Discovery Does Not Grant Authorization

This distinction is critical.

Suppose your scanner discovers:

```text
192.168.56.50
```

but your authorization covers only:

```text
192.168.56.20
192.168.56.21
```

The discovery result does not authorize you to assess `192.168.56.50`.

The correct action is:

```text id="l5a1v0"
Discovered
     ↓
Outside authorized scope?
     ↓
YES
     ↓
Do not assess it
     ↓
Document / escalate as appropriate
```

Discovery is information.

Authorization determines permission.

---

# 13. Expected vs Unexpected Hosts

Discovery becomes particularly useful when you have an expected asset list.

Example:

```text id="a1p8c3"
Expected:
192.168.56.20
192.168.56.21
192.168.56.22
```

Discovery returns:

```text
192.168.56.20
192.168.56.21
192.168.56.30
```

Now there are two observations:

```text
192.168.56.22 → expected but not observed
192.168.56.30 → observed but not expected
```

Do not immediately conclude that either is an error.

Investigate.

Possible explanations include:

* Host offline.
* Network filtering.
* Incorrect inventory.
* DHCP changes.
* Different network segment.
* Scanner visibility limitation.
* Unexpected asset.
* Scope documentation problem.

---

# 14. Discovery as an Information-Gathering Step

Use discovery to answer:

```text id="b0d9xv"
What exists?
What responds?
What is visible?
What should I assess next?
```

Then use vulnerability assessment to answer:

```text id="g0t4cx"
What security weaknesses can be identified?
```

This separation makes your workflow easier to understand and troubleshoot.

---

# 15. Target Validation Before Launch

Before launching a scan, verify the target independently where practical.

For an authorized lab target, confirm:

* Correct IP address.
* Correct hostname.
* Correct system.
* Expected network.
* Expected services.
* Expected operating state.

The purpose is not to perform an entire pentest before using Nessus.

The purpose is to prevent an obvious targeting mistake.

---

# 16. Target Validation Questions

Ask:

```text id="8q4u1h"
Is this the system I intended to assess?
        ↓
Is it inside the authorized scope?
        ↓
Is it reachable?
        ↓
Is it the correct environment?
        ↓
Does the target match the assessment request?
```

If any answer is uncertain, resolve the uncertainty before launching.

---

# 17. DNS and Hostnames

Hostnames can be convenient:

```text id="q1by74"
server01.lab.local
```

But hostnames can introduce ambiguity.

For example:

```text id="c5m4l7"
server01.lab.local
       ↓
DNS resolution
       ↓
192.168.56.20
```

Before assessing a hostname, understand what it resolves to.

A hostname can change its underlying IP address.

Therefore, for important assessments, document both:

```text
Hostname:
Resolved IP:
```

where appropriate.

---

# 18. Dynamic Environments

Modern environments can change frequently.

Examples:

* DHCP.
* Cloud instances.
* Containers.
* Auto-scaling.
* Virtual machines.
* Load balancers.
* Dynamic DNS.

A target identified yesterday may not represent the same system today.

Therefore:

> **Target identity should be verified at assessment time when the environment is dynamic.**

Do not blindly reuse old target assumptions.

---

# 19. Exclusions

Exclusions are useful when an authorized assessment covers a broad scope but certain systems must not be assessed.

Conceptually:

```text id="j1v9b6"
Authorized Scope
      ↓
Remove Explicit Exclusions
      ↓
Assessment Targets
```

Examples might include:

* Critical infrastructure.
* Sensitive systems.
* Systems with separate authorization.
* Temporary exclusions.
* Systems under maintenance.

The exact exclusion mechanism depends on the Nessus workflow and version.

---

# 20. Exclusions Must Be Documented

Do not rely on memory.

Record:

```text id="w0n1ft"
Authorized Scope:
Exclusions:
Reason:
Who/what defined the exclusion:
Date:
```

This matters because an exclusion can explain why an expected host has no results.

---

# 21. Inclusion vs Exclusion Logic

Consider:

```text id="b0b8cz"
Authorized:
192.168.56.0/24

Excluded:
192.168.56.50
```

The intended assessment is conceptually:

```text id="i0j9q2"
192.168.56.0/24
       MINUS
192.168.56.50
```

Before launch, verify that the resulting scope matches the intended assessment.

---

# 22. Discovery Can Reveal Scope Problems

Suppose your authorized scope is:

```text
192.168.56.20
192.168.56.21
```

You discover:

```text
192.168.56.20
192.168.56.21
192.168.56.22
```

Do not automatically add `.22`.

Instead:

```text id="r7k6dd"
Unexpected Host
      ↓
Is it authorized?
      │
 ┌────┴────┐
 NO       YES
 │          │
Do not     Confirm
assess     intended scope
```

If authorization is unclear, stop and resolve it.

---

# 23. Discovery and Vulnerability Assessment Can Be Separate

A useful operational pattern is:

```text id="n4k2tz"
Phase 1
Discovery
   ↓
Build/verify asset picture
   ↓
Phase 2
Vulnerability Assessment
   ↓
Analyze findings
```

This can be useful when the asset inventory is uncertain.

However, not every assessment needs a separate discovery phase.

If the target is already known and verified, a separate discovery exercise may add little value.

---

# 24. Avoid Redundant Discovery

Do not create unnecessary assessments merely because discovery sounds useful.

Ask:

> "What uncertainty am I trying to remove?"

If the answer is:

> "I already know the target, its address, its ownership, and its intended assessment scope."

then a separate discovery exercise may not be necessary.

The workflow should remain proportional to the problem.

---

# 25. Discovery Results and Next Decisions

After discovery, classify results.

```text id="7z0bwr"
Discovered Host
      ↓
Is it expected?
      │
 ┌────┴────┐
 YES       NO
 │          │
Continue   Investigate
 │          │
 ↓          ↓
Assess     Authorization
as planned  / inventory
```

For expected hosts:

* Continue with the intended workflow.

For unexpected hosts:

* Do not automatically expand scope.
* Determine whether they are authorized.
* Document the observation.

---

# 26. Target Coverage

Target coverage answers:

> "Did the assessment actually reach the systems I intended to assess?"

For example:

```text id="x8zvhm"
Intended:
A, B, C, D

Reached:
A, B, D

Not reached:
C
```

The assessment has incomplete target coverage.

This does not necessarily mean the entire assessment failed.

It means the result must be interpreted with that limitation.

---

# 27. Partial Target Coverage

When coverage is incomplete:

```text id="u9xg6c"
Identify Missing Target
       ↓
Determine Why
       ↓
Assess Impact
       ↓
Correct if Necessary
       ↓
Rerun Missing Scope
```

Possible causes:

* Host offline.
* Firewall.
* Routing problem.
* DNS problem.
* Wrong target.
* Temporary outage.
* Authentication issue.
* Scanner limitation.

---

# 28. Target Expansion

Target expansion should be deliberate.

Bad workflow:

```text id="k5r1jv"
Found another host
      ↓
Add it immediately
      ↓
Scan
```

Better workflow:

```text id="f1l3b4"
Found another host
      ↓
Check authorization
      ↓
Check assessment objective
      ↓
Confirm target identity
      ↓
Update scope if authorized
      ↓
Assess
```

---

# 29. Target Reduction

The same principle applies when the scope is too broad.

Suppose the assessment request is:

> "Assess the three application servers."

But the entered target is:

```text
192.168.56.0/24
```

Reduce the target to the intended systems if the broader range is unnecessary.

Target reduction can:

* Reduce noise.
* Reduce assessment time.
* Reduce operational impact.
* Simplify analysis.
* Make reporting clearer.

---

# 30. Target Definition Record

For each important assessment, record:

```text id="2v4zv7"
Assessment:
Objective:
Authorized Scope:
Actual Targets:
Excluded Targets:
Target Type:
Hostname(s):
IP Address(es):
Discovery Performed:
Expected Hosts:
Observed Hosts:
Unexpected Hosts:
Missing Hosts:
Target Coverage:
```

This record makes later troubleshooting much easier.

---

# 31. Practical Exercise 1 — Single Host

Use your authorized Nessus lab.

Choose one known lab VM.

Record:

```text
Hostname:
IP:
Authorized:
Expected Services:
```

Create a Nessus assessment against only that host.

Before launching, verify:

```text id="f3r0da"
[ ] Correct host
[ ] Correct IP
[ ] Authorized
[ ] Correct workflow
[ ] No accidental additional targets
```

Launch the assessment.

Compare the actual result with your expectations.

---

# 32. Practical Exercise 2 — Small Range

In an isolated authorized lab, use a small range containing multiple systems.

For example:

```text
192.168.56.20-192.168.56.25
```

Determine:

1. Which systems are reachable?
2. Which systems are expected?
3. Which systems are not responding?
4. Which systems should be assessed?
5. Are all systems inside authorization?

Record:

```text
Target Range:
Expected Hosts:
Discovered Hosts:
Missing Hosts:
Unexpected Hosts:
Assessment Decision:
```

---

# 33. Practical Exercise 3 — Discovery Before Assessment

Perform a discovery-oriented assessment against an authorized lab range.

Then classify each result:

```text
Host
 ├── Expected + Reachable
 ├── Expected + Unreachable
 ├── Unexpected + Authorized
 └── Unexpected + Unauthorized/Unknown
```

For each category, decide the next action.

Do not assess an unauthorized or unknown system simply because Nessus discovered it.

---

# 34. Practical Exercise 4 — Hostname Verification

Choose an authorized lab hostname.

Determine:

```text
Hostname:
Current IP:
```

Then verify that the resolved system is the intended target.

Record:

```text
Hostname Resolution:
Expected System:
Actual System:
Assessment Target:
```

The objective is to learn that a hostname is an identifier, not automatically a permanent identity.

---

# 35. Practical Exercise 5 — Scope Mismatch

Create this scenario in your lab:

```text
Authorized:
Host A
Host B

Entered:
Host A
Host B
Host C
```

Before launching, identify the problem.

Correct the target list.

Then record:

```text
Original Target:
Problem:
Corrected Target:
Reason:
```

This is a simple exercise, but it develops an important professional habit:

> **Review the actual target list instead of trusting your intention.**

---

# 36. Practical Exercise 6 — Missing Host

Imagine the authorized scope is:

```text
Host A
Host B
Host C
```

The scan reaches:

```text
Host A
Host B
```

Host C is unreachable.

Determine:

1. What could cause the problem?
2. What evidence should you collect?
3. Can you conclude that Host C has no vulnerabilities?
4. What should happen before the assessment is considered complete?

Correct reasoning:

You cannot treat an unreachable host as assessed.

---

# 37. Common Mistakes

## Mistake 1 — Scanning the Whole Subnet for Convenience

Bad:

> "The target is somewhere in this subnet, so I'll scan everything."

Better:

> "I will define the smallest authorized target set that answers the assessment question."

---

## Mistake 2 — Treating Discovery as Authorization

Bad:

> "Nessus found it, so I can scan it."

Better:

> "Nessus found it; now I must determine whether it is authorized and relevant."

---

## Mistake 3 — Trusting Old IP Addresses

Bad:

> "This was the server's IP last week."

Better:

> "I will verify the target identity for the current assessment."

---

## Mistake 4 — Ignoring Missing Hosts

Bad:

> "The scan finished, so all targets were assessed."

Better:

> "I will verify target coverage before interpreting the results."

---

## Mistake 5 — Overusing Exclusions

Bad:

> "I'll exclude anything that looks difficult."

Better:

> "Every exclusion must have a legitimate reason and be documented."

---

# 38. Professional Decision Rule

Before launching, complete this sentence:

> **"I am assessing these targets because they are __________, they are authorized under __________, and they are required to answer __________."**

Example:

> "I am assessing this server because it is the authorized application host, it is included in the approved lab scope, and it is required to answer the vulnerability assessment objective."

This forces you to connect:

```text
Authorization
+
Target
+
Objective
```

---

# 39. Target Selection Checklist

```text id="h0g3u4"
[ ] Authorization verified
[ ] Assessment objective defined
[ ] Target identity verified
[ ] Correct target syntax selected
[ ] Target scope minimized appropriately
[ ] Exclusions documented
[ ] Discovery requirement determined
[ ] Expected hosts known where possible
[ ] Unexpected hosts handled safely
[ ] Missing hosts identified
[ ] Target coverage understood
[ ] Operational impact considered
```

---

# 40. Completion Criteria

You have completed this file when you can independently:

* Translate an authorized scope into Nessus targets.
* Explain the difference between authorization and technical targeting.
* Use individual hosts, ranges, and network scopes appropriately.
* Explain target definition versus discovery.
* Determine whether a discovered host should be assessed.
* Handle unexpected hosts without automatically expanding scope.
* Identify missing targets after an assessment.
* Explain partial target coverage.
* Use exclusions deliberately and document them.
* Verify hostname/IP relationships when necessary.
* Reduce unnecessarily broad target definitions.
* Explain why the chosen target set matches the assessment objective.

The final test is:

> **Given an authorized assessment scope and a Nessus target list, can you determine whether the target definition is correct, whether discovery is necessary, whether any discovered systems require further authorization review, and whether the final results actually cover the intended targets?**

If yes, you are ready to move from choosing the workflow to configuring how that workflow should actually execute.
