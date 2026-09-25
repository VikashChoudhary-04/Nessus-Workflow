# Scan Settings

## Objective

Learn how to configure Nessus scan settings deliberately instead of changing options randomly or enabling everything.

By the end of this file, you should be able to:

* Understand what scan settings control.
* Separate essential settings from optional tuning.
* Configure an assessment around its objective.
* Understand discovery-related settings at a practical level.
* Understand assessment and performance settings.
* Recognize when a setting can materially change results.
* Avoid unnecessary aggressive configuration.
* Document important configuration decisions.
* Explain why a particular setting was changed.
* Troubleshoot unexpected results caused by configuration.

---

# 1. Configuration Comes After Workflow Selection

The assessment sequence is:

```text id="s9v7kq"
AUTHORIZATION
     ↓
SCOPE
     ↓
OBJECTIVE
     ↓
WORKFLOW
     ↓
TARGETS
     ↓
SCAN SETTINGS
     ↓
CREDENTIALS
     ↓
PRE-LAUNCH REVIEW
     ↓
EXECUTION
```

Do not begin with:

> "Which settings should I enable?"

Begin with:

> **"What configuration is required to answer my assessment question?"**

---

# 2. What Are Scan Settings?

Scan settings control how Nessus performs an assessment.

Depending on your Nessus version, edition, and selected workflow, settings can influence areas such as:

* Targets.
* Discovery.
* Assessment behavior.
* Credentials.
* Plugins.
* Performance.
* Advanced options.
* Scheduling.
* Reporting-related behavior.

The exact interface and available controls vary.

Therefore:

> **Learn what a setting changes, not where the setting happens to appear in the UI.**

---

# 3. The Configuration Mental Model

Use this model:

```text id="j8uw40"
ASSESSMENT QUESTION
       ↓
REQUIRED EVIDENCE
       ↓
WORKFLOW
       ↓
CONFIGURATION
       ↓
EXECUTION
       ↓
RESULTS
```

Every meaningful configuration decision should connect to the assessment question.

For example:

```text id="t0pf4n"
Question:
"What is externally visible?"

        ↓

Need:
Network/service visibility

        ↓

Configuration:
Appropriate discovery and assessment behavior
```

---

# 4. Configuration Categories

A practical way to think about Nessus settings is:

```text id="f6cy7j"
Scan Configuration
│
├── Target
│
├── Discovery
│
├── Assessment
│
├── Credentials
│
├── Plugins
│
├── Performance
│
├── Advanced
│
└── Scheduling
```

Not every scan exposes every category.

Some settings may appear only for particular workflows, credentials, editions, or product versions.

---

# 5. Essential vs Optional Settings

Not every visible option deserves modification.

## Essential Settings

Usually include things such as:

* Correct targets.
* Appropriate workflow.
* Required credentials.
* Required assessment scope.
* Important exclusions.
* Required discovery behavior.

## Optional Settings

May include:

* Performance tuning.
* Concurrency.
* Timeouts.
* Advanced behavior.
* Specialized checks.
* Scheduling options.

The correct approach is:

> **Change a setting only when you understand why it is needed.**

---

# 6. The Configuration Decision Rule

Before changing a setting, ask:

```text id="psq9xg"
What problem am I solving?
        ↓
What does this setting change?
        ↓
Could it affect coverage?
        ↓
Could it affect scan duration?
        ↓
Could it affect target stability?
        ↓
Could it change the interpretation of results?
        ↓
Do I need this change?
```

If you cannot answer these questions, leave the setting at its appropriate default until you understand it.

---

# 7. Default Settings Are a Starting Point

Defaults are useful because they provide a known baseline.

Do not assume:

> "Default means perfect for every assessment."

Instead:

```text id="m9d8v4"
Default Configuration
       ↓
Compare With Objective
       ↓
Is Anything Missing?
       │
   ┌───┴───┐
  NO      YES
   │        │
Use       Change
default   intentionally
```

The objective determines whether customization is necessary.

---

# 8. Target Settings

Target configuration determines what Nessus should assess.

Before launch, verify:

```text id="a0p8dk"
[ ] Correct target
[ ] Correct target count
[ ] Correct ranges
[ ] Correct exclusions
[ ] Correct hostname/IP resolution
```

A perfect vulnerability assessment against the wrong target is still the wrong assessment.

Target accuracy has priority over advanced tuning.

---

# 9. Discovery Settings

Discovery determines how Nessus identifies and interacts with systems and services within the assessment scope.

Depending on the workflow, discovery-related behavior may include concepts such as:

* Host discovery.
* Port discovery.
* Service identification.
* Operating-system identification.
* Network enumeration.
* Protocol/service detection.

The exact controls differ between workflows.

---

# 10. Host Discovery

Host discovery helps determine whether systems respond.

Conceptually:

```text id="qul3kl"
Target Scope
     ↓
Host Discovery
     ↓
Reachable / Observable Hosts
```

This can be useful when the scope is a network or range.

For a single known lab host, you may already know that the host exists and is reachable.

The important question is:

> "Does host discovery add useful information for this assessment?"

---

# 11. Port and Service Discovery

Vulnerability assessment depends on knowing what services are available to assess.

Conceptually:

```text id="n3m0u8"
Host
 ↓
Ports
 ↓
Services
 ↓
Technology / Version Evidence
 ↓
Relevant Security Checks
```

Do not assume that every open port automatically means a vulnerability.

A discovered service is an observation.

The assessment determines whether security weaknesses can be identified.

---

# 12. Discovery Does Not Equal Exploitation

Nessus discovery may identify:

```text id="f4c0a5"
Port 443
HTTPS
Web server
```

This does not mean:

```text id="j1z8ha"
"HTTPS is vulnerable."
```

The information only establishes what was observed.

Further checks are required to determine whether a vulnerability exists.

---

# 13. Assessment Settings

Assessment settings control what Nessus does after establishing the target and available services.

Depending on workflow and version, this may involve:

* Vulnerability checks.
* Service-specific checks.
* Web-related checks.
* Database-related checks.
* Local checks.
* Patch-related checks.
* Configuration checks.
* Other plugin-based assessments.

The exact available options depend on the installed product and selected workflow.

---

# 14. Don't Enable Every Assessment Option

A common beginner strategy is:

> "Enable every assessment option so nothing is missed."

This can be counterproductive.

Potential consequences include:

* Longer scans.
* More network activity.
* Greater target load.
* More irrelevant results.
* More difficult troubleshooting.
* Increased complexity.
* More difficult result interpretation.

Instead:

> **Select coverage appropriate to the assessment objective.**

---

# 15. Plugins and Settings

Nessus performs many checks through plugins.

A simplified model is:

```text id="7avf42"
Target
  ↓
Discovery
  ↓
Available Evidence
  ↓
Relevant Plugins
  ↓
Findings
```

Scan settings can influence which categories of checks are executed.

Therefore, changing plugin-related configuration can change the result set.

This is why plugin configuration belongs to assessment design, not cosmetic customization.

---

# 16. Performance Settings

Performance settings control how aggressively or efficiently Nessus performs its work.

Depending on the product/version, controls can affect concepts such as:

* Concurrent checks.
* Host concurrency.
* Network timing.
* Connection behavior.
* Timeouts.
* Scanner resource use.

The exact controls and safe values vary.

---

# 17. More Aggressive Is Not Automatically Better

Suppose you increase concurrency significantly.

Possible result:

```text id="g2d0bb"
Higher Concurrency
      ↓
More Parallel Activity
      ↓
Potentially Faster Execution
      +
Potentially More Load
      +
Potentially More Network Noise
      +
Potentially More Target Impact
```

Therefore:

> **Performance tuning is a trade-off, not a competition to maximize speed.**

---

# 18. Performance vs Coverage

A useful distinction:

### Coverage

How much of the intended assessment is actually performed.

### Performance

How efficiently the assessment executes.

Do not sacrifice required coverage simply to make a scan finish faster.

Conversely, do not make a scan unnecessarily slow when a safe configuration can achieve the same objective.

---

# 19. Performance Tuning Decision

Before changing performance settings, ask:

```text id="j5g6r4"
Is the scan too slow?
       ↓
Why?
       ↓
Is the cause understood?
       ↓
Can configuration safely address it?
       ↓
Will the change affect coverage or target impact?
```

Do not tune performance merely because another assessor uses different values.

---

# 20. Timeouts

Timeout-related settings can influence how long Nessus waits for responses.

Conceptually:

```text id="4rdr2x"
Request
  ↓
Target response
  ↓
Response received?
  │
 ┌┴───────┐
YES      NO
 │        │
Continue  Timeout behavior
```

Increasing a timeout may help with slow targets.

But excessive timeout values can also prolong assessments unnecessarily.

Use them only when there is a defined reason.

---

# 21. Network Conditions Matter

A Nessus scan does not operate in isolation.

Its behavior depends on:

```text id="e8f51s"
Scanner
   ↓
Network
   ↓
Security Controls
   ↓
Target
```

Possible influences include:

* Firewalls.
* IDS/IPS.
* Rate limiting.
* Routing.
* VPNs.
* Proxies.
* Network latency.
* Packet loss.
* Service availability.

Therefore, an unexpected result may be caused by the environment rather than a Nessus configuration error.

---

# 22. Advanced Settings

Advanced settings should be treated carefully.

A useful rule:

> **Do not change an advanced setting unless you can explain the problem it solves and the consequences of changing it.**

For each advanced change, record:

```text id="s9b5hz"
Setting:
Original:
New:
Reason:
Expected Effect:
Potential Risk:
Observed Effect:
```

This prevents "configuration drift."

---

# 23. Configuration Drift

Configuration drift occurs when an assessment slowly accumulates unexplained changes.

For example:

```text id="8d2wsl"
Default
 ↓
Concurrency changed
 ↓
Timeout changed
 ↓
Plugin category changed
 ↓
Discovery changed
 ↓
Advanced option changed
```

Eventually nobody remembers why the settings were changed.

The result becomes harder to reproduce and troubleshoot.

Avoid this.

---

# 24. Baseline Configuration

Create a simple baseline:

```text id="x9m5tq"
Workflow:
Targets:
Discovery:
Assessment:
Credentials:
Plugins:
Performance:
Advanced:
Scheduling:
```

Then record only intentional deviations.

This makes later comparison much easier.

---

# 25. Configuration and Reproducibility

If an assessment produces an important result, another assessor should ideally be able to understand how it was produced.

Record:

```text id="z6d3v9"
Assessment:
Date:
Nessus Version:
Edition:
Workflow:
Targets:
Credentials:
Important Settings:
Plugin Configuration:
Performance Changes:
Start:
End:
```

Exact export/configuration capabilities vary by Nessus version and edition, so documentation may need to supplement built-in history.

---

# 26. Scheduling Settings

Scheduling determines when an assessment executes.

Scheduling may be useful for:

* Recurring assessments.
* Maintenance windows.
* Regular vulnerability monitoring.
* Coordinating with system owners.
* Avoiding peak business periods.

But scheduling does not replace scope review.

Before scheduling a recurring assessment, verify that:

* Targets remain authorized.
* Credentials remain valid.
* The assessment remains appropriate.
* The operational window remains suitable.
* The configuration has not become outdated.

---

# 27. Recurring Scans Require Maintenance

A recurring assessment can become dangerous if nobody reviews it.

Consider:

```text id="6ljh1q"
Month 1
Authorized Target
      ↓
Month 2
Target Changed
      ↓
Month 3
System Reassigned
      ↓
Month 4
Authorization Changed
```

The recurring configuration may still exist.

That does not mean it should continue unchanged.

Recurring assessments require ownership and periodic review.

---

# 28. Configuration by Objective

Use these examples.

## Objective: External Vulnerability Assessment

Priorities:

```text id="m8j1h4"
Correct target
↓
External perspective
↓
Appropriate discovery
↓
Relevant vulnerability checks
↓
Safe execution
```

---

## Objective: Authenticated Host Assessment

Priorities:

```text id="b9y2dc"
Correct target
↓
Authentication
↓
Host-level visibility
↓
Relevant vulnerability checks
↓
Authentication verification
```

---

## Objective: Configuration Assessment

Priorities:

```text id="f8r7wx"
Correct target
↓
Required baseline
↓
Relevant compliance/configuration checks
↓
Expected evidence
```

The exact settings depend on the workflow and available Nessus capability.

---

# 29. Configuration Change Test

Before changing a setting, write:

```text id="3b9v0f"
Problem:
```

Then:

```text id="z1x8yw"
Setting:
```

Then:

```text id="9v9c1b"
Expected Effect:
```

Then:

```text id="1p8x2h"
Possible Side Effects:
```

Finally:

```text id="v4x3u7"
Success Criteria:
```

If you cannot define these, do not change the setting yet.

---

# 30. Practical Exercise 1 — Baseline Scan

Use your authorized lab.

Create a simple vulnerability assessment using appropriate defaults.

Record:

```text id="2k1m4q"
Workflow:
Target:
Discovery:
Assessment:
Credentials:
Plugins:
Performance:
Advanced:
```

Run the scan.

Record:

```text id="v3w7qk"
Duration:
Hosts Assessed:
Findings:
Errors:
```

This becomes your baseline.

---

# 31. Practical Exercise 2 — One Controlled Change

Repeat the assessment with exactly one deliberate configuration change.

Before the scan:

```text id="w1s6q9"
Setting:
Original Value:
New Value:
Reason:
Expected Effect:
```

After the scan:

```text id="f5k7r2"
Actual Effect:
Duration Difference:
Result Difference:
Unexpected Effects:
```

The objective is to learn cause and effect.

Do not change several settings at once.

---

# 32. Practical Exercise 3 — Discovery Decision

Use an authorized lab network.

Determine whether discovery is necessary.

Write:

```text id="r5f0x3"
Objective:
Known Targets:
Known Reachability:
Uncertainty:
Does Discovery Add Value?
Why?
```

Then choose whether to use a discovery-oriented workflow or proceed directly to vulnerability assessment.

---

# 33. Practical Exercise 4 — Performance Investigation

If a lab scan takes significantly longer than expected, do not immediately change performance settings.

Investigate:

```text id="q7p8d1"
Target Size:
Target Responsiveness:
Network Conditions:
Authentication:
Errors:
Scanner Load:
Workflow:
Configuration:
```

Only after identifying a plausible cause should you consider a configuration change.

---

# 34. Practical Exercise 5 — Configuration Comparison

Perform two scans with deliberately different assessment configurations in your authorized lab.

Keep everything else as constant as practical.

Compare:

```text id="d8g6x2"
Configuration Difference:
Execution Difference:
Result Difference:
Coverage Difference:
Risk/Impact Difference:
Interpretation:
```

The goal is to understand that configuration affects evidence.

---

# 35. Common Mistakes

## Mistake 1 — Changing Settings Without a Problem

Bad:

> "This option looks useful."

Better:

> "I have a defined assessment problem and this setting addresses it."

---

## Mistake 2 — Maximizing Concurrency

Bad:

> "Higher concurrency is always faster and therefore better."

Better:

> "Concurrency must be balanced against scanner resources, network behavior, target stability, and assessment requirements."

---

## Mistake 3 — Changing Five Settings at Once

Bad:

```text id="h3x5n2"
Timeout ↑
Concurrency ↑
Discovery changed
Plugins changed
Advanced setting changed
```

Now you cannot easily explain why the result changed.

Better:

```text id="4v8s2a"
One controlled change
        ↓
Observe
        ↓
Understand
        ↓
Next change if necessary
```

---

## Mistake 4 — Copying Settings From Another Environment

Bad:

> "This worked for another server, so I'll use it here."

Better:

> "I will determine whether this environment has the same requirement."

---

## Mistake 5 — Treating Defaults as Permanent Truth

Bad:

> "Defaults are always correct."

Better:

> "Defaults provide a baseline; the assessment objective determines whether customization is necessary."

---

## Mistake 6 — Ignoring Configuration History

Bad:

> "Someone changed the settings months ago."

Better:

> "I need to understand what changed and why before interpreting results."

---

# 36. Configuration Troubleshooting

When results look unexpected, check configuration before assuming the vulnerability data is wrong.

Use:

```text id="5u3j8p"
Unexpected Result
      ↓
Target Correct?
      ↓
Workflow Correct?
      ↓
Discovery Appropriate?
      ↓
Credentials Correct?
      ↓
Plugin/Assessment Coverage Appropriate?
      ↓
Performance Settings Reasonable?
      ↓
Advanced Changes?
      ↓
Network Conditions?
```

This prevents random troubleshooting.

---

# 37. Configuration and Result Interpretation

Remember:

```text id="k1d8z6"
Result
  =
Target
+
Workflow
+
Configuration
+
Credentials
+
Plugins
+
Environment
```

Therefore:

> **A Nessus result should always be interpreted in the context of how the assessment was configured.**

The same target can produce different results under different assessment conditions.

---

# 38. Professional Configuration Record

For an important assessment, maintain:

```text id="r8q1s6"
## Assessment Configuration

### Objective
[What question is being answered]

### Workflow
[Selected workflow]

### Targets
[Target list]

### Discovery
[Relevant configuration]

### Assessment
[Relevant configuration]

### Credentials
[Authenticated / Unauthenticated]

### Plugins
[Relevant configuration]

### Performance
[Important changes]

### Advanced
[Important changes]

### Scheduling
[If applicable]

### Configuration Rationale
[Why important changes were made]

### Expected Effect
[What the changes should accomplish]
```

Do not store passwords or other sensitive secrets in this document.

---

# 39. Configuration Review Before Launch

Use this final review:

```text id="0c9b5n"
[ ] Workflow matches objective
[ ] Targets are correct
[ ] Discovery behavior is appropriate
[ ] Assessment coverage is appropriate
[ ] Credentials are intentional
[ ] Plugin configuration is understood
[ ] Performance settings are reasonable
[ ] Advanced settings are justified
[ ] Scheduling is appropriate
[ ] Important changes are documented
[ ] Potential impact is understood
```

---

# 40. Professional Decision Rule

Before launching, complete:

> **"I changed this setting because __________. I expect it to __________. The main risk is __________. I will know it worked if __________."**

Example:

> "I changed this setting because the authorized lab target responds slowly. I expect Nessus to allow more time for responses. The main risk is increased scan duration. I will know it worked if the assessment progresses past the previous timeout behavior without introducing unexpected errors."

This is the level of reasoning you should develop.

---

# 41. Completion Criteria

You have completed this file when you can independently:

* Explain what Nessus scan settings control.
* Separate essential configuration from optional tuning.
* Configure a scan based on its assessment objective.
* Explain discovery-related configuration.
* Explain how assessment settings affect coverage.
* Understand why plugin configuration can change results.
* Explain performance trade-offs.
* Avoid unnecessary aggressive configuration.
* Make controlled configuration changes.
* Document why important settings were changed.
* Troubleshoot unexpected results by reviewing configuration.
* Recognize configuration drift.
* Maintain a reproducible assessment configuration record.

The final test is:

> **Given an authorized Nessus assessment objective, can you choose the workflow, configure only the settings that matter, explain every important deviation from the baseline, and predict how those decisions may affect execution, coverage, and results?**

If yes, you are ready to move to one of the most important Nessus capabilities: **authenticated assessment and credential configuration**.
