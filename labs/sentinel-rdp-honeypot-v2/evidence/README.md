# Sentinel RDP Honeypot v2 Evidence Governance

## Purpose

This directory defines the evidence-governance model for the Sentinel RDP
Honeypot v2 lab.

The model is designed to ensure that implementation, validation, detection,
containment, teardown, cost, and portfolio claims remain traceable to reviewed
evidence. It also separates private raw evidence from public, sanitized
artifacts committed to Git.

This directory does not prove that the Azure environment has been deployed,
that testing has occurred, or that any control has been validated.

## Current project state

At the creation of this evidence-governance baseline:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection validation has not started.
- Teardown validation has not started.
- Cost reconciliation has not started.
- No v2 execution evidence has been captured.
- Public implementation and validation claims remain design-stage only.

No evidence record may imply a later project state until the corresponding
activity has occurred and passed the required evidence review.

## Governing principles

Evidence handling for this lab follows these principles:

1. Evidence must support a defined test, control, finding, or claim.
2. Raw evidence remains private and outside Git.
3. Only sanitized and explicitly approved evidence may enter the public
   repository.
4. Evidence lifecycle status and test outcome are separate fields.
5. Failed, blocked, and inconclusive tests remain traceable.
6. Successful retests do not erase earlier failed attempts.
7. Evidence must preserve enough context to be reproducible and reviewable.
8. Sanitization must not materially alter the technical meaning of an artifact.
9. Integrity records should use SHA-256 where appropriate.
10. Public evidence must not exceed the claims it supports.
11. Safety and containment take priority over evidence preservation.
12. Private course material must not be copied into the repository.

## Evidence boundary

### Private evidence outside Git

The following material must remain in a private evidence location outside the
repository:

- raw screenshots;
- raw command output;
- unredacted Azure Portal exports;
- unredacted Azure CLI or PowerShell output;
- raw Windows event records;
- raw Log Analytics or Microsoft Sentinel query results;
- tenant IDs and subscription IDs;
- complete Azure resource IDs;
- active public IP addresses, DNS names, or endpoints;
- usernames, email addresses, account identifiers, or credential material;
- budget, billing, invoice, payment, or credit details;
- private course material;
- third-party scripts or datasets pending provenance and license review;
- unreviewed evidence containing unknown sensitive information;
- evidence rejected for public release.

Private storage paths must not be recorded in public repository files when they
would reveal local usernames, private directory structures, synchronized-drive
locations, or other sensitive metadata.

### Public evidence permitted in Git

The repository may contain only evidence that has been sanitized, reviewed, and
approved for public use, such as:

- sanitized configuration summaries;
- redacted screenshots;
- sanitized command-output excerpts;
- reviewed KQL queries and aggregated results;
- synthetic controlled-test records;
- AMA and DCR validation summaries;
- analytics-rule validation summaries;
- alert or incident-object summaries with explicit limitations;
- teardown and orphan-check summaries;
- sanitized cost-review summaries;
- evidence manifests;
- claim-to-evidence mappings;
- failed-test summaries that do not expose sensitive data;
- documented limitations and unresolved findings.

An artifact being technically useful does not automatically make it safe for
public release.

## Identifier system

The project uses stable identifiers to connect plans, tests, evidence,
detections, and public claims.

| Identifier | Purpose | Example |
|---|---|---|
| `EV-###` | Evidence record or artifact | `EV-001` |
| `TST-###` | Defined validation test | `TST-001` |
| `CLM-###` | Governed public or portfolio claim | `CLM-001` |
| `Q-###` | KQL query or query family | `Q-001` |
| `AR-###` | Microsoft Sentinel analytics rule | `AR-001` |

Identifiers must not be silently reused. If an artifact or test is superseded,
its original identifier remains in the audit trail and the replacement receives
a new identifier or an explicitly versioned relationship.

## Evidence lifecycle status

Evidence lifecycle status describes what has happened to an evidence artifact.
It does not describe whether the associated test passed.

Allowed lifecycle values are:

| Status | Meaning |
|---|---|
| `Planned` | Evidence requirements are defined, but nothing has been captured. |
| `Captured-Private` | Raw evidence exists in approved private storage outside Git. |
| `Sanitized` | A public-safe derivative has been prepared but not yet reviewed. |
| `Reviewed` | The derivative has completed technical and redaction review. |
| `Approved-Public` | The reviewed derivative is authorized for repository use. |
| `Rejected` | The artifact is unsuitable for public release or technical reliance. |
| `Superseded` | A newer artifact replaces it without deleting its audit history. |

No item may be marked `Captured-Private` merely because evidence capture is
planned. No item may be marked `Approved-Public` without review.

## Test outcome

Test outcome records the result of an executed test independently from evidence
lifecycle status.

Allowed outcome values are:

| Outcome | Meaning |
|---|---|
| `Not Run` | The test has not been executed. |
| `Passed` | The observed result met the documented acceptance criteria. |
| `Failed` | The observed result did not meet the documented acceptance criteria. |
| `Blocked` | A documented dependency or authorization boundary prevented execution. |
| `Inconclusive` | Execution occurred, but the result does not support a reliable conclusion. |
| `Not Applicable` | The test was reviewed and determined not to apply to the scoped implementation. |

A sanitized screenshot can have lifecycle status `Approved-Public` while its
associated test outcome is `Failed`. These fields must never be collapsed into
one status.

## Claim maturity hierarchy

Public claims follow this hierarchy:

1. `Designed`
2. `Implemented`
3. `Validated`
4. `Demonstrated`
5. `Completed`

### Designed

The control, query, rule, workflow, or architecture is documented but has not
been proven in the live v2 environment.

### Implemented

The component exists in the scoped environment and has implementation evidence.
Implementation alone does not prove correct behavior.

### Validated

A defined test was executed, acceptance criteria were met, and reviewed evidence
supports the result.

### Demonstrated

A sanitized and approved public artifact communicates the validated behavior
without exposing restricted information or overstating the result.

### Completed

All scoped completion criteria for the relevant milestone have been met,
including required validation, limitations, cleanup, evidence disposition, and
applicable cost review.

Claims must not skip maturity levels without supporting evidence. Framework
mapping does not establish implementation, validation, compliance, certification,
or authorization.

## Minimum evidence metadata

Every evidence record must identify, when applicable:

- evidence ID;
- descriptive title;
- related test ID;
- related claim ID;
- related query ID;
- related analytics-rule ID;
- capture timestamp in UTC;
- capture source or tool;
- source environment;
- configuration or artifact version;
- evidence lifecycle status;
- test outcome;
- acceptance criteria;
- observed result;
- private raw-evidence integrity hash;
- public artifact path;
- public artifact integrity hash;
- sanitization actions;
- reviewer;
- review date;
- limitations;
- supersession relationship;
- retention or disposition decision.

Sensitive private paths, identifiers, or values must not be copied into the
public record.

## Evidence workflow

Evidence must proceed through the following workflow:

1. Define the control, test, acceptance criteria, and required evidence.
2. Confirm that execution is authorized and that safety gates are satisfied.
3. Execute the approved test without expanding its authorized scope.
4. Capture raw evidence directly into approved private storage.
5. Assign identifiers and record the actual outcome.
6. Calculate SHA-256 hashes where integrity verification is appropriate.
7. Preserve failed, blocked, or inconclusive results.
8. Create a sanitized derivative for potential public use.
9. Review the derivative for technical accuracy, sensitive data, licensing,
   provenance, and claim alignment.
10. Approve or reject the derivative for public release.
11. Add approved public evidence to the manifest.
12. Link governed claims to sufficient evidence in the claim-evidence matrix.
13. Mark older records as superseded rather than deleting their history.

No public artifact should be committed merely because raw evidence was
captured.

## Evidence-type requirements

### Screenshots

A screenshot record should identify:

- the evidence ID;
- what the screenshot is intended to prove;
- the relevant resource, control, query, test, or rule;
- the UTC capture time;
- the applicable configuration version;
- required cropping or redaction;
- limitations of what is visible.

Browser tabs, bookmarks, account details, tenant information, subscription
information, active endpoints, notifications, and unrelated desktop content
must be reviewed before release.

### Command and configuration output

Command-output evidence should preserve:

- the command or a sanitized equivalent;
- the tool and version when relevant;
- the execution context;
- the expected result;
- the observed result;
- exit status when available;
- enough output to support the conclusion;
- any redaction that changes visible values but not technical meaning.

Secrets and sensitive identifiers must not be replaced with realistic-looking
values that could be mistaken for live data. Use explicit placeholders.

### KQL evidence

KQL evidence should record:

- query ID and version;
- query text or repository path;
- target table;
- validated field names;
- query time range;
- execution timestamp in UTC;
- result count;
- aggregation or filtering applied;
- controlled-test identifiers when used;
- schema assumptions;
- known false-positive or false-negative limitations.

A query returning rows does not independently prove malicious activity,
successful compromise, or human attribution.

### AMA and DCR evidence

Azure Monitor Agent and Data Collection Rule evidence should establish, as
applicable:

- the agent installation or association state;
- the relevant DCR configuration;
- the configured collection source;
- the Log Analytics destination;
- expected table availability;
- telemetry freshness;
- observed schema and fields;
- any delay, gap, or ingestion failure.

The expected use of `SecurityEvent` must be validated against actual data before
the project claims that the table and required fields are available.

### Authentication-event evidence

Windows Security Event ID 4625 represents a failed logon and is not
intrinsically an RDP event. Evidence must validate the remote-interactive
context, including the observed logon type and relevant schema fields.

Windows Security Event ID 4624 represents a successful logon and is not
automatically malicious. Any suspicious-success claim requires additional
context and correlation.

### Alert and incident evidence

Alert or Microsoft Sentinel incident-object evidence should identify:

- analytics-rule ID and version;
- rule state;
- execution cadence and lookback;
- threshold;
- query version;
- entity mappings;
- alert creation result;
- incident-creation behavior when applicable;
- controlled-test relationship;
- limitations and analyst interpretation.

An alert or incident object proves that configured detection logic produced an
object under observed conditions. It does not independently prove compromise.

### Teardown evidence

Teardown evidence should cover:

- scoped resource deletion;
- resource-group or individual-resource checks;
- orphan-resource checks;
- active endpoint removal;
- NSG and public-IP disposition;
- telemetry retention or deletion decision;
- private evidence disposition;
- immediate post-cleanup verification;
- scheduled 24-hour verification;
- scheduled 72-hour verification.

Resource deletion alone is not sufficient evidence that cleanup is complete.

### Cost evidence

Cost evidence should record:

- the observation period;
- runtime;
- relevant service scope;
- sanitized cost or usage summary;
- budget and alert configuration where applicable;
- immediate review;
- 24-hour review;
- 72-hour review;
- delayed-charge limitations;
- unexplained residual usage or cost.

Budgets and alerts are detective controls, not guaranteed hard stops. Runtime
and teardown remain the primary cost boundaries for this lab.

## Integrity requirements

Use SHA-256 hashes when an artifact's integrity, provenance, or
private-to-public derivation needs to be demonstrated.

Recommended command:

```bash
shasum -a 256 <file>
```

The private evidence record may retain hashes for both the raw artifact and its
sanitized derivative. The public manifest should include only hashes and
metadata that are approved for public disclosure.

A hash demonstrates file consistency. It does not prove that the artifact is
accurate, complete, authentic, or sufficient for a claim.

## Failed-test preservation

Failed, blocked, and inconclusive tests are part of the engineering record.

They must:

- retain their original test and evidence identifiers;
- record the actual observed result;
- document the failed acceptance criterion;
- identify known causes or unresolved hypotheses;
- preserve approved supporting evidence;
- link to corrective work when applicable;
- remain traceable after a successful retest.

A retest receives its own record or explicit version relationship. It must not
silently replace the earlier result.

## Claim-evidence rules

A claim may advance beyond `Designed` only when the required implementation or
validation activity has actually occurred.

A validation claim requires:

- a defined and versioned configuration;
- a defined test;
- documented acceptance criteria;
- an actual observed result;
- a passing outcome;
- reviewed evidence;
- documented limitations;
- public wording that matches the evidence.

The following substitutions are prohibited:

- design documentation presented as implementation evidence;
- implementation evidence presented as validation evidence;
- an alert presented as proof of compromise;
- an IP geolocation result presented as human attribution;
- framework mapping presented as compliance or authorization;
- resource deletion presented as complete cleanup;
- a budget alert presented as a guaranteed spending stop;
- a successful retest used to erase a prior failed test.

## Public evidence index

The public evidence-governance files are:

- `README.md` — governing evidence model and workflow;
- `redaction-notes.md` — sanitization and disclosure rules;
- `manifest.md` — public approved-evidence inventory;
- `claim-evidence-matrix.md` — governed claims and supporting evidence;
- `templates/evidence-record-template.md` — reusable evidence-record structure.

These files establish the governance system. They do not establish that
execution evidence currently exists.

## Current evidence declaration

At this stage, there are no captured, sanitized, reviewed, or approved Sentinel
RDP Honeypot v2 execution-evidence artifacts.

Any future evidence must be added through the workflow defined in this
directory. Until then, all implementation, validation, detection, teardown,
cost, and portfolio claims remain limited to their verified design-stage state.
