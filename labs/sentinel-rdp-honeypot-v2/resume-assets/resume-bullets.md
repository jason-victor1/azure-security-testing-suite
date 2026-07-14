# Sentinel RDP Honeypot v2 Resume-Bullet Governance Template

## 1. Purpose

This document governs potential resume bullets for Sentinel RDP Honeypot v2.

It is designed to prevent:

- design work from being presented as implementation;
- implementation from being presented as validation;
- a query result from being presented as an alert;
- an alert or incident object from being presented as compromise;
- an unexecuted test plan from being presented as a completed result;
- a deletion request from being presented as verified cleanup;
- an immediate billing view from being presented as final cost;
- a framework mapping from being presented as compliance; and
- future work from being presented as a completed achievement.

This file is currently a **blocked resume-bullet template**. It does not authorize publication of any Sentinel RDP Honeypot v2 resume bullet.

## 2. Current resume state

| Field | Current state |
|---|---|
| Azure execution | Paused |
| Azure v2 resources | Not deployed |
| Public TCP/3389 exposure | Unauthorized |
| Controlled authentication testing | Not started |
| Telemetry and schema validation | Not started |
| `AR-001` deployment | Disabled, undeployed, and unverified |
| Alert and incident validation | Not started |
| Teardown and cost closure | Not started |
| Execution evidence | None captured or approved |
| Governed implementation claims | Blocked |
| Governed validation claims | Blocked |
| Resume publication decision | Not authorized |
| Strongest current project level | Designed / Planned only |
| Template status | Reviewed structure; blocked |
| Last updated | 2026-07-14 |

No bullet in this file may be copied to a resume until its exact wording is approved by the claim-evidence matrix.

## 3. Governing documents

Use this file with:

1. [Project README](../README.md);
2. [Threat model](../docs/threat-model.md);
3. [Scope and credibility notes](../docs/scope-and-credibility-notes.md);
4. [Project roadmap](../docs/checklist-to-project-roadmap.md);
5. [KQL query catalog](../kql/hunting-queries.md);
6. [Analytics-rule catalog](../sentinel/analytics-rules-catalog.md);
7. [Deployment runbook](../runbooks/deployment-runbook.md);
8. [Authentication-triage runbook](../runbooks/rdp-authentication-triage.md);
9. [Teardown runbook](../runbooks/teardown-runbook.md);
10. [Cost-control checklist](../runbooks/cost-control-checklist.md);
11. [Incident timeline](../reports/incident-timeline.md);
12. [Findings report](../reports/findings-report.md);
13. [Remediation recommendations](../reports/remediation-recommendations.md);
14. [Interview talking points](interview-talking-points.md);
15. [Evidence governance](../evidence/README.md);
16. [Evidence manifest](../evidence/manifest.md); and
17. [Claim-evidence matrix](../evidence/claim-evidence-matrix.md).

The claim-evidence matrix is authoritative for resume publication.

## 4. Resume-bullet rules

1. Record the exact proposed bullet before publication.
2. Assign a governed `CLM-###` identifier.
3. State the intended claim level.
4. Identify the specific implementation and validation evidence required.
5. Identify material limitations.
6. Identify the bounded personal-lab scope.
7. Use “designed” for design-stage work.
8. Use “implemented” only when the relevant component exists with implementation evidence.
9. Use “validated” only when a defined test passed with reviewed evidence.
10. Use “demonstrated” only when approved public evidence communicates validated behavior.
11. Use “completed” only after every applicable completion and closure requirement passes.
12. Do not imply enterprise SOC ownership.
13. Do not imply production authority or 24×7 monitoring.
14. Do not infer RDP solely from Event ID 4625.
15. Do not infer compromise from an alert or incident object.
16. Do not infer human identity or nationality from IP or GeoIP.
17. Do not present framework mapping as compliance.
18. Do not describe cleanup as complete before terminal-state and residual-resource verification.
19. Do not describe cost closure before delayed billing review.
20. Preserve failed, blocked, and inconclusive test history.
21. Keep raw evidence and private identifiers outside Git.
22. Include only metrics that are directly supported by reviewed evidence.
23. Do not use estimated alert counts, event counts, cost, runtime, or detection rates as actual results.
24. Do not use future-tense plans as completed achievements.
25. Publication requires an explicit `Approved` decision.

## 5. Claim maturity and resume use

| Claim level | Resume use |
|---|---|
| Designed | May be considered only when the exact design claim is approved and materially relevant |
| Implemented | Requires implementation evidence for every claimed component |
| Validated | Requires a passing test, actual result, reviewed evidence, and limitations |
| Demonstrated | Requires approved public-safe evidence that communicates the validated behavior |
| Completed | Requires the full project completion, cleanup, cost, and evidence gates |
| Mapped | May describe a relationship to a framework, but not compliance |
| Informed by | May describe design influence, but not control implementation |

Current resume use remains blocked even for design-stage wording because no exact bullet has been approved.

## 6. Required bullet record

Create one record for each proposed bullet.

| Field | Recorded value |
|---|---|
| Claim ID | Not assigned |
| Exact proposed bullet | Not populated |
| Target role | Not populated |
| Claim level | Not assessed |
| Bounded scope | Personal Azure and Microsoft Sentinel lab |
| Implemented components claimed | None |
| Validated behaviors claimed | None |
| Quantitative results claimed | None |
| Related test IDs | None |
| Related query IDs | None |
| Related analytics-rule IDs | None |
| Related evidence IDs | None |
| Material limitations | Azure execution has not started |
| Claim-evidence decision | Blocked |
| Resume publication decision | Not authorized |
| Reviewer | Not assigned |
| Review date | Not populated |

A bullet record with missing evidence cannot be approved by changing the wording alone.

## 7. Current publication block

The following conditions currently block all Sentinel RDP Honeypot v2 resume bullets:

- Azure v2 resources do not exist;
- AMA and the DCR are not implemented;
- the authentication destination table is not validated;
- `SecurityEvent` field population is not validated;
- controlled Event ID 4625 testing has not occurred;
- controlled Event ID 4624 testing has not occurred;
- remote-interactive context is not validated;
- threshold tests have not occurred;
- `AR-001` is undeployed;
- no v2 alert exists;
- no v2 incident object exists;
- no v2 triage outcome exists;
- containment is unvalidated;
- teardown is unvalidated;
- orphan review is unvalidated;
- cost data does not exist;
- no public execution evidence is approved; and
- no exact resume claim is approved.

## 8. Design-stage accomplishments inventory

The following reviewed artifacts exist, but this inventory is **not a set of approved resume bullets**:

- authoritative threat model and trust boundaries;
- architecture and scope documentation;
- production-safe design contrast;
- theory-to-lab mapping;
- implementation roadmap and definition of done;
- evidence-governance standard;
- redaction and sanitization rules;
- evidence manifest and claim-evidence matrix;
- deployment, authentication-triage, teardown, and cost-control runbooks;
- `Q-001` through `Q-020`;
- `AR-001` version `1.0.0` design;
- incident, findings, and remediation templates;
- interview talking points;
- repository validation and secret-scanning gates.

These artifacts support design-stage discussion. They do not prove Azure implementation or validation.

## 9. Unapproved design-stage bullet examples

The following examples are for governance review only. They are not authorized for resume use.

### Example A — Architecture and safety

> Designed a bounded Azure and Microsoft Sentinel authentication-monitoring lab with explicit trust boundaries, public-exposure gates, stop conditions, teardown controls, and evidence-governance requirements.

Current decision: `Blocked — exact wording not approved for resume publication`.

### Example B — Detection engineering

> Designed twenty versioned KQL queries and a disabled scheduled-rule specification for Windows remote-interactive authentication analysis.

Current decision: `Blocked — query execution and resume wording are unapproved`.

### Example C — Evidence governance

> Designed an evidence-governance workflow separating private raw evidence, sanitized public artifacts, test outcomes, claim maturity, and failed-test preservation.

Current decision: `Blocked — exact wording not approved for resume publication`.

These examples must not be used as accomplishments until the claim-evidence matrix explicitly approves them.

## 10. Prohibited current bullet patterns

Do not use any bullet that claims or implies that you:

- deployed the v2 Azure environment;
- implemented AMA or the DCR;
- validated `SecurityEvent`;
- generated production telemetry;
- deployed or tuned `AR-001`;
- detected or blocked an attacker;
- triaged a confirmed incident;
- confirmed compromise;
- identified a person from an IP address;
- validated containment;
- completed cleanup;
- achieved zero cost;
- completed the project;
- built an enterprise SOC;
- owned production incident response;
- implemented 24×7 monitoring;
- achieved NIST, CIS, or ISO compliance; or
- obtained an Authority to Operate.

## 11. Architecture bullet gate

An architecture-focused bullet requires:

- exact bounded scope;
- reviewed architecture and threat model;
- explicit design-stage wording unless implementation exists;
- no production-safe public RDP claim;
- no enterprise SOC implication;
- no framework-compliance claim;
- claim-evidence review; and
- explicit publication approval.

Current state: `Blocked`.

## 12. Azure implementation bullet gate

An implementation-focused bullet requires evidence for every claimed component, including as applicable:

- intended tenant and subscription;
- resource-group isolation;
- VM and network controls;
- Windows Firewall;
- AMA;
- DCR;
- Log Analytics;
- Microsoft Sentinel;
- analytics-rule deployment;
- runtime and cost controls; and
- versioned configuration state.

Current state: `Blocked — Azure execution has not started`.

## 13. Telemetry bullet gate

A telemetry-focused bullet requires:

- actual destination table;
- source host;
- time range;
- freshness and latency;
- required field population;
- controlled authentication evidence;
- ingestion gaps or limitations;
- related `TST-###` and `EV-###` identifiers; and
- reviewed publication wording.

Current state: `Blocked`.

## 14. Authentication-analysis bullet gate

A failed-authentication bullet requires:

- Event ID 4625 evidence;
- validated remote-interactive context when RDP-specific wording is used;
- source, account, host, and time context;
- controlled-test separation;
- pattern evidence for password-guessing or spraying wording;
- reviewed limitations; and
- approved evidence.

A successful-authentication bullet requires:

- Event ID 4624 evidence;
- validated context;
- authorization or control-test status;
- source, account, host, and time context;
- follow-on activity where relevant; and
- no unsupported compromise conclusion.

Current state: `Blocked`.

## 15. Detection bullet gate

A detection-focused bullet requires:

- exact query and rule version;
- validated schema and fields;
- below-threshold test;
- threshold-matching test;
- expected and actual results;
- alert validation;
- incident-object validation where claimed;
- false-positive and false-negative limitations;
- evidence IDs; and
- approved wording.

Current state: `Blocked`.

## 16. Triage and incident bullet gate

A triage-focused bullet requires:

- event-to-alert traceability;
- alert-to-incident linkage;
- defined disposition;
- controlled-versus-organic classification;
- successful-logon correlation where relevant;
- analyst decision;
- response action;
- evidence;
- limitations; and
- no unsupported “confirmed incident” or compromise wording.

Current state: `Blocked`.

## 17. Containment bullet gate

A containment-focused bullet requires separate evidence for:

- public exposure removal;
- resulting NSG state;
- VM containment;
- resulting VM state;
- operator decision;
- timing;
- stop condition or test;
- evidence preservation impact; and
- limitations.

Current state: `Blocked`.

## 18. Cleanup bullet gate

A cleanup-focused bullet requires:

- resource deletion;
- verified terminal state;
- resource-group verification;
- subscription-wide orphan review;
- identity and assignment disposition;
- public IP and NSG disposition;
- DCR and analytics-object disposition;
- workspace and telemetry disposition;
- immediate verification;
- 24-hour verification;
- 72-hour verification; and
- reviewed limitations.

A deletion command alone cannot support a cleanup-completion bullet.

Current state: `Blocked`.

## 19. Cost bullet gate

A cost-focused bullet requires:

- scoped runtime;
- relevant service scope;
- sanitized actual cost or usage;
- immediate review;
- 24-hour review;
- 72-hour review;
- billing-delay limitation;
- budget and alert context where claimed; and
- reviewed evidence.

Current state: `Blocked`.

## 20. Quantitative-claim gate

Before using any number, verify:

- what was counted;
- the exact time range;
- source and query;
- deduplication method;
- controlled versus organic classification;
- exclusions;
- failed or missing data;
- evidence ID;
- whether the number is actual or planned; and
- whether the number materially supports the bullet.

Current quantitative resume claims: `None authorized`.

## 21. Post-implementation bullet placeholders

Do not use until implementation evidence exists.

### Implementation placeholder A

Exact wording: Not populated.

Required evidence:

- implemented Azure scope;
- implemented AMA and DCR;
- configuration versions;
- implementation evidence IDs;
- bounded scope and limitations.

Publication decision: `Blocked`.

### Implementation placeholder B

Exact wording: Not populated.

Required evidence:

- deployed `AR-001`;
- rule version and settings;
- entity-mapping state;
- implementation evidence;
- explicit statement that validation remains separate.

Publication decision: `Blocked`.

## 22. Post-validation bullet placeholders

Do not use until tests pass with reviewed evidence.

### Validation placeholder A

Exact wording: Not populated.

Required evidence:

- `TST-010` through `TST-016` as applicable;
- actual query and rule results;
- alert and incident-object evidence;
- failed-test history;
- limitations;
- approved public evidence.

Publication decision: `Blocked`.

### Validation placeholder B

Exact wording: Not populated.

Required evidence:

- `TST-018` through `TST-025` as applicable;
- exposure-removal and containment evidence;
- teardown and orphan review;
- immediate, 24-hour, and 72-hour cost review;
- limitations;
- approved public evidence.

Publication decision: `Blocked`.

## 23. Bullet quality checklist

Before approval, verify:

- [ ] Exact wording is recorded.
- [ ] A claim ID exists.
- [ ] Claim level is correct.
- [ ] Personal-lab scope is explicit.
- [ ] Every claimed component was actually implemented.
- [ ] Every claimed behavior was actually validated.
- [ ] Quantitative values are evidence-supported.
- [ ] Event, detection, alert, incident, and compromise are not conflated.
- [ ] Controlled and organic activity are not conflated.
- [ ] IP or GeoIP is not presented as human identity.
- [ ] Framework mapping is not presented as compliance.
- [ ] Cleanup and cost closure are evidence-supported.
- [ ] Failed or inconclusive history remains visible.
- [ ] Limitations are stated.
- [ ] Public evidence is sanitized and approved.
- [ ] README and reports use the same lifecycle state.
- [ ] Claim-evidence matrix authorizes the exact wording.
- [ ] Publication decision is `Approved`.

## 24. Claim regression

A previously approved resume bullet must be reduced, corrected, or removed when:

- later evidence invalidates the wording;
- a configuration or rule changes materially;
- a required test fails;
- evidence is incomplete or unsafe;
- a metric is found to be inaccurate;
- a public artifact is rejected;
- cleanup or cost closure is incomplete;
- scope changes; or
- the bullet exceeds the project’s demonstrated maturity.

Claim regression must preserve the prior decision history.

## 25. Publication decision table

| Proposed claim ID | Exact bullet reviewed | Evidence sufficient | Limitations acceptable | Claim-evidence decision | Resume decision |
|---|---|---|---|---|---|
| None | No Sentinel RDP Honeypot v2 bullet is approved | No | Not assessed | Blocked | Not authorized |

## 26. Current resume declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Telemetry, schema, detection, alert, and incident validation have not started.
- No v2 execution evidence exists.
- No implementation or validation claim is approved.
- No quantitative result is approved.
- No Sentinel RDP Honeypot v2 resume bullet is authorized.
- Public claims remain limited to Designed or Planned.
- This file is a governance template and does not authorize resume publication.
