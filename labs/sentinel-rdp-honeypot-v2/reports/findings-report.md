# Sentinel RDP Honeypot v2 Findings Report Template

## 1. Purpose

This document is the findings-report template for Sentinel RDP Honeypot v2.

It is designed to present reviewed project results without collapsing:

- design into implementation;
- implementation into validation;
- a Windows event into an attack;
- a detection result into an alert;
- an alert into a confirmed incident;
- an incident object into compromise;
- an IP address into human identity;
- a deletion request into completed cleanup; or
- an immediate cost view into final cost.

This file is currently an **unpopulated design-stage template**. It does not establish that Azure resources were deployed, tests were executed, findings were observed, controls were validated, alerts were generated, cleanup was completed, or cost was reconciled.

## 2. Current status

| Field | Current state |
|---|---|
| Azure execution | Paused |
| Azure v2 resources | Not deployed |
| Public TCP/3389 exposure | Unauthorized |
| Controlled authentication testing | Not started |
| Telemetry and schema validation | Not started |
| Detection validation | Not started |
| Alert and incident validation | Not started |
| Teardown validation | Not started |
| Cost reconciliation | Not started |
| Recorded execution findings | None |
| Execution evidence | None captured or approved |
| Public claim level | Designed / Planned only |
| Template status | Reviewed structure; unpopulated |
| Last updated | 2026-07-14 |

No finding may be presented as observed merely because a risk, test, query, or expected result was documented.

## 3. Governing documents

Use this report with:

1. [Project README](../README.md);
2. [Threat model](../docs/threat-model.md);
3. [Architecture overview](../docs/architecture-overview.md);
4. [Scope and credibility notes](../docs/scope-and-credibility-notes.md);
5. [Project roadmap](../docs/checklist-to-project-roadmap.md);
6. [Deployment runbook](../runbooks/deployment-runbook.md);
7. [Authentication-triage runbook](../runbooks/rdp-authentication-triage.md);
8. [Teardown runbook](../runbooks/teardown-runbook.md);
9. [Cost-control checklist](../runbooks/cost-control-checklist.md);
10. [KQL query catalog](../kql/hunting-queries.md);
11. [Analytics-rule catalog](../sentinel/analytics-rules-catalog.md);
12. [Incident timeline](incident-timeline.md);
13. [Remediation recommendations](remediation-recommendations.md);
14. [Evidence governance](../evidence/README.md);
15. [Redaction notes](../evidence/redaction-notes.md);
16. [Evidence manifest](../evidence/manifest.md);
17. [Claim-evidence matrix](../evidence/claim-evidence-matrix.md); and
18. [Evidence-record template](../evidence/templates/evidence-record-template.md).

When this report conflicts with the authoritative project state or reviewed evidence, use the weaker supported conclusion and correct the report.

## 4. Reporting principles

1. State verified facts before interpretations.
2. Identify the evidence and test supporting each material fact.
3. Mark unknown, blocked, failed, and inconclusive states explicitly.
4. Preserve failed findings after a successful retest.
5. Distinguish controlled, benign, organic, suspicious, unauthorized, and undetermined activity.
6. Treat Event ID 4625 as a failed logon unless remote-interactive context is separately validated.
7. Treat Event ID 4624 as a successful logon session that still requires context.
8. Treat a query match as a detection result, not proof of an alert.
9. Treat a Sentinel alert as a platform object, not proof of compromise.
10. Treat a Sentinel incident object as an investigation case, not a confirmed security incident.
11. Treat IP and GeoIP information as investigative context, not identity or nationality.
12. Separate exposure removal from VM containment.
13. Separate requested cleanup from verified terminal state.
14. Separate immediate, 24-hour, and 72-hour cost observations.
15. Keep raw evidence and private identifiers outside Git.
16. Link only sanitized, reviewed, and approved public evidence.
17. State limitations that materially affect interpretation.
18. Keep recommendations traceable to verified findings or documented design gaps.
19. Do not hide a result because it weakens a desired portfolio claim.
20. Safety and containment take priority over report completion.

## 5. Report metadata

Populate this section only after report preparation is authorized.

| Field | Recorded value |
|---|---|
| Report title | Sentinel RDP Honeypot v2 Findings Report |
| Report version | `0.1.0-template` |
| Reporting period UTC | Not populated — template only |
| Prepared by | Not populated — keep private where required |
| Reviewed by | Not populated — template only |
| Branch | Not populated — template only |
| Commit | Not populated — template only |
| Tenant alias | Not populated — template only |
| Subscription alias | Not populated — template only |
| Resource-group alias | Not populated — template only |
| Workspace alias | Not populated — template only |
| Test-session identifiers | Not populated — template only |
| Included evidence IDs | None |
| Public sanitization status | Not populated — template only |
| Publication decision | Not approved |

Aliases must not reveal restricted identifiers.

## 6. Executive summary

### 6.1 Current design-stage summary

Sentinel RDP Honeypot v2 currently has reviewed architecture, scope, evidence-governance, operational-runbook, KQL, and analytics-rule documentation.

The Azure environment has not been deployed. Collection, schema, controlled authentication, detection, alert, incident-object, containment, teardown, and cost behavior remain unvalidated.

### 6.2 Execution-result summary

No execution-result summary exists because Azure execution and testing have not started.

### 6.3 Current claim impact

Public descriptions remain limited to Designed or Planned.

No implementation, validation, demonstration, compromise, cleanup-completion, cost-closure, or project-completion claim is authorized.

## 7. Scope and authorization review

| Review area | Verified fact | Interpretation | Limitation | Related IDs | Evidence status |
|---|---|---|---|---|---|
| Azure authorization | Azure execution remains paused | No Azure implementation activity is authorized | No live environment exists | None | No execution evidence |
| Public exposure | Public TCP/3389 exposure is unauthorized | Organic observation cannot begin | No exposure test has occurred | `TST-018` | Blocked |
| Controlled testing | Controlled authentication testing has not started | Authentication and detection outcomes are unknown | Test data does not exist | `TST-010`–`TST-017` | Blocked |
| Portfolio publication | Execution claims remain blocked | Only design-stage content may be published | No approved execution evidence exists | Governed claims | Blocked |

Replace these rows only when reviewed evidence supports a later state.

## 8. Architecture and implementation review

| Review area | Expected design | Verified implementation fact | Interpretation | Limitation | Related test | Evidence ID |
|---|---|---|---|---|---|---|
| Resource-group isolation | Dedicated project scope | Not implemented | Implementation cannot be assessed | No Azure v2 resources exist | `TST-002` | None |
| Trusted connectivity | No peering or trusted route | Not implemented | Isolation cannot be validated | No Azure v2 network exists | `TST-003` | None |
| NSG state | No unauthorized broad exposure | Not implemented | Effective exposure is unknown | No v2 NSG exists | `TST-004` | None |
| Windows Firewall | Enabled | Not implemented | Host firewall state is unknown | No v2 VM exists | `TST-005` | None |
| AMA | Intended VM reports healthy agent state | Not implemented | Agent health is unknown | No v2 VM exists | `TST-006` | None |
| DCR | Intended DCR is associated | Not implemented | Collection route is unknown | No v2 DCR exists | `TST-007` | None |

A completed design document is not implementation evidence.

## 9. Telemetry-health and schema findings

### 9.1 Summary

No telemetry-health or schema finding has been observed.

### 9.2 Validation matrix

| Area | Query | Test | Expected result | Actual result | Outcome | Evidence ID | Finding status |
|---|---|---|---|---|---|---|---|
| AMA heartbeat | `Q-001` | `TST-006`, `TST-008` | Intended VM reports current heartbeat | Not executed | Blocked | None | Not assessed |
| Destination table | `Q-002` | `TST-009` | Actual authentication destination is recorded | Not executed | Blocked | None | Not assessed |
| Freshness and latency | `Q-003` | `TST-008` | Current telemetry arrives within acceptable bounds | Not executed | Blocked | None | Not assessed |
| Authentication schema | `Q-004` | `TST-010`–`TST-012` | Required fields exist | Not executed | Blocked | None | Not assessed |
| Field population | `Q-005` | `TST-010`–`TST-012` | Required fields have acceptable population | Not executed | Blocked | None | Not assessed |
| Source-host coverage | `Q-006` | `TST-008` | Intended source host appears | Not executed | Blocked | None | Not assessed |

Do not claim that `SecurityEvent` is the validated destination until actual data supports that conclusion.

## 10. Authentication findings

### 10.1 Failed authentication

| Test | Query | Verified fact | Interpretation | Limitation | Outcome | Evidence ID |
|---|---|---|---|---|---|---|
| `TST-010` | `Q-007`, `Q-011` | No controlled Event ID 4625 result exists | Failed-authentication behavior is unvalidated | Azure execution is paused | Blocked | None |
| `TST-011` | `Q-008`, `Q-011` | No remote-interactive context has been validated | RDP-specific wording is not authorized | Event ID 4625 alone is insufficient | Blocked | None |

Approved base wording after evidence exists:

> A failed logon was recorded.

Stronger remote-interactive wording requires validated logon-type or equivalent schema context.

### 10.2 Successful authentication

| Test | Query | Verified fact | Interpretation | Limitation | Outcome | Evidence ID |
|---|---|---|---|---|---|---|
| `TST-012` | `Q-009`, `Q-010`, `Q-012` | No controlled Event ID 4624 result exists | Successful-session behavior is unvalidated | No controlled test was executed | Blocked | None |
| Correlation review | `Q-019` | No failed-to-successful correlation result exists | No compromise hypothesis is supported | Temporal correlation alone would remain insufficient | Blocked | None |

A successful logon session is not automatically authorized, unauthorized, benign, or malicious.

## 11. Detection-engineering findings

### 11.1 Query validation

| Area | Query | Expected result | Actual result | Outcome | Evidence ID | Limitation |
|---|---|---|---|---|---|---|
| Below threshold | `Q-013` | Controlled activity remains below the detection threshold | Not executed | Blocked | None | No controlled events exist |
| Threshold matching | `Q-014` | Controlled activity satisfies the detection threshold | Not executed | Blocked | None | No controlled events exist |
| Canonical detection | `Q-020` version `1.0.0` | Qualifying source-host-window result | Not executed | Blocked | None | Schema and field quality are unvalidated |

### 11.2 Analytics rule

| Rule | Design state | Implementation fact | Validation fact | Interpretation | Limitation |
|---|---|---|---|---|---|
| `AR-001` version `1.0.0` | Reviewed, disabled by design | Undeployed | Unvalidated | Rule behavior is unknown | Entity mappings and platform settings remain untested |

A `Q-020` result would not independently prove that `AR-001` executed or created an alert.

## 12. Alert and incident-object findings

| Area | Test | Verified fact | Interpretation | Limitation | Outcome | Evidence ID |
|---|---|---|---|---|---|---|
| Alert creation | `TST-015` | No v2 alert exists | Alert behavior is unvalidated | `AR-001` is undeployed | Blocked | None |
| Incident-object behavior | `TST-016` | No v2 incident object exists | Incident creation and grouping are unvalidated | No alert exists | Blocked | None |
| Triage | Applicable triage test | No v2 alert or event set has been triaged | No disposition is available | No execution evidence exists | Blocked | None |

A true-positive detection means the rule correctly identified its intended behavior. It does not automatically mean compromise occurred.

## 13. Activity classification findings

Use the [authentication-triage runbook](../runbooks/rdp-authentication-triage.md).

| Activity set | Verified facts | Classification | Interpretation | Confidence | Evidence ID | Limitation |
|---|---|---|---|---|---|---|
| No activity set | No v2 authentication activity has been reviewed | Not applicable | No classification is authorized | Not assessed | None | Execution has not started |

Allowed classifications include:

- Controlled;
- Benign;
- Expected administrative;
- Organic;
- Suspicious;
- Unauthorized;
- Undetermined; and
- Not applicable.

`Organic` means the activity was not classified as a controlled test. It does not mean malicious.

## 14. Exposure and containment findings

| Control | Test | Expected result | Actual result | Outcome | Evidence ID | Limitation |
|---|---|---|---|---|---|---|
| Exposure removal | `TST-018` | Public TCP/3389 can be removed promptly | Not executed | Blocked | None | Exposure is unauthorized and no v2 NSG exists |
| VM containment | `TST-019` | VM can reach a verified safe state | Not executed | Blocked | None | No v2 VM exists |

An NSG update request is not proof that exposure was removed. A VM stop request is not proof that containment completed.

## 15. Teardown and cleanup findings

| Area | Test | Expected result | Actual result | Outcome | Evidence ID | Limitation |
|---|---|---|---|---|---|---|
| Scoped deletion | `TST-021` | Intended project resources reach verified terminal state | Not executed | Blocked | None | No v2 resources exist |
| Orphan review | `TST-022` | No unapproved residual resource remains | Not executed | Blocked | None | No teardown occurred |
| Identity disposition | Applicable teardown test | Temporary identities and assignments are removed | Not executed | Blocked | None | No v2 identity implementation exists |
| Telemetry disposition | Applicable teardown test | DCR, rule, workspace, and telemetry state are recorded | Not executed | Blocked | None | No v2 telemetry implementation exists |

Cleanup may be described as completed only after verified terminal state, residual review, identity disposition, telemetry disposition, and applicable limitations are recorded.

## 16. Cost findings

| Review | Test | Expected result | Actual result | Outcome | Evidence ID | Limitation |
|---|---|---|---|---|---|---|
| Immediate | `TST-023` | Preliminary scoped cost or usage observation | Not executed | Blocked | None | Azure execution has not started |
| 24-hour | `TST-024` | Delayed billing observation | Not executed | Blocked | None | No v2 cost data exists |
| 72-hour | `TST-025` | Final planned reconciliation observation | Not executed | Blocked | None | No v2 cost data exists |

An immediate billing view is preliminary and cannot support a final-cost or zero-cost claim.

## 17. Evidence and publication findings

| Review area | Verified fact | Interpretation | Limitation | Related artifact |
|---|---|---|---|---|
| Raw evidence | No v2 execution evidence has been captured | There is no implementation or validation evidence package | Azure execution has not started | Evidence governance |
| Public evidence | No execution artifact is approved for public use | Publication remains design-stage only | No sanitized execution derivative exists | Evidence manifest |
| Claim support | Governed implementation and validation claims remain blocked | Resume and portfolio promotion is unauthorized | Claim-evidence matrix has no passing execution support | Claim-evidence matrix |
| Failed-test preservation | No executed test record exists | Preservation requirements remain prospective | No test has run | Evidence governance |

## 18. Finding register

Add one row per reviewed finding. Do not create a finding from an expectation alone.

| Sequence | Finding title | Category | Verified fact | Interpretation | Risk or impact | Status | Related IDs | Evidence ID | Recommendation reference | Limitation |
|---:|---|---|---|---|---|---|---|---|---|---|
| 0 | No execution findings recorded | Project status | Azure execution and testing have not started | The report contains no observed implementation or validation finding | Not assessed | Not assessed | None | None | None | Design-stage status only |

Remove the status row only when the first evidence-supported finding is recorded.

### Allowed categories

- Authorization
- Architecture
- Identity
- Network exposure
- Host security
- Telemetry health
- Schema
- Authentication
- Detection
- Alert
- Incident-object workflow
- Triage
- Containment
- Teardown
- Cost
- Evidence governance
- Claim governance
- Documentation
- Limitation

### Allowed finding statuses

- Not assessed
- Open
- Accepted
- Mitigated
- Remediated
- Blocked
- Inconclusive
- Superseded
- Closed

A `Closed` status requires a recorded basis and reviewed evidence.

## 19. Detailed finding template

Copy this section for each evidence-supported finding.

### Finding title

| Field | Recorded value |
|---|---|
| Finding sequence | Not populated |
| Category | Not populated |
| Date identified UTC | Not populated |
| Identified by | Not populated |
| Related test IDs | Not populated |
| Related query IDs | Not populated |
| Related analytics-rule IDs | Not populated |
| Related evidence IDs | Not populated |
| Finding status | Not assessed |
| Priority | Not assessed |
| Claim impact | Not assessed |

#### Verified fact

State only what the reviewed evidence directly establishes.

#### Evidence basis

Identify the applicable test, configuration or query version, evidence ID, timestamp, and public-safe evidence path.

#### Interpretation

Explain what the fact may mean within the bounded project context.

#### Risk or impact

Describe the technical, safety, operational, cost, evidence, or claim impact.

#### Limitation

State missing evidence, alternative explanations, scope boundaries, timing uncertainty, field-quality issues, or other constraints.

#### Recommendation reference

Link to the corresponding entry in [remediation recommendations](remediation-recommendations.md).

#### Validation or closure criteria

Define the evidence required to accept, mitigate, remediate, supersede, or close the finding.

#### Claim impact

State whether the finding:

- leaves the claim unchanged;
- blocks promotion;
- requires regression;
- supports implementation;
- supports validation; or
- supports a bounded demonstration.

Do not promote a claim from this section alone. The claim-evidence matrix remains authoritative.

## 20. Limitations and unresolved questions

Current limitations include:

- no live Azure v2 environment;
- no implemented AMA or DCR;
- no validated authentication destination table;
- no validated `SecurityEvent` field population;
- no controlled Event ID 4625 result;
- no controlled Event ID 4624 result;
- no validated remote-interactive context;
- no executed threshold test;
- no deployed `AR-001`;
- no alert or incident-object evidence;
- no organic observation period;
- no containment validation;
- no teardown or orphan-review evidence;
- no cost data;
- no public execution evidence; and
- no approved implementation or validation claim.

Record unresolved questions without inventing answers.

## 21. Recommendations handoff

Recommendations belong in [remediation recommendations](remediation-recommendations.md).

Each recommendation must identify:

- the evidence-supported finding or documented design gap;
- the affected asset, control, workflow, or claim;
- proposed action;
- priority rationale;
- safety constraints;
- owner;
- validation criteria;
- rollback or contingency where applicable;
- evidence required for closure; and
- claim impact.

A recommendation does not prove that remediation occurred.

## 22. Claim-impact review

| Proposed claim area | Current report support | Strongest current wording |
|---|---|---|
| Architecture and safety design | Supported by reviewed documentation | Designed |
| Evidence governance | Supported as a documentation baseline | Designed / documented |
| Operational runbooks | Reviewed but unexecuted | Designed |
| KQL catalog | Reviewed but unexecuted | Designed |
| `AR-001` | Reviewed, disabled, undeployed, and unverified | Designed |
| Azure implementation | No support | Not permitted |
| Telemetry validation | No support | Not permitted |
| Detection validation | No support | Not permitted |
| Alert or incident workflow | No support | Not permitted |
| Containment validation | No support | Not permitted |
| Cleanup completion | No support | Not permitted |
| Cost closure | No support | Not permitted |
| Completed project | No support | Not permitted |

Resume bullets remain blocked until exact wording is approved by the claim-evidence matrix.

## 23. Quality-control checklist

Before publication, verify:

- [ ] The reporting period and scope are explicit.
- [ ] Every execution statement is supported by reviewed evidence.
- [ ] Facts, interpretations, limitations, and recommendations are separate.
- [ ] Each material finding links to related test and evidence identifiers.
- [ ] Failed, blocked, and inconclusive results remain visible.
- [ ] Controlled and organic or unclassified activity remain separate.
- [ ] Event ID 4625 is not presented as automatic proof of RDP or attack.
- [ ] Event ID 4624 is not presented as automatic proof of authorization or compromise.
- [ ] Query results, alerts, incident objects, and compromise conclusions remain separate.
- [ ] IP and GeoIP context is not presented as identity or nationality.
- [ ] Exposure removal and VM containment are separate.
- [ ] Cleanup request and verified terminal state are separate.
- [ ] Immediate, 24-hour, and 72-hour cost reviews are separate.
- [ ] Recommendations are traceable to findings or documented design gaps.
- [ ] Raw evidence and restricted identifiers remain outside Git.
- [ ] Public evidence references are sanitized, reviewed, and approved.
- [ ] Limitations and unresolved questions are disclosed.
- [ ] Claim impact matches the claim-evidence matrix.
- [ ] Resume and portfolio claims are not promoted prematurely.
- [ ] Secret and disclosure scans pass.

## 24. Findings-report completion criteria

This report is complete only when:

- report metadata and scope are recorded;
- material implementation and validation areas are assessed;
- observed facts are evidence-supported;
- interpretations are qualified;
- limitations and alternative explanations are recorded;
- failed, blocked, and inconclusive outcomes remain traceable;
- each material finding has a status and related identifiers;
- recommendations are linked without implying remediation;
- applicable containment, teardown, orphan, and cost findings are included;
- public evidence is approved;
- claim impact is reviewed;
- publication checks pass; and
- no mandatory report section relies on fabricated results.

A completed template structure is not a completed findings report.

## 25. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Telemetry, schema, detection, alert, and incident validation have not started.
- No execution finding has been recorded.
- No v2 execution evidence exists.
- No remediation result exists.
- No cleanup or cost finding exists.
- Resume and portfolio claims remain blocked.
- Public claims remain limited to Designed or Planned.
- This file is an unpopulated reporting template and does not fabricate results.
