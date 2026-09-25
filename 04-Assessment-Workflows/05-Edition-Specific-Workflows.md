# Edition-Specific Workflows

## Objective

Learn how to adapt Nessus workflows to the capabilities actually available in the installed **Nessus edition, version, license, and environment**.

By the end of this workflow, you should be able to:

* Identify the Nessus product context before designing an assessment.
* Distinguish workflow principles from edition-specific capabilities.
* Avoid assuming that every Nessus installation has the same features.
* Determine whether a required capability is actually available.
* Adapt an assessment when a desired feature is unavailable.
* Understand how licensing, version, plugins, and configuration can affect workflow choices.
* Avoid building assessments around undocumented assumptions.
* Document edition-specific limitations.
* Decide when a different workflow, scanner, or product capability is required.

---

# 1. Why Edition Awareness Matters

Nessus is not a single identical environment for every user.

The capabilities available to you can depend on factors such as:

```text id="w1k2p8"
Nessus Product / Edition
        +
Version
        +
License / Subscription
        +
Plugin / Content Availability
        +
Operating Environment
        +
Scanner Configuration
        ↓
Available Capabilities
```

Therefore, a workflow that works on one installation may not be available or appropriate on another.

A professional operator does not begin with:

> "I know where this feature is."

Instead:

> "What capability does this assessment require, and is it available in this environment?"

---

# 2. The Core Principle

Use this rule throughout the repository:

```text id="b4d7qx"
ASSESSMENT OBJECTIVE
        ↓
REQUIRED CAPABILITY
        ↓
IS CAPABILITY AVAILABLE?
        ↓
YES                  NO
 ↓                    ↓
USE IT          ADAPT WORKFLOW
                      ↓
              DOCUMENT LIMITATION
```

The objective remains stable.

The implementation may change.

---

# 3. Product Context Comes Before Workflow

Before designing an unfamiliar assessment, identify:

* Nessus product/edition.
* Nessus version.
* License or subscription state where relevant.
* Scanner type.
* Operating environment.
* Available plugins/content.
* Relevant integrations or management capabilities.
* Any organizational restrictions.

Record what you actually observe.

Example:

```text id="0h6f0v"
Product:
<observed Nessus product / edition>

Version:
<observed version>

License:
<observed license state>

Scanner:
<observed scanner>

Assessment Objective:
<assessment question>
```

Do not guess missing information.

---

# 4. Version and Edition Are Different Variables

A useful distinction:

### Edition

Determines the product capabilities or licensing context available to you.

### Version

Determines the specific software release and its current implementation.

Therefore:

```text id="zq5jgd"
Same Edition
+
Different Version
=
Potentially Different UI / Behavior / Features
```

And:

```text id="k2l9ws"
Same Version
+
Different Edition / License
=
Potentially Different Capabilities
```

Both must be considered.

---

# 5. Do Not Memorize the UI

A common beginner approach is:

> "The button should be here because I saw it in a tutorial."

This is unreliable.

UI elements can change because of:

* Version.
* Edition.
* License.
* User permissions.
* Product configuration.
* Feature availability.
* UI redesign.

Instead, learn the underlying workflow:

```text id="d8b2v1"
Objective
   ↓
Capability
   ↓
Configuration
   ↓
Execution
   ↓
Evidence
   ↓
Decision
```

The interface is only the mechanism used to implement it.

---

# 6. Capability-First Thinking

Suppose your assessment requires:

```text id="l6jq8n"
Capability:
Authenticated host-level vulnerability assessment
```

Do not immediately search for a specific button.

Determine:

1. Is authenticated assessment supported?
2. What target platform is involved?
3. What credential method is required?
4. What permissions are required?
5. What workflow provides the necessary evidence?
6. Is the required capability available in this installation?

Then configure the assessment.

---

# 7. Feature Availability Is an Assessment Constraint

Suppose the assessment requires a capability that your installation does not provide.

Do not silently substitute another workflow and present it as equivalent.

Instead:

```text id="1zj9u0"
Required Capability
       ↓
Unavailable
       ↓
Determine Alternative
       ↓
Does Alternative Answer Same Question?
       ↓
YES → Use and Document
NO  → Change Scope / Environment / Product
```

This preserves assessment integrity.

---

# 8. Do Not Assume Edition Names Are Enough

Product names and licensing structures can change over time.

Therefore, this repository intentionally avoids teaching:

```text id="0n9w6f"
"Edition X always has feature Y."
```

unless that relationship has been verified for the relevant version and product context.

Instead, use:

```text id="x7n5z4"
Observed Product
+
Observed Version
+
Observed License
+
Available Feature
```

This makes the workflow resilient to product changes.

---

# 9. Workflow Categories Still Apply

Even when editions differ, the core assessment categories remain useful:

```text id="t8i3ql"
Discovery
   ↓
Unauthenticated Vulnerability Assessment
   ↓
Authenticated Vulnerability Assessment
   ↓
Configuration / Compliance
   ↓
Specialized / Edition-Specific Capability
```

The edition may affect how each category is implemented.

The assessment question remains the foundation.

---

# 10. Specialized Capabilities

Some Nessus environments may expose capabilities that are not present in every installation.

Examples can include specialized workflows involving:

* Different assessment types.
* Compliance-oriented checks.
* Advanced credentialing.
* Specific target platforms.
* Specialized scan templates.
* Reporting or management functions.
* Organizational integrations.
* Additional operational controls.

The exact availability depends on the installed environment.

The correct approach is:

> Identify the capability from the current product environment instead of assuming it exists.

---

# 11. User Permissions Matter Too

A capability may exist in the product but still be unavailable to your user account.

Consider:

```text id="z4d0wk"
Feature Exists
      ↓
User Account
      ↓
Permission / Role
      ↓
Feature Accessible?
```

Therefore, distinguish:

```text id="g2k1mw"
Feature Does Not Exist
```

from:

```text id="v6y3qn"
Feature Exists But User Cannot Access It
```

These are different problems.

---

# 12. License State Can Matter

A product may be installed correctly but not have the required licensing state for a particular capability.

Use:

```text id="f3r7am"
Installed
   ↓
Running
   ↓
Activated / Licensed
   ↓
Required Capability Available?
```

If a feature is unavailable, determine whether the cause is:

* Product edition.
* License state.
* User permissions.
* Version.
* Configuration.
* Plugin/content availability.
* Unsupported target/workflow.

Do not immediately reinstall the product.

---

# 13. Plugin Availability Matters

Nessus functionality depends heavily on plugins and related content.

Therefore, an assessment can be affected by:

```text id="s5z2xj"
Plugin / Content State
       ↓
Available Checks
       ↓
Potential Evidence
       ↓
Assessment Coverage
```

If a finding or check is missing, investigate whether:

* Relevant plugins are available.
* Plugin updates are current.
* The check applies to the target.
* The plugin is enabled.
* The scan configuration permits it.

A missing result does not automatically indicate a missing product feature.

---

# 14. Distinguish Feature Problems From Configuration Problems

This distinction saves time.

### Example A

You cannot find a compliance workflow.

Possible causes:

```text id="d1v4ha"
Edition / License
User Permissions
Version
```

### Example B

The workflow exists but produces no expected findings.

Possible causes:

```text id="3t8qpk"
Target Reachability
Authentication
Plugin Coverage
Configuration
Target State
```

These are different troubleshooting paths.

---

# 15. Capability Verification Workflow

When you need an unfamiliar capability:

```text id="9m7jbe"
1. Define Assessment Requirement
        ↓
2. Identify Required Capability
        ↓
3. Identify Product / Edition
        ↓
4. Identify Version
        ↓
5. Check Current UI / Documentation
        ↓
6. Confirm Capability Availability
        ↓
7. Confirm User Permission
        ↓
8. Confirm License / Activation
        ↓
9. Configure
        ↓
10. Test in Authorized Environment
```

This is safer than assuming a tutorial matches your installation.

---

# 16. Documentation as Part of the Workflow

When a feature is unfamiliar or version-sensitive, use authoritative product documentation.

Prefer:

* Current Tenable documentation.
* Product release information.
* Official configuration guidance.
* Official plugin information.
* Your organization's approved documentation.

Third-party tutorials can be useful for orientation, but they may describe:

* Older versions.
* Different editions.
* Different licensing.
* Different UI.
* Deprecated workflows.

Treat them as supporting material rather than authoritative product truth.

---

# 17. Practical Rule for Documentation

When reading documentation, ask:

```text id="x8w0ls"
Does this document apply to:
        ↓
My product?
        ↓
My edition?
        ↓
My version?
        ↓
My target platform?
        ↓
My assessment objective?
```

If not, do not blindly copy the workflow.

---

# 18. Edition-Specific Workflow Record

Whenever a workflow depends on product-specific capabilities, record:

```text id="0f8q2a"
Product:
Edition:
Version:
License State:
User Role:
Required Capability:
Capability Available:
Relevant Configuration:
Target:
Assessment Objective:
Known Limitations:
Alternative Workflow:
```

This becomes particularly useful when troubleshooting.

---

# 19. Example — Required Capability Available

Suppose your assessment requires authenticated Linux vulnerability assessment.

You establish:

```text id="r6w9jm"
Product:
Supported Nessus installation

Capability:
Authenticated assessment

Target:
Authorized Linux lab

Authentication:
Supported SSH-based method

Result:
Capability available
```

Proceed normally.

The workflow is:

```text id="g0k6qr"
Objective
 ↓
Authenticated Workflow
 ↓
Credential Configuration
 ↓
Assessment
 ↓
Evidence
```

---

# 20. Example — Required Capability Unavailable

Suppose an assessment requires a feature unavailable in your current product context.

Do not pretend another feature provides the same result.

Instead:

```text id="5s4c9n"
Required Capability
       ↓
Unavailable
       ↓
Why?
       ├── Edition
       ├── License
       ├── Version
       ├── Permission
       └── Configuration
```

Then determine:

```text id="7r5bqa"
Can the assessment question still be answered?
        │
       YES
        ↓
Use documented alternative

       OR

        NO
        ↓
Use appropriate environment / product
```

---

# 21. Example — Feature Exists but Is Inaccessible

Suppose another administrator confirms that a capability exists, but your account cannot access it.

The correct conclusion is:

```text id="1p5r9k"
Feature:
Available

User Access:
Insufficient

Action:
Determine required role/permission
```

Do not conclude:

```text id="c0z8xj"
Nessus does not support the feature.
```

---

# 22. Edition-Specific Troubleshooting

Use this decision tree:

```text id="m2k8ye"
Expected Capability Missing
          ↓
Check Product / Edition
          ↓
Check Version
          ↓
Check License / Activation
          ↓
Check User Permissions
          ↓
Check Plugin / Content Availability
          ↓
Check Configuration
          ↓
Check Documentation
          ↓
Determine Actual Cause
```

Only after these checks should you decide that the capability is genuinely unavailable.

---

# 23. Practical Lab 1 — Identify Your Environment

## Objective

Build a product-context record for your Nessus installation.

Record:

```text id="m7m6fu"
Product:
Edition:
Version:
License / Activation State:
Operating Environment:
Scanner:
User Role:
Available Assessment Types:
Available Templates / Workflows:
```

Then answer:

1. Which assessment workflows are available?
2. Which capabilities require special permissions?
3. Which workflows depend on authentication?
4. Which capabilities appear edition-specific?
5. Which information is uncertain and needs verification?

---

# 24. Practical Lab 2 — Capability Mapping

Create a table:

| Assessment Requirement           | Required Capability                 | Available? | Evidence | Alternative |
| -------------------------------- | ----------------------------------- | ---------- | -------- | ----------- |
| Network discovery                | Discovery workflow                  |            |          |             |
| Remote vulnerability assessment  | Vulnerability workflow              |            |          |             |
| Authenticated Linux assessment   | SSH-based authentication            |            |          |             |
| Authenticated Windows assessment | Windows authentication              |            |          |             |
| Configuration assessment         | Configuration/compliance capability |            |          |             |
| Recurring assessment             | Scheduling capability               |            |          |             |
| Reporting/export                 | Available result/report mechanism   |            |          |             |

The objective is not to memorize feature availability.

The objective is to learn how to verify it.

---

# 25. Practical Lab 3 — Version-Aware Documentation

Choose one capability whose UI or behavior may vary by version.

Perform:

```text id="f1b4wy"
Current Product
       ↓
Current Version
       ↓
Official Documentation
       ↓
Compare Instructions
       ↓
Current UI
```

Record:

```text id="p3av9n"
Capability:
Documentation Version:
Installed Version:
Instructions Match:
Differences:
Current Implementation:
```

This develops an important professional skill:

> Recognizing when documentation is version-specific.

---

# 26. Practical Lab 4 — Missing Capability Investigation

## Scenario

You expect a capability to be available but cannot find it.

Do not immediately conclude that your edition lacks it.

Work through:

```text id="9l4e0p"
1. Product
2. Edition
3. Version
4. License
5. User Role
6. Configuration
7. Plugin / Content State
8. Official Documentation
```

Then record:

```text id="y2m5qk"
Expected Capability:
Observed:
Product:
Edition:
Version:
License:
User Role:
Documentation Evidence:
Root Cause:
Alternative:
```

---

# 27. Practical Lab 5 — Adapt an Assessment

## Scenario

Your original assessment plan requires a capability that is unavailable in the current environment.

Your task:

1. Define the original assessment question.
2. Identify the unavailable capability.
3. Determine why it is unavailable.
4. Determine whether an alternative workflow can answer the same question.
5. If yes, document the alternative.
6. If no, identify what environment or capability is required.

Record:

```text id="e9t4pj"
Original Question:
Required Capability:
Unavailable Because:
Alternative Considered:
Does Alternative Answer the Same Question?
Evidence:
Final Workflow:
Limitation:
```

---

# 28. Practical Lab 6 — Compare Environments

If you have access to two authorized Nessus environments with different product contexts, compare:

```text id="7h8s3m"
Environment A
      ↓
Capabilities
      ↓
Environment B
      ↓
Capabilities
      ↓
Assessment Impact
```

Focus on:

* Available workflows.
* Authentication capabilities.
* Configuration/compliance capabilities.
* Scheduling.
* Reporting.
* User permissions.
* Operational controls.

Do not assume that every difference is caused by edition.

Version, licensing, permissions, and configuration may also contribute.

---

# 29. Professional Decision Record

For an edition-specific assessment, record:

```text id="n4u3h5"
Assessment Objective:
Target:
Product:
Edition:
Version:
License State:
User Role:
Required Capability:
Capability Available:
Configuration:
Authentication:
Plugin / Content State:
Assessment Workflow:
Result:
Limitations:
Alternative Considered:
Final Decision:
```

This allows another operator to understand why a particular workflow was chosen.

---

# 30. Common Mistakes

## Mistake 1 — Assuming Every Nessus Installation Is Identical

It is not.

---

## Mistake 2 — Copying a Tutorial Without Checking Version

The interface or workflow may have changed.

---

## Mistake 3 — Assuming Edition Alone Explains Every Difference

Version, license, permissions, configuration, and content can also matter.

---

## Mistake 4 — Confusing Missing Permission With Missing Feature

A feature may exist but be unavailable to your user.

---

## Mistake 5 — Reinstalling Before Investigating

A missing feature may be caused by licensing, role, version, or configuration.

---

## Mistake 6 — Treating Third-Party Tutorials as Current Product Documentation

Tutorials can become outdated.

---

## Mistake 7 — Inventing an Equivalent Workflow

If a required capability is unavailable, another workflow is not automatically equivalent.

---

## Mistake 8 — Ignoring Plugin or Content State

A missing assessment result can be caused by content availability rather than product capability.

---

## Mistake 9 — Hard-Coding Version-Specific Instructions Into a General Workflow

This creates documentation that becomes brittle quickly.

---

## Mistake 10 — Hiding Limitations

Professional documentation should explicitly state when a capability was unavailable or when an alternative was used.

---

# 31. Decision Rule

Use this process whenever an assessment depends on a product-specific capability:

```text id="1i7t8j"
What question must I answer?
            ↓
What capability is required?
            ↓
What product / edition am I using?
            ↓
What version?
            ↓
What license state?
            ↓
What permissions do I have?
            ↓
Is the capability available?
       │
      YES
       ↓
Configure and test

       OR

       NO
       ↓
Why unavailable?
       ↓
Can an alternative answer the same question?
       │
   ┌───┴───┐
  YES      NO
   ↓        ↓
Adapt     Obtain appropriate
and       capability/environment
document
```

The key rule is:

> Never confuse "I cannot currently use this capability" with "the product does not support this capability."

Determine the actual cause.

---

# 32. Edition-Aware Assessment Checklist

## Product Context

* [ ] Product identified.
* [ ] Edition identified.
* [ ] Version identified.
* [ ] License/activation state understood.
* [ ] Scanner environment identified.
* [ ] User role understood.

## Assessment Design

* [ ] Assessment question defined.
* [ ] Required capability identified.
* [ ] Required evidence identified.
* [ ] Target authorized.

## Capability Verification

* [ ] Capability checked in current environment.
* [ ] Required permissions checked.
* [ ] License implications checked.
* [ ] Plugin/content requirements checked.
* [ ] Current documentation consulted when necessary.

## Workflow

* [ ] Appropriate workflow selected.
* [ ] Version-specific differences considered.
* [ ] Configuration documented.
* [ ] Authentication requirements understood.
* [ ] Limitations documented.

## Troubleshooting

* [ ] Missing capability investigated systematically.
* [ ] Feature vs permission distinguished.
* [ ] Feature vs configuration distinguished.
* [ ] Version differences considered.
* [ ] License differences considered.
* [ ] Alternative workflow evaluated.

## Closure

* [ ] Final workflow documented.
* [ ] Capability limitations documented.
* [ ] Alternative documented if used.
* [ ] Assessment interpretation accounts for limitations.

---

# 33. Completion Criteria

You have completed this workflow when you can independently:

* Identify the Nessus product and edition in use.
* Identify the installed version.
* Understand why edition and version both matter.
* Determine whether a required capability is actually available.
* Distinguish product limitations from permission problems.
* Distinguish licensing problems from configuration problems.
* Recognize plugin/content-related limitations.
* Find and interpret version-appropriate documentation.
* Adapt a workflow when a capability is unavailable.
* Determine whether an alternative actually answers the same assessment question.
* Document edition-specific limitations.
* Avoid making unsupported assumptions about product capabilities.
* Build assessments around objectives rather than UI labels.

The final test is:

> Given an unfamiliar Nessus installation and a new assessment requirement, can you determine what the environment actually supports, select an appropriate workflow, adapt when necessary, and clearly document any capability limitations without relying on assumptions from another Nessus version or edition?

If yes, the edition-specific workflow is understood.
