# Sentinel RDP Honeypot v2 Remediation Recommendations Template

## 1. Purpose

This document is the remediation-recommendations template for Sentinel RDP Honeypot v2.

It is designed to convert evidence-supported findings or documented design gaps into bounded, testable corrective actions without implying that:

- a finding was observed when it was only anticipated;
- a recommendation was approved merely because it was written;
- remediation was implemented merely because an action was assigned;
- remediation was validated merely because a change was made;
- cleanup completed merely because deletion was requested;
- risk was eliminated merely because a control was proposed; or
- a portfolio claim may be promoted without reviewed evidence.

This file is currently an **unpopulated design-stage template**. It does not establish that a live finding exists, a corrective action was approved, remediation was implemented, validation passed, cleanup completed, or risk was accepted.

## 2. Current status

| Field | Current state |
|---|---|
| Azure execution | Paused |
| Azure v2 resources | Not deployed |
| Public TCP/3389 exposure | Unauthorized |
| Controlled authentication testing | Not started |
| Recorded execution findings | None |
| Approved remediation actions | None |
| Implemented remediation actions | None |
| Validated remediation actions | None |
| Residual-risk decisions | None |
| Execution evidence | None captured or approved |
| Public claim level | Designed / Planned only |
| Template status | Reviewed structure; unpopulated |
| Last updated | 2026-07-14 |

No recommendation may be presented as completed merely because this template defines a proposed action or closure criterion.

## 3. Governing documents

Use this template with:

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
13. [Findings report](findings-report.md);
14. [Evidence governance](../evidence/README.md);
15. [Redaction notes](../evidence/redaction-notes.md);
16. [Evidence manifest](../evidence/manifest.md);
17. [Claim-evidence matrix](../evidence/claim-evidence-matrix.md); and
18. [Evidence-record template](../evidence/templates/evidence-record-template.md).

When a recommendation conflicts with the threat model, authorization boundary, current evidence, or a safer alternative, use the safer supported action and document the decision.

## 4. Recommendation principles

1. Link each execution-derived recommendation to a reviewed finding.
2. Label design-stage recommendations as design gaps rather than observed failures.
3. Separate the proposed action from implementation and validation state.
4. Record the affected asset, control, workflow, or claim.
5. State the risk or impact the recommendation is intended to address.
6. Define priority using evidence, safety, and project scope.
7. Identify prerequisites and authorization requirements.
8. Define an implementation owner before execution.
9. Define acceptance criteria before changing the environment.
10. Define validation tests and required evidence.
11. Define rollback, containment, or contingency actions.
12. Preserve failed and inconclusive remediation attempts.
13. Record residual risk after validation.
14. Keep public TCP/3389 exposure subject to a separate explicit authorization gate.
15. Do not weaken credentials or host controls to manufacture telemetry.
16. Do not disable Windows Firewall as a convenience.
17. Do not introduce trusted-network connectivity.
18. Do not use destructive automated response.
19. Do not delete evidence to make a result appear successful.
20. Keep raw evidence and private identifiers outside Git.
21. Do not present a recommendation as risk acceptance.
22. Do not present risk acceptance as remediation.
23. Do not present implementation as validation.
24. Do not promote public claims until the claim-evidence matrix supports them.
25. Safety and containment take priority over recommendation completion.

## 5. Recommendation metadata

Populate this section only when recommendation review is authorized.

| Field | Recorded value |
|---|---|
| Document version | `0.1.0-template` |
| Reporting period UTC | Not populated — template only |
| Prepared by | Not populated — keep private where required |
| Reviewed by | Not populated — template only |
| Branch | Not populated — template only |
| Commit | Not populated — template only |
| Tenant alias | Not populated — template only |
| Subscription alias | Not populated — template only |
| Resource-group alias | Not populated — template only |
| Workspace alias | Not populated — template only |
| Included finding sequences | None |
| Included evidence IDs | None |
| Publication decision | Not approved |

Aliases must not expose restricted identifiers.

## 6. Recommendation lifecycle

Recommendation lifecycle and validation outcome are separate.

| Lifecycle state | Meaning |
|---|---|
| Proposed | The action is documented but not approved |
| Reviewed | Technical, safety, scope, and evidence requirements were reviewed |
| Approved | The action is authorized for bounded implementation |
| In progress | Authorized implementation has started |
| Implemented | The action was completed as designed, but effectiveness is not yet validated |
| Validated | Defined acceptance criteria passed with reviewed evidence |
| Deferred | The action was intentionally postponed with a documented basis |
| Rejected | The action was declined because it is unsafe, unnecessary, unsupported, or out of scope |
| Superseded | A later recommendation replaces the action while preserving history |
| Closed | The recommendation reached a documented terminal decision |

Allowed validation outcomes:

- Not run
- Passed
- Failed
- Blocked
- Inconclusive
- Not applicable

An `Implemented` recommendation with a `Failed` validation outcome remains unresolved.

## 7. Priority model

| Priority | Use |
|---|---|
| Critical | Immediate action is required to address an active mandatory stop condition, unauthorized exposure, suspected compromise, unsafe cost growth, or evidence of uncontrolled scope |
| High | Action is required before testing, enablement, continued observation, or claim promotion |
| Medium | Action materially improves reliability, detection quality, evidence quality, or operational clarity |
| Low | Action improves maintainability or presentation without materially changing current risk |
| Not assessed | Evidence or scope is insufficient to assign priority |

Priority must be justified. It must not be inferred solely from a generic severity label or framework mapping.

## 8. Recommendation register

Add one row for each reviewed recommendation.

| Sequence | Recommendation title | Source type | Related finding or design gap | Priority | Lifecycle | Validation outcome | Owner | Target date | Related IDs | Evidence ID | Claim impact |
|---:|---|---|---|---|---|---|---|---|---|---|---|
| 0 | No remediation recommendation recorded | Project status | No execution finding exists | Not assessed | Proposed | Not run | Not assigned | Not applicable | None | None | Claims remain Designed / Planned |

Remove the status row only when the first reviewed recommendation is recorded.

### Allowed source types

- Evidence-supported finding
- Documented design gap
- Failed test
- Blocked test
- Inconclusive test
- Safety stop condition
- Cost-control exception
- Teardown exception
- Evidence-governance exception
- Claim-governance exception
- Platform or schema change
- Documentation inconsistency

A design gap must not be described as an observed implementation failure.

## 9. Detailed recommendation template

Copy this section for each reviewed recommendation.

### Recommendation title

| Field | Recorded value |
|---|---|
| Recommendation sequence | Not populated |
| Source type | Not populated |
| Related finding sequence | Not populated |
| Related test IDs | Not populated |
| Related query IDs | Not populated |
| Related analytics-rule IDs | Not populated |
| Related evidence IDs | Not populated |
| Affected asset, control, workflow, or claim | Not populated |
| Priority | Not assessed |
| Lifecycle state | Proposed |
| Validation outcome | Not run |
| Owner | Not assigned |
| Target date | Not populated |
| Claim impact | Not assessed |

#### Verified basis

State the evidence-supported finding, failed or blocked test, stop condition, design gap, or governance exception that justifies the recommendation.

Do not restate an expected result as an observed fact.

#### Risk or impact

Describe the technical, safety, operational, cost, evidence, privacy, or claim impact.

#### Proposed action

State the bounded corrective or preventive action.

#### Scope boundaries

State what the recommendation does and does not authorize.

#### Prerequisites

List required approvals, validated dependencies, tools, access, evidence preparation, cost controls, and rollback readiness.

#### Implementation steps

Record only reviewed steps. Do not execute Azure operations from this template.

#### Safety controls

Identify applicable stop conditions, exposure boundaries, credential controls, network-isolation requirements, evidence restrictions, and operator-presence requirements.

#### Validation plan

Define:

- related `TST-###` identifiers;
- applicable `Q-###` or `AR-###` versions;
- expected result;
- actual result field;
- pass and fail criteria;
- evidence requirements;
- reviewer; and
- retest conditions.

#### Rollback or contingency

Define how to restore a safe state or stop the action if implementation fails, scope changes, exposure becomes unsafe, or evidence cannot be preserved safely.

#### Residual risk

Record risk remaining after implementation and after validation.

#### Evidence required for closure

Identify the reviewed evidence needed to support implementation, validation, mitigation, remediation, acceptance, rejection, or closure.

#### Claim impact

State whether the recommendation:

- leaves claims unchanged;
- blocks claim promotion;
- requires claim regression;
- supports an implementation claim after evidence review;
- supports a validation claim after evidence review; or
- supports no public claim.

The claim-evidence matrix remains authoritative.

## 10. Architecture and authorization recommendations

Use this section for recommendations related to:

- tenant and subscription verification;
- resource-group isolation;
- trusted-network separation;
- operator identity and least privilege;
- MFA;
- disposable-resource boundaries;
- naming and tagging;
- budget and runtime controls;
- predeployment authorization; and
- public-exposure authorization.

| Recommendation area | Current verified state | Recommendation state | Authorization boundary |
|---|---|---|---|
| Azure deployment | Paused | No execution recommendation approved | Predeployment gate remains blocked |
| Public TCP/3389 | Unauthorized | No exposure recommendation approved | Separate explicit GO decision required |
| Controlled testing | Not started | No test-execution recommendation approved | Telemetry and safety prerequisites must pass |
| Portfolio claims | Designed / Planned only | No claim promotion approved | Claim-evidence review required |

These rows record current boundaries, not completed remediation.

## 11. Identity and credential recommendations

Recommendations in this category must preserve:

- MFA for the operator;
- least privilege;
- no production credentials;
- no reused credentials;
- no intentionally weak or guessable credentials;
- no cloud credentials stored on the VM;
- explicit account purpose;
- break-glass and recovery considerations where applicable; and
- removal of temporary assignments during teardown.

A recommendation to weaken authentication controls for telemetry generation is prohibited.

## 12. Network and host-security recommendations

Recommendations in this category must preserve:

- isolated scope;
- no VNet peering or trusted route;
- Windows Firewall enabled;
- no allow-all inbound rule;
- bounded NSG source and time conditions;
- explicit exposure-removal method;
- VM containment method;
- no unrelated services exposed;
- no production data; and
- no continuation after a mandatory stop condition.

A recommendation to disable Windows Firewall or broaden exposure without explicit authorization is prohibited.

## 13. Telemetry and schema recommendations

Potential recommendation areas include:

- AMA installation or health;
- DCR association;
- destination-table correction;
- event-channel scope;
- ingestion latency;
- source-host coverage;
- Event ID 4625 field population;
- Event ID 4624 field population;
- `LogonType` or equivalent remote-interactive context;
- source-IP field quality;
- account-field quality;
- timestamp consistency; and
- telemetry-health monitoring.

A recommendation to change queries before validating the actual schema must state that dependency.

## 14. Detection and analytics-rule recommendations

Potential recommendation areas include:

- query corrections;
- query version changes;
- threshold changes;
- frequency or lookback changes;
- aggregation-window changes;
- exclusions;
- severity;
- ATT&CK mapping;
- entity mappings;
- custom details;
- event grouping;
- incident settings;
- suppression;
- health monitoring; and
- rule disablement or retirement.

Any change to `Q-020` or `AR-001` requires versioning and applicable retesting.

No recommendation may authorize destructive automated response.

## 15. Triage and incident-workflow recommendations

Potential recommendation areas include:

- event-to-alert traceability;
- alert-to-incident linkage;
- disposition criteria;
- controlled-versus-organic classification;
- successful-logon correlation;
- duplicate-alert handling;
- incident grouping;
- closure rationale;
- false-positive review;
- false-negative review; and
- preservation of undetermined outcomes.

A platform incident object is an investigation case, not proof of confirmed compromise.

## 16. Containment recommendations

Containment recommendations must distinguish:

- public exposure removal;
- VM containment;
- account containment;
- evidence preservation;
- telemetry preservation;
- service shutdown;
- resource deletion; and
- emergency escalation.

A recommendation to remove exposure must define how the resulting state will be verified.

A recommendation to stop or delete a VM must define the safe terminal state and evidence implications.

## 17. Teardown and cleanup recommendations

Potential recommendation areas include:

- resource-group deletion;
- individual-resource deletion;
- orphan-resource search;
- public-IP disposition;
- NSG disposition;
- DCR disposition;
- analytics-rule disposition;
- workspace and telemetry retention;
- identity and assignment removal;
- evidence disposition;
- immediate verification;
- 24-hour verification; and
- 72-hour verification.

A deletion command or portal request is not sufficient closure evidence.

Cleanup may be described as completed only after terminal-state verification, residual review, identity and telemetry disposition, and recorded limitations.

## 18. Cost recommendations

Potential recommendation areas include:

- budget thresholds;
- alert recipients;
- VM size;
- runtime;
- public-exposure duration;
- Log Analytics daily cap;
- retention;
- query cadence;
- resource tagging;
- immediate cost review;
- 24-hour review;
- 72-hour review; and
- response to unexpected cost growth.

A recommendation to declare zero cost before billing settlement is prohibited.

## 19. Evidence and publication recommendations

Potential recommendation areas include:

- missing evidence metadata;
- incomplete hashes;
- unsafe screenshots;
- unredacted identifiers;
- unsupported claim wording;
- missing failed-test history;
- absent public approval;
- incorrect evidence lifecycle;
- incorrect test outcome;
- stale claim-evidence mappings;
- evidence supersession; and
- private-versus-public storage violations.

Evidence sanitization must preserve technical meaning.

A recommendation to publish raw evidence or restricted identifiers is prohibited.

## 20. Risk acceptance and exception handling

Risk acceptance is not remediation.

A risk-acceptance record must identify:

- the specific unresolved risk;
- evidence basis;
- affected scope;
- reason remediation is not being performed;
- decision owner;
- duration;
- compensating controls;
- monitoring requirement;
- review date;
- revocation criteria;
- claim impact; and
- public-disclosure decision.

This personal lab does not imply enterprise risk-acceptance authority.

## 21. Validation and closure matrix

| Recommendation sequence | Implementation evidence | Validation test | Expected result | Actual result | Outcome | Residual risk | Closure decision | Reviewer |
|---:|---|---|---|---|---|---|---|---|
| 0 | None | None | Not applicable | Not run | Blocked | Not assessed | No recommendation exists | None |

A recommendation cannot be marked `Validated` or `Closed` solely because implementation evidence exists.

## 22. Failed-remediation preservation

Failed, blocked, and inconclusive remediation attempts must:

- retain their original recommendation sequence;
- retain related finding, test, and evidence identifiers;
- record the actual implementation state;
- record the failed acceptance criterion;
- preserve approved evidence;
- document rollback or containment actions;
- record residual risk;
- link corrective follow-up work;
- remain traceable after a successful retest; and
- trigger claim regression when prior wording is no longer supported.

A successful retest must not erase the earlier failure.

## 23. Recommendation quality checklist

Before approval, verify:

- [ ] The recommendation is linked to an evidence-supported finding or clearly labeled design gap.
- [ ] Verified basis and interpretation are separate.
- [ ] Priority has a documented rationale.
- [ ] Scope and authorization boundaries are explicit.
- [ ] An implementation owner is identified.
- [ ] Prerequisites are defined.
- [ ] Safety controls are defined.
- [ ] Acceptance criteria are testable.
- [ ] Validation tests and evidence requirements are identified.
- [ ] Rollback or contingency is defined.
- [ ] Residual risk can be recorded.
- [ ] Failed or inconclusive attempts will remain traceable.
- [ ] Raw evidence and restricted identifiers remain outside Git.
- [ ] No destructive automated response is introduced.
- [ ] No credential weakening is introduced.
- [ ] Windows Firewall remains enabled.
- [ ] Public exposure remains separately authorized.
- [ ] Cleanup verification is stronger than deletion request evidence.
- [ ] Cost closure includes delayed reviews where applicable.
- [ ] Claim impact is recorded.
- [ ] The claim-evidence matrix controls public wording.

## 24. Template completion criteria

This template is structurally complete when it:

- defines recommendation lifecycle separately from validation outcome;
- defines priority and source types;
- provides a recommendation register;
- provides a detailed recommendation structure;
- covers architecture, identity, network, host, telemetry, detection, triage, containment, teardown, cost, evidence, and claim governance;
- defines implementation, validation, rollback, residual-risk, and evidence requirements;
- preserves failed remediation attempts;
- separates risk acceptance from remediation;
- contains no fabricated implementation or validation result;
- keeps Azure execution and public exposure blocked;
- keeps resume and portfolio promotion evidence-gated; and
- passes link, disclosure, and secret review.

A structurally complete template does not mean remediation was performed.

## 25. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- No execution finding has been recorded.
- No remediation recommendation has been approved.
- No remediation action has been implemented.
- No remediation action has been validated.
- No risk-acceptance decision exists.
- No v2 execution evidence exists.
- Resume and portfolio claims remain blocked.
- Public claims remain limited to Designed or Planned.
- This file is an unpopulated remediation template and does not fabricate corrective-action results.
