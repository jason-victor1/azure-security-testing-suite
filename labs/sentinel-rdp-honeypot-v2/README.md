# Sentinel RDP Honeypot v2

## Project status

| Field | Current state |
|---|---|
| Project type | Personal Azure cloud-security and Microsoft Sentinel detection-engineering lab |
| Lifecycle state | Design and documentation baseline |
| Azure execution | Paused |
| Azure v2 resources | Not deployed |
| Public TCP/3389 exposure | Unauthorized |
| Controlled authentication testing | Not started |
| `SecurityEvent` schema validation | Not started |
| `AR-001` deployment | Draft design only; disabled, undeployed, and unverified |
| Alert and incident validation | Not started |
| Teardown and cost closure | Not started |
| Public execution evidence | None captured or approved |
| Portfolio claim level | Designed / Planned only |
| Last updated | 2026-07-14 |

> Sentinel RDP Honeypot v2 is a self-built Azure cloud-security and Microsoft Sentinel detection-engineering lab designed to validate a bounded Windows remote-interactive authentication workflow.

This repository currently demonstrates the **engineering design, safety controls, operational procedures, evidence governance, and detection specifications** for the lab. It does not claim that the Azure environment has been deployed or that collection, detection, triage, containment, cleanup, or cost closure has been validated.

## Why this project exists

The project translates prior learning material into an original, current, and evidence-governed security-engineering workflow.

Its objective is not merely to expose a Windows host and collect failed logons. The objective is to design and eventually validate a bounded lifecycle that connects:

1. Azure scope and authorization;
2. disposable infrastructure;
3. Windows Security Event collection;
4. telemetry-health and schema validation;
5. controlled authentication testing;
6. KQL investigation;
7. scheduled detection logic;
8. alert and incident-object triage;
9. evidence preservation and sanitization;
10. emergency containment;
11. teardown and orphan-resource review;
12. delayed cost reconciliation; and
13. evidence-gated public claims.

The project intentionally separates **design**, **implementation**, **validation**, **demonstration**, and **completion**.

## Current milestone state

| Milestone | State |
|---|---|
| Commit 1 — Architecture, scope, and planning | Complete |
| Commit 2 — Evidence governance | Validated |
| Commit 3 — Operational runbooks | Validated |
| Commit 4 — Detection engineering | Validated |
| Commit 5 — Reporting and portfolio templates | In progress |
| Repository-wide static integration review | Not started |
| Predeployment authorization gate | Blocked |
| Azure deployment | Blocked |
| Controlled authentication testing | Blocked |
| Detection and incident validation | Blocked |
| Public-exposure observation | Blocked |
| Teardown and cost closure | Blocked |
| Portfolio-claim promotion | Blocked |

Static documentation checks do not authorize Azure work.

## Core engineering questions

The project is structured to answer the following questions with evidence rather than assumption:

- Is the intended Azure subscription and resource scope correct?
- Is the Windows VM isolated from trusted environments?
- Is Windows Firewall enabled?
- Are Azure Monitor Agent and the intended Data Collection Rule implemented correctly?
- Do authentication events reach the expected table?
- Are telemetry freshness and ingestion latency acceptable?
- Which fields are actually populated for Event IDs 4625 and 4624?
- Does the collected logon type support remote-interactive wording?
- Do below-threshold and threshold-matching tests behave as expected?
- Does `AR-001` produce the configured alert behavior?
- Does Microsoft Sentinel create or correlate the expected incident object?
- Can controlled testing be distinguished from organic or unclassified activity?
- Can public TCP/3389 exposure be removed promptly?
- Can the VM be safely contained?
- Can all scoped resources and residual objects be dispositioned?
- Can project cost be reconciled after billing data settles?
- Which public claims are supported by reviewed evidence?

## Intended architecture

The design uses:

- a dedicated Azure resource group;
- an isolated disposable Windows VM;
- an NSG with exposure controlled by explicit authorization gates;
- Windows Firewall kept enabled;
- Azure Monitor Agent;
- a Data Collection Rule;
- a Log Analytics workspace;
- Microsoft Sentinel;
- Windows Security Events;
- versioned KQL queries;
- one scheduled analytics-rule design, `AR-001`;
- private raw-evidence storage outside Git; and
- sanitized public artifacts only after review.

The authoritative architecture and risk boundary are documented in:

- [System boundary and threat model](docs/threat-model.md)
- [Architecture overview](docs/architecture-overview.md)
- [Production-safe design contrast](docs/production-safe-design-contrast.md)

## Intended data flow

1. The approved operator confirms tenant, subscription, resource group, identity, cost, and safety prerequisites.
2. The operator deploys only the scoped disposable environment.
3. Windows Security Events are collected through Azure Monitor Agent and the approved Data Collection Rule.
4. Telemetry health, destination table, freshness, latency, and field population are validated.
5. Controlled failed and successful remote-interactive authentication tests are executed only after authorization.
6. Versioned KQL queries analyze the resulting events.
7. `AR-001` remains disabled until its query, fields, mappings, threshold behavior, and operational settings pass review.
8. Authorized rule testing evaluates detection results, alert creation, and incident-object behavior separately.
9. Triage distinguishes controlled, benign, suspicious, undetermined, and unauthorized activity.
10. Evidence is captured privately, sanitized, reviewed, and linked to governed claims.
11. Public exposure and the VM are contained when testing ends or a stop condition triggers.
12. Azure resources, identities, telemetry objects, and costs are dispositioned and reconciled.

## Detection-engineering design

### Query catalog

The [KQL query catalog](kql/hunting-queries.md) defines `Q-001` through `Q-020`.

The query sequence places telemetry and schema checks before behavioral detections:

| Query range | Purpose |
|---|---|
| `Q-001`–`Q-006` | Heartbeat, route, freshness, latency, schema, and source-host validation |
| `Q-007`–`Q-010` | Failed and successful authentication inspection |
| `Q-011`–`Q-014` | Controlled failed/successful tests and threshold checks |
| `Q-015`–`Q-019` | Behavioral summaries, classification, and correlation |
| `Q-020` | Canonical detection query for `AR-001` |

All queries are currently reviewed but unexecuted. Their execution status remains blocked.

### Analytics rule

The [analytics-rule catalog](sentinel/analytics-rules-catalog.md) defines `AR-001`.

| Setting | Design value |
|---|---|
| Rule type | Scheduled |
| Canonical query | `Q-020` version `1.0.0` |
| Query frequency | 5 minutes |
| Query period | 15 minutes |
| Aggregation window | 5 minutes |
| Draft threshold | 5 failed remote-interactive logons |
| Severity | Medium |
| ATT&CK mapping | `T1110.001` Password Guessing |
| IP entity mapping | Conditional on validated field quality |
| Host entity mapping | Conditional on validated field format |
| Account entity mapping | Deferred |
| Suppression | Disabled for initial validation |
| Automated response | None |
| Current state | Disabled, blocked, undeployed, and unverified |

A query result means that configured conditions were satisfied. It does not independently prove malicious intent, successful access, compromise, or human attribution.

## Authentication terminology

### Event ID 4625

Event ID 4625 records a failed logon.

It is not automatically:

- an RDP event;
- password guessing;
- brute force;
- malicious activity; or
- evidence of compromise.

Remote-interactive wording requires validated logon-type or equivalent schema context.

### Event ID 4624

Event ID 4624 records creation of a successful logon session.

It is not automatically:

- authorized access;
- unauthorized access;
- malicious activity; or
- evidence of compromise.

A suspicious-success hypothesis requires source, account, host, timing, controlled-test context, and relevant follow-on activity.

## Alert, incident, and compromise boundaries

The project uses the following analytical progression:

| Stage | Meaning |
|---|---|
| Windows event | Operating-system activity was recorded |
| Validated observable | A field or fact was confirmed in collected data |
| Authentication pattern | Related observables were grouped by source, account, host, and time |
| Detection result | Query conditions were satisfied |
| Alert | The analytics rule generated a security alert |
| Incident object | Microsoft Sentinel created or correlated an investigation case |
| Compromise hypothesis | Available evidence suggests unauthorized access may have succeeded |
| Confirmed unauthorized activity | Defined evidence supports an unauthorized harmful event |

An alert or incident object does not independently establish compromise.

## Safety model

The authoritative [threat model](docs/threat-model.md) defines risk, controls, exposure gates, and stop conditions.

Core safety boundaries include:

- no production data;
- no production or reused credentials;
- no intentionally weak or guessable credentials;
- no trusted-network connectivity;
- no VNet peering;
- Windows Firewall remains enabled;
- no allow-all inbound rule;
- public TCP/3389 exposure requires a separate explicit authorization decision;
- the operator remains present during any approved exposure;
- exposure is time-boxed;
- cost and runtime limits are recorded;
- emergency exposure removal and VM containment are ready;
- safety takes priority over evidence preservation;
- no hack-back or source-infrastructure interaction;
- no destructive automated response; and
- no continuation after a mandatory stop condition.

## Operational runbooks

| Artifact | Purpose | Execution state |
|---|---|---|
| [Deployment runbook](runbooks/deployment-runbook.md) | Phased deployment, validation, authorization, and rollback workflow | Reviewed; not executed |
| [Authentication-triage runbook](runbooks/rdp-authentication-triage.md) | Event, alert, incident, and disposition workflow | Reviewed; not executed |
| [Cost-control checklist](runbooks/cost-control-checklist.md) | Budget, runtime, ingestion, and delayed cost reconciliation | Reviewed; not executed |
| [Teardown runbook](runbooks/teardown-runbook.md) | Normal and emergency containment, deletion, and orphan review | Reviewed; not executed |

## Controlled validation plan

The roadmap defines `TST-001` through `TST-025`.

Key detection and response tests include:

| Test | Intended validation | Current outcome |
|---|---|---|
| `TST-008` | Telemetry health | Blocked |
| `TST-009` | Destination table | Blocked |
| `TST-010` | Controlled Event ID 4625 | Blocked |
| `TST-011` | Remote-interactive context | Blocked |
| `TST-012` | Controlled Event ID 4624 | Blocked |
| `TST-013` | Below-threshold detection behavior | Blocked |
| `TST-014` | Threshold-matching detection behavior | Blocked |
| `TST-015` | Alert creation | Blocked |
| `TST-016` | Incident-object behavior | Blocked |
| `TST-017` | Controlled-versus-organic separation | Blocked |
| `TST-018` | Exposure removal | Blocked |
| `TST-019` | VM containment | Blocked |
| `TST-020` | Evidence sanitization | Blocked |
| `TST-021`–`TST-022` | Cleanup and orphan review | Blocked |
| `TST-023`–`TST-025` | Immediate, 24-hour, and 72-hour cost review | Blocked |

Blocked means that a dependency or authorization boundary prevents execution. It does not mean that a test passed or failed.

## Evidence governance

The project maintains a formal evidence model:

- [Evidence governance standard](evidence/README.md)
- [Redaction and sanitization rules](evidence/redaction-notes.md)
- [Public evidence manifest](evidence/manifest.md)
- [Claim-evidence matrix](evidence/claim-evidence-matrix.md)
- [Evidence-record template](evidence/templates/evidence-record-template.md)

Core evidence rules:

1. Raw evidence remains private and outside Git.
2. Only sanitized, reviewed, and explicitly approved derivatives may enter the repository.
3. Evidence lifecycle and test outcome are separate.
4. Failed, blocked, and inconclusive tests remain traceable.
5. SHA-256 is used where artifact-integrity tracking is appropriate.
6. Public evidence must not expose secrets, private identifiers, live endpoints, raw event data, or local-system metadata.
7. A screenshot alone is not sufficient evidence of effective operation.
8. Evidence cannot support a stronger claim than it actually proves.

There are currently no captured, sanitized, reviewed, or approved v2 execution-evidence artifacts.

## Claim maturity

Public claims follow this hierarchy:

1. Designed
2. Implemented
3. Validated
4. Demonstrated
5. Completed

Current approved claims are limited to design-stage statements, including:

- designed a bounded Azure and Microsoft Sentinel detection-engineering lab;
- defined the system boundary, trust boundaries, risks, and stop conditions;
- designed AMA/DCR collection and schema validation;
- designed controlled failed and successful authentication testing;
- designed a versioned KQL catalog and `AR-001`;
- designed evidence, containment, teardown, and cost-closure workflows; and
- mapped selected project elements to relevant security concepts without claiming compliance.

The following are not currently supportable:

- deployed the v2 Azure environment;
- implemented or validated AMA and the DCR;
- validated the `SecurityEvent` schema;
- implemented or validated `AR-001`;
- generated or triaged a v2 alert;
- demonstrated an incident workflow;
- validated emergency containment;
- completed cleanup;
- completed cost closure;
- demonstrated an operational end-to-end workflow; or
- completed Sentinel RDP Honeypot v2.

The authoritative claim state is maintained in the [claim-evidence matrix](evidence/claim-evidence-matrix.md).

## Reporting and portfolio controls

The reporting templates are designed to separate:

- verified facts;
- interpretations;
- limitations;
- findings;
- recommendations;
- evidence status; and
- claim impact.

The following Commit 5 artifacts remain under development:

- [Incident timeline](reports/incident-timeline.md)
- [Findings report](reports/findings-report.md)
- [Remediation recommendations](reports/remediation-recommendations.md)
- [Interview talking points](resume-assets/interview-talking-points.md)
- [Resume bullets](resume-assets/resume-bullets.md)
- [Scoring worksheet](rubric/scoring-worksheet.md)

Resume bullets remain blocked until the claim-evidence matrix supports their exact wording.

## Repository map

```text
sentinel-rdp-honeypot-v2/
├── docs/                 Architecture, scope, threat model, and roadmap
├── evidence/             Evidence governance, manifest, claim matrix, and template
├── kql/                  Versioned KQL query catalog
├── reports/              Timeline, findings, and remediation templates
├── resume-assets/        Evidence-gated interview and resume material
├── rubric/               Validation and quality scoring worksheet
├── runbooks/             Deployment, triage, cost, and teardown procedures
└── sentinel/             Scheduled analytics-rule catalog
```

## Key documents

### Architecture and scope

- [System boundary and threat model](docs/threat-model.md)
- [Architecture overview](docs/architecture-overview.md)
- [Scope and credibility notes](docs/scope-and-credibility-notes.md)
- [Production-safe design contrast](docs/production-safe-design-contrast.md)
- [Theory-to-lab mapping](docs/theory-to-lab-mapping.md)
- [Project roadmap and definition of done](docs/checklist-to-project-roadmap.md)

### Operations and detection

- [Deployment runbook](runbooks/deployment-runbook.md)
- [Authentication-triage runbook](runbooks/rdp-authentication-triage.md)
- [Cost-control checklist](runbooks/cost-control-checklist.md)
- [Teardown runbook](runbooks/teardown-runbook.md)
- [KQL query catalog](kql/hunting-queries.md)
- [Analytics-rule catalog](sentinel/analytics-rules-catalog.md)

### Evidence and claims

- [Evidence governance](evidence/README.md)
- [Redaction notes](evidence/redaction-notes.md)
- [Public evidence manifest](evidence/manifest.md)
- [Claim-evidence matrix](evidence/claim-evidence-matrix.md)
- [Evidence-record template](evidence/templates/evidence-record-template.md)

## What this project does not claim

This project does not claim:

- enterprise SOC ownership;
- production or 24×7 monitoring;
- production incident authority;
- a production-ready public RDP design;
- a mature deception platform;
- penetration-testing authorization;
- malware analysis;
- threat-actor attribution;
- attacker identity from IP or GeoIP;
- formal compliance;
- an Authority to Operate;
- successful compromise from failed logons;
- compromise from a Sentinel alert or incident object;
- complete cleanup from a deletion request;
- final cost from an immediate billing view; or
- completed implementation or validation before evidence exists.

## Planned next steps

1. Complete the remaining Commit 5 reporting and portfolio templates.
2. Perform the repository-wide static integration review.
3. Resolve every mandatory predeployment documentation and readiness item.
4. Record an explicit go/no-go decision before any Azure execution.
5. Keep public exposure blocked unless a separate authorization gate passes.
6. Execute only approved, bounded validation steps.
7. Preserve failed and inconclusive outcomes.
8. Complete containment, cleanup, orphan review, and delayed cost reconciliation.
9. Promote claims only to the strongest level supported by reviewed evidence.

## Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection and incident validation have not started.
- No v2 execution evidence exists.
- No public evidence artifact has been approved.
- Resume bullets remain blocked.
- Public claims remain limited to Designed or Planned.
- This README is a design-stage project overview, not proof of live execution.
