# Discovery

## Objective

Learn how to use Nessus for authorized discovery and turn discovery results into informed assessment decisions.

By the end of this file, you should be able to:

* Explain the purpose of discovery.
* Define an appropriate discovery objective.
* Build a discovery scope from authorization.
* Distinguish host discovery from vulnerability assessment.
* Interpret discovered hosts, ports, and services.
* Identify expected, missing, and unexpected systems.
* Understand discovery limitations.
* Decide when discovery should lead to a vulnerability assessment.
* Avoid treating discovery results as proof of security or vulnerability.
* Document discovery results professionally.

---

# 1. What Discovery Is

Discovery answers a foundational question:

> **"What systems and services are observable within my authorized scope?"**

A simplified workflow is:

```text id="n4d7x2"
AUTHORIZED SCOPE
       ↓
TARGET RANGE / TARGET LIST
       ↓
DISCOVERY
       ↓
HOSTS
       ↓
PORTS / SERVICES
       ↓
TECHNOLOGY OBSERVATIONS
       ↓
NEXT ASSESSMENT DECISION
```

Discovery establishes an environment picture.

It does not automatically establish whether those systems are vulnerable.

---

# 2. Discovery vs Vulnerability Assessment

These are different activities.

## Discovery

Answers:

> "What is present or reachable?"

## Vulnerability Assessment

Answers:

> "What security weaknesses can Nessus identify under these assessment conditions?"

Example:

```text id="h2k9m5"
Discovery
↓
192.168.56.20 is reachable
↓
TCP/22 appears open
↓
SSH service identified
```

That does **not** automatically mean:

```text id="q7m3v1"
SSH is vulnerable.
```

The discovery result provides evidence about exposure.

A vulnerability assessment requires additional checks.

---

# 3. Discovery Objective

Before creating a discovery assessment, define what you need to learn.

Possible objectives:

* Identify reachable hosts.
* Identify exposed ports.
* Identify visible services.
* Build or verify an asset inventory.
* Determine whether expected systems are online.
* Prepare targets for a later vulnerability assessment.
* Investigate an authorized network segment.

Avoid vague objectives such as:

> "Discover everything."

Instead define a measurable question.

Example:

> "Identify the reachable hosts and exposed network services within the authorized lab subnet."

---

# 4. Authorization Comes First

Discovery is still active interaction with systems.

Therefore:

```text id="a6p8z3"
Authorization
     ↓
Scope
     ↓
Discovery
```

Do not scan an unknown public network simply because you want to see what is there.

Only perform discovery against:

* Systems you own.
* Systems you are explicitly authorized to assess.
* Intentionally vulnerable training environments.
* Other environments where the required permission is established.

---

# 5. Define the Discovery Scope

Start with:

```text id="u8c2n6"
Authorized Scope
```

Then determine:

```text id="w4m7r1"
Actual Discovery Targets
```

Use the smallest scope that answers the question.

For example:

```text id="j3x9q5"
Objective:
Identify hosts in a lab segment

Authorized:
192.168.56.0/24

Discovery:
192.168.56.0/24
```

If the objective concerns only three hosts, a full subnet may be unnecessary.

---

# 6. Discovery Workflow

A practical discovery process is:

```text id="p5n8c2"
1. Confirm authorization
        ↓
2. Define objective
        ↓
3. Define scope
        ↓
4. Configure discovery
        ↓
5. Review target
        ↓
6. Launch
        ↓
7. Monitor
        ↓
8. Review discovered hosts
        ↓
9. Review ports/services
        ↓
10. Compare against expectations
        ↓
11. Decide next action
```

The final step is important.

Discovery should produce a decision.

---

# 7. Discovery Configuration

The exact Nessus discovery options depend on the installed version, edition, and selected workflow.

Relevant concepts may include:

* Host discovery.
* Port scanning.
* Service identification.
* Operating-system identification.
* Network enumeration.
* Additional protocol/service detection.

Do not assume every option exists in every Nessus installation.

Use the controls actually available in your environment.

---

# 8. Host Discovery

Host discovery determines which systems appear reachable or observable.

Conceptually:

```text id="m7v2q4"
Target Scope
     ↓
Host Discovery
     ↓
Host A → Responding
Host B → Responding
Host C → Not Observed
```

Important:

> **Not observed does not automatically mean nonexistent.**

A host can be missed because of:

* Firewall behavior.
* Filtering.
* Routing.
* Host state.
* Network segmentation.
* Discovery method.
* Scanner visibility.

---

# 9. Discovery and False Negatives

A discovery result is influenced by the method used.

Suppose:

```text id="k3n8w6"
Host exists
+
Host blocks discovery traffic
=
Host may not appear as expected
```

Therefore:

> **Absence from discovery results is not automatically proof that the system does not exist.**

If an expected system is missing, investigate.

---

# 10. Port Discovery

Once a host is identified, Nessus may determine which ports are accessible.

Conceptually:

```text id="f8x4p2"
Host
 ↓
Port Discovery
 ↓
Open / Accessible Ports
 ↓
Service Identification
```

Example:

```text id="r7m1c9"
192.168.56.20

22/tcp
80/tcp
443/tcp
```

These are observations.

They are not automatically vulnerabilities.

---

# 11. Service Identification

Nessus can use network information to identify services.

Example:

```text id="d4q8y3"
TCP/22
   ↓
SSH

TCP/80
   ↓
HTTP

TCP/443
   ↓
HTTPS
```

Service identification helps determine what should be assessed next.

It can also provide context for vulnerability checks.

---

# 12. Ports vs Services

Do not confuse:

```text id="s2m6v8"
Port
```

with:

```text id="h9q3x1"
Service
```

A port is a network endpoint.

A service is the application/protocol behavior associated with that endpoint.

For example:

```text id="x4v7k2"
443/tcp
   ↓
HTTPS service
```

The port alone does not establish the exact software or version.

---

# 13. Service Identification Is Not Always Perfect

Network services can:

* Use non-standard ports.
* Hide or alter banners.
* Proxy traffic.
* Use TLS.
* Change behavior based on requests.
* Restrict unauthenticated identification.

Therefore:

> **Service identification should be treated as evidence, not absolute truth.**

If the result matters, validate it through appropriate authorized methods.

---

# 14. Operating-System Identification

Depending on the workflow and available detection methods, Nessus may attempt to infer the target operating system.

Conceptually:

```text id="w5c9m3"
Network Evidence
      ↓
OS Identification
      ↓
Likely OS
```

Treat this as an identification result with confidence and limitations.

Do not assume every OS identification is perfect.

---

# 15. Discovery Output

A useful discovery record looks like:

```text id="q8n4v6"
Host:
IP:
Hostname:
Reachable:
Open / Accessible Ports:
Detected Services:
Possible OS:
Important Observations:
```

For multiple systems, use a table.

| Host   | Reachable | Ports  | Services  | OS Observation |
| ------ | --------- | ------ | --------- | -------------- |
| Host A | Yes       | 22, 80 | SSH, HTTP | Linux-like     |
| Host B | Yes       | 443    | HTTPS     | Unknown        |
| Host C | No        | —      | —         | Unknown        |

The exact information available depends on the discovery workflow.

---

# 16. Expected vs Observed Inventory

One of the most useful discovery exercises is comparing expected and observed assets.

Example:

### Expected

```text id="z5c8r2"
192.168.56.20
192.168.56.21
192.168.56.22
```

### Observed

```text id="m2q7x4"
192.168.56.20
192.168.56.21
192.168.56.30
```

Now investigate:

```text id="v8n3p6"
Expected but missing:
192.168.56.22

Observed but unexpected:
192.168.56.30
```

Do not automatically expand the assessment.

---

# 17. Unexpected Host Decision

Use:

```text id="c4m9x2"
Unexpected Host
      ↓
Is it inside authorization?
      │
 ┌────┴────┐
 NO       YES
 │          │
Do not     Verify
assess     objective
             ↓
          Include if
          appropriate
```

If authorization is unclear:

> **Stop and resolve the scope question.**

---

# 18. Missing Host Decision

For an expected but missing host:

```text id="n6r2w8"
Expected Host
      ↓
Not Observed
      ↓
Investigate
```

Possible causes:

* Host offline.
* Firewall.
* Routing.
* Network segmentation.
* Incorrect IP.
* DNS issue.
* Discovery limitation.
* Temporary service/network condition.

Do not treat the missing host as assessed.

---

# 19. Discovery and Target Validation

Discovery can validate assumptions about the target environment.

For example:

You were told:

> "192.168.56.20 is the web server."

Discovery shows:

```text id="j8p3v5"
192.168.56.20
22/tcp → SSH
80/tcp → HTTP
443/tcp → HTTPS
```

This supports the expectation that the system exposes web services.

But if discovery instead shows:

```text id="f5m7c1"
192.168.56.20
3389/tcp → RDP
```

you should investigate the mismatch before proceeding.

---

# 20. Discovery Can Change the Next Step

Discovery should influence decisions.

Example:

```text id="s3q9m6"
Discovery
   ↓
Web service found
   ↓
External web vulnerability assessment
```

Another:

```text id="p7x4k2"
Discovery
   ↓
SSH service found
   ↓
Determine whether authenticated Linux assessment is required
```

Another:

```text id="v1m8n5"
Discovery
   ↓
Database service found
   ↓
Determine whether database-specific assessment is in scope
```

Discovery does not decide the next step automatically.

It provides evidence for the decision.

---

# 21. Discovery Does Not Prove Security

Suppose discovery finds:

```text id="d2q6x8"
22/tcp
80/tcp
443/tcp
```

You cannot conclude:

> "The host is secure."

Likewise, finding no open ports does not prove that the system is secure.

The correct conclusion is limited:

> "These services were observable under the discovery conditions."

---

# 22. Discovery Does Not Prove Vulnerability

Suppose:

```text id="k7n4m2"
8080/tcp
```

is identified.

Do not conclude:

> "The application on 8080 is vulnerable."

You need appropriate vulnerability assessment evidence.

---

# 23. Discovery and Attack Surface

Discovery helps establish an attack-surface picture.

Conceptually:

```text id="h3r8p1"
Host
 ├── Port 22
 │     └── SSH
 │
 ├── Port 80
 │     └── HTTP
 │
 └── Port 443
       └── HTTPS
```

This can help prioritize which assessment workflows are relevant.

The discovery output is an input to the next decision.

---

# 24. Discovery vs Enumeration

In practical security work, these terms can overlap.

For this repository, use the distinction:

### Discovery

Establish:

* Hosts.
* Reachability.
* Basic exposed services.

### Enumeration

Gather deeper information about a discovered service or system.

Nessus can provide information that overlaps these concepts, depending on the workflow.

The important thing is not terminology.

The important thing is knowing what information you actually obtained.

---

# 25. Discovery and Service Exposure

For each discovered service, ask:

```text id="u4x8m7"
What is exposed?
        ↓
Is it expected?
        ↓
Is it authorized?
        ↓
Does it require further assessment?
        ↓
What workflow answers that question?
```

This turns raw discovery data into an assessment plan.

---

# 26. Discovery Configuration Trade-Offs

Discovery can involve choices about:

* Which hosts to identify.
* Which ports to examine.
* How much service identification to perform.
* How much additional enumeration to attempt.

More extensive discovery may provide more information but can also increase:

* Scan duration.
* Network traffic.
* Target interaction.
* Result volume.

Use the minimum discovery depth that answers the question.

---

# 27. Discovery and Production Systems

In production, consider:

* Network sensitivity.
* Monitoring systems.
* IDS/IPS.
* Rate limits.
* Device stability.
* Legacy equipment.
* Fragile services.
* Maintenance windows.

Some systems can react unexpectedly to scanning traffic.

Therefore:

> **Discovery is not automatically harmless simply because it is not exploitation.**

Follow the approved scope and operational constraints.

---

# 28. Discovery Troubleshooting

If expected hosts are missing:

```text id="g8v2c5"
Target Correct?
      ↓
Network Reachable?
      ↓
Routing Correct?
      ↓
Firewall Filtering?
      ↓
Host Online?
      ↓
Discovery Method Appropriate?
      ↓
Nessus Scanner Healthy?
```

If services are missing:

```text id="n3m7x1"
Host Reachable?
      ↓
Port Accessible?
      ↓
Service Running?
      ↓
Service Filtering?
      ↓
Identification Method Appropriate?
```

---

# 29. Discovery Result Validation

When a discovery result is important, validate it through another authorized source when practical.

Examples:

* Known asset inventory.
* System owner information.
* Host configuration.
* Authorized network documentation.
* Service configuration.
* Controlled lab knowledge.

Do not assume Nessus must be the only source of truth.

---

# 30. Practical Exercise 1 — Single Host Discovery

Choose one authorized lab host.

Run an appropriate discovery-oriented assessment.

Record:

```text id="p4x8c6"
Host:
Reachable:
Hostname:
Ports:
Services:
OS Observation:
Unexpected Findings:
```

Then compare the results with what you already know about the system.

---

# 31. Practical Exercise 2 — Small Subnet Discovery

Use an isolated authorized lab subnet.

For example:

```text id="r7m2v9"
192.168.56.0/24
```

Perform discovery.

Create:

| IP | Reachable | Ports | Services | Expected? |
| -- | --------- | ----- | -------- | --------- |
|    |           |       |          |           |
|    |           |       |          |           |
|    |           |       |          |           |

Then identify:

* Expected hosts.
* Missing hosts.
* Unexpected hosts.
* Services requiring further assessment.

---

# 32. Practical Exercise 3 — Service-Based Next Step

Choose one discovered service.

For example:

```text id="k5q8n3"
HTTP
```

Answer:

```text id="s1m6x4"
What was discovered?
What does the evidence actually prove?
What does it not prove?
What vulnerability-assessment question should come next?
Which workflow could answer it?
```

This exercise teaches you to turn discovery into a decision.

---

# 33. Practical Exercise 4 — Expected Host Missing

Use a lab where you know an authorized host exists.

Temporarily make it unavailable or otherwise prevent discovery from seeing it.

Run discovery.

Determine:

```text id="x9p2m7"
Expected:
Observed:
Missing:
Likely Explanation:
Additional Validation:
Next Action:
```

Restore the lab environment afterward.

---

# 34. Practical Exercise 5 — Unexpected Service

Choose an authorized lab host where you can safely introduce a controlled service change.

For example:

```text id="m4v8c2"
Before:
Known services

Change:
Authorized lab configuration change

After:
Discovery result
```

Compare the two discovery results.

Explain:

* What changed.
* Why Nessus observed a difference.
* What vulnerability assessment should follow.

---

# 35. Practical Exercise 6 — Build an Attack-Surface Map

Using your discovery results, create:

```text id="h7n3q9"
HOST A
 ├── 22/tcp → SSH
 ├── 80/tcp → HTTP
 └── 443/tcp → HTTPS

HOST B
 └── 3389/tcp → RDP
```

Then add:

```text id="c2m8x5"
Next Assessment:
Reason:
```

for each host/service where further assessment is appropriate.

---

# 36. Common Mistakes

## Mistake 1 — Treating Discovery as Vulnerability Assessment

Bad:

> "I found the service, so I found the vulnerability."

Better:

> "I identified an exposed service that may require further assessment."

---

## Mistake 2 — Expanding Scope Automatically

Bad:

> "Nessus discovered another host, so I'll scan it."

Better:

> "I will verify authorization and relevance before adding it."

---

## Mistake 3 — Treating Missing Hosts as Secure

Bad:

> "Nessus didn't find the host, so there is no problem."

Better:

> "The host was not observed under these discovery conditions."

---

## Mistake 4 — Assuming Service Identification Is Perfect

Bad:

> "Nessus says Apache, therefore it must be Apache."

Better:

> "Nessus identified the service as Apache based on available evidence; important conclusions should be validated when necessary."

---

## Mistake 5 — Over-Scanning

Bad:

> "I'll discover every possible port and service because more data is always better."

Better:

> "I'll use appropriate discovery depth for the assessment objective and environment."

---

# 37. Professional Discovery Record

For an important discovery assessment:

```text id="z6m3q8"
## Discovery Assessment

### Objective
[What discovery should determine]

### Authorization
[Authorized scope]

### Targets
[Targets]

### Discovery Configuration
[Important configuration]

### Execution
[Start / end / final state]

### Hosts Observed
[List]

### Expected Hosts
[List]

### Missing Hosts
[List]

### Unexpected Hosts
[List]

### Services Observed
[Summary]

### Important Observations
[Observations]

### Limitations
[Known limitations]

### Next Assessment
[What should happen next and why]
```

---

# 38. Professional Decision Rule

After discovery, complete:

> **"Discovery established __________. The important uncertainty is __________. Based on the evidence, the next assessment should be __________ because __________."**

Example:

> "Discovery established that the authorized server exposes HTTP and HTTPS. The important uncertainty is whether the web services contain identifiable vulnerabilities. Based on the evidence, the next assessment should be an appropriate vulnerability assessment of the authorized host because service exposure alone does not establish vulnerability."

---

# 39. Discovery Checklist

Before launch:

```text id="u5r8m2"
[ ] Authorization confirmed
[ ] Objective defined
[ ] Scope defined
[ ] Targets verified
[ ] Discovery depth appropriate
[ ] Operational impact considered
[ ] Expected hosts identified
```

After execution:

```text id="j2c7x9"
[ ] Final state checked
[ ] Hosts reviewed
[ ] Missing hosts identified
[ ] Unexpected hosts identified
[ ] Ports reviewed
[ ] Services reviewed
[ ] Important observations recorded
[ ] Limitations recorded
[ ] Next assessment decision made
```

---

# 40. Completion Criteria

You have completed this file when you can independently:

* Define a discovery objective.
* Build a discovery scope from authorization.
* Explain host discovery.
* Explain port and service discovery.
* Distinguish ports from services.
* Interpret discovery results as observations rather than conclusions.
* Identify expected, missing, and unexpected hosts.
* Handle unexpected systems without automatically expanding scope.
* Recognize discovery limitations.
* Use discovery results to choose a sensible next assessment.
* Document discovery professionally.
* Explain why discovery does not prove either security or vulnerability.

The final test is:

> **Given an authorized network or host scope, can you perform discovery, determine what is actually observable, identify important discrepancies, understand the limitations of the results, and turn the discovery evidence into the next appropriate vulnerability-assessment decision?**

If yes, you are ready for the next workflow: **unauthenticated vulnerability assessment**.
