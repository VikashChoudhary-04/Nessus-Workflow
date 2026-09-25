# Installation and Initialization

## Objective

Install and initialize Nessus so that the scanner is ready for authorized vulnerability-assessment work.

By the end of this workflow, you should be able to:

* identify the Nessus edition you are using
* obtain the appropriate installer
* install Nessus
* start the Nessus service/application
* access the Nessus web interface
* complete initial setup
* activate the product where required
* allow required initialization/update processes to complete
* confirm scanner readiness
* recognize common setup problems
* determine whether Nessus is actually ready to perform a scan

The goal is not simply:

> "Nessus is installed."

The goal is:

> **"Nessus is correctly initialized, healthy, activated/licensed as required, and ready for an authorized assessment."**

---

# 1. Before Installation

Do not begin by downloading an installer randomly.

First determine what you are actually installing.

Record:

```text
Nessus Edition:
____________________________

Operating System:
____________________________

Architecture:
____________________________

Nessus Version:
____________________________

License / Activation Method:
____________________________

Lab Scanner:
____________________________
```

If you do not yet know the edition or version, determine it before continuing.

---

# 2. Understand the Edition

Nessus functionality varies according to product edition, licensing, and version.

Do not assume that a feature documented for one Nessus product is automatically available in another.

Before installation, determine whether you are using a product such as:

* Nessus Essentials
* Nessus Professional
* Nessus Expert
* Nessus Manager

The exact capabilities available to you must be verified against the current Tenable documentation for your product/version.

### Decision

```text
Do I know my Nessus edition?

        YES
         ↓
Continue

        NO
         ↓
Determine edition first
```

Do not build an assessment workflow around a feature before confirming that your edition supports it.

---

# 3. Obtain Nessus From the Appropriate Source

Use Tenable's official distribution/documentation resources for the installer and current installation instructions.

Do not use:

* unofficial repackaged installers
* random download mirrors
* modified installers
* unknown third-party packages

For this repository, the installation procedure should always be checked against the current Tenable documentation because package names, supported operating systems, and installation procedures can change.

---

# 4. Choose the Installation Platform

Nessus can be deployed on supported operating systems and architectures.

The exact package depends on the platform.

Conceptually:

```text
Operating System
       ↓
Supported Nessus Version
       ↓
Correct Package
       ↓
Installation
```

Do not install a package merely because its filename looks similar to your operating system.

Verify:

```text
Operating System
+
Architecture
+
Nessus Version
```

before installation.

---

# 5. Installation Principle

The installation process should be treated as three separate stages:

```text
Install
   ↓
Initialize
   ↓
Verify
```

Do not confuse a successful installation with a ready scanner.

---

# 6. Install Nessus

Follow the current Tenable installation procedure for your operating system.

The exact commands or package-management steps depend on the platform and should therefore be taken from the current official Tenable installation documentation.

During installation, record:

```text
Installation Date:
____________________________

Nessus Version:
____________________________

Installation Platform:
____________________________

Installation Result:
____________________________
```

If the installation reports an error, do not continue to activation until the installation problem is understood.

---

# 7. Verify the Nessus Service

After installation, verify that Nessus is actually running.

Conceptually:

```text
Installation completed
        ↓
Is Nessus running?
        │
   ┌────┴────┐
   │         │
  YES        NO
   │         │
   ↓         ↓
Continue   Troubleshoot
```

A successful package installation does not guarantee that the Nessus service started successfully.

Check the service using the normal service-management mechanism for your operating system.

---

# 8. Access the Nessus Web Interface

Nessus is operated primarily through its web interface.

Open the Nessus interface using the address and port provided by the current installation documentation.

Do not assume that a web interface being reachable automatically means the scanner is ready.

You still need to complete initialization.

---

# 9. Complete Initial Setup

The initial setup normally establishes the information Nessus needs before normal operation.

Depending on the product/version, this can include:

* creating the initial administrator account
* selecting or confirming the product
* entering activation information where required
* allowing plugins/components to initialize
* accepting required terms
* waiting for initial setup to complete

Follow the current interface presented by your installation.

### Important rule

Do not skip initialization because you want to start scanning immediately.

The scanner must first reach a known-good state.

---

# 10. Activation and Licensing

Activation behavior depends on the Nessus edition.

The activation process may require:

* an activation code
* an account
* license information
* offline activation procedures
* another product-specific mechanism

Use the activation method associated with your edition.

Do not assume that:

```text
Nessus installed
```

means:

```text
Nessus activated
```

These are separate states.

---

# 11. Plugin Initialization and Updates

Nessus relies heavily on plugins to perform vulnerability and configuration checks.

After initial installation or activation, Nessus may need to:

* download plugins
* compile plugins
* initialize plugin data
* update plugin information
* perform other initialization tasks

During this stage, the scanner may not yet be ready for normal assessment work.

Use the Nessus interface to determine whether initialization/update activity has completed.

---

# 12. Why Plugin State Matters

Consider:

```text
Nessus installed
       ↓
Plugins not ready
       ↓
Assessment capability not fully ready
```

Therefore:

> **Do not interpret an uninitialized scanner as a production-ready vulnerability scanner.**

Before your first assessment, verify that the scanner has completed its required initialization/update process.

---

# 13. First Readiness Check

Before creating a scan, verify:

```text
[ ] Nessus installation completed
[ ] Nessus service is running
[ ] Web interface is accessible
[ ] Initial administrator setup is complete
[ ] Correct product/edition is selected
[ ] Activation/licensing is complete where required
[ ] Plugin initialization/update is complete
[ ] No blocking setup error is displayed
```

Only when these conditions are satisfied should you proceed to scan configuration.

---

# 14. The Scanner Readiness Model

Use this mental model:

```text
Installed?
   │
   ├── NO → Install
   │
   └── YES
         ↓
Running?
   │
   ├── NO → Troubleshoot service
   │
   └── YES
         ↓
Accessible?
   │
   ├── NO → Troubleshoot interface/network/service
   │
   └── YES
         ↓
Initialized?
   │
   ├── NO → Complete initialization
   │
   └── YES
         ↓
Activated/Licensed?
   │
   ├── NO → Complete applicable activation
   │
   └── YES
         ↓
Plugins Ready?
   │
   ├── NO → Wait/troubleshoot initialization
   │
   └── YES
         ↓
READY
```

This is more important than memorizing an installation command.

---

# 15. Verify the Installed Version

Once Nessus is accessible, identify the installed version.

Record:

```text
Nessus Version:
____________________________
```

This matters because the UI and capabilities can change between versions.

Throughout this repository, when a UI path or capability is version-sensitive, verify it against the version currently installed.

---

# 16. Verify the Edition

Identify the product/edition currently activated.

Record:

```text
Nessus Product:
____________________________

Edition:
____________________________
```

This becomes important later when deciding whether a particular workflow is available.

For example:

```text
Assessment Requirement
        ↓
Does my edition support it?
        │
   ┌────┴────┐
   │         │
  YES        NO
   │         │
Continue   Choose another supported workflow
```

Never design a workflow around an unavailable capability.

---

# 17. Verify Administrative Access

Confirm that you can:

* log in
* access the main Nessus interface
* access scan functionality
* access relevant settings
* view the scanner state

Do not change advanced configuration yet.

At this stage, the objective is **readiness**, not optimization.

---

# 18. Do Not Tune Nessus Yet

A common beginner mistake is immediately changing:

* advanced settings
* performance settings
* plugin selections
* scanner configuration
* concurrency
* timeouts
* discovery behavior

before understanding what the defaults do.

For the first assessment:

> **Prefer the default configuration unless the assessment objective requires a specific change.**

Later modules will teach when configuration changes are justified.

---

# 19. Troubleshooting: Installation Failed

### Symptom

The installation does not complete.

### Check

```text
Operating system
Architecture
Installer/package
Permissions
System requirements
Installation logs
Existing Nessus installation
```

### Decision

```text
Correct package and supported platform?
        │
   ┌────┴────┐
   │         │
  NO        YES
   │         │
Correct    Investigate
package    installation error
```

### Verify

The Nessus service should be installed and capable of starting.

Do not continue until the installation is healthy.

---

# 20. Troubleshooting: Nessus Service Is Not Running

### Symptom

The web interface is unavailable.

### Possible causes

* service did not start
* service stopped
* installation problem
* configuration problem
* operating-system issue
* resource problem
* port conflict

### Check

```text
Is the Nessus service installed?
Is it running?
Did it start successfully?
Are there service errors?
Is another process using the required port?
```

### Fix

Address the actual cause rather than repeatedly refreshing the browser.

### Verify

Confirm:

```text
Service running
      +
Web interface accessible
```

---

# 21. Troubleshooting: Web Interface Unavailable

### Symptom

The Nessus service appears to be running but the interface cannot be reached.

### Check

```text
Service status
Listening port
Local firewall
Binding/interface
Browser connection
Host networking
```

Determine whether the problem is:

```text
Nessus service
      OR
Network/interface
      OR
Browser/client
```

Do not assume the scanner itself is broken.

---

# 22. Troubleshooting: Activation Problem

### Symptom

Activation does not complete.

### Check

```text
Correct product
Correct activation information
Network connectivity
DNS
System time
Proxy requirements
License/account state
Tenable service availability
```

If the environment requires an offline activation process, follow the current Tenable procedure for that product.

### Verify

The product should show the expected activated/licensed state.

---

# 23. Troubleshooting: Plugins Are Not Ready

### Symptom

Nessus is installed and accessible, but plugin initialization/update is incomplete.

### Check

```text
Internet connectivity
DNS
Proxy configuration
Activation state
Plugin update status
Available disk space
System resources
Nessus logs/status
```

### Decision

```text
Initialization still progressing normally?
        │
   ┌────┴────┐
   │         │
  YES        NO
   │         │
Wait      Investigate
```

Do not repeatedly restart Nessus simply because initialization takes time.

---

# 24. Troubleshooting: Version or Edition Mismatch

### Symptom

A tutorial says a feature should exist, but you cannot find it.

### Do not immediately assume:

> "Nessus is broken."

First determine:

```text
Which Nessus version am I using?
Which edition am I using?
Is the feature supported by this edition?
Has the UI changed?
Is the feature licensed separately?
```

Use current Tenable documentation to verify.

---

# 25. Practical Exercise — Installation Record

Create your own installation record:

```text
Nessus Product:
________________________________

Edition:
________________________________

Version:
________________________________

Operating System:
________________________________

Architecture:
________________________________

Activation Status:
________________________________

Plugin Status:
________________________________

Web Interface:
________________________________

Scanner Status:
________________________________
```

Do not fill fields with assumptions.

Use the actual Nessus interface/system state.

---

# 26. Practical Exercise — Readiness Decision

Imagine the following state:

```text
Nessus installed: YES
Service running: YES
Web interface: YES
Activation: YES
Plugins: INITIALIZING
```

Should you start your first vulnerability scan?

```text
Decision:
________________________________
```

Explain why.

---

# 27. Practical Exercise — Diagnose the State

Consider:

```text
Nessus installed: YES
Service running: YES
Web interface: NO
```

What should you investigate first?

Choose the category:

```text
A. Vulnerability plugins
B. Service/network/interface availability
C. Scan policy
D. Target credentials
```

Then explain what evidence you would check.

---

# 28. Practical Exercise — Feature Availability

Suppose a future workflow requires a Nessus capability you cannot find.

Use this process:

```text
Feature missing
      ↓
Check Nessus version
      ↓
Check Nessus edition
      ↓
Check current Tenable documentation
      ↓
Check licensing/capability requirements
      ↓
Determine whether the feature is actually available
```

Do not invent a replacement workflow until you understand why the feature is unavailable.

---

# 29. First-Scan Readiness Gate

You are ready to proceed to the first scan only when:

```text
[ ] Nessus is installed
[ ] Nessus service is running
[ ] Web interface works
[ ] Administrator access works
[ ] Product/edition is known
[ ] Version is known
[ ] Activation/licensing is complete where required
[ ] Plugins are ready
[ ] No blocking errors remain
[ ] Authorized lab target exists
[ ] Lab scope is defined
```

The last two conditions come from the previous foundation module.

Nessus readiness alone is not enough.

---

# 30. Completion Criteria

You have completed this file when you can independently:

* determine which Nessus edition you are using
* determine the installed version
* install Nessus using the appropriate official procedure
* start and verify the Nessus service
* access the web interface
* complete initial setup
* complete applicable activation
* determine whether plugins are ready
* recognize that installation and readiness are different states
* troubleshoot basic setup failures
* determine whether a feature may be unavailable because of version or edition
* verify that Nessus is ready before creating a scan

Your final mental model should be:

```text
Install
   ↓
Initialize
   ↓
Activate / License
   ↓
Update / Initialize Plugins
   ↓
Verify Scanner
   ↓
Verify Edition + Version
   ↓
Verify Lab Scope
   ↓
READY FOR FIRST ASSESSMENT
```

Do not proceed to scanning merely because the Nessus web interface opens.

A scanner is ready when the **platform, product state, plugins, target, authorization, and assessment conditions** are ready.
