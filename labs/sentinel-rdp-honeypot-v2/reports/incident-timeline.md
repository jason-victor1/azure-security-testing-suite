# Sentinel RDP Honeypot v2 Incident Timeline Template

## 1. Purpose

This document is the chronology template for Sentinel RDP Honeypot v2.

It is designed to preserve the sequence of:

- approved test preparation;
- Windows authentication events;
- KQL query execution;
- detection results;
- Microsoft Sentinel alert creation;
- incident-object creation or correlation;
- analyst decisions;
- exposure changes;
- containment actions;
- evidence capture;
- teardown;
- cost-review checkpoints; and
- claim-impact decisions.

This file is currently an **empty design-stage template**. It does not establish that any Azure resource, event, detection, alert, incident object, response action, teardown step, evidence artifact, or cost review exists.

## 2. Current status

| Field | Current state |
|---|---|
| Azure execution | Paused |
| Azure v2 resources | Not deployed |
| Public TCP/3389 exposure | Unauthorized |
| Controlled authentication testing | Not started |
| Detection validation | Not started |
| Alert validation | Not started |
| Incident-object validation | Not started |
| Teardown validation | Not started |
| Cost reconciliation | Not started |
| Timeline entries | None |
| Execution evidence | None captured or approved |
| Public claim level | Designed / Planned only |
| Template status | Reviewed structure; unpopulated |
| Last updated | 2026-07-14 |

No timestamped execution row may be added merely because an activity is planned.

## 3. Governing documents

Use this template with:

1. [Project README](../README.md);
2. [Threat model](../docs/threat-model.md);
3. [Scope and credibility notes](../docs/scope-and-credibility-notes.md);
4. [Project roadmap](../docs/checklist-to-project-roadmap.md);
5. [Deployment runbook](../runbooks/deployment-runbook.md);
6. [Authentication-triage runbook](../runbooks/rdp-authentication-triage.md);
7. [Teardown runbook](../runbooks/teardown-runbook.md);
8. [Cost-control checklist](../runbooks/cost-control-checklist.md);
9. [KQL query catalog](../kql/hunting-queries.md);
10. [Analytics-rule catalog](../sentinel/analytics-rules-catalog.md);
11. [Evidence governance](../evidence/README.md);
12. [Redaction notes](../evidence/redaction-notes.md);
13. [Evidence manifest](../evidence/manifest.md);
14. [Claim-evidence matrix](../evidence/claim-evidence-matrix.md); and
15. [Evidence-record template](../evidence/templates/evidence-record-template.md).

When this timeline conflicts with the authoritative project state or reviewed evidence, correct the timeline and use the weaker supported conclusion.

## 4. Timeline rules

1. Use UTC for every execution timestamp.
2. Preserve the source of each timestamp.
3. Record the observed fact separately from interpretation.
4. Record decisions separately from the event that prompted them.
5. Record the actual action taken, not only the intended action.
6. Link material rows to applicable `TST-###`, `Q-###`, `AR-###`, `EV-###`, and `CLM-###` identifiers.
7. Preserve failed, blocked, and inconclusive outcomes.
8. Do not replace an earlier failed row with a later successful row.
9. Treat controlled-test activity separately from organic or unclassified activity.
10. Do not infer human identity from an IP address or GeoIP result.
11. Do not infer RDP solely from Event ID 4625.
12. Do not infer compromise from a detection result, alert, or incident object.
13. Record exposure removal and VM containment as separate actions.
14. Record cleanup requests and verified terminal states separately.
15. Record immediate, 24-hour, and 72-hour cost reviews separately.
16. Keep raw evidence and private identifiers outside Git.
17. Add only sanitized, reviewed, public-safe references to this file.
18. Safety and containment take priority over completing the timeline.

## 5. Timestamp hierarchy

Use the most authoritative available timestamp for the fact being recorded.

| Timestamp type | Examples | Use |
|---|---|---|
| Source event time | Windows event `TimeGenerated` or source event time | Authentication-event chronology |
| Ingestion time | Log Analytics ingestion time | Latency and collection chronology |
| Query execution time | KQL execution record | Investigation chronology |
| Detection time | Scheduled-rule evaluation or result time | Detection chronology |
| Alert creation time | Microsoft Sentinel alert time | Alert chronology |
| Incident-object time | Incident creation, update, assignment, or closure time | Case chronology |
| Operator time | Command, portal, or decision timestamp | Administrative chronology |
| Evidence capture time | Evidence-record UTC time | Evidence chronology |
| Azure state time | Resource-state or Activity Log time | Deployment and containment chronology |
| Billing observation time | Cost-management observation time | Cost chronology |

When two systems disagree, record both timestamps and the limitation. Do not silently normalize or discard the discrepancy.

## 6. Timeline scope metadata

Populate this section only after execution is authorized.

| Field | Recorded value |
|---|---|
| Timeline purpose | Not populated — template only |
| Timeline start UTC | Not populated — template only |
| Timeline end UTC | Not populated — template only |
| Operator | Not populated — keep private where required |
| Branch | Not populated — template only |
| Commit | Not populated — template only |
| Tenant alias | Not populated — template only |
| Subscription alias | Not populated — template only |
| Resource-group alias | Not populated — template only |
| Workspace alias | Not populated — template only |
| VM alias | Not populated — template only |
| Test-session identifier | Not populated — template only |
| Exposure authorization record | Not populated — template only |
| Maximum runtime | Not populated — template only |
| Maximum exposure duration | Not populated — template only |
| Timeline reviewer | Not populated — template only |
| Public sanitization status | Not populated — template only |

Aliases must not reveal restricted identifiers.

## 7. Primary chronology

Add one row for each material event, decision, or action.

| UTC timestamp | Timestamp source | Phase | Entry type | Observed fact | Interpretation | Decision or action | Activity class | Related IDs | Evidence status | Test outcome | Limitations |
|---|---|---|---|---|---|---|---|---|---|---|---|
| No entries — Azure execution and testing have not started | Project status | Documentation | Status declaration | No v2 execution timeline exists | No implementation or validation conclusion is available | Keep the template unpopulated until authorized execution occurs | Not applicable | None | No execution evidence | Blocked | Design-stage declaration only |

Remove the status-declaration row only when the first authorized execution entry is recorded.

### Allowed phase values

- Authorization
- Deployment
- Telemetry health
- Schema validation
- Controlled failed authentication
- Controlled successful authentication
- Detection validation
- Alert validation
- Incident-object validation
- Organic or unclassified observation
- Triage
- Exposure removal
- VM containment
- Teardown
- Orphan review
- Evidence review
- Cost review
- Claim review
- Closure

### Allowed entry-type values

- State verification
- Windows event
- Query execution
- Detection result
- Alert
- Incident-object event
- Analyst observation
- Analyst decision
- Exposure change
- Response action
- Evidence capture
- Evidence review
- Test outcome
- Configuration change
- Teardown action
- Cost observation
- Claim decision
- Limitation
- Correction

### Allowed activity-class values

- Controlled
- Organic
- Benign
- Expected administrative
- Suspicious
- Unauthorized
- Undetermined
- Not applicable

`Organic` means the activity was not classified as a controlled test. It does not mean malicious.

## 8. Detection-to-incident sequence

Use this table to show the relationship between underlying events and platform objects without collapsing them into one claim.

| Sequence stage | UTC timestamp | Object or record | Verified fact | Related IDs | Evidence ID | Limitation |
|---|---|---|---|---|---|---|
| Source Windows event | Not populated | Not populated | Not populated | Not populated | Not populated | No execution has occurred |
| Query result | Not populated | Not populated | Not populated | Not populated | Not populated | No execution has occurred |
| Scheduled-rule evaluation | Not populated | Not populated | Not populated | Not populated | Not populated | No execution has occurred |
| Alert | Not populated | Not populated | Not populated | Not populated | Not populated | No alert exists |
| Incident object | Not populated | Not populated | Not populated | Not populated | Not populated | No incident object exists |
| Triage decision | Not populated | Not populated | Not populated | Not populated | Not populated | No triage occurred |
| Closure decision | Not populated | Not populated | Not populated | Not populated | Not populated | No case was closed |

A later stage does not retroactively strengthen an earlier fact beyond the available evidence.

## 9. Controlled-test chronology

### 9.1 Controlled failed authentication

Record the sequence for `TST-010` and `TST-011`.

| UTC timestamp | Step | Expected result | Actual result | Related query | Evidence ID | Outcome | Limitation |
|---|---|---|---|---|---|---|---|
| Not populated | Test authorization | Not populated | Not executed | `Q-011` | None | Blocked | Azure execution is paused |
| Not populated | Failed authentication action | Event ID 4625 under approved conditions | Not executed | `Q-011` | None | Blocked | No controlled test exists |
| Not populated | Remote-interactive validation | Validated logon type or equivalent context | Not executed | `Q-008`, `Q-011` | None | Blocked | Event ID 4625 alone is insufficient |

### 9.2 Controlled successful authentication

Record the sequence for `TST-012`.

| UTC timestamp | Step | Expected result | Actual result | Related query | Evidence ID | Outcome | Limitation |
|---|---|---|---|---|---|---|---|
| Not populated | Test authorization | Approved controlled-success procedure | Not executed | `Q-012` | None | Blocked | Azure execution is paused |
| Not populated | Successful authentication action | Event ID 4624 under approved conditions | Not executed | `Q-012` | None | Blocked | No controlled test exists |
| Not populated | Remote-interactive validation | Validated logon type or equivalent context | Not executed | `Q-010`, `Q-012` | None | Blocked | Event ID 4624 alone does not establish authorization or compromise |

### 9.3 Threshold behavior

Record the sequence for `TST-013` and `TST-014`.

| UTC timestamp | Test | Expected result | Actual result | Query and version | Rule and version | Evidence ID | Outcome |
|---|---|---|---|---|---|---|---|
| Not populated | Below-threshold | No `AR-001` alert expected | Not executed | `Q-013` / `1.0.0` | `AR-001` / `1.0.0` | None | Blocked |
| Not populated | Threshold matching | Qualifying detection result expected | Not executed | `Q-014`, `Q-020` / `1.0.0` | `AR-001` / `1.0.0` | None | Blocked |

A qualifying query result is not proof that an alert or incident object was created.

## 10. Alert and incident-object chronology

Use this section for `TST-015` and `TST-016`.

| UTC timestamp | Platform object | State change | Verified fact | Analyst action | Evidence ID | Outcome | Limitation |
|---|---|---|---|---|---|---|---|
| Not populated | Alert | Not created | No v2 alert exists | None | None | Blocked | `AR-001` is undeployed |
| Not populated | Incident object | Not created | No v2 incident object exists | None | None | Blocked | Incident behavior is unvalidated |

Do not use the phrase “confirmed incident” merely because Microsoft Sentinel created an incident object.

## 11. Triage chronology

Use the [authentication-triage runbook](../runbooks/rdp-authentication-triage.md).

| UTC timestamp | Triage step | Evidence reviewed | Verified fact | Interpretation | Disposition | Action | Related IDs |
|---|---|---|---|---|---|---|---|
| No entries | No triage performed | None | No alert or event set has been reviewed | No triage conclusion exists | Not applicable | None | None |

Potential dispositions must match the runbook and available evidence. Preserve `Undetermined` when evidence is insufficient.

## 12. Successful-logon correlation chronology

Use this section only when Event ID 4624 or equivalent successful-session evidence is within scope.

| UTC timestamp | Failed-event context | Successful-event context | Correlation basis | Verified fact | Hypothesis | Response decision | Evidence ID |
|---|---|---|---|---|---|---|---|
| No entries | None | None | None | No successful-logon correlation has been performed | No hypothesis is authorized | None | None |

A temporal relationship does not prove that the same person generated the failed and successful events.

## 13. Exposure and containment chronology

Use separate entries for public exposure, exposure removal, and VM containment.

| UTC timestamp | Control or asset | Prior state | Action | Resulting state | Verification method | Test ID | Evidence ID | Outcome |
|---|---|---|---|---|---|---|---|---|
| No entries | Public TCP/3389 | Unauthorized | None | Not exposed by project authorization | Project status | `TST-018` | None | Blocked |
| No entries | Windows VM | Not deployed | None | No v2 VM exists | Project status | `TST-019` | None | Blocked |

An NSG update request is not proof that exposure was removed. A VM stop request is not proof that the VM reached a safe terminal state.

## 14. Teardown and cleanup chronology

| UTC timestamp | Cleanup scope | Requested action | Verified terminal state | Residual review | Evidence ID | Outcome | Limitation |
|---|---|---|---|---|---|---|---|
| No entries | Resource group | None | Not applicable | Not performed | None | Blocked | No v2 resources exist |
| No entries | Identities and assignments | None | Not applicable | Not performed | None | Blocked | No v2 implementation exists |
| No entries | DCR and analytics objects | None | Not applicable | Not performed | None | Blocked | No v2 implementation exists |
| No entries | Workspace and telemetry | None | Not applicable | Not performed | None | Blocked | No retention decision exists |
| No entries | Subscription-wide orphan search | None | Not performed | Not performed | None | Blocked | Teardown has not started |

Cleanup can be described as completed only after terminal deletion, residual-resource review, identity and telemetry disposition, and applicable limitations are recorded.

## 15. Cost chronology

| Observation UTC | Observation type | Service scope | Runtime or period | Sanitized observed cost or usage | Evidence ID | Outcome | Limitation |
|---|---|---|---|---|---|---|---|
| Not populated | Immediate review | Not applicable | Not applicable | No v2 cost data exists | None | Blocked | Azure execution has not started |
| Not populated | 24-hour review | Not applicable | Not applicable | No v2 cost data exists | None | Blocked | Azure execution has not started |
| Not populated | 72-hour review | Not applicable | Not applicable | No v2 cost data exists | None | Blocked | Azure execution has not started |

An immediate billing view is preliminary and cannot support a final-cost claim.

## 16. Evidence chronology

| UTC timestamp | Evidence ID | Related test or claim | Lifecycle transition | Public artifact | Review decision | Limitation |
|---|---|---|---|---|---|---|
| No entries | None | None | No evidence captured | None | None | No v2 execution evidence exists |

Evidence lifecycle values and test outcomes must remain separate.

## 17. Decision log

Record decisions when scope, safety, interpretation, or response changes.

| Decision UTC | Decision owner | Decision | Basis | Alternatives considered | Safety impact | Evidence impact | Claim impact |
|---|---|---|---|---|---|---|---|
| No entries | None | No execution decision recorded | Azure execution remains paused | None | Exposure remains unauthorized | No evidence captured | Claims remain Designed / Planned |

## 18. Corrections and supersession

Never silently edit a material chronology fact after publication.

Use this table for corrections:

| Correction UTC | Original row timestamp | Original statement | Corrected statement | Reason | Evidence ID | Reviewer |
|---|---|---|---|---|---|---|
| No entries | None | None | None | No correction exists | None | None |

A correction must preserve the original record and explain why it changed.

## 19. Facts, interpretations, and limitations review

Before approving a public timeline, verify:

- [ ] Every material statement is classified as a fact, interpretation, decision, action, or limitation.
- [ ] Each fact has an authoritative timestamp source.
- [ ] Controlled and organic or unclassified activity are separated.
- [ ] Event ID 4625 is not presented as automatic proof of RDP or attack.
- [ ] Event ID 4624 is not presented as automatic proof of authorized or unauthorized access.
- [ ] Detection results, alerts, and incident objects remain distinct.
- [ ] No alert or incident object is presented as proof of compromise.
- [ ] No IP or GeoIP value is presented as human identity or nationality.
- [ ] Failed, blocked, and inconclusive tests remain visible.
- [ ] Exposure removal and VM containment are recorded separately.
- [ ] Cleanup requests and verified terminal states are recorded separately.
- [ ] Immediate, 24-hour, and 72-hour cost observations remain separate.
- [ ] Raw evidence and private identifiers remain outside Git.
- [ ] Public evidence references have been reviewed and approved.
- [ ] Claim impact matches the claim-evidence matrix.

## 20. Timeline completion criteria

This timeline is complete for an applicable execution window only when:

- the scope and UTC period are recorded;
- relevant event, query, detection, alert, incident, decision, and response times are present;
- timestamp sources are identified;
- facts and interpretations are separated;
- controlled and organic or unclassified activity are separated;
- applicable test outcomes are recorded;
- failed and inconclusive steps remain traceable;
- evidence IDs link to reviewed evidence;
- containment and teardown actions are represented;
- cleanup verification and residual review are represented;
- applicable cost checkpoints are represented;
- corrections are traceable;
- limitations are explicit; and
- public wording is approved through the claim-evidence process.

A populated table alone does not make the project complete.

## 21. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection, alert, and incident-object validation have not started.
- No timeline event has been recorded.
- No v2 execution evidence exists.
- No incident chronology has been demonstrated.
- No containment, cleanup, or cost chronology exists.
- Public claims remain limited to Designed or Planned.
- This file is an unpopulated reporting template and does not fabricate execution results.
