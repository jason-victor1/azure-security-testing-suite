# Sentinel RDP Honeypot v2 Scoring Worksheet

## 1. Purpose

This worksheet provides a structured quality and validation rubric for Sentinel RDP Honeypot v2.

It is designed to assess whether the project has sufficient evidence, safety controls, technical quality, documentation quality, and closure discipline to support stronger lifecycle claims.

The worksheet separates:

- documentation quality;
- implementation state;
- validation results;
- evidence quality;
- safety and authorization;
- detection-engineering quality;
- incident-analysis quality;
- teardown and cost closure;
- portfolio and interview credibility; and
- project-completion readiness.

This file is currently a **design-stage scoring template**. It does not establish that any Azure resource, test, detection, alert, incident object, containment action, cleanup step, cost review, or public claim has passed.

## 2. Current scoring state

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
| Score calculation | Not performed |
| Overall project rating | Not assessed |
| Completion decision | Not permitted |
| Public claim level | Designed / Planned only |
| Worksheet status | Reviewed structure; unpopulated |
| Last updated | 2026-07-14 |

A score must not be assigned merely because a control, test, or artifact is planned.

## 3. Governing documents

Use this worksheet with:

1. [Project README](../README.md);
2. [Threat model](../docs/threat-model.md);
3. [Architecture overview](../docs/architecture-overview.md);
4. [Scope and credibility notes](../docs/scope-and-credibility-notes.md);
5. [Production-safe design contrast](../docs/production-safe-design-contrast.md);
6. [Theory-to-lab mapping](../docs/theory-to-lab-mapping.md);
7. [Project roadmap](../docs/checklist-to-project-roadmap.md);
8. [Deployment runbook](../runbooks/deployment-runbook.md);
9. [Authentication-triage runbook](../runbooks/rdp-authentication-triage.md);
10. [Teardown runbook](../runbooks/teardown-runbook.md);
11. [Cost-control checklist](../runbooks/cost-control-checklist.md);
12. [KQL query catalog](../kql/hunting-queries.md);
13. [Analytics-rule catalog](../sentinel/analytics-rules-catalog.md);
14. [Incident timeline](../reports/incident-timeline.md);
15. [Findings report](../reports/findings-report.md);
16. [Remediation recommendations](../reports/remediation-recommendations.md);
17. [Interview talking points](../resume-assets/interview-talking-points.md);
18. [Resume-bullet governance](../resume-assets/resume-bullets.md);
19. [Evidence governance](../evidence/README.md);
20. [Redaction notes](../evidence/redaction-notes.md);
21. [Evidence manifest](../evidence/manifest.md);
22. [Claim-evidence matrix](../evidence/claim-evidence-matrix.md); and
23. [Evidence-record template](../evidence/templates/evidence-record-template.md).

When the worksheet conflicts with project status or reviewed evidence, the weaker supported conclusion controls.

## 4. Scoring principles

1. Score only what was actually reviewed.
2. Do not award implementation points for design documentation.
3. Do not award validation points for configuration screenshots alone.
4. Do not award detection points for an unexecuted query.
5. Do not award alert points for a query result.
6. Do not award incident points for an alert.
7. Do not award compromise points for an incident object.
8. Do not award RDP-specific points from Event ID 4625 alone.
9. Do not award cleanup points from a deletion request alone.
10. Do not award final-cost points from an immediate billing view.
11. Preserve failed, blocked, and inconclusive outcomes.
12. Record the evidence ID for every awarded implementation or validation point.
13. Keep evidence lifecycle status separate from test outcome.
14. Treat controlled and organic or unclassified activity separately.
15. Treat IP and GeoIP as investigative context, not identity.
16. Treat framework mapping as design context, not compliance.
17. Apply zero points where evidence is required but absent.
18. Use `Not Applicable` only with a documented scope rationale.
19. Do not use a high total score to override a mandatory stop condition.
20. Do not use a high documentation score to authorize Azure execution.
21. Do not use a passing rubric to authorize public TCP/3389 exposure.
22. Do not use the rubric as enterprise risk-acceptance authority.
23. Do not promote resume or interview claims without claim-evidence approval.
24. Safety, authorization, teardown, and evidence integrity are non-compensating gates.
25. A project may remain incomplete despite a high subtotal.

## 5. Scoring scale

Use this scale for each scored criterion.

| Score | Meaning |
|---:|---|
| 0 | Not started, missing, unsupported, unsafe, or required evidence absent |
| 1 | Planned or partially documented; major gaps remain |
| 2 | Reviewed design exists; implementation or validation not established |
| 3 | Implemented or partially validated with material limitations |
| 4 | Validated with reviewed evidence and acceptable limitations |
| 5 | Demonstrated with approved public evidence, reproducibility, and strong closure discipline |
| N/A | Reviewed and documented as outside the bounded project scope |

A score of `2` is the maximum for a design-only criterion that requires later execution.

## 6. Outcome values

Use one of the following:

- Not started
- Planned
- Reviewed
- Implemented
- Passed
- Failed
- Blocked
- Inconclusive
- Not Applicable
- Superseded

Outcome and numeric score must remain logically consistent.

## 7. Evidence requirements for scoring

Every criterion scored above `2` must include, where applicable:

- related `TST-###`;
- related `EV-###`;
- related `Q-###`;
- related `AR-###`;
- configuration or artifact version;
- expected result;
- actual result;
- test outcome;
- reviewer;
- review date;
- limitations; and
- public-evidence status.

A numeric score alone is not evidence.

## 8. Mandatory non-compensating gates

The project cannot be rated `Validated`, `Demonstrated`, or `Completed` when any applicable mandatory gate fails.

| Gate | Current state | Required terminal state |
|---|---|---|
| Correct tenant and subscription | Not assessed | Passed |
| Authorized operator identity | Not assessed | Passed |
| MFA and least privilege | Not assessed | Passed |
| No production or reused credentials | Design requirement only | Passed |
| No trusted-network connectivity | Design requirement only | Passed |
| Windows Firewall enabled | Not assessed | Passed |
| Public exposure authorization | Not granted | Passed or Not Applicable |
| Emergency exposure removal | Not validated | Passed |
| VM containment | Not validated | Passed |
| Raw evidence remains private | Documentation baseline | Passed |
| Public evidence sanitized and reviewed | No execution evidence | Passed where applicable |
| Resource teardown | Not started | Passed |
| Orphan-resource review | Not started | Passed |
| Identity and telemetry disposition | Not started | Passed |
| Immediate cost review | Not started | Passed |
| 24-hour cost review | Not started | Passed |
| 72-hour cost review | Not started | Passed |
| Claim-evidence approval | Blocked | Passed for each published claim |

A high score cannot compensate for a failed mandatory gate.

## 9. Score summary

Populate only after criteria are reviewed.

| Domain | Maximum points | Awarded points | Percentage | Domain rating | Evidence complete |
|---|---:|---:|---:|---|---|
| Documentation and architecture | 50 | Not scored | Not calculated | Not assessed | No |
| Safety and authorization | 50 | Not scored | Not calculated | Not assessed | No |
| Azure implementation | 50 | Not scored | Not calculated | Not assessed | No |
| Telemetry and schema | 50 | Not scored | Not calculated | Not assessed | No |
| Detection engineering | 60 | Not scored | Not calculated | Not assessed | No |
| Triage and incident analysis | 50 | Not scored | Not calculated | Not assessed | No |
| Evidence governance | 50 | Not scored | Not calculated | Not assessed | No |
| Containment and teardown | 50 | Not scored | Not calculated | Not assessed | No |
| Cost closure | 30 | Not scored | Not calculated | Not assessed | No |
| Reporting and remediation | 40 | Not scored | Not calculated | Not assessed | No |
| Portfolio and interview credibility | 40 | Not scored | Not calculated | Not assessed | No |
| Repository quality and security | 30 | Not scored | Not calculated | Not assessed | No |
| **Total** | **560** | **Not scored** | **Not calculated** | **Not assessed** | **No** |

Do not calculate a final rating while required evidence is absent.

## 10. Rating bands

Apply only after mandatory gates are evaluated.

| Percentage | Provisional rating | Meaning |
|---:|---|---|
| 0–39% | Insufficient | Major design, implementation, evidence, or safety gaps |
| 40–59% | Developing | Partial foundation; significant gaps remain |
| 60–74% | Designed | Coherent reviewed design; execution or validation remains incomplete |
| 75–84% | Implemented | Material implementation exists; validation remains incomplete |
| 85–94% | Validated | Defined tests passed with reviewed evidence; closure may remain |
| 95–100% | Demonstrated | Strong validated evidence, reproducibility, and approved public artifacts |

`Completed` is not a percentage band. It requires every applicable completion and closure gate to pass.

## 11. Documentation and architecture rubric

| ID | Criterion | Weight | Current outcome | Current score | Evidence IDs | Limitation |
|---|---|---:|---|---:|---|---|
| DOC-001 | Threat model defines assets, trust boundaries, risks, controls, gates, and stop conditions | 5 | Reviewed | 2 | Documentation only | No live validation |
| DOC-002 | Architecture overview matches the authoritative threat model | 5 | Reviewed | 2 | Documentation only | No implementation evidence |
| DOC-003 | Scope and credibility notes define lifecycle and claim boundaries | 5 | Reviewed | 2 | Documentation only | Claims remain design-stage |
| DOC-004 | Production-safe contrast clearly separates lab and enterprise design | 5 | Reviewed | 2 | Documentation only | No production implementation |
| DOC-005 | Theory-to-lab mapping is bounded and does not claim compliance | 5 | Reviewed | 2 | Documentation only | Framework mappings are contextual |
| DOC-006 | Roadmap defines dispositions, tests, phases, and completion criteria | 5 | Reviewed | 2 | Documentation only | Execution not started |
| DOC-007 | Architecture terminology is consistent across artifacts | 5 | Not assessed | 0 | None | Static integration review pending |
| DOC-008 | All required relative links resolve | 5 | Partially reviewed | 2 | Local validation output | Repository-wide check pending |
| DOC-009 | No mandatory artifact is empty | 5 | In progress | 1 | Commit 5 worktree | Scoring worksheet still under construction |
| DOC-010 | Documentation reflects final implemented state | 5 | Blocked | 0 | None | No implementation exists |

Current documentation subtotal must not be promoted beyond the evidence actually reviewed.

## 12. Safety and authorization rubric

| ID | Criterion | Weight | Current outcome | Current score | Related tests | Evidence IDs |
|---|---|---:|---|---:|---|---|
| SAF-001 | Correct tenant and subscription confirmed | 5 | Not started | 0 | `TST-001` | None |
| SAF-002 | Dedicated resource-group isolation confirmed | 5 | Not started | 0 | `TST-002` | None |
| SAF-003 | No peering or trusted route confirmed | 5 | Not started | 0 | `TST-003` | None |
| SAF-004 | NSG setup state is safe before exposure | 5 | Not started | 0 | `TST-004` | None |
| SAF-005 | Windows Firewall enabled | 5 | Not started | 0 | `TST-005` | None |
| SAF-006 | Operator MFA and least privilege confirmed | 5 | Not started | 0 | Predeployment gate | None |
| SAF-007 | No production or reused credentials used | 5 | Planned | 1 | Predeployment gate | None |
| SAF-008 | Public exposure receives separate explicit authorization | 5 | Blocked | 0 | Exposure gate | None |
| SAF-009 | Mandatory stop conditions are actionable | 5 | Reviewed design | 2 | `TST-018`, `TST-019` | Documentation only |
| SAF-010 | Operator presence, time limit, and runtime limit are recorded | 5 | Planned | 1 | Exposure gate | None |

No safety score authorizes deployment or exposure.

## 13. Azure implementation rubric

| ID | Criterion | Weight | Current outcome | Current score | Related tests | Evidence IDs |
|---|---|---:|---|---:|---|---|
| AZR-001 | Intended resource group implemented | 5 | Not started | 0 | `TST-002` | None |
| AZR-002 | Disposable Windows VM implemented | 5 | Not started | 0 | Deployment tests | None |
| AZR-003 | NSG implemented according to approved state | 5 | Not started | 0 | `TST-004` | None |
| AZR-004 | Windows Firewall implemented and verified | 5 | Not started | 0 | `TST-005` | None |
| AZR-005 | AMA implemented on intended VM | 5 | Not started | 0 | `TST-006` | None |
| AZR-006 | DCR implemented and associated | 5 | Not started | 0 | `TST-007` | None |
| AZR-007 | Log Analytics workspace implemented | 5 | Not started | 0 | Deployment tests | None |
| AZR-008 | Microsoft Sentinel configured | 5 | Not started | 0 | Deployment tests | None |
| AZR-009 | `AR-001` deployed in disabled validation state | 5 | Not started | 0 | Rule implementation test | None |
| AZR-010 | Implemented configuration matches reviewed design | 5 | Blocked | 0 | Integration review | None |

Documentation artifacts do not earn Azure implementation points.

## 14. Telemetry and schema rubric

| ID | Criterion | Weight | Current outcome | Current score | Query | Test | Evidence IDs |
|---|---|---:|---|---:|---|---|---|
| TEL-001 | Intended host reports current heartbeat | 5 | Blocked | 0 | `Q-001` | `TST-006`, `TST-008` | None |
| TEL-002 | Authentication destination table is recorded | 5 | Blocked | 0 | `Q-002` | `TST-009` | None |
| TEL-003 | Telemetry freshness is acceptable | 5 | Blocked | 0 | `Q-003` | `TST-008` | None |
| TEL-004 | Ingestion latency is measured and acceptable | 5 | Blocked | 0 | `Q-003` | `TST-008` | None |
| TEL-005 | Required authentication schema is observed | 5 | Blocked | 0 | `Q-004` | `TST-010`–`TST-012` | None |
| TEL-006 | Required fields have acceptable population | 5 | Blocked | 0 | `Q-005` | `TST-010`–`TST-012` | None |
| TEL-007 | Intended source host coverage is confirmed | 5 | Blocked | 0 | `Q-006` | `TST-008` | None |
| TEL-008 | Event ID 4625 is captured under controlled conditions | 5 | Blocked | 0 | `Q-007`, `Q-011` | `TST-010` | None |
| TEL-009 | Remote-interactive context is validated | 5 | Blocked | 0 | `Q-008`, `Q-011` | `TST-011` | None |
| TEL-010 | Event ID 4624 is captured under controlled conditions | 5 | Blocked | 0 | `Q-009`, `Q-012` | `TST-012` | None |

Event ID 4625 alone cannot earn remote-interactive validation points.

## 15. Detection-engineering rubric

| ID | Criterion | Weight | Current outcome | Current score | Query or rule | Evidence IDs |
|---|---|---:|---|---:|---|---|
| DET-001 | Query catalog is versioned and ordered | 5 | Reviewed | 2 | `Q-001`–`Q-020` | Documentation only |
| DET-002 | Queries are read-only | 5 | Reviewed | 2 | `Q-001`–`Q-020` | Documentation only |
| DET-003 | Schema-dependent logic is explicitly gated | 5 | Reviewed | 2 | Query catalog | Documentation only |
| DET-004 | Below-threshold behavior is validated | 5 | Blocked | 0 | `Q-013`, `AR-001` | None |
| DET-005 | Threshold-matching behavior is validated | 5 | Blocked | 0 | `Q-014`, `Q-020`, `AR-001` | None |
| DET-006 | `Q-020` and `AR-001` canonical query remain identical | 5 | Reviewed | 2 | Version `1.0.0` | Static comparison only |
| DET-007 | Alert creation is validated separately from query results | 5 | Blocked | 0 | `AR-001` | None |
| DET-008 | Incident-object behavior is validated separately from alerts | 5 | Blocked | 0 | Sentinel incident settings | None |
| DET-009 | Entity mappings are validated against actual fields | 5 | Blocked | 0 | `AR-001` | None |
| DET-010 | Threshold, frequency, lookback, and grouping are tuned | 5 | Blocked | 0 | `AR-001` | None |
| DET-011 | False-positive and false-negative limitations are assessed | 5 | Blocked | 0 | Query and rule set | None |
| DET-012 | Rule health and failure conditions are monitored | 5 | Blocked | 0 | Rule-health design | None |

A query or rule design cannot score above `2` before execution evidence exists.

## 16. Triage and incident-analysis rubric

| ID | Criterion | Weight | Current outcome | Current score | Related tests | Evidence IDs |
|---|---|---:|---|---:|---|---|
| TRI-001 | Triage begins with scope and test-session verification | 5 | Reviewed design | 2 | Triage tests | Documentation only |
| TRI-002 | Controlled and organic or unclassified activity are separated | 5 | Reviewed design | 2 | `TST-017` | Documentation only |
| TRI-003 | Event, detection, alert, and incident-object stages remain distinct | 5 | Reviewed design | 2 | `TST-015`, `TST-016` | Documentation only |
| TRI-004 | Event ID 4625 interpretation is bounded | 5 | Reviewed design | 2 | `TST-010`, `TST-011` | Documentation only |
| TRI-005 | Event ID 4624 interpretation is bounded | 5 | Reviewed design | 2 | `TST-012` | Documentation only |
| TRI-006 | Failed-to-successful correlation is evidence-based | 5 | Blocked | 0 | Correlation test | None |
| TRI-007 | IP and GeoIP remain investigative context only | 5 | Reviewed design | 2 | Triage review | Documentation only |
| TRI-008 | Analyst disposition is recorded with limitations | 5 | Blocked | 0 | Triage record | None |
| TRI-009 | Response decision is traceable to evidence | 5 | Blocked | 0 | Incident workflow | None |
| TRI-010 | Closure rationale distinguishes case closure from compromise conclusion | 5 | Blocked | 0 | Incident workflow | None |

A Sentinel incident object cannot earn compromise-confirmation points.

## 17. Evidence-governance rubric

| ID | Criterion | Weight | Current outcome | Current score | Evidence IDs |
|---|---|---:|---|---:|---|
| EVD-001 | Public and private evidence boundaries are explicit | 5 | Validated documentation | 4 | Governance documents |
| EVD-002 | Stable identifiers are defined | 5 | Validated documentation | 4 | Governance documents |
| EVD-003 | Evidence lifecycle and test outcome are separate | 5 | Validated documentation | 4 | Governance documents |
| EVD-004 | Failed, blocked, and inconclusive tests are preserved | 5 | Validated documentation | 4 | Governance documents |
| EVD-005 | Minimum metadata is defined | 5 | Validated documentation | 4 | Evidence template |
| EVD-006 | SHA-256 integrity tracking is supported | 5 | Validated documentation | 4 | Governance documents |
| EVD-007 | Redaction and public-disclosure controls are defined | 5 | Validated documentation | 4 | Redaction notes |
| EVD-008 | Public evidence requires review and approval | 5 | Validated documentation | 4 | Governance documents |
| EVD-009 | Claim-evidence matrix controls public wording | 5 | Validated documentation | 4 | Claim matrix |
| EVD-010 | Execution evidence package is complete | 5 | Blocked | 0 | No execution evidence |

Documentation-validation points do not prove Azure execution.

## 18. Containment and teardown rubric

| ID | Criterion | Weight | Current outcome | Current score | Test | Evidence IDs |
|---|---|---:|---|---:|---|---|
| CLN-001 | Public exposure can be removed promptly | 5 | Blocked | 0 | `TST-018` | None |
| CLN-002 | Resulting NSG state is verified | 5 | Blocked | 0 | `TST-018` | None |
| CLN-003 | VM can reach a verified safe state | 5 | Blocked | 0 | `TST-019` | None |
| CLN-004 | Emergency containment decision is documented | 5 | Reviewed design | 2 | Emergency runbook | Documentation only |
| CLN-005 | Resource-group deletion reaches terminal state | 5 | Blocked | 0 | `TST-021` | None |
| CLN-006 | Subscription-wide orphan review passes | 5 | Blocked | 0 | `TST-022` | None |
| CLN-007 | Temporary identities and assignments are removed | 5 | Blocked | 0 | Teardown test | None |
| CLN-008 | Public IP and NSG disposition are recorded | 5 | Blocked | 0 | Teardown test | None |
| CLN-009 | DCR and analytics-object disposition are recorded | 5 | Blocked | 0 | Teardown test | None |
| CLN-010 | Workspace, telemetry, and evidence disposition are recorded | 5 | Blocked | 0 | Teardown test | None |

A deletion command or portal request earns zero cleanup-completion points without terminal-state verification.

## 19. Cost-closure rubric

| ID | Criterion | Weight | Current outcome | Current score | Test | Evidence IDs |
|---|---|---:|---|---:|---|---|
| CST-001 | Budget and alert controls are confirmed | 5 | Not started | 0 | Cost preflight | None |
| CST-002 | Runtime and exposure-duration limits are recorded | 5 | Planned | 1 | Cost preflight | None |
| CST-003 | Log Analytics cap and retention are reviewed | 5 | Planned | 1 | Cost preflight | None |
| CST-004 | Immediate cost review is recorded | 5 | Blocked | 0 | `TST-023` | None |
| CST-005 | 24-hour cost review is recorded | 5 | Blocked | 0 | `TST-024` | None |
| CST-006 | 72-hour cost review is recorded | 5 | Blocked | 0 | `TST-025` | None |

No zero-cost or final-cost claim is authorized.

## 20. Reporting and remediation rubric

| ID | Criterion | Weight | Current outcome | Current score | Evidence IDs |
|---|---|---:|---|---:|---|
| RPT-001 | Incident timeline separates facts, interpretation, decisions, and limitations | 5 | Reviewed template | 2 | Documentation only |
| RPT-002 | Findings report separates facts, evidence, interpretation, risk, and limitations | 5 | Reviewed template | 2 | Documentation only |
| RPT-003 | Remediation template separates proposal, implementation, validation, and residual risk | 5 | Reviewed template | 2 | Documentation only |
| RPT-004 | Timeline is populated with authoritative timestamps | 5 | Blocked | 0 | None |
| RPT-005 | Findings are evidence-supported | 5 | Blocked | 0 | None |
| RPT-006 | Recommendations link to findings or design gaps | 5 | Blocked | 0 | None |
| RPT-007 | Failed remediation attempts remain traceable | 5 | Planned | 1 | Documentation only |
| RPT-008 | Reports reflect current lifecycle state | 5 | Partially reviewed | 2 | Commit 5 hashes |

Templates do not earn completed-report points.

## 21. Portfolio and interview credibility rubric

| ID | Criterion | Weight | Current outcome | Current score | Evidence IDs |
|---|---|---:|---|---:|---|
| PRT-001 | README states the current lifecycle accurately | 5 | Reviewed | 2 | Documentation only |
| PRT-002 | Interview summary is bounded to design-stage work | 5 | Reviewed | 2 | Documentation only |
| PRT-003 | Interview material discloses limitations | 5 | Reviewed | 2 | Documentation only |
| PRT-004 | Resume publication remains blocked | 5 | Reviewed | 2 | Governance document |
| PRT-005 | Exact resume claims require claim IDs and evidence | 5 | Reviewed | 2 | Governance document |
| PRT-006 | Quantitative claims require reviewed measurement evidence | 5 | Reviewed | 2 | Governance document |
| PRT-007 | Stronger interview STAR results are unpopulated | 5 | Reviewed | 2 | Governance document |
| PRT-008 | Public execution evidence is approved and linked | 5 | Blocked | 0 | None |

Design-stage credibility points do not authorize implementation claims.

## 22. Repository quality and security rubric

| ID | Criterion | Weight | Current outcome | Current score | Evidence |
|---|---|---:|---|---:|---|
| REP-001 | Exact branch and commit checkpoints are verified | 4 | Passed locally | 4 | Local gate output |
| REP-002 | Worktree and index scope are validated before changes | 4 | Passed locally | 4 | Local gate output |
| REP-003 | Reviewed artifact hashes are frozen and rechecked | 4 | Passed locally | 4 | Local gate output |
| REP-004 | Markdown whitespace and code-fence checks pass | 4 | Partially passed | 3 | Per-artifact gates |
| REP-005 | Relative links resolve | 4 | Partially passed | 3 | Per-artifact gates |
| REP-006 | Disclosure scans pass | 4 | Partially passed | 3 | Per-artifact gates |
| REP-007 | Gitleaks passes | 3 | Last passed at Commit 4 | 2 | Prior commit gate |
| REP-008 | TruffleHog passes | 3 | Last passed at Commit 4 | 2 | Prior commit gate |

Repository-wide static and secret scans remain required before Commit 5 is staged or committed.

## 23. Test-coverage matrix

| Test range | Purpose | Current outcome | Score impact |
|---|---|---|---|
| `TST-001`–`TST-005` | Scope, isolation, NSG, and firewall | Not started / blocked | Safety and implementation remain unscored |
| `TST-006`–`TST-009` | AMA, DCR, telemetry health, and destination | Blocked | Telemetry remains unvalidated |
| `TST-010`–`TST-012` | Controlled 4625, logon context, and 4624 | Blocked | Authentication claims remain blocked |
| `TST-013`–`TST-014` | Below-threshold and threshold matching | Blocked | Detection validation remains blocked |
| `TST-015`–`TST-016` | Alert and incident-object behavior | Blocked | Alert and incident claims remain blocked |
| `TST-017` | Controlled-versus-organic separation | Blocked | Activity classification remains unvalidated |
| `TST-018`–`TST-019` | Exposure removal and VM containment | Blocked | Containment remains unvalidated |
| `TST-020` | Evidence sanitization | Blocked | Public execution evidence remains unavailable |
| `TST-021`–`TST-022` | Cleanup and orphan review | Blocked | Cleanup remains incomplete |
| `TST-023`–`TST-025` | Immediate, 24-hour, and 72-hour cost review | Blocked | Cost closure remains incomplete |

## 24. Claim-maturity decision matrix

| Claim level | Minimum rubric conditions | Current decision |
|---|---|---|
| Designed | Reviewed architecture, safety, query, runbook, and governance documentation | Supported for bounded design statements |
| Implemented | Applicable Azure implementation criteria score at least 3 with evidence | Blocked |
| Validated | Applicable tests pass; validation criteria score at least 4 with evidence | Blocked |
| Demonstrated | Validated behavior has approved public-safe evidence | Blocked |
| Completed | All mandatory gates and project-completion criteria pass | Not permitted |

A domain score cannot independently promote a claim.

## 25. Current provisional assessment

The current project state supports the following assessment:

| Area | Current assessment |
|---|---|
| Architecture and planning | Reviewed design baseline |
| Evidence governance | Validated documentation baseline |
| Operational runbooks | Reviewed design-stage artifacts |
| Detection engineering | Reviewed query and rule design |
| Reporting templates | In progress |
| Azure implementation | Not started |
| Telemetry and schema | Not validated |
| Detection behavior | Not validated |
| Alert and incident workflow | Not validated |
| Containment | Not validated |
| Teardown and cost closure | Not validated |
| Public execution evidence | None |
| Resume publication | Blocked |
| Overall lifecycle | Designed / Planned |

No total percentage or completion rating is assigned at this stage.

## 26. Remediation trigger rules

Create or update a remediation recommendation when:

- a criterion scores `0` or `1` after an expected execution milestone;
- a mandatory gate fails;
- a test fails or is inconclusive;
- a claimed configuration does not match evidence;
- a query or rule version changes;
- telemetry routing or schema differs from design;
- an entity mapping is unreliable;
- alert or incident behavior differs from expectation;
- containment or cleanup does not reach a verified state;
- billing data exceeds the expected scope;
- evidence is incomplete or unsafe;
- a public claim exceeds the strongest supported level; or
- a repository integration check fails.

A low score alone does not authorize a risky corrective action.

## 27. Score-calculation procedure

After evidence review:

1. Confirm all applicable criteria.
2. Record outcome and evidence IDs.
3. Assign a score using the defined scale.
4. Record limitations.
5. Sum awarded points by domain.
6. Divide by applicable maximum points.
7. Evaluate mandatory gates.
8. Apply the provisional rating band.
9. Determine the strongest supported claim level separately.
10. Record remediation triggers.
11. Obtain reviewer approval.
12. Update the claim-evidence matrix and public artifacts where authorized.

Do not automate score promotion without human review.

## 28. Score-change log

| Change UTC | Criterion | Prior score | New score | Reason | Evidence ID | Reviewer |
|---|---|---:|---:|---|---|---|
| No entries | None | Not scored | Not scored | No scoring event has occurred | None | None |

Score history must remain traceable.

## 29. Rubric quality checklist

Before using the worksheet, verify:

- [ ] Current project status is accurate.
- [ ] Mandatory gates are evaluated separately from numeric scores.
- [ ] Design, implementation, validation, demonstration, and completion remain distinct.
- [ ] No design-only criterion exceeds the allowed design-stage score.
- [ ] Every score above `2` has reviewed evidence.
- [ ] Outcome and numeric score are logically consistent.
- [ ] `Not Applicable` has a documented rationale.
- [ ] Failed, blocked, and inconclusive outcomes remain visible.
- [ ] Event, detection, alert, incident, and compromise remain distinct.
- [ ] Event ID 4625 is not treated as automatic proof of RDP.
- [ ] Event ID 4624 is not treated as automatic proof of authorized or unauthorized access.
- [ ] Controlled and organic or unclassified activity remain separate.
- [ ] IP or GeoIP is not presented as identity or nationality.
- [ ] Exposure removal and VM containment remain separate.
- [ ] Deletion request and verified cleanup remain separate.
- [ ] Immediate, 24-hour, and 72-hour cost reviews remain separate.
- [ ] Evidence lifecycle and test outcome remain separate.
- [ ] Resume and interview claims match the claim-evidence matrix.
- [ ] Repository-wide static and secret scans pass.
- [ ] Reviewer approval is recorded.

## 30. Project-completion gate

The project may be rated `Completed` only when:

- all five milestone commits are complete;
- repository-wide integration checks pass;
- mandatory documentation is accurate;
- applicable Azure resources were implemented;
- telemetry and schema were validated;
- controlled authentication tests passed or were transparently dispositioned;
- detection behavior was validated;
- alert and incident-object behavior was validated where applicable;
- containment was validated;
- teardown and orphan review passed;
- identity and telemetry objects were dispositioned;
- immediate, 24-hour, and 72-hour cost reviews completed;
- evidence was reviewed and sanitized;
- findings and recommendations were completed;
- limitations and failed tests were disclosed;
- README, interview, and resume claims were evidence-approved; and
- no unapproved active resource or public exposure remains.

A high numeric score cannot substitute for these requirements.

## 31. Current scoring declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Telemetry, schema, detection, alert, and incident validation have not started.
- No v2 execution evidence exists.
- No final score has been calculated.
- No overall rating has been assigned.
- Project completion is not permitted.
- Resume publication remains blocked.
- Public claims remain limited to Designed or Planned.
- This file is an unpopulated scoring template and does not fabricate validation results.
