# Partial-Information Decision Labs

## Objective

This lab develops the ability to operate Nessus when the available information is incomplete, ambiguous, conflicting, or unreliable.

In real assessments, the operator rarely receives a perfect package containing:

* complete authorization records
* accurate asset inventories
* confirmed network paths
* verified credentials
* known scanner position
* complete business context
* perfectly configured targets
* complete vulnerability evidence
* confirmed remediation status

Professional assessment work requires deciding **what can be done now, what must be verified first, what can safely be assumed, and what must remain explicitly unknown**.

The goal of this lab is not to guess correctly.

The goal is to make the **next defensible decision**.

---

## Core Mental Model

Use this workflow whenever information is incomplete:

```text
WHAT IS KNOWN?
        ↓
WHAT IS UNKNOWN?
        ↓
WHICH UNKNOWN AFFECTS THE DECISION?
        ↓
CAN I SAFELY PROCEED WITHOUT IT?
        ↓
IF NOT, WHAT IS THE LOWEST-IMPACT WAY TO VERIFY IT?
        ↓
WHAT RESULT DO I EXPECT?
        ↓
WHAT DID I OBSERVE?
        ↓
WHAT DOES THAT EVIDENCE SUPPORT?
        ↓
WHAT REMAINS UNCERTAIN?
        ↓
WHAT IS THE NEXT DECISION?
```

The key principle is:

> Missing information is not permission to invent an answer.

---

## What Counts as Partial Information?

Partial information exists whenever the operator knows enough to begin reasoning but not enough to confidently complete the decision.

Examples include:

* the target is known but authorization is unclear
* the hostname is known but the current IP is unknown
* the scanner is known but its network position is unclear
* credentials exist but their permissions are unknown
* a scan completed but coverage is uncertain
* a finding exists but applicability is unclear
* a vulnerability disappeared but the reason is unknown
* remediation is claimed but not independently verified
* the target was scanned but the relevant service was unreachable
* the Nessus edition is unknown
* a finding changed after a plugin update
* the asset exists but business criticality is unknown
* the scan is incomplete but a stakeholder requests a conclusion
* a target appears inside a supplied range but may not be authorized

---

## Partial-Information Rules

### Rule 1 — Separate Facts From Assumptions

Record information in three categories:

| Category | Meaning                                          |
| -------- | ------------------------------------------------ |
| Known    | Directly supported by available evidence         |
| Unknown  | Not established yet                              |
| Assumed  | Temporarily treated as true for a defined reason |

An assumption should never silently become a fact.

---

### Rule 2 — Not Every Unknown Blocks Progress

Some unknowns can safely remain unresolved temporarily.

For example:

```text
Unknown:
Exact business owner

Known:
Target is authorized
Target is reachable
Assessment objective is vulnerability identification
```

The missing owner may not prevent the technical assessment from beginning.

By contrast:

```text
Unknown:
Whether the target is authorized
```

This may block scanning entirely.

The important question is:

> Does this unknown change whether or how I am authorized to act?

---

### Rule 3 — Identify Decision-Critical Unknowns

Ask:

```text
If this unknown were true, would I choose a different action?
```

If the answer is yes, the unknown is decision-critical.

Examples:

* Is the target actually in scope?
* Is the scanner allowed to reach the environment?
* Are credentials authorized for this assessment?
* Is the system production-critical?
* Is the finding supported by sufficient evidence?
* Was the retest performed from the same assessment perspective?

---

### Rule 4 — Verify Before Escalating Impact

When information is incomplete, prefer:

```text
Documentation
    ↓
Configuration evidence
    ↓
Network evidence
    ↓
Service evidence
    ↓
Nessus evidence
    ↓
Low-impact validation
    ↓
Controlled testing
```

Do not jump directly to the most intrusive action simply because the initial evidence is incomplete.

---

### Rule 5 — Unknown Is a Valid Result

Examples:

```text
Authentication status: Unknown
Coverage: Partial
Finding applicability: Unresolved
Business criticality: Not provided
Remediation status: Not independently verified
```

These are more professional than unsupported conclusions.

---

## Partial-Information Decision Record

Use this structure for each scenario:

```text
Scenario:
Objective:

Known:
-

Unknown:
-

Decision-Critical Unknown:
-

Constraints:
-

Possible Actions:
1.
2.
3.

Selected Action:
-

Why:
-

Expected Result:
-

Actual Result:
-

Remaining Uncertainty:
-

Next Decision:
-
```

---

# Lab 1 — Authorization Record Is Incomplete

## Scenario

You receive:

```text
Target: 10.10.20.15
Purpose: Vulnerability assessment
```

No authorization document is attached.

### Known

* A target address exists.
* Someone requested an assessment.

### Unknown

* Whether the target is authorized.
* Who owns it.
* Whether the requested assessment is approved.

### Decision

Do not begin scanning merely because a target was supplied.

### Next Action

Verify authorization and scope before technical assessment.

### Expected Result

A clear authorization boundary is established.

### Learning Point

A target request is not equivalent to authorization.

---

# Lab 2 — Target Is Known but Ownership Is Unknown

## Scenario

The assessment scope explicitly contains:

```text
10.10.20.0/24
```

You are asked to scan the environment, but no asset owner information is provided.

### Known

* The range is authorized.
* The assessment objective is defined.

### Unknown

* Individual asset ownership.
* Business criticality.

### Decision

If the authorization clearly covers the range, the missing owner information does not automatically prevent technical assessment.

However, business-context-dependent prioritization may need to remain incomplete.

### Expected Result

Technical assessment proceeds within scope while business-context gaps are documented.

### Next Decision

Obtain ownership and criticality information before final business-priority conclusions.

---

# Lab 3 — Hostname Without Confirmed IP

## Scenario

You receive:

```text
portal.example.test
```

The environment recently changed DNS.

### Known

* The hostname is part of the authorized application scope.

### Unknown

* Current IP address.
* Whether multiple addresses are returned.
* Whether DNS points to the expected environment.

### Decision

Verify name resolution before assuming the destination.

### Expected Result

The current resolution path is documented.

### Learning Point

A hostname can identify an application while still leaving the technical target ambiguous.

---

# Lab 4 — IP Address Without Host Identity

## Scenario

The target list contains:

```text
10.10.30.25
```

No hostname or asset description is available.

### Known

* The address is authorized.

### Unknown

* Asset role.
* Owner.
* Operating system.
* Business importance.

### Decision

You may perform the authorized technical assessment if the scope permits it.

Do not invent asset identity.

### Documentation

```text
Asset identity:
Unknown at assessment start

Technical target:
10.10.30.25

Business context:
Not provided
```

---

# Lab 5 — Scanner Position Is Unknown

## Scenario

A scan was requested against an internal server.

The scanner is operational, but nobody has documented where the scanner sits relative to the target network.

### Known

* Scanner is functioning.
* Target is authorized.

### Unknown

* Routing path.
* Firewall traversal.
* Network segmentation.
* Whether the scanner has the intended perspective.

### Decision

Determine scanner placement and network path before interpreting missing services or findings.

### Learning Point

The same target can produce different evidence from different network positions.

---

# Lab 6 — Ping Fails

## Scenario

The target does not respond to ICMP.

### Known

* ICMP is not responding.

### Unknown

* Whether the host is actually offline.
* Whether ICMP is filtered.
* Whether required assessment ports are reachable.

### Incorrect Decision

> The host is down.

### Better Decision

Test the relevant network path and assessment services using authorized, low-impact checks.

### Learning Point

```text
ICMP failure ≠ host absence
```

---

# Lab 7 — Ping Works but Scan Finds Nothing

## Scenario

The target responds to ICMP but Nessus reports little useful service information.

### Known

* Host appears reachable.
* Nessus produced limited results.

### Unknown

* Required ports may be filtered.
* Services may be unavailable.
* Scanner position may be incorrect.
* Target may restrict the scanner source.
* Configuration may not match the objective.

### Decision

Investigate the path:

```text
Scanner
↓
Routing
↓
Firewall
↓
Target
↓
Port
↓
Service
↓
Nessus Configuration
```

Do not conclude that the host has no vulnerabilities.

---

# Lab 8 — Credential Exists but Permissions Are Unknown

## Scenario

An administrator provides credentials and says:

> "These should work."

### Known

* Credentials were supplied.
* Their use is authorized.

### Unknown

* Account privileges.
* Required remote-access permissions.
* Whether the account can retrieve the information Nessus needs.

### Decision

Verify authentication and effective permissions.

### Learning Point

```text
Credential supplied
        ↓
Credential accepted
        ↓
Required permissions available
        ↓
Relevant assessment data collected
```

These are different states.

---

# Lab 9 — Authentication Appears Successful

## Scenario

Nessus reports successful authentication on a host.

### Known

* Authentication succeeded according to available evidence.

### Unknown

* Whether all intended checks were possible.
* Whether required privileges were available.
* Whether every target in the assessment authenticated successfully.

### Decision

Review authentication evidence and coverage.

### Incorrect Conclusion

> The authenticated scan has complete visibility.

### Correct Interpretation

Authentication success is evidence of access, not proof of complete assessment coverage.

---

# Lab 10 — One Host Authenticates, Another Does Not

## Scenario

A ten-host assessment uses the same intended credentials.

Results:

```text
8 hosts — authenticated
2 hosts — authentication unavailable
```

### Known

* Authentication worked on most hosts.

### Unknown

* Why the two hosts failed.
* Whether their operating systems differ.
* Whether network or account restrictions exist.

### Decision

Treat authentication as host-specific coverage.

### Next Action

Investigate the two failures separately.

### Learning Point

Do not generalize:

```text
"Credentials work."
```

into:

```text
"Authentication coverage is complete."
```

---

# Lab 11 — Manual Login Works

## Scenario

An administrator manually logs into a server using the supplied account.

Nessus does not obtain authenticated results.

### Known

* Account credentials can be used interactively.
* Nessus does not have the expected authenticated evidence.

### Unknown

* Remote authentication method.
* Required service availability.
* Account restrictions.
* Privilege requirements.
* Nessus credential configuration.

### Decision

Troubleshoot by layer:

```text
Network
↓
Required Service
↓
Authentication Method
↓
Account
↓
Permissions
↓
Target Restrictions
↓
Nessus Configuration
```

### Learning Point

Manual login success does not prove Nessus can perform authenticated assessment.

---

# Lab 12 — Finding Based on Version Evidence

## Scenario

Nessus reports a vulnerability because a package appears to have an affected version.

The target uses a vendor-maintained operating system.

### Known

* Version evidence indicates potential exposure.

### Unknown

* Whether the vendor backported the security fix.
* Whether the affected component is actually in use.
* Whether the package state is current.

### Decision

Investigate package/vendor evidence before declaring the finding confirmed.

### Learning Point

Version strings may require platform-specific interpretation.

---

# Lab 13 — Finding Has High Severity but Weak Context

## Scenario

A high-severity finding appears on a host.

You do not know:

* whether the service is externally reachable
* whether the affected component is active
* whether compensating controls exist

### Known

* Nessus produced the finding.
* Severity is high according to Nessus.

### Unknown

* Environmental exposure.
* Applicability context.
* Business impact.

### Decision

Investigate before assigning final remediation priority.

### Learning Point

Severity is an input to prioritization, not the complete decision.

---

# Lab 14 — Business Criticality Is Missing

## Scenario

Several findings have similar technical characteristics.

No asset criticality information is available.

### Decision

Do not invent criticality.

Document:

```text
Business criticality:
Not provided
```

Technical prioritization can continue where justified, but business-context conclusions should remain qualified.

---

# Lab 15 — Finding Disappeared

## Scenario

A previous assessment showed a finding.

The next assessment does not.

### Known

* Finding was previously observed.
* Finding is absent from the new result.

### Unknown

* Whether remediation occurred.
* Whether plugin behavior changed.
* Whether configuration changed.
* Whether authentication changed.
* Whether target scope changed.
* Whether network visibility changed.

### Decision

Investigate the difference before marking the issue remediated.

### Learning Point

```text
Finding absent
≠
Remediation proven
```

---

# Lab 16 — Scope Changed Between Assessments

## Scenario

Assessment A covered:

```text
10.10.10.10
10.10.10.11
10.10.10.12
```

Assessment B covered:

```text
10.10.10.10
10.10.10.11
```

The finding count decreased.

### Known

* Scope changed.

### Unknown

* Whether findings actually disappeared because of remediation.

### Decision

Do not compare raw counts as if the assessments were equivalent.

### Learning Point

Normalize scope before interpreting trends.

---

# Lab 17 — Authentication Changed Between Assessments

## Scenario

Assessment A was unauthenticated.

Assessment B was authenticated.

The second assessment finds more vulnerabilities.

### Known

* Assessment perspectives differ.

### Unknown

* Whether any increase represents newly introduced vulnerabilities.

### Decision

Treat the assessments as different visibility conditions.

### Conclusion

The increase may reflect broader visibility rather than deterioration.

---

# Lab 18 — Plugin Content Changed

## Scenario

A finding appears after a plugin/content update.

### Known

* Plugin/content state changed.

### Unknown

* Whether the target changed.
* Whether detection logic changed.
* Whether the finding was previously undetected.

### Decision

Compare plugin/content state before attributing the change to target remediation or deterioration.

---

# Lab 19 — Scan Completed Suspiciously Quickly

## Scenario

A scan expected to take significantly longer completes very quickly.

### Known

* Scan status is completed.

### Unknown

* Whether targets were actually reached.
* Whether discovery occurred.
* Whether services were accessible.
* Whether plugins executed as expected.
* Whether configuration changed.

### Decision

Review:

```text
Target count
↓
Host coverage
↓
Reachability
↓
Services
↓
Plugin activity
↓
Errors
↓
Scan configuration
```

### Learning Point

Completed status is not the same as meaningful coverage.

---

# Lab 20 — Scan Is Still Running

## Scenario

A scan has been running longer than expected.

### Known

* Scan is active.
* Expected duration has been exceeded.

### Unknown

* Whether it is making meaningful progress.
* Whether a target is slow.
* Whether network conditions changed.
* Whether scanner resources are constrained.

### Decision

Observe before stopping.

Check:

* progress
* target behavior
* scanner resources
* errors
* connection behavior
* assessment window

### Learning Point

Long-running does not automatically mean broken.

---

# Lab 21 — Stakeholder Requests a Security Guarantee

## Scenario

A stakeholder asks:

> "Can you confirm the environment is secure?"

### Known

* A Nessus assessment was performed.

### Unknown

* Whether every possible vulnerability was detected.
* Whether every asset was covered.
* Whether application-layer issues were assessed.
* Whether configuration was fully evaluated.

### Decision

Describe the assessment scope, coverage, findings, limitations, and remaining uncertainty.

### Learning Point

A vulnerability assessment cannot automatically establish absolute security.

---

# Lab 22 — Finding Without Supporting Evidence

## Scenario

A finding appears in a report, but the detailed evidence is unavailable.

### Known

* Finding exists in the result set.

### Unknown

* Detection basis.
* Affected component.
* Applicability.
* Confidence.

### Decision

Do not treat the finding as fully established until supporting evidence is recovered or the finding is independently investigated.

### Acceptable Status

```text
Unresolved — evidence insufficient for independent confirmation.
```

---

# Lab 23 — Unexpected Host Appears

## Scenario

Discovery identifies a host that was not in the expected inventory.

### Known

* Host is technically reachable.
* Host appears within the scanned technical boundary.

### Unknown

* Whether it is authorized.
* Who owns it.
* Why it exists.

### Decision

Do not automatically expand the assessment.

Verify authorization and ownership.

### Learning Point

Discovery can reveal information without expanding authorization.

---

# Lab 24 — Expected Host Is Missing

## Scenario

The authorized scope contains a host that does not appear in results.

### Known

* Host should be assessed.
* It was not observed.

### Unknown

* DNS issue.
* Routing issue.
* Firewall restriction.
* Host outage.
* Incorrect target definition.
* Scanner-position problem.

### Decision

Investigate coverage before declaring the host unaffected.

---

# Lab 25 — Required Port Is Missing

## Scenario

An application is expected to run on a particular TCP port.

Nessus does not identify it.

### Known

* Application is expected.
* Port is absent from observed results.

### Unknown

* Service may be down.
* Port may be filtered.
* Service may use a different port.
* Scanner may lack access.

### Decision

Verify the network and service path.

---

# Lab 26 — Load Balancer Is Present

## Scenario

A hostname resolves to multiple addresses.

Different scans show different findings.

### Known

* Multiple backend paths may exist.

### Unknown

* Whether all backend systems were reached.
* Whether traffic distribution changed.
* Whether scanner position affects routing.

### Decision

Investigate target architecture before interpreting findings as simple remediation or regression.

### Learning Point

Application identity and individual backend identity are not always equivalent.

---

# Lab 27 — DNS Changed During Assessment

## Scenario

A hostname resolved to one address before the scan and another afterward.

### Known

* DNS changed.

### Unknown

* Which system was actually assessed.
* Whether both systems were in scope.
* Whether findings are directly comparable.

### Decision

Document resolution at assessment time and verify target identity.

---

# Lab 28 — Target Rebooted During Scan

## Scenario

A server rebooted during assessment.

### Known

* Target availability changed during execution.

### Unknown

* Which checks completed before reboot.
* Which services were available afterward.
* Whether results represent full coverage.

### Decision

Treat the assessment as potentially incomplete.

Determine whether a controlled rerun is required.

---

# Lab 29 — Firewall Restriction Appears

## Scenario

The scanner reaches some services but not others.

### Known

* Selective connectivity exists.

### Unknown

* Firewall policy.
* ACL behavior.
* Source-based filtering.
* Whether inaccessible services are intentionally restricted.

### Decision

Investigate network controls before changing Nessus aggressiveness.

---

# Lab 30 — Intermittent Reachability

## Scenario

A target is reachable during some checks and unreachable during others.

### Known

* Connectivity is unstable.

### Unknown

* Network instability.
* Load balancing.
* Rate limiting.
* Target resource pressure.
* Security control behavior.

### Decision

Collect evidence over controlled observations.

Do not interpret inconsistent results as stable target state.

---

# Lab 31 — Scanner Resource Pressure

## Scenario

The scan is slow and scanner CPU or memory utilization is high.

### Known

* Scanner resources are constrained.

### Unknown

* Whether scanner capacity is the primary cause.
* Whether target/network behavior also contributes.

### Decision

Correlate scanner resource observations with scan behavior.

Possible actions include:

* reduce concurrent workload
* reschedule
* adjust scope
* review configuration
* increase scanner capacity where appropriate

Do not change multiple variables without documenting them.

---

# Lab 32 — Production Impact Is Suspected

## Scenario

A production application begins responding slowly during a vulnerability assessment.

### Known

* Timing overlaps with the assessment.
* Impact is possible.

### Unknown

* Whether the scan caused the degradation.
* Which traffic or target is responsible.
* Whether business impact is increasing.

### Decision

Follow the agreed stop/escalation procedure.

Preserve evidence and avoid increasing scan intensity while causality is uncertain.

### Learning Point

Operational safety takes precedence over obtaining additional scan data.

---

# Lab 33 — Validation May Be Disruptive

## Scenario

A finding may require a test that could affect service availability.

### Known

* Nessus has produced a finding.
* Additional evidence would increase confidence.

### Unknown

* Exact impact of the proposed test.
* Whether the test is authorized.
* Whether the target can tolerate it.

### Decision

Do not perform the disruptive validation by default.

Seek explicit authorization, appropriate timing, and recovery planning.

---

# Lab 34 — Remediation Is Claimed

## Scenario

An administrator says:

> "The vulnerability has been fixed."

### Known

* Remediation is claimed.

### Unknown

* Whether the change was applied correctly.
* Whether the correct asset was changed.
* Whether the underlying condition is gone.

### Decision

Treat the statement as remediation evidence, not independent verification.

Perform an appropriate retest or verification step.

---

# Lab 35 — Retest Uses Different Credentials

## Scenario

Original assessment:

```text
Authenticated
```

Retest:

```text
Unauthenticated
```

The finding is absent.

### Known

* Assessment perspective changed.

### Unknown

* Whether remediation actually removed the condition.

### Decision

Do not treat the result as directly equivalent.

Align the retest perspective or gather additional evidence.

---

# Lab 36 — Retest Uses Different Scanner Position

## Scenario

The original scan came from an internal scanner.

The retest came from an external scanner.

The finding disappeared.

### Known

* Network perspective changed.

### Unknown

* Whether exposure changed.
* Whether filtering differs.
* Whether the scanner can observe the same condition.

### Decision

Interpret the result as a perspective change until equivalence is established.

---

# Lab 37 — Finding Persists After Patch

## Scenario

The owner reports that a patch was installed, but Nessus still reports the finding.

### Known

* Patch is claimed to be installed.
* Nessus still detects the issue.

### Unknown

* Package version.
* Reboot state.
* Backported patch status.
* Scanner evidence.
* Whether the affected component remains active.

### Decision

Investigate the discrepancy instead of immediately declaring either side correct.

---

# Lab 38 — Target Was Rebuilt

## Scenario

A server was destroyed and rebuilt.

The old finding is no longer present.

### Known

* Original asset changed substantially.

### Unknown

* Whether the new system has equivalent identity.
* Whether the same vulnerability was actually remediated.
* Whether the new configuration introduces other risks.

### Decision

Treat the rebuilt system as a changed assessment condition.

Document the asset lifecycle and assess the new state appropriately.

---

# Lab 39 — Out-of-Scope Target Requested

## Scenario

During an assessment, an administrator asks:

> "Can you scan this other server too?"

### Known

* The server may be technically reachable.
* It is not in the current scope.

### Unknown

* Whether additional authorization exists.

### Decision

Do not scan it until scope authorization is confirmed.

---

# Lab 40 — Scope Is Broad and Ambiguous

## Scenario

The authorization says:

```text
All corporate systems
```

No technical boundary is provided.

### Known

* General intent exists.

### Unknown

* Exact systems.
* Networks.
* Exclusions.
* Production restrictions.
* Third-party systems.

### Decision

Do not translate an ambiguous statement into an unlimited technical target list.

Obtain a concrete scope definition.

---

# Lab 41 — Credential Expired

## Scenario

A recurring authenticated assessment suddenly loses authentication coverage.

### Known

* Authentication previously worked.
* Current authentication is failing.

### Unknown

* Password expiration.
* Account lockout.
* Permission change.
* Service restriction.
* Nessus configuration change.

### Decision

Investigate the authentication lifecycle rather than repeatedly launching scans.

---

# Lab 42 — Shared Credential Suddenly Fails Everywhere

## Scenario

A recurring assessment loses authentication on many systems simultaneously.

### Known

* Multiple hosts changed state at the same time.

### Unknown

* Credential expiration.
* Central identity-system change.
* Account lockout.
* Authentication policy change.

### Decision

Investigate the common dependency first.

### Learning Point

A fleet-wide simultaneous failure often justifies checking shared infrastructure before troubleshooting every target independently.

---

# Lab 43 — One Host Suddenly Fails Authentication

## Scenario

Only one system loses authenticated coverage.

### Known

* Other systems still authenticate.

### Unknown

* Host-specific permissions.
* Local account state.
* Service configuration.
* Host firewall.
* System-specific policy.

### Decision

Investigate the host-specific path first.

---

# Lab 44 — Finding Count Increased After Authentication

## Scenario

Unauthenticated assessment:

```text
12 findings
```

Authenticated assessment:

```text
31 findings
```

### Known

* Assessment perspective changed.

### Unknown

* Whether the environment became less secure.

### Decision

Interpret the additional findings as potentially increased visibility.

Do not treat the raw count increase as evidence of deterioration.

---

# Lab 45 — Large Number of Low-Severity Findings

## Scenario

The scan produces hundreds of low-severity findings.

### Known

* Finding volume is high.

### Unknown

* Whether many findings share a common root cause.
* Whether some are informational or duplicative.
* Whether one remediation would address many findings.

### Decision

Group related findings and investigate common causes.

### Learning Point

A long finding list is not automatically a long remediation list.

---

# Lab 46 — One High-Severity Finding Has Weak Evidence

## Scenario

A high-severity result appears, but evidence is indirect.

### Known

* Nessus reports the issue.
* Severity is high.

### Unknown

* Applicability.
* Current package/configuration state.
* Exploitability in the actual environment.

### Decision

Prioritize investigation and validation of the evidence.

Do not increase the claim beyond what the evidence supports.

---

# Lab 47 — Low-Severity Finding Has Broad Root Cause

## Scenario

A low-severity finding appears on hundreds of systems because of one shared configuration.

### Known

* Individual severity is low.
* Scope is broad.

### Unknown

* Business impact.
* Exposure.
* Whether the configuration supports more serious attack paths.

### Decision

Investigate the root cause and environmental context before assigning remediation priority.

---

# Lab 48 — No Business Owner Is Available

## Scenario

A finding requires an owner for remediation, but ownership records are incomplete.

### Decision

Do not invent an owner.

Document:

```text
Technical owner:
Unknown

Business owner:
Unknown

Required action:
Ownership assignment
```

The technical finding can remain open while ownership is resolved.

---

# Lab 49 — Finding Cannot Be Independently Validated

## Scenario

A target cannot be safely tested further because validation would create unacceptable operational risk.

### Known

* Nessus evidence exists.
* Direct validation is unsafe.

### Unknown

* Whether the condition can be independently confirmed.

### Decision

Preserve the Nessus evidence and report the limitation.

Possible status:

```text
Unresolved — independent validation not performed due to operational constraints.
```

### Learning Point

A professional assessment can contain unresolved findings.

---

# Lab 50 — Assessment Deadline Is Near

## Scenario

The reporting deadline is approaching, but several findings remain uncertain.

### Incorrect Response

Remove uncertain findings so the report looks cleaner.

### Better Response

Separate findings into:

```text
Supported
Validated
Unresolved
Not Applicable
False Positive
Changed Condition
```

Document limitations.

### Learning Point

A deadline changes scheduling pressure, not the evidentiary standard.

---

# Lab 51 — Scope Records Conflict

## Scenario

The ticket lists:

```text
10.10.10.0/24
```

The authorization document lists:

```text
10.10.20.0/24
```

### Known

* Two scope records disagree.

### Unknown

* Which boundary is authoritative.

### Decision

Stop before scanning the disputed range and resolve the authorization conflict.

---

# Lab 52 — Scanner Has Been Replaced

## Scenario

A recurring assessment now runs from a newly deployed scanner.

Results differ significantly.

### Known

* Scanner changed.

### Unknown

* Network position.
* Configuration equivalence.
* Plugin/content state.
* Performance characteristics.

### Decision

Verify scanner equivalence before interpreting result changes as target changes.

---

# Lab 53 — Template Is Old

## Scenario

A recurring assessment uses a template created months ago.

### Known

* Template is old.

### Unknown

* Whether its configuration still matches the current objective.
* Whether plugin/content behavior changed.
* Whether target architecture changed.

### Decision

Review the assessment configuration before reuse.

### Learning Point

Reusable configuration reduces work but does not eliminate review.

---

# Lab 54 — "Enable Everything"

## Scenario

A stakeholder asks:

> "Can you just enable every plugin so we don't miss anything?"

### Known

* More plugin coverage may increase detection opportunities.

### Unknown

* Operational impact.
* Scan duration.
* Target sensitivity.
* Whether every plugin is relevant to the assessment.

### Decision

Select coverage according to the assessment objective and operational constraints.

### Learning Point

Maximum configuration is not automatically maximum assessment quality.

---

# Lab 55 — Results Differ After Configuration Change

## Scenario

Assessment A used conservative settings.

Assessment B used more aggressive settings.

Finding counts increased.

### Known

* Configuration changed.

### Unknown

* Whether target state changed.
* Whether detection coverage changed.

### Decision

Attribute the difference cautiously.

Document the configuration change before comparing results.

---

# Lab 56 — Finding Is Present on One Similar Host

## Scenario

Five apparently similar servers are assessed.

One has a finding that the others do not.

### Known

* One host differs.

### Unknown

* Configuration differences.
* Software versions.
* Patch state.
* Network exposure.
* Authentication differences.

### Decision

Investigate the outlier rather than assuming the result is false.

---

# Lab 57 — Documentation Is Missing

## Scenario

A previous operator left only a scan result.

No record explains:

* objective
* scope
* scanner position
* credentials
* configuration
* limitations

### Known

* Results exist.

### Unknown

* Assessment conditions.

### Decision

Treat historical comparability as limited.

Recover evidence where possible before using the assessment as a baseline.

---

# Lab 58 — Owner Disagrees With Finding

## Scenario

An administrator says:

> "That vulnerability cannot exist on this server."

### Known

* Nessus produced a finding.
* Owner disputes it.

### Unknown

* Whether the technical condition still exists.
* Whether vendor-specific patching or configuration explains the discrepancy.

### Decision

Compare evidence from both sides.

Do not resolve the disagreement through authority alone.

---

# Lab 59 — Finding Appears Only Once

## Scenario

Three comparable scans are performed.

A finding appears in only one.

### Known

* Detection is inconsistent.

### Unknown

* Intermittent service state.
* Network conditions.
* Load balancing.
* Target changes.
* Detection behavior.

### Decision

Investigate reproducibility and environmental conditions.

### Learning Point

A single observation can be meaningful without being sufficient for certainty.

---

# Lab 60 — Security Control Blocks Validation

## Scenario

A network security control prevents the scanner from reaching a service required for validation.

### Known

* Validation path is blocked.

### Unknown

* Whether the underlying vulnerability exists.

### Decision

Document the limitation and seek an approved alternative evidence source.

Do not disable security controls without authorization.

---

# Lab 61 — Target Is Technically Reachable but Operationally Restricted

## Scenario

The system is reachable, but the asset owner states that scanning is allowed only during a maintenance window.

### Known

* Technical connectivity exists.
* Operational restriction exists.

### Decision

Respect the assessment window.

Technical reachability does not override operational constraints.

---

# Lab 62 — Scan Was Interrupted

## Scenario

A scan was stopped halfway through because of an operational issue.

### Known

* Partial results exist.

### Unknown

* Which intended checks completed.
* Which targets remain unassessed.

### Decision

Determine actual coverage before using the results for final conclusions.

---

# Lab 63 — Finding Count Increased After Plugin Update

## Scenario

A recurring scan finds several new issues immediately after plugin updates.

### Known

* Plugin/content state changed.
* Findings appeared afterward.

### Unknown

* Whether target state changed.

### Decision

Review the new detections individually and document plugin/content state.

Do not automatically describe the increase as newly introduced vulnerabilities.

---

# Lab 64 — Finding Count Decreased After Plugin Update

## Scenario

Several findings disappear after content changes.

### Known

* Plugin/content state changed.

### Unknown

* Whether detection logic changed.
* Whether target state changed.

### Decision

Investigate the changed detection before declaring remediation.

---

# Lab 65 — Scan Results Are Missing Expected Detail

## Scenario

A scan completes, but expected host/service information is absent.

### Known

* Scan completed.
* Expected information is missing.

### Unknown

* Target reachability.
* Discovery configuration.
* Scanner position.
* Service exposure.
* Plugin execution.

### Decision

Treat missing detail as a coverage question first.

---

# Lab 66 — Stakeholder Wants a Single Number

## Scenario

Management asks:

> "How many vulnerabilities do we have?"

### Known

* Nessus produced a set of findings.

### Unknown

* Whether counts represent unique root causes.
* Whether findings are validated.
* Whether scope is complete.
* Whether business context is available.

### Decision

Provide counts only with their scope, methodology, and limitations.

Where appropriate, supplement counts with:

* affected assets
* validated findings
* root causes
* priority context
* coverage limitations

---

# Lab 67 — A Finding Is Marked "Informational"

## Scenario

A finding is informational but reveals a useful service or configuration detail.

### Known

* Severity label is informational.

### Unknown

* Whether the information contributes to a larger security issue.

### Decision

Do not automatically discard informational evidence.

Use it as context when it affects interpretation or next steps.

---

# Lab 68 — Exposure Is Unknown

## Scenario

A vulnerability is confirmed technically, but you do not know whether the affected service is externally reachable.

### Decision

Separate:

```text
Vulnerability existence
```

from:

```text
Exposure
```

Determine network exposure before making external-risk claims.

---

# Lab 69 — Compensating Control Is Claimed

## Scenario

A team says:

> "The firewall protects this vulnerable service."

### Known

* A compensating control is claimed.

### Unknown

* Actual enforcement.
* Scope of the control.
* Exceptions.
* Internal accessibility.
* Whether the control addresses the relevant attack path.

### Decision

Treat the control as environmental context requiring appropriate evidence.

---

# Lab 70 — Finding Is Present on an Asset With Unknown Business Importance

## Scenario

A vulnerability is validated, but the asset has no business classification.

### Decision

Technical finding status can be established.

Business priority remains partially unresolved.

Document the missing context rather than assigning arbitrary importance.

---

# Lab 71 — Different Teams Provide Different Facts

## Scenario

Network team says:

> "Port 443 is open."

System team says:

> "The service is disabled."

Nessus reports intermittent observations.

### Decision

Do not choose a statement simply because it came from a particular team.

Correlate:

```text
Network evidence
+
Host evidence
+
Service evidence
+
Nessus evidence
+
Timing
```

Then document the observed state and remaining uncertainty.

---

# Lab 72 — Scan Window Is Too Short

## Scenario

The authorized maintenance window ends before the scan can complete.

### Known

* Full assessment cannot safely finish within the window.

### Unknown

* Whether partial results are sufficient for the objective.

### Decision

Do not extend beyond the approved window without authorization.

Either:

* stop safely and document coverage
* obtain an approved extension
* reschedule the assessment

---

# Lab 73 — A Finding Has Changed Severity

## Scenario

A later assessment shows a different severity for the same issue.

### Known

* Severity changed.

### Unknown

* Plugin logic changed.
* Environmental information changed.
* Configuration changed.
* Asset context changed.

### Decision

Investigate the reason for the change before interpreting it as a change in underlying risk.

---

# Lab 74 — Retest Finds Nothing on a Rebuilt Host

## Scenario

The original system was replaced.

The replacement has no finding.

### Known

* Original asset no longer exists.
* Replacement has been assessed.

### Unknown

* Whether remediation occurred on the original system.
* Whether the replacement has equivalent architecture.

### Decision

Record the asset lifecycle change.

Do not describe the result simply as "patched."

---

# Lab 75 — A Finding Cannot Be Reproduced

## Scenario

A previously observed finding cannot be reproduced.

### Known

* Historical evidence exists.
* Current testing does not reproduce it.

### Unknown

* Whether the condition was transient.
* Whether the target changed.
* Whether the detection environment changed.

### Decision

Compare historical and current conditions before closing the issue.

Possible status:

```text
Changed condition / unable to reproduce
```

---

# Lab 76 — Remediation Is Partially Complete

## Scenario

A configuration issue affected 100 systems.

The owner says 70 systems have been fixed.

### Known

* Remediation is claimed for part of the population.

### Unknown

* Which 70 systems.
* Whether the remaining 30 are still affected.

### Decision

Track remediation at the asset level where possible.

Do not mark the entire finding population remediated.

---

# Lab 77 — One Root Cause Produces Many Findings

## Scenario

A single outdated component produces multiple Nessus findings.

### Known

* Several findings point to the same underlying component.

### Unknown

* Whether one remediation resolves all of them.
* Whether dependencies remain.

### Decision

Investigate the shared root cause.

### Learning Point

Finding count and remediation count are different dimensions.

---

# Lab 78 — A New Host Appears in a Recurring Assessment

## Scenario

A recurring scan previously covered 20 hosts.

The current run discovers 23.

### Known

* Three additional hosts are visible.

### Unknown

* Whether they are newly deployed, previously missed, or unauthorized.

### Decision

Verify authorization and inventory before automatically adding them to the assessment baseline.

---

# Lab 79 — A Host Disappears From a Recurring Assessment

## Scenario

A previously observed host no longer appears.

### Known

* Host disappeared from the current result set.

### Unknown

* Decommissioning.
* DNS change.
* Network segmentation.
* Scanner access issue.
* Target-list change.

### Decision

Investigate the disappearance before treating it as asset retirement.

---

# Lab 80 — The Assessment Objective Is Missing

## Scenario

You receive:

```text
Please scan these servers.
```

No objective is provided.

### Known

* Targets may be identifiable.
* A scan has been requested.

### Unknown

* Discovery?
* Vulnerability assessment?
* Configuration assessment?
* Compliance?
* External exposure?
* Authenticated assessment?

### Decision

Clarify the assessment question before selecting the workflow.

### Learning Point

A target list alone does not define an assessment.

---

# Lab 81 — Multiple Objectives Are Mixed Together

## Scenario

The request says:

> "Check these servers for vulnerabilities, compliance, exposed services, and configuration problems."

### Known

* Multiple objectives exist.

### Unknown

* Which objectives have priority.
* Which workflows and evidence are required.

### Decision

Separate the objectives into explicit assessment workflows.

For example:

```text
Discovery
+
Vulnerability Assessment
+
Configuration / Compliance
```

Do not assume one scan configuration answers every question equally well.

---

# Lab 82 — Assessment Is Technically Complete but Operationally Unclear

## Scenario

Nessus reports:

```text
Completed
```

No one knows whether the results were reviewed.

### Known

* Technical execution completed.

### Unknown

* Result quality.
* Coverage.
* Findings investigated.
* Validation performed.
* Reporting status.

### Decision

Treat scan completion as the end of execution, not the end of assessment.

---

# Lab 83 — Report Is Ready but One Critical Finding Is Unresolved

## Scenario

The report deadline is today.

One significant finding has conflicting evidence.

### Decision

Include the finding with its uncertainty and validation status if it remains relevant.

Do not convert uncertainty into certainty simply to produce a cleaner report.

---

# Lab 84 — Remediation Changes the Asset

## Scenario

The owner removes the vulnerable service rather than patching it.

### Known

* Original vulnerable service is no longer expected to exist.

### Unknown

* Whether the service is actually inaccessible.
* Whether an alternative service exposes the same risk.

### Decision

Retest the relevant exposure and verify the intended state.

### Learning Point

Remediation can change the condition being assessed rather than simply changing a version number.

---

# Lab 85 — A Finding Is Not Applicable After Architecture Change

## Scenario

The affected component was removed from the system during a redesign.

### Known

* Current architecture no longer contains the component.

### Decision

Document the architectural change and mark the historical finding appropriately.

Do not describe the result simply as "patched" if patching did not occur.

---

# Lab 86 — The Same Finding Returns Later

## Scenario

A finding was closed after remediation.

Months later it reappears.

### Known

* Historical remediation occurred.
* Current assessment detects the condition again.

### Unknown

* Configuration drift.
* Software reinstallation.
* Deployment change.
* Failed maintenance process.

### Decision

Treat it as a reappearance and investigate the control that allowed recurrence.

---

# Lab 87 — A Finding Exists but the Asset Is No Longer Authorized

## Scenario

A historical report contains a vulnerable asset that is no longer in the current scope.

### Decision

Do not carry the asset into the current assessment merely because it appeared historically.

Historical data and current authorization are separate.

---

# Lab 88 — Assessment Conditions Are Not Comparable

## Scenario

Two assessments differ in:

* target scope
* scanner position
* authentication
* configuration
* plugin state

### Decision

Do not perform a simplistic trend comparison.

First determine which differences prevent direct comparison.

### Learning Point

A change in results is meaningful only when the assessment conditions are understood.

---

# Lab 89 — Evidence Is Available but Context Is Missing

## Scenario

You have a screenshot or exported finding showing a vulnerability.

No date, target identity, scanner, or assessment context is available.

### Known

* Some evidence exists.

### Unknown

* When it was collected.
* Against which target.
* Under which configuration.
* Whether it is still current.

### Decision

Preserve the evidence but qualify its use until context is recovered.

---

# Lab 90 — The Next Action Is Not Obvious

## Scenario

You have:

```text
A confirmed finding
+
Unknown business impact
+
Known internal exposure
+
Known remediation owner
```

### Question

What should happen next?

### Reasoning

Not every unknown must be resolved before action.

Technical validity is established.

Business impact may affect prioritization but does not necessarily block remediation discussion.

### Next Action

Document the confirmed technical condition, obtain business context, and coordinate remediation according to the environment's process.

---

# Advanced Partial-Information Labs

The following scenarios require combining multiple incomplete-information problems.

---

# Lab 91 — Partial Scope + Partial Authentication

## Scenario

A 30-host assessment has:

* 28 hosts observed
* 24 hosts authenticated
* 4 hosts unauthenticated
* 2 hosts missing

### Question

Can the assessment be reported as complete?

### Decision

No.

### Reasoning

There are at least two coverage gaps:

```text
2 hosts not observed
4 hosts without authenticated coverage
```

The assessment may still contain useful findings, but completeness must be qualified.

---

# Lab 92 — Partial Scope + Finding Disappearance

## Scenario

A previous assessment found a vulnerability on a host.

The current assessment does not report the host at all.

### Question

Was the vulnerability remediated?

### Decision

Not established.

### Reasoning

The host itself is missing from current coverage.

The absence of the finding cannot be interpreted independently of the missing host.

---

# Lab 93 — Authentication Changed + Finding Disappeared

## Scenario

Original assessment:

```text
Authenticated
Finding present
```

Current assessment:

```text
Unauthenticated
Finding absent
```

### Decision

Do not conclude remediation.

### Reasoning

The assessment perspective changed.

---

# Lab 94 — Scanner Position Changed + Port Disappeared

## Scenario

An internal scan saw port 445.

An external scan does not.

### Decision

Do not automatically conclude the service was removed.

### Reasoning

Network perspective changed.

---

# Lab 95 — Plugin Update + Remediation Claim

## Scenario

A finding disappears immediately after both:

* a plugin update
* a claimed remediation

### Question

Which caused the change?

### Decision

The available information is insufficient to attribute causality.

### Next Action

Review plugin behavior and obtain independent evidence of the target state.

---

# Lab 96 — Production Impact + Incomplete Scan

## Scenario

A scan causes suspected production instability and is stopped.

The results are incomplete.

### Decision

Prioritize operational safety and preserve the partial assessment evidence.

Document:

* why the scan stopped
* observed impact
* coverage achieved
* targets not completed
* configuration
* next approved action

Do not resume blindly.

---

# Lab 97 — Critical Finding + Validation Constraint

## Scenario

A high-impact finding appears on a production system.

Direct validation may disrupt the system.

### Decision

Use the least-impact evidence available first.

Possible evidence sources:

```text
Nessus plugin evidence
↓
Package/version evidence
↓
Configuration evidence
↓
Service evidence
↓
Approved non-disruptive verification
```

If uncertainty remains, report it explicitly.

---

# Lab 98 — Remediation Claimed + Asset Rebuilt

## Scenario

The administrator says:

> "We fixed the issue by rebuilding the server."

The replacement server has a different hostname and IP.

### Decision

Treat this as an asset lifecycle change.

Verify:

* replacement identity
* authorization
* architecture
* vulnerable component state
* exposure
* relevant configuration

Do not automatically equate rebuild with remediation.

---

# Lab 99 — Broad Scope + Unknown Production Systems

## Scenario

Authorization says:

```text
All systems in Network X
```

Network X contains production and development assets.

No production exclusions are documented.

### Decision

Do not assume all systems can be scanned identically.

Clarify operational constraints, exclusions, assessment windows, and target handling before execution.

---

# Lab 100 — Final Conclusion With Multiple Unknowns

## Scenario

An assessment has:

```text
95% target coverage
90% authentication coverage
2 unresolved significant findings
1 scope ambiguity
Incomplete business criticality data
```

A stakeholder asks:

> "Can we close the assessment?"

### Decision

Do not reduce this to a yes/no technical conclusion.

Assess whether the remaining gaps are acceptable under the assessment's defined success criteria and governance process.

Document:

```text
Coverage:
Partial

Authentication:
Partial

Significant unresolved findings:
2

Scope:
One unresolved ambiguity

Business context:
Incomplete

Conclusion:
Qualified based on documented limitations
```

### Learning Point

A professional conclusion reflects both evidence and uncertainty.

---

# Partial-Information Challenge Matrix

Use this matrix when practicing.

| Situation                | Primary Unknown       | First Question                         |
| ------------------------ | --------------------- | -------------------------------------- |
| Missing authorization    | Permission to act     | Am I authorized?                       |
| Missing host             | Coverage              | Was the host reachable?                |
| Missing service          | Network/service path  | Is the service observable?             |
| Failed authentication    | Access capability     | Where does authentication fail?        |
| Finding disappears       | Cause of change       | What assessment condition changed?     |
| High severity            | Applicability/context | Is the finding supported and relevant? |
| Remediation claimed      | Verification          | What evidence proves remediation?      |
| Scope changed            | Comparability         | Are the runs comparable?               |
| Plugin changed           | Detection behavior    | Did detection logic/content change?    |
| Scanner changed          | Perspective           | Is scanner position equivalent?        |
| DNS changed              | Target identity       | What system was actually assessed?     |
| Production impact        | Operational safety    | Should assessment continue?            |
| Missing owner            | Accountability        | Who owns remediation?                  |
| Missing business context | Priority              | What context is required?              |
| Incomplete scan          | Coverage              | What was actually assessed?            |

---

# Partial-Information Workflow

When facing an unfamiliar situation, use this sequence:

```text
1. STOP ASSUMING
       ↓
2. LIST KNOWN FACTS
       ↓
3. LIST UNKNOWN FACTS
       ↓
4. IDENTIFY DECISION-CRITICAL UNKNOWNS
       ↓
5. CHECK AUTHORIZATION
       ↓
6. CHECK OPERATIONAL CONSTRAINTS
       ↓
7. DETERMINE WHETHER WORK CAN SAFELY CONTINUE
       ↓
8. CHOOSE LOWEST-IMPACT EVIDENCE-GATHERING ACTION
       ↓
9. PREDICT EXPECTED RESULT
       ↓
10. OBSERVE ACTUAL RESULT
       ↓
11. UPDATE THE FACT SET
       ↓
12. RECORD REMAINING UNCERTAINTY
       ↓
13. MAKE THE NEXT DECISION
```

---

# Practical Lab — Build Your Own Partial-Information Cases

Create at least five scenarios from your own authorized Nessus lab.

For each scenario, intentionally remove one important piece of information.

Examples:

```text
Remove:
- scanner position
- authentication status
- business criticality
- exact target identity
- plugin/content state
- remediation evidence
- network exposure
```

Then complete:

```text
Known:
-

Unknown:
-

Decision-Critical Unknown:
-

What I Need to Verify:
-

Lowest-Impact Verification:
-

Expected Result:
-

Actual Result:
-

Remaining Uncertainty:
-

Next Decision:
-
```

Do not fill unknown fields with guesses.

---

# Practical Lab — Three-Level Uncertainty Drill

Take one assessment scenario and solve it three times.

## Level 1 — Low Uncertainty

Provide:

* scope
* objective
* target
* scanner position
* authentication
* configuration
* expected result

Make the decision.

---

## Level 2 — Moderate Uncertainty

Remove:

* scanner position
* business criticality
* authentication status

Determine which missing information matters immediately and which can wait.

---

## Level 3 — High Uncertainty

Remove:

* exact scope
* authorization details
* objective
* authentication
* scanner position
* target identity

Determine what must be resolved before any assessment activity can begin.

### Goal

Learn to distinguish:

```text
Information that improves the assessment
```

from:

```text
Information required before action is authorized
```

---

# Practical Lab — Unknown vs Assumption

Create a table:

| Statement                               | Classification                           |
| --------------------------------------- | ---------------------------------------- |
| "The target is authorized."             | Known only if supported by authorization |
| "The host is down because ping failed." | Unsupported assumption                   |
| "Authentication succeeded on Host A."   | Known if supported by evidence           |
| "No finding means no vulnerability."    | Unsupported conclusion                   |
| "The owner says it was patched."        | Reported claim                           |
| "The retest confirmed remediation."     | Conclusion requiring evidence            |
| "The scanner is internal."              | Known only if verified                   |
| "The firewall caused the timeout."      | Hypothesis until supported               |
| "The assessment covered all hosts."     | Conclusion requiring coverage evidence   |
| "Business criticality is unknown."      | Explicit unknown                         |

The goal is to train yourself to recognize hidden assumptions.

---

# Practical Lab — Evidence Escalation

For an uncertain finding, attempt to resolve it using increasingly stronger evidence:

```text
Level 1
Nessus finding details
        ↓
Level 2
Plugin evidence
        ↓
Level 3
Target version/configuration evidence
        ↓
Level 4
Service-level evidence
        ↓
Level 5
Approved non-destructive validation
        ↓
Level 6
Controlled testing
```

At each level ask:

```text
Did this evidence resolve the uncertainty?
```

If yes, stop.

Do not perform additional testing merely because it is possible.

---

# Practical Lab — Coverage Uncertainty

Take one completed assessment and answer:

```text
Authorized targets:
-

Targets actually reached:
-

Targets not reached:
-

Hosts discovered:
-

Expected hosts missing:
-

Unexpected hosts:
-

Authenticated hosts:
-

Unauthenticated hosts:
-

Services observed:
-

Expected services missing:
-

Scan errors:
-

Assessment interruptions:
-

Coverage limitations:
-
```

Then answer:

> What conclusion can I safely make from this assessment?

---

# Practical Lab — Retest Uncertainty

Take a historical finding and compare the original assessment with the retest.

Record:

```text
Original scope:
-

Retest scope:
-

Original scanner position:
-

Retest scanner position:
-

Original authentication:
-

Retest authentication:
-

Original configuration:
-

Retest configuration:
-

Original plugin/content state:
-

Retest plugin/content state:
-

Target changes:
-

Observed result:
-

Can remediation be independently established?
-

Why?
-
```

This exercise teaches whether the retest actually tests the same condition.

---

# Professional Partial-Information Record

For real assessment work, maintain a concise uncertainty record:

```text
Assessment:
Date:

Question:
-

Known Facts:
-

Unknown Facts:
-

Assumptions:
-

Decision-Critical Unknowns:
-

Authorization Constraints:
-

Operational Constraints:
-

Evidence Available:
-

Evidence Missing:
-

Verification Action:
-

Expected Result:
-

Observed Result:
-

Interpretation:
-

Remaining Uncertainty:
-

Impact on Coverage:
-

Impact on Findings:
-

Impact on Priority:
-

Next Action:
-

Owner:
-

Date / Time:
-
```

---

# Common Mistakes

## Mistake 1 — Guessing Missing Information

Bad:

> "The host is probably production."

Better:

> "Production status is not established."

---

## Mistake 2 — Treating a Claim as Proof

Bad:

> "The administrator said it was patched, so the finding is closed."

Better:

> "Remediation was reported; independent verification remains pending."

---

## Mistake 3 — Treating Missing Findings as Proof

Bad:

> "The finding disappeared, so the vulnerability is fixed."

Better:

> "The finding was not observed in the current assessment; assessment conditions and evidence must be compared."

---

## Mistake 4 — Ignoring Assessment Perspective

Bad:

> "The vulnerability disappeared."

Better:

> "The vulnerability was not observed from the current assessment perspective."

---

## Mistake 5 — Expanding Scope to Resolve Uncertainty

Bad:

> "I found another host, so I'll scan it."

Better:

> "The host was discovered; authorization must be verified before additional assessment."

---

## Mistake 6 — Performing High-Impact Validation Too Early

Bad:

> "The evidence is incomplete, so I'll test the vulnerability directly."

Better:

> "Use the least-impact evidence capable of resolving the uncertainty."

---

## Mistake 7 — Hiding Uncertainty in the Final Report

Bad:

> "All systems were assessed."

when coverage was incomplete.

Better:

> "Assessment coverage was partial; the following targets were not successfully assessed."

---

## Mistake 8 — Confusing Unknown With False

```text
Unknown
≠
False
```

If evidence is insufficient, preserve the uncertainty.

---

# Decision Rules

Use these rules repeatedly:

### Rule A

```text
If authorization is uncertain:
STOP and verify authorization.
```

### Rule B

```text
If technical coverage is uncertain:
Investigate coverage before drawing broad conclusions.
```

### Rule C

```text
If finding applicability is uncertain:
Investigate and validate using the least-impact method available.
```

### Rule D

```text
If remediation is claimed but unverified:
Treat remediation as a claim until verified.
```

### Rule E

```text
If assessment conditions changed:
Check comparability before interpreting result differences.
```

### Rule F

```text
If business context is missing:
Do not invent business priority.
```

### Rule G

```text
If operational impact is suspected:
Follow safety and escalation procedures before continuing.
```

### Rule H

```text
If uncertainty remains:
Document it explicitly.
```

---

# Completion Criteria

You have completed this lab when you can independently:

* distinguish known facts from assumptions
* identify decision-critical unknowns
* determine which unknowns block action
* distinguish authorization uncertainty from technical uncertainty
* investigate missing target coverage
* reason about partial authentication
* account for scanner position
* account for DNS and network changes
* distinguish exposure from vulnerability
* distinguish severity from priority
* interpret version evidence cautiously
* recognize plugin/content changes
* recognize configuration drift
* recognize assessment-perspective changes
* determine whether two assessments are comparable
* investigate disappearing findings
* investigate reappearing findings
* distinguish remediation claims from verification
* document unresolved findings
* document incomplete coverage
* avoid expanding scope without authorization
* choose low-impact evidence-gathering actions
* recognize when operational safety overrides assessment completion
* preserve uncertainty instead of inventing certainty
* determine the next defensible action from incomplete information

---

# Final Mental Model

When information is incomplete, do not ask:

> "What is probably happening?"

Ask:

```text
WHAT DO I ACTUALLY KNOW?
        ↓
WHAT DO I NOT KNOW?
        ↓
WHICH UNKNOWN CHANGES MY DECISION?
        ↓
AM I AUTHORIZED TO ACT?
        ↓
WHAT CONSTRAINTS APPLY?
        ↓
CAN I SAFELY PROCEED?
        ↓
WHAT IS THE LEAST-IMPACT WAY TO GATHER MORE EVIDENCE?
        ↓
WHAT SHOULD I EXPECT?
        ↓
WHAT ACTUALLY HAPPENED?
        ↓
WHAT DOES THE EVIDENCE SUPPORT?
        ↓
WHAT REMAINS UNKNOWN?
        ↓
WHAT IS THE NEXT DEFENSIBLE ACTION?
```

The professional skill being developed here is not certainty.

It is **controlled decision-making under uncertainty**.
