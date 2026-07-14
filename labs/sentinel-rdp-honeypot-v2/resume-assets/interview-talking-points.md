# Sentinel RDP Honeypot v2 Interview Talking Points

## 1. Purpose

This document defines evidence-gated interview material for Sentinel RDP Honeypot v2.

It is designed to help explain:

- the engineering problem;
- architecture and trust boundaries;
- safety and authorization controls;
- telemetry and schema-validation strategy;
- KQL and analytics-rule design;
- triage and incident reasoning;
- evidence governance;
- teardown and cost controls;
- current limitations; and
- the distinction between designed, implemented, validated, demonstrated, and completed work.

This is a **design-stage interview template**. It does not establish that Azure resources were deployed, tests were executed, detections fired, alerts were triaged, containment was validated, cleanup completed, or cost was reconciled.

## 2. Current interview state

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
| Resume bullets | Blocked |
| Interview claim level | Designed / Planned only |
| Template status | Reviewed structure; design-stage content |
| Last updated | 2026-07-14 |

Interview answers must remain at the strongest level supported by reviewed evidence.

## 3. Governing documents

Use this file with:

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
17. [Evidence governance](../evidence/README.md);
18. [Evidence manifest](../evidence/manifest.md); and
19. [Claim-evidence matrix](../evidence/claim-evidence-matrix.md).

When an interview answer conflicts with project status or reviewed evidence, use the weaker supported wording.

## 4. Interview-answer rules

1. State the current lifecycle stage early.
2. Distinguish design decisions from executed work.
3. Use “designed,” “specified,” “documented,” or “planned” for unexecuted capabilities.
4. Use “implemented” only after the component exists with implementation evidence.
5. Use “validated” only after a defined test passed with reviewed evidence.
6. Use “demonstrated” only after an approved public-safe artifact communicates validated behavior.
7. Use “completed” only after all applicable closure criteria pass.
8. State material limitations without being prompted.
9. Separate Event ID 4625 from RDP-specific interpretation.
10. Separate Event ID 4624 from authorization or compromise conclusions.
11. Separate query results, alerts, incident objects, and compromise.
12. Separate controlled activity from organic or unclassified activity.
13. Treat IP and GeoIP as investigative context, not human identity.
14. Separate exposure removal from VM containment.
15. Separate deletion requests from verified cleanup.
16. Separate immediate cost observations from final reconciliation.
17. Mention failed or inconclusive tests when they materially affect the conclusion.
18. Keep raw evidence and private identifiers outside the interview narrative.
19. Do not imply enterprise SOC ownership, production authority, or 24×7 operations.
20. Do not present framework mapping as compliance.
21. Do not describe a personal lab decision as enterprise risk acceptance.
22. Do not imply that a platform incident object is a confirmed security incident.
23. Do not omit authorization, safety, evidence, or cleanup controls to make the story sound more impressive.
24. Do not use future work as a completed achievement.
25. The claim-evidence matrix controls stronger wording.

## 5. Thirty-second project summary

### Current approved version

> Sentinel RDP Honeypot v2 is a self-built Azure cloud-security and Microsoft Sentinel detection-engineering lab that I designed to validate a bounded Windows remote-interactive authentication workflow. The current repository contains the architecture, threat model, evidence-governance model, deployment and teardown runbooks, twenty versioned KQL queries, and a disabled scheduled-rule specification. Azure execution is still paused, so I describe the project as designed rather than implemented or validated.

### Do not say yet

Do not state that you:

- deployed the v2 environment;
- implemented AMA or the DCR;
- validated `SecurityEvent`;
- deployed or validated `AR-001`;
- generated or triaged a v2 alert;
- demonstrated compromise;
- validated containment;
- completed cleanup;
- reconciled final cost; or
- completed the project.

## 6. Two-minute project overview

Use this sequence:

1. **Problem:** authentication telemetry is easy to overinterpret without schema, test, and evidence discipline.
2. **Scope:** a bounded personal Azure and Microsoft Sentinel lab, not an enterprise SOC.
3. **Architecture:** disposable Windows VM, NSG, Windows Firewall, AMA, DCR, Log Analytics, Sentinel, KQL, and one disabled scheduled-rule design.
4. **Safety:** no production data, no reused credentials, no trusted connectivity, explicit exposure authorization, runtime limits, and emergency containment.
5. **Detection:** health and schema queries precede behavioral detection logic.
6. **Validation:** controlled 4625 and 4624 tests, below-threshold and threshold-matching tests, alert and incident-object checks, and cleanup/cost closure.
7. **Evidence:** raw evidence stays private; only sanitized and reviewed derivatives may be public.
8. **Current state:** documentation through detection engineering is validated; execution remains blocked.
9. **Next step:** finish reporting templates, complete static integration, and then perform a separate predeployment go/no-go review.

## 7. What problem were you solving?

### Approved talking point

The project addresses a common detection-engineering problem: a security team can collect authentication events and still make weak conclusions if it has not validated the destination table, field population, logon type, source context, threshold behavior, and platform-object lifecycle.

The design therefore makes telemetry health and schema validation prerequisites for behavioral detection and claim promotion.

### Evidence boundary

Current support comes from reviewed design artifacts. No live telemetry result exists yet.

## 8. Why did you redesign the original lab?

### Approved talking point

The earlier learning experience focused on producing visible authentication telemetry. The v2 redesign converts that into an original engineering system with:

- an authoritative threat model;
- authorization and stop conditions;
- evidence and claim governance;
- cost and teardown controls;
- versioned KQL;
- detection lifecycle management;
- explicit test identifiers;
- failed-test preservation;
- public/private evidence separation; and
- repository-wide quality gates.

### Stronger post-validation version

Only after execution evidence exists, add the specific implementation and validation decisions that changed because of real platform behavior.

## 9. Architecture explanation

### Approved design-stage answer

The design uses a dedicated resource group containing an isolated disposable Windows VM, network controls, Azure Monitor Agent, a Data Collection Rule, a Log Analytics workspace, and Microsoft Sentinel.

Windows authentication telemetry is intended to flow through AMA and the DCR into the validated destination table. KQL queries first check heartbeat, routing, freshness, latency, schema, field population, and source-host coverage. Controlled tests then evaluate failed and successful authentication, threshold behavior, alert creation, and incident-object behavior.

### Trust boundaries to mention

- operator to Azure control plane;
- public internet to VM;
- VM to Azure Monitor Agent;
- DCR to Log Analytics;
- Log Analytics to Sentinel;
- private raw evidence to sanitized public evidence; and
- design-stage documentation to evidence-supported public claims.

### Current limitation

The actual Azure control-plane and telemetry path has not been implemented or validated for v2.

## 10. Why public RDP is treated as a separate gate

### Approved talking point

Public TCP/3389 exposure is not implied by the architecture. It requires a separate explicit authorization decision after deployment, telemetry health, schema validation, controlled testing, containment readiness, runtime limits, and cost controls pass.

### Safety points

- no production data;
- no trusted-network connectivity;
- no VNet peering;
- Windows Firewall stays enabled;
- no allow-all inbound rule;
- operator remains present;
- exposure is time-boxed;
- stop conditions are predefined;
- exposure removal and VM containment are independently testable; and
- safety overrides evidence preservation.

### Current state

Public exposure remains unauthorized.

## 11. Telemetry-health strategy

### Approved talking point

Before looking for suspicious behavior, I designed queries to answer whether the data can be trusted:

- `Q-001` checks heartbeat recency;
- `Q-002` compares potential authentication destination tables;
- `Q-003` evaluates freshness and ingestion latency;
- `Q-004` probes the authentication schema;
- `Q-005` profiles field population; and
- `Q-006` verifies source-host coverage.

This prevents a detection result from being treated as reliable when the underlying collection route or schema is unknown.

### Current state

The queries are reviewed but unexecuted.

## 12. Event ID 4625 reasoning

### Approved talking point

Event ID 4625 means a failed logon occurred. It is not automatically an RDP event, attack, brute-force attempt, or compromise.

Remote-interactive wording requires validated logon-type or equivalent schema context. A pattern such as repeated failures against one account may be consistent with password guessing, but the conclusion remains qualified by source, account, host, time, test context, and field quality.

### Prohibited shortcut

Do not say that every 4625 event is an RDP attack.

## 13. Event ID 4624 reasoning

### Approved talking point

Event ID 4624 records creation of a successful logon session. It does not establish whether access was authorized, unauthorized, benign, or malicious.

A suspicious-success hypothesis requires correlation with source, account, host, timing, controlled-test metadata, and relevant follow-on activity.

### Prohibited shortcut

Do not describe a 4624 event as compromise without additional evidence.

## 14. KQL design approach

### Approved talking point

The catalog contains `Q-001` through `Q-020` and is ordered from data-quality validation to behavior analysis.

The progression is:

1. collection health;
2. destination and schema;
3. failed and successful authentication inspection;
4. controlled-test queries;
5. threshold validation;
6. behavioral summaries;
7. controlled-versus-organic classification;
8. failed-to-successful correlation; and
9. the canonical scheduled-rule query.

All KQL is read-only.

## 15. `AR-001` design

### Approved talking point

`AR-001` is a scheduled-rule design based on `Q-020` version `1.0.0`.

Its draft settings are:

- 5-minute frequency;
- 15-minute lookback;
- 5-minute aggregation window;
- threshold of 5 failed remote-interactive logons;
- Medium severity;
- conservative `T1110.001` Password Guessing mapping;
- conditional IP and host mappings;
- account mapping deferred;
- suppression disabled for initial validation;
- no destructive or automatic response; and
- disabled, blocked, undeployed, and unverified state.

### Why account mapping is deferred

Account mapping depends on actual validated field population and format. Designing a mapping before validating the schema could produce misleading entities.

## 16. How would you validate the detection?

### Approved design-stage answer

The validation plan separates:

- `TST-013`: below-threshold activity should not create the expected detection outcome;
- `TST-014`: threshold-matching activity should satisfy the query conditions;
- `TST-015`: alert creation must be verified separately; and
- `TST-016`: incident-object behavior must be verified separately.

This sequence prevents a query result from being presented as proof of an alert or incident.

### Evidence expected later

- query version;
- rule version;
- test-session identifier;
- expected and actual result;
- alert linkage;
- incident linkage;
- entity-mapping result;
- failed-test history;
- reviewed evidence; and
- limitations.

## 17. Alert, incident, and compromise distinction

### Approved talking point

I use a staged analytical model:

1. Windows event;
2. validated observable;
3. authentication pattern;
4. detection result;
5. alert;
6. incident object;
7. compromise hypothesis; and
8. confirmed unauthorized activity.

Each stage requires additional evidence. A Sentinel incident object is an investigation case, not proof of a confirmed compromise.

## 18. Controlled versus organic activity

### Approved talking point

Controlled activity must have an approved source, account, time window, and test-session record.

Organic means the activity was not classified as controlled. It does not automatically mean malicious. When evidence is incomplete, the disposition remains undetermined.

## 19. Triage approach

### Approved design-stage answer

The triage runbook starts with scope, authorization, test-session, and telemetry checks before interpreting the pattern.

It then reviews:

- event type;
- logon context;
- source;
- account;
- host;
- time window;
- controlled-test metadata;
- query and rule version;
- alert and incident linkage;
- successful-logon correlation;
- stop conditions;
- evidence status; and
- disposition.

Possible outcomes include controlled, benign, expected administrative, suspicious, unauthorized, undetermined, and not applicable.

## 20. Successful-logon correlation

### Approved talking point

A successful authentication near failed attempts may justify investigation, but temporal proximity alone does not prove that the same person generated the events or that access was unauthorized.

The analysis would require account, source, host, session, timing, controlled-test context, and follow-on activity.

## 21. Evidence-governance approach

### Approved talking point

Raw evidence remains private and outside Git. Public artifacts must be sanitized, reviewed, and approved.

The evidence model separates:

- evidence lifecycle status; and
- test outcome.

For example, an approved public artifact can document a failed test. The failed result remains part of the engineering record and cannot be silently replaced by a successful retest.

### Stable identifiers

- `EV-###` for evidence;
- `TST-###` for tests;
- `CLM-###` for claims;
- `Q-###` for queries; and
- `AR-###` for analytics rules.

## 22. Why hashes are used

### Approved talking point

SHA-256 is used where artifact integrity, provenance, or private-to-public derivation needs to be tracked.

A hash demonstrates file consistency. It does not prove that the artifact is accurate, authentic, complete, or sufficient for a claim.

## 23. Teardown strategy

### Approved talking point

Teardown is treated as an engineering phase rather than an afterthought.

The design distinguishes:

- exposure removal;
- VM containment;
- resource deletion;
- resource-group verification;
- subscription-wide orphan review;
- identity and role-assignment cleanup;
- DCR and analytics-object disposition;
- workspace and telemetry disposition;
- evidence disposition;
- immediate verification;
- 24-hour verification; and
- 72-hour verification.

A deletion request alone is not proof that cleanup completed.

## 24. Cost-control strategy

### Approved talking point

The project defines:

- budget and alert prerequisites;
- VM-size and runtime limits;
- exposure-duration limits;
- Log Analytics daily-cap review;
- retention review;
- tagging;
- immediate cost observation;
- 24-hour review; and
- 72-hour reconciliation.

An immediate billing view is preliminary and cannot support a zero-cost or final-cost claim.

## 25. Production-safe contrast

### Approved talking point

The intentionally observable personal lab is not a production-safe public RDP architecture.

A production design would normally favor:

- private access;
- just-in-time access;
- hardened administrative paths;
- stronger segmentation;
- enterprise identity controls;
- formal monitoring and response ownership;
- change control;
- recovery planning;
- compliance and authorization processes; and
- managed operational accountability.

The lab exists to validate a bounded telemetry and detection workflow, not to recommend public RDP for production administration.

## 26. Framework-mapping answer

### Approved talking point

Selected project controls are mapped to relevant security concepts and frameworks to show design reasoning.

The mapping does not establish compliance, certification, authorization, or enterprise control effectiveness.

## 27. Strongest current accomplishments

The following design-stage statements are supportable:

- designed a bounded Azure and Microsoft Sentinel detection-engineering lab;
- defined an authoritative threat model and trust boundaries;
- designed conditional exposure gates and mandatory stop conditions;
- created evidence and claim-governance rules;
- created deployment, triage, teardown, and cost-control runbooks;
- created twenty versioned KQL queries;
- specified `AR-001` with conservative lifecycle controls;
- separated events, detections, alerts, incident objects, and compromise;
- defined twenty-five validation tests;
- defined reporting, remediation, and portfolio-gating structures; and
- implemented repository hygiene, link, disclosure, hash, and secret-scanning gates for the documentation workflow.

The final statement refers to the documentation-repository workflow, not Azure implementation.

## 28. Current limitations to disclose

State these when relevant:

- no live Azure v2 environment;
- no implemented AMA or DCR;
- no validated destination table;
- no validated authentication schema;
- no controlled 4625 result;
- no controlled 4624 result;
- no validated remote-interactive context;
- no deployed `AR-001`;
- no v2 alert;
- no v2 incident object;
- no organic observation period;
- no containment validation;
- no cleanup evidence;
- no cost data;
- no approved public execution evidence; and
- no authorized implementation, validation, demonstration, or completion claim.

## 29. Design-stage STAR response template

### Situation

Explain the detection-engineering or governance problem that motivated the design.

### Task

Define the bounded objective and safety constraints.

### Action

Describe the artifacts and engineering decisions you completed:

- threat model;
- architecture;
- evidence governance;
- runbooks;
- KQL;
- analytics-rule specification;
- reporting controls; and
- static validation gates.

### Result

State only the reviewed design-stage result:

> I produced a versioned, internally consistent, evidence-gated design baseline that is ready for a separate predeployment review. Azure execution remains paused, so I do not claim implementation or validation.

## 30. Post-implementation STAR placeholder

Do not use this section until implementation evidence exists.

### Situation

Not populated — execution has not started.

### Task

Not populated — implementation scope not executed.

### Action

Not populated — no Azure action recorded.

### Result

Not populated — no implementation evidence exists.

## 31. Post-validation STAR placeholder

Do not use this section until defined tests pass with reviewed evidence.

### Situation

Not populated — no validation window exists.

### Task

Not populated — no executed test set exists.

### Action

Not populated — no validation action recorded.

### Result

Not populated — no validated result exists.

## 32. Likely interview questions

### Why not deploy immediately?

Because documentation quality does not authorize cloud execution. Identity, subscription, cost, network, evidence, and containment prerequisites must pass a separate go/no-go review.

### Why validate schema before detection logic?

Because table routing and field population can differ from assumptions. Detection and entity mapping must use observed data.

### Why keep `AR-001` disabled?

Because its query, fields, threshold behavior, mappings, grouping, alert creation, and incident settings are unvalidated.

### Why preserve failed tests?

Because failure history explains engineering decisions, prevents cherry-picking, and supports honest retesting.

### Why separate evidence lifecycle from test outcome?

Because a public-safe artifact can accurately document a failed or blocked test.

### Why avoid destructive automation?

Because the lab does not have production incident authority, and automated destructive action could create unnecessary safety, evidence, and scope risks.

### Why is cleanup not a single delete command?

Because resource state, orphan resources, identities, telemetry objects, and delayed billing must be verified separately.

### How do you prevent overclaiming?

By controlling wording through lifecycle states, evidence IDs, test outcomes, the claim-evidence matrix, and publication review.

## 33. Questions to ask an interviewer

Potential questions:

- How does your team validate telemetry schema before productionizing detections?
- How are analytics-rule versions, tests, and evidence tracked?
- How does the team distinguish a platform incident object from a confirmed security incident?
- What controls exist for entity-mapping quality?
- How are failed detection tests preserved?
- How are response automations authorized and bounded?
- How are cleanup, retention, and cost ownership handled in temporary environments?
- How does the team govern public or executive claims from detection results?
- How are cloud-security labs or proof-of-concepts transitioned into production-safe designs?

## 34. Claim-promotion checklist

Before using stronger interview wording, verify:

- [ ] The exact proposed statement is recorded as a governed claim.
- [ ] Required implementation or validation activity occurred.
- [ ] The relevant configuration or query version is known.
- [ ] Expected and actual results are recorded.
- [ ] The test outcome passed.
- [ ] Supporting evidence is reviewed.
- [ ] Public evidence is sanitized and approved where needed.
- [ ] Material limitations are stated.
- [ ] Failed or inconclusive history remains visible.
- [ ] Teardown and cost state do not contradict the claim.
- [ ] The claim-evidence matrix authorizes the wording.
- [ ] The README and reports reflect the same lifecycle state.

## 35. Interview quality checklist

Before an interview, verify:

- [ ] The opening summary states the current lifecycle stage.
- [ ] Designed and implemented work are not conflated.
- [ ] Implemented and validated work are not conflated.
- [ ] Event, detection, alert, incident, and compromise terminology is precise.
- [ ] 4625 and 4624 limitations are understood.
- [ ] Controlled and organic activity are separated.
- [ ] Safety and authorization controls are explained.
- [ ] Evidence governance is explained.
- [ ] Failed-test preservation is explained.
- [ ] Teardown and cost closure are explained.
- [ ] Production-safe contrast is clear.
- [ ] Framework mapping is not presented as compliance.
- [ ] Current limitations can be stated directly.
- [ ] Resume wording matches the claim-evidence matrix.
- [ ] No private identifier or raw evidence is disclosed.
- [ ] No future action is presented as completed work.

## 36. Current interview declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Telemetry, schema, detection, alert, and incident validation have not started.
- No v2 execution evidence exists.
- No implementation or validation STAR result is populated.
- Resume bullets remain blocked.
- Interview claims remain limited to Designed or Planned.
- This file supports honest design-stage discussion and does not fabricate execution results.
