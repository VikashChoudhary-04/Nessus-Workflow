# Network and Target Problems

## Objective

Learn how to troubleshoot Nessus when the scanner is operational but cannot properly reach, identify, or assess the intended targets.

A Nessus assessment can fail even when the scanner itself is healthy.

The failure may exist somewhere along this path:

```text id="8x4m2q"
NESSUS SCANNER
      ↓
SCANNER NETWORK
      ↓
ROUTING
      ↓
FIREWALL / ACL
      ↓
TARGET NETWORK
      ↓
TARGET HOST
      ↓
TARGET SERVICE
      ↓
ASSESSMENT
```

By the end of this workflow, you should be able to:

* distinguish scanner health from target reachability
* troubleshoot routing and connectivity problems
* investigate firewall and ACL behavior
* understand TCP/UDP visibility limitations
* troubleshoot hostname and DNS problems
* investigate missing hosts
* investigate unexpected hosts
* troubleshoot service discovery problems
* recognize target-side filtering and rate limiting
* understand how network position affects Nessus results
* distinguish target failure from scanner failure
* determine when a scan should be rerun
* document network-related coverage limitations

---

# 1. Network Troubleshooting Mental Model

Do not start by changing Nessus scan settings.

Start with:

```text id="m6q2v8"
EXPECTED TARGET
      ↓
TARGET IDENTIFICATION
      ↓
ROUTE
      ↓
CONNECTIVITY
      ↓
PORT / SERVICE
      ↓
PROTOCOL
      ↓
APPLICATION RESPONSE
      ↓
NESSUS ASSESSMENT
```

Each layer answers a different question.

---

# 2. Define the Expected Target

Before troubleshooting, establish:

```text id="q7m4x2"
What target should Nessus reach?
```

Record:

* IP address
* hostname
* network range
* protocol
* expected service
* expected port
* authorized scope
* expected network location

Example:

```text id="c5n8w3"
Target:
10.10.10.20

Expected:
HTTPS
443/tcp

Purpose:
Authorized vulnerability assessment
```

Do not troubleshoot an address that was never confirmed as the intended target.

---

# 3. Scope Before Connectivity

A target being reachable does not mean it is authorized.

Use:

```text id="r8m3q5"
Authorization
     ↓
Scope
     ↓
Target
     ↓
Connectivity
```

Never expand a scan simply because another host appears reachable.

If Nessus discovers an unexpected host:

```text id="v4x7m2"
Unexpected Host
      ↓
Verify Authorization
      ↓
In Scope?
 ┌────┴────┐
YES       NO
 ↓         ↓
Assess    Do Not Assess
```

---

# 4. Determine the Scanner's Network Position

Network position is fundamental.

Ask:

```text id="h3q8m1"
Where is Nessus?
```

Examples:

```text id="n7c4x2"
Scanner
  ↓
Internal Network
  ↓
Target
```

versus:

```text id="p5m8q3"
Scanner
  ↓
External Network
  ↓
Internet-Facing Target
```

The same target can produce different observations from different network locations.

---

# 5. Network Perspective Changes Results

Consider a service protected by an internal firewall.

From the internet:

```text id="f2q7m4"
Port 443 → Reachable
Port 22  → Filtered
```

From an internal segment:

```text id="z6m3x8"
Port 443 → Reachable
Port 22  → Reachable
```

Both observations can be correct.

Therefore:

```text id="w5n8c2"
Nessus Result
     ↓
Scanner Position
     ↓
Network Controls
     ↓
Observed Exposure
```

Do not treat network visibility as an absolute property of the target.

---

# 6. Start With Basic Reachability

For an authorized target, establish whether the scanner can reach the host at all.

Possible checks include:

* routing
* ICMP where appropriate
* TCP connectivity
* UDP behavior where relevant
* DNS resolution
* service availability

Do not rely on one protocol as proof of reachability.

---

# 7. ICMP Is Not the Same as Host Reachability

A host may:

```text id="q3m7x8"
Block ICMP
```

while still allowing:

```text id="n5c2v6"
TCP 443
```

Therefore:

```text id="x8m4q1"
Ping Fails
      ≠
Host Definitely Down
```

Likewise:

```text id="r6m2z9"
Ping Succeeds
      ≠
All Services Reachable
```

Use the protocol relevant to the assessment objective.

---

# 8. Routing Problems

If the scanner cannot reach the target network, investigate the route.

Conceptually:

```text id="m7x4c2"
Scanner
  ↓
Default / Specific Route
  ↓
Gateway
  ↓
Target Network
```

Possible causes:

* missing route
* incorrect route
* wrong gateway
* network segmentation
* VPN not connected
* routing policy
* asymmetric routing

Do not change routing on production systems without authorization.

---

# 9. VPN Problems

A scanner may depend on a VPN to reach an assessment network.

Symptoms may include:

* targets suddenly unreachable
* previously reachable hosts disappear
* only some networks are inaccessible
* routes disappear
* DNS behavior changes

Troubleshooting:

```text id="c8m4x7"
VPN State
   ↓
Assigned Address
   ↓
Routes
   ↓
DNS
   ↓
Target Reachability
```

Compare with a known-good state.

---

# 10. Firewall and ACL Problems

A firewall can:

* block a connection
* reject a connection
* silently drop packets
* allow one source but block another
* allow one port but block another
* rate-limit traffic

Therefore distinguish:

```text id="y2q8m5"
Open
Closed
Filtered / Dropped
```

The exact interpretation depends on the protocol and scanning method.

---

# 11. Firewall Troubleshooting

If a target is unexpectedly unreachable:

```text id="v6m3x8"
Scanner
 ↓
Source IP
 ↓
Firewall / ACL
 ↓
Target
```

Ask:

* Is the scanner source allowed?
* Is the target network allowed?
* Is the required port allowed?
* Is traffic direction correct?
* Is a security device filtering the traffic?
* Was a rule recently changed?

Do not disable the firewall simply to make the scan work.

---

# 12. Source-Based Filtering

A target may allow:

```text id="k4n8q2"
Trusted Scanner IP → Service
```

but deny:

```text id="s7m3x5"
Other Source → Service
```

Therefore, testing connectivity from a different machine may not reproduce Nessus's behavior.

Always consider the scanner's source address.

---

# 13. Security Devices Between Scanner and Target

Traffic may pass through:

* firewalls
* routers
* load balancers
* intrusion-prevention systems
* web application firewalls
* proxies
* network access controls
* segmentation controls

These can alter what Nessus observes.

A finding or missing finding may therefore depend partly on the network path.

---

# 14. Load Balancers

A hostname may point to a load balancer rather than directly to one server.

Example:

```text id="x8m2q5"
Hostname
   ↓
Load Balancer
   ↓
Server A
Server B
Server C
```

A Nessus assessment may therefore observe:

* one service endpoint
* different backend responses
* changing server versions
* inconsistent behavior

If a finding appears inconsistently, investigate whether multiple backend systems exist.

---

# 15. NAT

Network Address Translation can affect visibility.

For example:

```text id="p3q7m1"
Scanner
   ↓
NAT
   ↓
Target
```

The target may see a translated source address rather than the scanner's original address.

This can affect:

* firewall rules
* logging
* source-based access
* routing
* attribution

Document relevant network architecture when it affects assessment interpretation.

---

# 16. DNS Problems

Nessus may receive a hostname as a target.

Investigate:

```text id="n6m4x8"
Hostname
   ↓
DNS Resolution
   ↓
IP Address
   ↓
Reachability
```

Potential problems:

* DNS record missing
* stale DNS record
* split-horizon DNS
* wrong DNS server
* VPN-specific DNS
* multiple addresses
* changed IP

---

# 17. Split-Horizon DNS

The same hostname may resolve differently depending on the network.

Example:

```text id="h5x2m9"
External DNS:
example.local → 203.0.113.10

Internal DNS:
example.local → 10.10.10.10
```

A scanner on the internal network may see a different target than an external scanner.

Therefore record the actual resolved address when investigating.

---

# 18. Multiple DNS Addresses

A hostname can resolve to multiple addresses.

For example:

```text id="m8q3v7"
app.example
 ├── 10.10.10.10
 ├── 10.10.10.11
 └── 10.10.10.12
```

One backend may be patched while another remains vulnerable.

If findings vary across runs, investigate whether the hostname resolves to multiple systems.

---

# 19. DNS Does Not Prove Service Availability

Successful DNS resolution means:

```text id="r4m8x2"
Name → Address
```

It does not prove:

```text id="c7q3n5"
Address → Reachable
```

or:

```text id="v2m8q4"
Port → Open
```

Keep these layers separate.

---

# 20. IP Address Changes

Cloud and dynamic environments may change addresses.

A previous assessment may target:

```text id="k8m3q1"
10.10.10.20
```

while the current hostname resolves to:

```text id="p6x4n9"
10.10.10.35
```

A finding difference may therefore reflect infrastructure change rather than remediation.

Track:

* hostname
* resolved address
* asset identity
* assessment date

---

# 21. Target Missing From Discovery

Suppose you expect:

```text id="w4q7m2"
Host A
Host B
Host C
```

but Nessus identifies:

```text id="n8x3c5"
Host A
Host C
```

Do not immediately conclude Host B is down.

Investigate:

```text id="m2v6q8"
Authorization
 ↓
Target Definition
 ↓
DNS / Routing
 ↓
Host Reachability
 ↓
Discovery Configuration
 ↓
Firewall
 ↓
Host State
```

---

# 22. Missing Host Does Not Prove Nonexistence

A host can be missing because:

* powered off
* network disconnected
* firewall filtering
* discovery configuration
* routing problem
* DNS problem
* wrong target definition
* temporary outage
* scanner source restricted

Therefore:

```text id="q7m4x2"
Not Observed
    ≠
Does Not Exist
```

---

# 23. Unexpected Host

Suppose a discovery assessment identifies:

```text id="x3n8m5"
10.10.10.50
```

but the approved scope contains only:

```text id="h6q2v9"
10.10.10.10 - 10.10.10.30
```

Do not automatically scan the new host.

Instead:

```text id="c4m7x8"
Unexpected Host
      ↓
Verify Ownership
      ↓
Verify Authorization
      ↓
Determine Scope
```

Only assess it if authorization permits.

---

# 24. Target List Problems

Target lists can contain:

* incorrect IPs
* stale hostnames
* duplicates
* malformed ranges
* unintended ranges
* missing systems

Review:

```text id="v5m2q8"
Target Input
   ↓
Resolved Targets
   ↓
Expected Targets
```

Do not assume the scanner interpreted your target list the way you intended.

---

# 25. CIDR and Range Mistakes

A small targeting error can dramatically expand scope.

Conceptually:

```text id="z8q3m4"
Single Host
```

is very different from:

```text id="n6x7c2"
Network Range
```

Before launching:

* verify network boundaries
* verify intended host count
* confirm exclusions
* confirm authorization

Target minimization is a safety control.

---

# 26. Hostname vs IP Targeting

Hostname targeting can be useful when infrastructure changes dynamically.

IP targeting can provide a stable technical reference when the asset identity is known.

The correct choice depends on the assessment objective.

Consider:

```text id="r7m3x9"
Does the objective follow:
Asset Identity?
       or
Current IP?
```

Document the choice when it affects reproducibility.

---

# 27. Port Problems

A service may be:

```text id="x5q8m2"
Open
Closed
Filtered
Unavailable
Intermittent
```

Nessus may therefore fail to identify a service that is only intermittently reachable.

Investigate:

* service state
* firewall behavior
* network path
* port configuration
* load balancing
* service startup state

---

# 28. Service Running but Not Reachable

A target-side service can be healthy locally while inaccessible remotely.

Example:

```text id="m4v8q3"
Application Running
        ↓
Local Access Works
        ↓
Remote Access Blocked
```

Possible causes:

* local firewall
* service bound only to localhost
* wrong interface
* network ACL
* security group
* segmentation

Do not conclude that the service is down based solely on remote failure.

---

# 29. Service Bound to the Wrong Interface

A service may listen on:

```text id="h8m2x6"
127.0.0.1
```

instead of:

```text id="q4n7v3"
Target Network Interface
```

The service is running but remotely unreachable.

This is a target-side configuration issue, not necessarily a Nessus problem.

---

# 30. Security Groups and Cloud Controls

Cloud environments can introduce controls such as:

* security groups
* network ACLs
* private endpoints
* routing tables
* load balancers
* service meshes

When a cloud target is unreachable:

```text id="c8m4q2"
Scanner
 ↓
Cloud Network Path
 ↓
Security Controls
 ↓
Target
```

Investigate the actual path.

Do not assume a public cloud hostname means the service is publicly reachable.

---

# 31. Host-Based Firewalls

A host may have its own firewall.

Examples include:

* Windows Firewall
* Linux firewall frameworks
* endpoint security controls
* application-level filtering

A scanner may be able to reach one port but not another.

Compare:

```text id="v7m2x9"
Expected Service
vs.
Observed Reachability
```

---

# 32. Rate Limiting

Targets may limit connection attempts.

Possible symptoms:

* intermittent connectivity
* inconsistent service discovery
* timeouts
* scans become slower over time
* repeated connection failures

Potential sources:

* firewall
* IPS
* application
* load balancer
* host protection
* service configuration

Do not immediately increase scan concurrency.

That may make rate limiting worse.

---

# 33. Connection Resets

A target may actively terminate connections.

Possible causes:

* application behavior
* firewall
* IPS
* overload
* rate limiting
* protocol mismatch

Investigate whether the reset is:

```text id="p4m8x2"
Consistent
or
Intermittent
```

Consistency can provide an important clue.

---

# 34. Timeouts

A timeout means the expected response was not received within the relevant period.

Possible causes:

* network loss
* filtering
* overloaded target
* slow service
* routing problem
* firewall behavior
* scanner resource pressure

Do not automatically increase timeouts.

First determine why responses are slow or absent.

---

# 35. UDP Troubleshooting

UDP differs from TCP because there is no connection handshake equivalent to TCP's SYN/ACK process.

Therefore:

```text id="y6q2m8"
No UDP Response
      ≠
UDP Service Definitely Closed
```

A UDP service may be silent when open.

Interpret UDP observations carefully.

---

# 36. TCP vs UDP Expectations

Use the protocol that matches the service.

Example:

```text id="m3x7q5"
HTTPS → TCP
DNS → Commonly UDP, may also use TCP
SNMP → Commonly UDP
```

Do not infer service state using the wrong protocol.

The exact service behavior can vary.

---

# 37. Service Identification Problems

Nessus may identify a service incorrectly or incompletely.

Possible reasons:

* unusual port
* custom application
* encrypted protocol
* proxy/load balancer
* non-standard banner
* service changed behavior
* filtering

Treat service identification as evidence, not absolute truth.

Validate important findings.

---

# 38. Non-Standard Ports

A service does not have to use its default port.

Example:

```text id="q8m4v2"
HTTPS
→ 8443/tcp
```

Do not assume:

```text id="n7x3c5"
443 = HTTPS
```

Port numbers are clues.

Service identification should use actual evidence.

---

# 39. TLS Inspection and Proxies

Traffic may pass through security devices that alter or inspect TLS.

Possible effects include:

* different certificate
* different service response
* connection interception
* protocol modification
* different observed hostname

If Nessus reports unexpected TLS information:

```text id="f5m8x2"
Confirm Network Path
        ↓
Confirm TLS Endpoint
        ↓
Identify Intermediary
        ↓
Interpret Finding
```

---

# 40. Web Application Firewalls

A WAF can affect web assessment behavior.

Possible effects:

* blocked requests
* challenge responses
* rate limiting
* altered responses
* different status codes
* intermittent detection

Therefore:

```text id="x4q7m8"
Nessus Finding
      ↓
Observed Through WAF
      ↓
May Reflect Protected Endpoint
```

This does not automatically mean the underlying application is secure.

---

# 41. IPS / IDS Interference

Security monitoring systems may:

* alert
* block
* rate-limit
* reset connections
* temporarily blacklist scanner traffic

If assessment behavior changes after scanning begins, investigate whether an intermediary reacted to the scanner.

Coordinate authorized testing appropriately.

---

# 42. Target-Side Resource Constraints

A vulnerable or fragile target may become slower during assessment.

Symptoms include:

* high CPU
* service restarts
* connection failures
* application errors
* timeouts

If this occurs:

```text id="c6m2x8"
Stop / Reduce Impact
        ↓
Confirm Target Health
        ↓
Review Assessment Configuration
        ↓
Coordinate Test Window
        ↓
Resume Only When Appropriate
```

Assessment safety takes precedence over scan completion.

---

# 43. Scan Impact and Network Stability

A scan can generate substantial traffic depending on:

* target count
* plugin coverage
* discovery configuration
* concurrency
* service behavior
* assessment type

Before increasing scan intensity, consider:

```text id="m7q4x2"
Target Capacity
Network Capacity
Assessment Window
Security Controls
Stop Conditions
```

More traffic does not necessarily produce better results.

---

# 44. Intermittent Targets

If a target is reachable sometimes but not always:

```text id="n8x3m5"
Time
 ↓
Reachable?
 ↓
Service State?
 ↓
Network State?
 ↓
Security Controls?
```

Record timestamps.

Intermittent problems are often difficult to diagnose without a timeline.

---

# 45. Target Reboot During Scan

If the target reboots during assessment:

```text id="q4m8x2"
Assessment Starts
      ↓
Target Available
      ↓
Target Reboots
      ↓
Connectivity Lost
      ↓
Target Returns
```

The final assessment may contain incomplete coverage.

Document the interruption.

Do not automatically interpret missing findings as absence.

---

# 46. Target Changed During Assessment

A target may be modified while Nessus is scanning it.

Examples:

* software upgraded
* service restarted
* firewall changed
* host replaced
* cloud instance redeployed

This can create inconsistent evidence.

Record significant changes that occurred during the assessment window.

---

# 47. Network Troubleshooting Workflow

Use:

```text id="w3n7m4"
1. Confirm Authorization
        ↓
2. Confirm Target
        ↓
3. Confirm Scanner Position
        ↓
4. Check DNS / Name Resolution
        ↓
5. Check Routing
        ↓
6. Check Basic Reachability
        ↓
7. Check Required Port
        ↓
8. Check Service
        ↓
9. Check Firewalls / ACLs
        ↓
10. Check Intermediaries
        ↓
11. Check Target Health
        ↓
12. Reassess Nessus Configuration
        ↓
13. Retest
        ↓
14. Document Coverage
```

---

# 48. Troubleshoot From the Outside In

A useful model:

```text id="x8m4q2"
Nessus
  ↓
Network
  ↓
Target Host
  ↓
Port
  ↓
Service
  ↓
Application
```

If the host itself cannot be reached, do not begin with application-level troubleshooting.

Move deeper only after the previous layer is working.

---

# 49. Practical Lab 1 — Missing Host

## Scenario

You expect three authorized lab hosts:

```text id="f5m8q3"
Host A
Host B
Host C
```

Nessus discovers only:

```text id="r7x2n6"
Host A
Host C
```

## Task

Investigate:

* target definition
* routing
* host state
* firewall
* discovery configuration
* network position

Determine why Host B was not observed.

Do not add unrelated hosts to the assessment.

---

# 50. Practical Lab 2 — DNS Problem

## Scenario

A hostname is included in the target list, but the expected host is not assessed.

## Task

Determine:

```text id="m4q8x2"
Hostname
 ↓
Resolved Address
 ↓
Expected Address
 ↓
Reachability
 ↓
Service
```

Record:

```text id="n7c3v5"
Hostname:
Expected IP:
Resolved IP:
Reachable:
Expected Service:
Observed Service:
Conclusion:
```

---

# 51. Practical Lab 3 — Firewall Restriction

## Scenario

The service works from one authorized system but not from the Nessus scanner.

## Task

Determine whether the scanner source is being filtered.

Check:

* scanner source address
* target firewall
* network ACL
* required port
* routing

Do not disable the firewall.

---

# 52. Practical Lab 4 — Service on Non-Standard Port

## Scenario

An authorized lab application runs on a non-standard TCP port.

## Task

Determine:

1. Is the port reachable?
2. What service is actually running?
3. Does Nessus identify it?
4. Does the assessment include the relevant port?
5. Are findings associated with the service?

Record the evidence.

---

# 53. Practical Lab 5 — Load-Balanced Target

## Scenario

A hostname resolves to multiple addresses.

## Task

Determine:

```text id="c6m2x8"
Hostname
 ↓
Addresses
 ↓
Backend Services
 ↓
Version Differences
 ↓
Finding Differences
```

If different backends produce different results, document the infrastructure difference.

---

# 54. Practical Lab 6 — Intermittent Reachability

## Scenario

A lab target is sometimes reachable and sometimes unavailable.

## Task

Create a timeline:

```text id="p8m3x5"
Time
Reachability
Port State
Service State
Nessus Observation
```

Determine whether the problem appears to be:

* target-side
* network-side
* scanner-side
* intermittent security-control behavior

---

# 55. Practical Lab 7 — Scanner Position Comparison

## Objective

Understand how network perspective changes assessment results.

Use an authorized lab environment with controlled network segmentation.

Perform comparable assessments from two permitted network positions.

Record:

```text id="v4q7m2"
Scanner Position A:
Visible Hosts:
Visible Services:

Scanner Position B:
Visible Hosts:
Visible Services:

Differences:
Explanation:
Assessment Implication:
```

Do not infer that one view represents the complete environment.

---

# 56. Network Troubleshooting Record

Use:

```text id="k7m3x8"
Network / Target Troubleshooting Record
=======================================

Assessment:

Target:

Authorization:

Scanner:

Scanner Network Position:

Expected IP:

Observed IP:

DNS:

Route:

Basic Reachability:

Required Port:

Observed Port State:

Service:

Firewall / ACL:

Intermediaries:

Target Health:

Authentication:

Observed Error:

Timeline:

Hypothesis:

Action:

Result:

Coverage Impact:

Root Cause:

Follow-Up:
```

---

# 57. Common Network Troubleshooting Mistakes

## Mistake 1 — Treating Ping Failure as Host Failure

### Problem

ICMP is blocked.

### Better approach

Test the protocol and service relevant to the assessment.

---

## Mistake 2 — Troubleshooting the Wrong Target

### Problem

A stale hostname or incorrect IP is being assessed.

### Better approach

Verify target identity first.

---

## Mistake 3 — Expanding Scope Because a Host Was Discovered

### Problem

Discovery is mistaken for authorization.

### Better approach

Verify authorization before assessment.

---

## Mistake 4 — Disabling Firewalls

### Problem

Security controls are removed to make the scan work.

### Better approach

Identify the specific blocked path and coordinate an authorized rule change if required.

---

## Mistake 5 — Increasing Scan Aggressiveness

### Problem

Timeouts lead to more traffic and higher concurrency.

### Better approach

Determine whether the target or network is rate-limiting or overloaded.

---

## Mistake 6 — Assuming DNS Resolution Means Reachability

### Problem

Hostname resolution succeeds, but the service is unavailable.

### Better approach

Test DNS, routing, connectivity and service separately.

---

## Mistake 7 — Ignoring Load Balancers

### Problem

Different backends produce different evidence.

### Better approach

Identify whether the hostname represents multiple endpoints.

---

## Mistake 8 — Ignoring Scanner Position

### Problem

An external scan is interpreted as a complete internal assessment.

### Better approach

Document the network perspective.

---

## Mistake 9 — Treating Missing Findings as Proof

### Problem

A target was unreachable, but the report says no vulnerability was found.

### Better approach

Document the coverage limitation.

---

## Mistake 10 — Ignoring Target Impact

### Problem

A fragile system becomes unstable during scanning.

### Better approach

Use conservative configuration and stop when target safety requires it.

---

# 58. Decision Tree

Use this troubleshooting tree:

```text id="r5x8m3"
Target Problem
      ↓
Is the target authorized?
      ↓
YES
      ↓
Is the target correctly identified?
      ↓
YES
      ↓
Does the scanner have a route?
      ↓
YES
      ↓
Can the scanner reach the host?
      ↓
YES
      ↓
Can the scanner reach the required port?
      ↓
YES
      ↓
Does the expected service respond?
      ↓
YES
      ↓
Is an intermediary altering traffic?
      ↓
YES / NO
      ↓
Check Nessus configuration
      ↓
Assess
```

If a step fails, troubleshoot that layer before proceeding.

---

# 59. When to Stop Troubleshooting and Change the Assessment

Sometimes the correct solution is not to keep increasing scanner aggressiveness.

Examples:

```text id="x4m7q2"
Target unstable
      ↓
Stop / Reschedule

Target unauthorized
      ↓
Do Not Scan

Network path unavailable
      ↓
Restore Authorized Connectivity

Service intentionally inaccessible
      ↓
Document Limitation

Scanner position incorrect
      ↓
Move Scanner / Change Authorized Perspective
```

Operational judgment matters as much as technical troubleshooting.

---

# 60. Coverage Impact

Every network problem should be translated into assessment impact.

Examples:

```text id="m8q3v5"
Host unreachable
      ↓
Host coverage incomplete
```

```text id="q6x2n8"
Port filtered
      ↓
Service assessment incomplete
```

```text id="v4m7c2"
DNS resolved to wrong asset
      ↓
Target identity uncertain
```

```text id="p5x8q3"
Load balancer hides backend variation
      ↓
Backend coverage may differ
```

This connects troubleshooting to reporting.

---

# 61. Completion Criteria

You have completed this workflow when you can independently:

* verify the intended target
* verify authorization and scope
* identify the scanner's network position
* troubleshoot routing
* troubleshoot DNS
* distinguish ICMP from actual service reachability
* investigate firewalls and ACLs
* investigate host-based filtering
* investigate security groups and cloud controls
* investigate load balancers
* investigate NAT
* investigate intermittent connectivity
* investigate TCP and UDP differences
* troubleshoot non-standard ports
* recognize service binding problems
* recognize rate limiting
* recognize target instability
* determine the effect of network problems on assessment coverage
* document unresolved reachability issues
* decide whether to continue, modify, reschedule or stop an assessment

---

# 62. Final Mental Model

Remember:

```text id="n3x8m5"
AUTHORIZATION
      ↓
TARGET IDENTITY
      ↓
SCANNER POSITION
      ↓
DNS
      ↓
ROUTING
      ↓
HOST REACHABILITY
      ↓
PORT
      ↓
SERVICE
      ↓
INTERMEDIARIES
      ↓
TARGET HEALTH
      ↓
NESSUS ASSESSMENT
      ↓
COVERAGE
```

When a Nessus target does not behave as expected, do not immediately ask:

> "Which Nessus setting should I change?"

Ask:

> **"At which layer does the expected path from scanner to target break, what evidence proves it, and what does that break mean for assessment coverage?"**
