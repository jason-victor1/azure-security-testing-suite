# Claim-Evidence Matrix

## Purpose

This document is the authoritative public-claim control for Sentinel RDP
Honeypot v2.

It identifies:

- currently approved design-stage claims;
- claims that remain blocked;
- the evidence and tests required for claim promotion;
- prohibited interpretations;
- claim regression and retirement conditions;
- portfolio, resume, interview, and demonstration approval boundaries.

The matrix does not prove that Azure execution, authentication testing,
detection validation, evidence capture, teardown, or cost closure has occurred.

## Authority

Claim decisions must remain consistent with:

1. `../docs/threat-model.md`;
2. `../docs/scope-and-credibility-notes.md`;
3. `README.md`;
4. `redaction-notes.md`;
5. `manifest.md`;
6. `../docs/checklist-to-project-roadmap.md`.

When documents or evidence disagree, the weaker supportable claim controls.

The threat model controls system boundaries, exposure conditions, risks, and
mandatory stop conditions. This matrix controls whether specific public,
portfolio, resume, interview, report, or demonstration wording is authorized.

## Current project state

At the creation of this matrix:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- AMA and DCR have not been implemented for v2.
- The expected `SecurityEvent` table and fields have not been validated.
- `AR-001` remains a draft design.
- Alert and incident-object behavior has not been validated.
- No v2 execution evidence has been captured.
- No v2 evidence artifact has reached `Approved-Public`.
- Teardown and cost closure have not been validated.
- Resume and portfolio implementation claims remain blocked.
- Public implementation, validation, demonstration, and completion claims are
  not authorized.

Only the design-stage claims explicitly approved below may currently be used.

## Claim maturity and decision model

### Maturity levels

| Maturity | Meaning |
|---|---|
| `Designed` | The architecture, control, query, rule, test, or procedure is documented. |
| `Implemented` | The scoped configuration or procedure exists and has implementation evidence. |
| `Validated` | A defined test passed and reviewed evidence supports the result. |
| `Demonstrated` | Approved public evidence communicates the validated bounded workflow. |
| `Completed` | All applicable implementation, validation, evidence, closure, and claim gates passed. |

Claims must not skip required maturity levels.

### Framework relationship labels

`Mapped` and `Informed by` are framework-relationship labels. They are not
implementation or validation maturity levels.

A framework relationship does not establish:

- compliance;
- certification;
- authorization;
- control effectiveness;
- enterprise adoption;
- regulatory applicability.

### Public decisions

| Decision | Meaning |
|---|---|
| `Approved` | The exact bounded wording may be used at the recorded maturity. |
| `Blocked` | The claim is defined but cannot be used until its promotion gate passes. |
| `Prohibited` | The wording or interpretation exceeds project scope or evidence. |
| `Regress` | A previously approved claim must be reduced to a weaker maturity. |
| `Retired` | The claim is no longer valid for current project state. |
| `Superseded` | A replacement claim or version controls while history remains traceable. |

## Claim-record requirements

Each governed claim must record:

- claim ID;
- category;
- approved or proposed wording;
- current maturity or relationship label;
- public decision;
- current support;
- related test IDs;
- required evidence;
- limitations;
- promotion gate;
- regression triggers;
- supersession relationship when applicable.

Design documents can support a `Designed` claim without an execution-evidence
ID. Any claim of `Implemented`, `Validated`, `Demonstrated`, or `Completed`
requires traceable evidence and, where public evidence is used, corresponding
approved manifest entries.

## Current approved design-stage claims

| Claim ID | Approved public wording | Current state | Decision | Current support | Limitations |
|---|---|---|---|---|---|
| CLM-001 | Designed a bounded Azure and Microsoft Sentinel detection-engineering lab for Windows remote-interactive authentication telemetry. | `Designed` | `Approved` | Architecture, threat model, scope notes, and roadmap | Does not claim deployment, telemetry collection, detection operation, or validation. |
| CLM-002 | Defined an authoritative system boundary and threat model for the lab. | `Designed` | `Approved` | `docs/threat-model.md` | Risk ratings and controls remain unverified until implementation evidence exists. |
| CLM-003 | Defined trust boundaries for internet exposure, the Windows workload, Azure administration, telemetry, and evidence publication. | `Designed` | `Approved` | Threat model and architecture overview | Does not prove that the live Azure environment satisfies those boundaries. |
| CLM-004 | Designed conditional public-exposure gates and mandatory stop conditions. | `Designed` | `Approved` | Threat model and roadmap | Public TCP/3389 exposure remains unauthorized. |
| CLM-005 | Designed an AMA and DCR collection workflow for Windows Security Events. | `Designed` | `Approved` | Architecture, roadmap, and scope notes | AMA and the DCR are not implemented for v2. |
| CLM-006 | Designed validation for the expected `SecurityEvent` route, telemetry health, schema, and authentication fields. | `Designed` | `Approved` | Roadmap and scope notes | The actual table and fields remain unvalidated. |
| CLM-007 | Designed controlled validation for failed and successful remote-interactive authentication. | `Designed` | `Approved` | Roadmap controlled-test catalog and scope notes | No controlled authentication test has started. |
| CLM-008 | Designed `AR-001` as a scheduled rule for repeated failed remote-interactive authentication. | `Designed` | `Approved` | Threat model, roadmap, and canonical design decisions | The rule is draft, disabled, undeployed, and unverified. |
| CLM-009 | Documented the distinction between Windows events, detection results, alerts, incident objects, compromise hypotheses, and confirmed unauthorized activity. | `Designed` | `Approved` | Scope notes and threat model | Does not claim that any v2 alert, incident, or compromise occurred. |
| CLM-010 | Designed evidence governance that separates private raw evidence from reviewed public artifacts. | `Designed` | `Approved` | Evidence-governance documents | No v2 execution evidence has been captured or approved publicly. |
| CLM-011 | Designed normal and emergency containment, exposure removal, teardown, and orphan-resource verification requirements. | `Designed` | `Approved` | Threat model and roadmap | The procedures have not been executed or validated. |
| CLM-012 | Designed runtime, budget-alert, cost-monitoring, and delayed cost-reconciliation requirements. | `Designed` | `Approved` | Threat model and roadmap | Budget alerts are not guaranteed hard stops; no v2 cost closure exists. |
| CLM-013 | Mapped selected project design elements to relevant NIST and CIS concepts and documented where those concepts informed design decisions. | `Mapped` / `Informed by` | `Approved` | `docs/theory-to-lab-mapping.md` | Does not establish NIST, CIS, HIPAA, HITRUST, PCI DSS, GDPR, RMF, or other compliance or authorization. |

## Blocked implementation and validation claims

The following claims are defined for future evaluation but are not currently
authorized.

| Claim ID | Proposed future claim | Required maturity | Decision | Related tests | Minimum promotion evidence |
|---|---|---|---|---|---|
| CLM-014 | Implemented the scoped Azure v2 environment. | `Implemented` | `Blocked` | Applicable deployment tests | Reviewed configuration and execution evidence for the actual scoped resources. |
| CLM-015 | Implemented AMA on the intended Windows VM. | `Implemented` | `Blocked` | `TST-006` | Agent or extension state tied to the intended VM and configuration version. |
| CLM-016 | Implemented and associated the intended DCR. | `Implemented` | `Blocked` | `TST-007` | DCR definition, collection source, destination, and target association evidence. |
| CLM-017 | Validated collection of the expected Windows authentication telemetry. | `Validated` | `Blocked` | `TST-010`, `TST-011`, `TST-012` | Actual table, source computer, timestamps, freshness, fields, and controlled-event evidence. |
| CLM-018 | Validated a controlled Event ID 4625 failed logon. | `Validated` | `Blocked` | `TST-010` | Controlled-test record, actual 4625 event, required fields, result, and limitations. |
| CLM-019 | Validated failed remote-interactive authentication context. | `Validated` | `Blocked` | `TST-011` | Observed logon type or equivalent validated context; Event ID 4625 alone is insufficient. |
| CLM-020 | Validated a controlled Event ID 4624 successful logon. | `Validated` | `Blocked` | `TST-012` | Approved controlled-success record, actual event fields, source, timing, and limitations. |
| CLM-021 | Implemented `AR-001`. | `Implemented` | `Blocked` | Configuration review | Deployed rule configuration, query version, cadence, lookback, threshold, severity, state, and validated entity mappings. |
| CLM-022 | Validated `AR-001` below-threshold and threshold-matching behavior. | `Validated` | `Blocked` | `TST-013`, `TST-014` | Both test records, actual results, rule and query versions, and reviewed evidence. |
| CLM-023 | Validated Microsoft Sentinel alert creation for the controlled threshold-matching test. | `Validated` | `Blocked` | `TST-015` | Alert evidence linked to the controlled test and underlying events. |
| CLM-024 | Demonstrated the configured Microsoft Sentinel incident-object workflow. | `Demonstrated` | `Blocked` | `TST-016` | Incident-object behavior, investigation record, disposition, closure decision, and approved public evidence. |
| CLM-025 | Validated separation of controlled-test and organic activity. | `Validated` | `Blocked` | `TST-017` | Test-session identifiers, source, account, host, time-window, and classification evidence. |
| CLM-026 | Validated removal of public TCP/3389 exposure. | `Validated` | `Blocked` | `TST-018` | Timed exposure-removal test, effective-rule verification, observed result, and evidence. |
| CLM-027 | Validated VM containment through deallocation or deletion. | `Validated` | `Blocked` | `TST-019` | Timed containment test, terminal state, observed result, and evidence. |
| CLM-028 | Validated evidence sanitization and approved a public v2 artifact. | `Validated` / `Demonstrated` | `Blocked` | `TST-020` | Private raw record, sanitized derivative, technical and redaction review, public approval, manifest entry, and integrity record. |
| CLM-029 | Completed cleanup of scoped Azure resources. | `Completed` | `Blocked` | `TST-021`, `TST-022` | Terminal resource-group deletion, subscription-wide orphan search, identity and telemetry disposition, and limitations. |
| CLM-030 | Completed v2 cost closure. | `Completed` | `Blocked` | `TST-023`, `TST-024`, `TST-025` | Immediate, 24-hour, and 72-hour cost records with delayed-charge limitations and unresolved residuals disclosed. |
| CLM-031 | Demonstrated the bounded Sentinel collection, detection, triage, evidence, containment, cleanup, and cost-closure workflow. | `Demonstrated` | `Blocked` | All applicable validation tests | Validated component chain, approved public evidence, documented failures and limitations, cleanup, and cost closure. |
| CLM-032 | Completed Sentinel RDP Honeypot v2. | `Completed` | `Blocked` | All applicable project tests | Every applicable project completion gate, all milestone commits, evidence package, reports, teardown, cost closure, and claim review. |
| CLM-033 | Approved a resume or portfolio implementation claim. | Evidence-dependent | `Blocked` | Applicable supporting tests | Exact wording linked to sufficient reviewed evidence, approved public artifacts, scope limits, and publication review. |

## Claim-specific evidence rules

### AMA and DCR

An AMA implementation claim requires evidence that the intended VM has the
expected agent state.

A DCR implementation claim requires evidence of:

- the DCR definition;
- selected collection source;
- configured destination;
- intended target association.

Configuration evidence does not prove successful ingestion.

### `SecurityEvent` and schema

The project expects Windows Security Events collected through AMA to populate
`SecurityEvent`, but the actual table and fields must be validated.

A schema claim requires actual evidence of:

- table availability;
- source computer;
- timestamps;
- freshness and latency;
- Event ID fields;
- logon-type or equivalent context;
- account, host, and source fields needed by the scoped query;
- observed gaps or field-quality limitations.

### Event ID 4625

Event ID 4625 supports the statement:

> A failed logon was recorded.

It supports a failed remote-interactive claim only after the required context
has been validated.

It does not independently prove:

- RDP activity;
- brute force;
- malicious intent;
- successful compromise;
- a confirmed incident.

### Event ID 4624

Event ID 4624 supports the statement:

> A successful logon session was recorded.

A successful remote-interactive claim requires validated context.

A malicious-success or compromise claim requires additional source, account,
controlled-test, timing, authorization, and follow-on activity evidence.

### `AR-001`, alerts, and incident objects

An `AR-001` implementation claim requires the deployed rule configuration and
query version.

A detection-validation claim requires both:

- below-threshold behavior;
- threshold-matching behavior.

An alert proves that configured detection logic generated an alert under the
observed conditions. An incident object proves that the platform created or
correlated a case object.

Neither independently proves compromise.

### Controlled and organic activity

Organic internet activity is not guaranteed and is not required for core
validation.

A controlled-versus-organic separation claim requires:

- test-session IDs;
- approved test sources;
- explicit time windows;
- synthetic accounts;
- relevant host information;
- triage classification.

Organic activity must not be retroactively described as controlled testing.

### Containment and cleanup

Exposure removal must be validated separately from VM containment.

Resource deletion must not be described as complete cleanup until:

- deletion reaches terminal state;
- orphan-resource checks complete;
- identities and role assignments are dispositioned;
- public endpoints are removed;
- DCR and analytics objects are dispositioned;
- telemetry retention or deletion is recorded;
- delayed cost reviews are complete.

### Cost closure

An immediate cost view is preliminary.

A final cost-closure claim requires:

- the observation period;
- runtime;
- relevant service scope;
- immediate review;
- 24-hour review;
- 72-hour review;
- delayed-charge limitations;
- unexplained residual usage or cost.

Budgets and alerts are detective controls, not guaranteed hard stops.

## Prohibited claims and interpretations

The following claims remain prohibited regardless of whether a related artifact
exists, unless project scope is formally changed and separately authorized:

- built or operated an enterprise SOC;
- owned production or 24×7 incident response;
- implemented a production-ready public RDP architecture;
- created a production-proven or enterprise-grade honeypot;
- confirmed compromise solely from Event ID 4625;
- confirmed malicious activity solely from Event ID 4624;
- confirmed an attack solely from a detection result;
- treated an alert or incident object as proof of compromise;
- identified a human attacker from an IP address;
- treated GeoIP as physical-location or nationality attribution;
- claimed APT or named-threat-actor activity without separate evidence;
- treated framework mapping as compliance, certification, authorization, or an
  authority to operate;
- claimed complete cleanup from a deletion request alone;
- claimed zero cost or final cost from an immediate cost view;
- described a budget alert as a guaranteed hard stop;
- presented private course material as original project evidence;
- presented synthetic data as live telemetry;
- presented unexecuted design material as implemented or validated;
- described public TCP/3389 exposure as currently authorized.

## Claim promotion gate

A claim may be promoted only when:

1. its exact wording is defined;
2. the relevant configuration or artifact version is known;
3. the required test is defined;
4. acceptance criteria are documented;
5. the test was actually executed;
6. the actual observed result is recorded;
7. the required result passed;
8. failed, blocked, and inconclusive attempts remain traceable;
9. supporting evidence completed technical review;
10. public artifacts completed sanitization and redaction review;
11. required evidence has a stable `EV-###` identifier;
12. approved public evidence appears in `manifest.md`;
13. limitations are documented;
14. the public wording does not exceed the evidence;
15. the threat model and project scope still authorize the claim.

Implementation and validation claims must not be promoted merely because a
portal screen, command, file, query, rule, alert, or incident object exists.

## Claim regression and retirement

An approved claim must regress, be retired, or be superseded when:

- later testing invalidates the claim;
- a relevant test fails;
- a configuration changes materially;
- a query or analytics-rule version changes;
- required evidence becomes incomplete, unavailable, rejected, or unsafe;
- an approved public artifact fails integrity or redaction review;
- Azure or Microsoft Sentinel behavior changes;
- the claim no longer matches project scope;
- cleanup or delayed-cost findings contradict completion;
- a previously unknown limitation materially changes interpretation.

Regression does not erase the previous claim record or supporting test history.

## Portfolio, resume, interview, and demonstration approval

### Portfolio and repository descriptions

Design-stage descriptions may use only claims marked `Approved` in the current
design-stage matrix.

Implementation, validation, demonstration, and completion wording remains
blocked until its exact claim passes the promotion gate.

### Resume bullets

Resume bullets must not present future work as a completed achievement.

Each approved bullet must identify:

- what was implemented;
- what was validated;
- the bounded personal-lab scope;
- the evidence supporting the result;
- any material limitation.

### Interview talking points

Before Azure execution, approved discussion is limited to:

- architecture decisions;
- threat modeling;
- trust boundaries;
- exposure gates;
- monitoring prerequisites;
- detection hypotheses;
- validation design;
- evidence governance;
- planned containment, teardown, and cost controls.

Actual test outcomes, alerts, incidents, cleanup results, and cost results may be
discussed only after they occur and are reviewed.

### Demonstrations

A demonstration must distinguish:

- current live state;
- previously captured evidence;
- synthetic examples;
- unexecuted design content.

A screenshot of a deleted resource must not be presented as current live state.

## Current claim declaration

The currently approved claims are `CLM-001` through `CLM-013` at their recorded
design-stage or framework-relationship state.

Claims `CLM-014` through `CLM-033` remain blocked.

No claim in this matrix authorizes:

- Azure deployment;
- public TCP/3389 exposure;
- controlled authentication testing;
- evidence capture;
- detection validation;
- portfolio-claim promotion;
- a pull request.

No v2 execution evidence currently exists, and no implementation, validation,
demonstration, or completion claim is approved.
