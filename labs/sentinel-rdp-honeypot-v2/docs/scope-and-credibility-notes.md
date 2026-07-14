# Scope and Credibility Notes

| Field | Value |
|---|---|
| Project | Sentinel RDP Honeypot v2 |
| Document role | Public status and claim-boundary standard |
| Design status | Approved baseline |
| Execution status | Not started |
| Public claim state | Design-stage claims only |
| Azure deployment status | Paused |
| Validation evidence | Not captured |
| Resume and interview claims | Restricted until evidence exists |
| Last updated | 2026-07-13 |

> This document defines how the project may be described publicly at each lifecycle stage. It does not independently prove implementation, validation, control effectiveness, compromise, compliance, or enterprise operational maturity.

---

## 1. Purpose and authority

This document establishes credible wording for:

- the project README;
- GitHub descriptions;
- architecture and report summaries;
- portfolio pages;
- resume bullets;
- interview talking points;
- professional profiles;
- demonstrations and walkthroughs.

The authoritative system boundary, risks, exposure conditions, and stop conditions are maintained in [`threat-model.md`](threat-model.md).

The authoritative evidence and claim relationships will be maintained in:

- `../evidence/README.md`;
- `../evidence/manifest.md`;
- `../evidence/claim-evidence-matrix.md`.

When a public claim conflicts with the threat model, project status, or available evidence, the weaker supported claim must be used.

---

## 2. Current project state

| Capability | Current state |
|---|---|
| System boundary and threat model | Approved design |
| Architecture | Approved design |
| Deployment runbook | Pre-revision content; modernization pending |
| Cost controls | Pre-revision content; modernization pending |
| Triage runbook | Empty pre-revision skeleton |
| Teardown runbook | Pre-revision content; modernization pending |
| KQL catalog | Empty pre-revision skeleton |
| Analytics-rule catalog | Empty pre-revision skeleton |
| Evidence governance | Empty pre-revision skeleton |
| Azure v2 resources | Not deployed |
| AMA/DCR collection | Not implemented for v2 |
| `SecurityEvent` schema | Not validated |
| Scheduled rule `AR-001` | Draft design only |
| Controlled tests | Not executed |
| Alert and incident behavior | Not validated |
| Teardown and cost closure | Not validated |
| Public portfolio evidence | Not captured |

Current public descriptions must remain at the **Designed** or **Planned** level.

---

## 3. Approved project descriptions

### 3.1 Design-stage description

Use while Azure execution remains paused:

> Sentinel RDP Honeypot v2 is a self-built Azure cloud-security and Microsoft Sentinel detection-engineering lab designed to validate a bounded Windows remote-interactive authentication workflow.

### 3.2 Expanded design-stage description

> The project is designed around an isolated disposable Windows VM, Windows Security Events collected through Azure Monitor Agent and a Data Collection Rule, Microsoft Sentinel analytics, controlled authentication testing, alert triage, evidence governance, emergency containment, teardown, and cost closure.

### 3.3 Implemented but not fully validated

Use only after the relevant Azure resources and configurations exist:

> Sentinel RDP Honeypot v2 implements a bounded Azure and Microsoft Sentinel authentication-monitoring lab. Validation of collection, detection, triage, and cleanup remains in progress.

### 3.4 Post-validation description

Use only after the definition of done and claim-evidence gates pass:

> Sentinel RDP Honeypot v2 demonstrates a bounded Microsoft Sentinel collection, detection, triage, evidence, containment, cleanup, and cost-closure workflow for Windows remote-interactive authentication telemetry.

### 3.5 Required limitation

The post-validation description must be accompanied by:

> The project does not represent enterprise SOC ownership, production incident authority, continuous monitoring, formal authorization, regulatory compliance, or production-ready public RDP administration.

---

## 4. Claim hierarchy

Public claims must use the strongest level supported by evidence—never a stronger one.

| Claim level | Meaning | Minimum support |
|---|---|---|
| **Designed** | Architecture, control, query, or procedure has been specified | Approved documentation |
| **Implemented** | The configuration or procedure was executed | Configuration or execution evidence |
| **Validated** | A defined test produced the expected result | Test record, actual result, version, and evidence |
| **Demonstrated** | A bounded concept or workflow was shown | Scoped evidence and explicit limitations |
| **Mapped** | A project element was related to a framework or control | Source, rationale, and mapping boundary |
| **Informed by** | A principle influenced the design | Documented design decision |
| **Completed** | All applicable definition-of-done and closure criteria passed | Full evidence package, teardown, and cost closure |

The following transitions are prohibited without evidence:

- Designed → Implemented
- Implemented → Validated
- Alert generated → Compromise confirmed
- Framework mapping → Compliance
- Resource deletion requested → Cleanup completed
- Immediate cost view → Final cost
- Platform incident object → Confirmed security incident

---

## 5. Current approved claims

The following statements are currently supportable:

- Designed a bounded Azure and Microsoft Sentinel detection-engineering lab.
- Defined an authoritative system boundary and threat model.
- Defined trust boundaries for public exposure, telemetry, Azure administration, and evidence publication.
- Designed conditional public-exposure gates and mandatory stop conditions.
- Designed a workflow for AMA/DCR collection, KQL analysis, scheduled analytics, triage, evidence, teardown, and cost closure.
- Documented the difference between failed authentication, alerts, incident objects, compromise hypotheses, and confirmed unauthorized activity.
- Planned controlled validation for failed and successful remote-interactive authentication.
- Designed evidence and claim-governance requirements.

The following are not yet supportable:

- Deployed the v2 Azure environment.
- Configured or validated AMA and the DCR for v2.
- Confirmed `SecurityEvent` field population.
- Deployed or validated `AR-001`.
- Generated or triaged a v2 Sentinel alert.
- Validated successful-logon correlation.
- Validated emergency teardown.
- Verified complete cleanup.
- Reconciled final v2 project cost.

---

## 6. Authentication terminology

### Event ID 4625

Approved wording:

> A failed logon was recorded.

Stronger wording allowed after validating remote-interactive context:

> A failed remote-interactive logon was recorded.

Approved pattern wording where supported:

> The activity is consistent with repeated remote-interactive password guessing.

Not automatically permitted:

- RDP attack
- brute-force attack
- attacker compromised the VM
- malicious RDP session
- confirmed incident

### Event ID 4624

Approved wording:

> A successful logon session was recorded.

Where remote-interactive context is validated:

> A successful remote-interactive logon was recorded.

A successful logon is not automatically malicious. The source, account, controlled-test record, timing, and follow-on activity must be investigated.

### Brute force and password spraying

Use these terms only when the attempt pattern supports them.

- Repeated attempts against one account may be consistent with password guessing.
- Low-volume attempts distributed across multiple accounts may be consistent with password spraying.
- Mixed or incomplete evidence must remain qualified.

The destination port alone does not prove the authentication pattern.

---

## 7. Alert, incident, and compromise boundaries

The project uses this analytical progression:

| Stage | Meaning |
|---|---|
| Windows event | Recorded operating-system activity |
| Validated observable | A field or fact confirmed in the collected data |
| Authentication pattern | Related observables grouped by source, account, host, and time |
| Detection result | Query conditions were satisfied |
| Alert | The analytics rule generated a security alert |
| Incident object | The platform created or correlated an investigation case |
| Compromise hypothesis | Evidence suggests unauthorized access may have succeeded |
| Confirmed unauthorized activity | Defined evidence supports an unauthorized harmful event |

Approved wording:

> Microsoft Sentinel created an alert for the controlled threshold-matching test.

> The platform created or correlated an incident object for investigation.

Not automatically permitted:

> Sentinel confirmed an attack.

> The incident proves the VM was compromised.

> The alert identified the attacker.

A true-positive detection means the rule correctly identified the behavior it was designed to detect. It does not automatically mean successful compromise occurred.

---

## 8. Prohibited and restricted claims

### 8.1 Enterprise SOC claims

Do not use:

- built an enterprise SOC;
- operated a production SOC;
- owned 24×7 incident response;
- provided continuous enterprise monitoring;
- implemented a complete SOC platform.

Preferred wording:

> Built a bounded Microsoft Sentinel detection-engineering and SecOps workflow.

The phrase “self-built Azure SOC” should not be the primary project description.

### 8.2 Production claims

Do not use:

- production-ready public RDP architecture;
- hardened production honeypot;
- production-proven detection;
- enterprise-grade deployment;
- safe for production administration.

Preferred wording:

> Designed and tested within a disposable, time-boxed personal lab.

### 8.3 Compliance claims

Do not use:

- NIST compliant;
- CIS compliant;
- HIPAA compliant;
- HITRUST certified;
- PCI DSS compliant;
- GDPR compliant;
- formally authorized;
- received an ATO.

Permitted wording where documented:

- mapped selected project controls to NIST guidance;
- informed by CSF 2.0 and RMF concepts;
- compared project controls with selected SP 800-53 controls;
- used CIS Controls as prioritization context;
- documented privacy and evidence-handling considerations.

Framework mapping does not establish compliance or authorization.

### 8.4 Threat-actor attribution

Do not claim:

- a named threat actor performed the activity;
- an APT targeted the lab;
- the source country was the attacker’s nationality;
- the GeoIP location was the actor’s physical location;
- one IP address represents one person;
- the activity targeted Jason personally.

Permitted wording:

> The source IP was enriched with approximate network-location or provider information for investigative context.

### 8.5 Attack and compromise claims

Do not infer successful compromise from:

- Event ID 4625;
- repeated failed logons;
- a Sentinel alert;
- an incident object;
- an IP reputation match;
- GeoIP enrichment;
- an exposed TCP/3389 service;
- organic internet scanning.

Confirmed compromise requires evidence beyond a detection trigger.

### 8.6 Cost and cleanup claims

Do not use:

- zero-cost project;
- no charges occurred;
- all resources were deleted;
- cleanup was complete;
- billing was fully closed;

unless the teardown, orphan-resource search, telemetry disposition, and delayed cost reviews support the statement.

---

## 9. Honeypot terminology

The repository may retain the project name **Sentinel RDP Honeypot v2**.

The term “honeypot” must be understood as:

> A disposable, intentionally observable lab endpoint used to collect and analyze bounded authentication telemetry.

The project does not claim to implement:

- a mature deception platform;
- production deception engineering;
- high-interaction malware analysis;
- attacker engagement;
- credential collection;
- long-term unattended exposure;
- full adversary emulation.

The endpoint is not kept online to encourage successful unknown compromise.

---

## 10. Real-world and organic activity

Do not promise:

- real-world brute-force attacks;
- live attacker engagement;
- guaranteed malicious traffic;
- a defined number of hostile sources;
- successful attack capture.

Preferred wording before observation:

> The time-boxed endpoint may receive unsolicited internet authentication activity.

Preferred wording after observation:

> Organic internet-originated authentication activity was observed during the documented interval.

Any organic activity must remain separate from controlled-test activity through:

- test-session IDs;
- approved test sources;
- explicit time windows;
- synthetic accounts;
- triage records.

Organic activity must not be retroactively labeled as a controlled test.

---

## 11. Resume, interview, and portfolio controls

### 11.1 Resume bullets

Resume bullets remain blocked until the claim-evidence matrix supports the exact wording.

A resume bullet must identify:

- what was implemented;
- what was validated;
- the bounded project scope;
- the evidence supporting the result;
- any material limitation.

Do not use future-tense plans as completed resume achievements.

### 11.2 Interview talking points

Before Azure execution, interviews may discuss:

- architecture decisions;
- risk analysis;
- trust boundaries;
- public-exposure controls;
- monitoring prerequisites;
- detection hypotheses;
- validation design;
- evidence governance;
- planned teardown and cost controls.

After validation, interviews may discuss actual:

- configuration choices;
- schema findings;
- test outcomes;
- tuning decisions;
- alert and incident behavior;
- failed tests;
- containment decisions;
- cleanup and cost results.

### 11.3 Demonstrations

A demonstration must distinguish:

- live platform state;
- previously captured evidence;
- synthetic examples;
- unexecuted design material.

A screenshot must not be presented as live evidence after the underlying resource has been deleted unless its capture context is documented.

---

## 12. Evidence requirements for stronger claims

| Proposed claim | Minimum evidence |
|---|---|
| AMA implemented | Extension state and intended VM |
| DCR implemented | DCR definition and target association |
| Security events collected | Expected table, source computer, timestamps, and controlled event |
| Schema validated | Actual 4625 and 4624 field-population evidence |
| Detection implemented | Deployed rule configuration and query version |
| Detection validated | Below-threshold and threshold-matching tests |
| Alert triaged | Alert, triage record, and disposition |
| Incident workflow demonstrated | Incident-object evidence and closure decision |
| Successful-logon correlation validated | Approved 4624 test and correlation result |
| Emergency response validated | Exposure-removal or safe response test |
| Cleanup completed | Resource deletion and subscription-wide orphan search |
| Cost closure completed | Immediate, 24-hour, and 72-hour reconciliation |
| Portfolio claim approved | Claim-evidence matrix and publication review |

A configuration screenshot alone is not sufficient evidence of effective operation.

---

## 13. Public evidence boundaries

Public evidence must not expose:

- passwords, tokens, keys, or cookies;
- tenant or subscription IDs;
- full Azure resource IDs where unnecessary;
- active public endpoints;
- full source IP addresses;
- personal email addresses;
- real usernames;
- billing identifiers;
- private course source material;
- unreviewed raw event records;
- unsupported attribution.

Permitted public artifacts include:

- sanitized configuration summaries;
- aggregated KQL results;
- synthetic test records;
- redacted screenshots;
- evidence manifests;
- validation-result tables;
- limitations and failed-test summaries;
- cleanup and cost-closure summaries.

Evidence governance is authoritative in `../evidence/README.md`.

---

## 14. Claim promotion and regression

A claim may be promoted only when:

1. the relevant configuration version is known;
2. the required test is defined;
3. the actual result is recorded;
4. the result passed;
5. the supporting evidence is reviewed;
6. limitations are stated;
7. the public wording matches the evidence.

A previously approved claim must be reduced or retired when:

- later testing invalidates it;
- a configuration changes materially;
- a rule or query version is replaced;
- evidence is found to be incomplete or unsafe;
- Azure or Sentinel behavior changes;
- the claim exceeds the project’s actual scope.

Failed tests must remain traceable and must not be silently replaced by successful retests.

---

## 15. Review triggers

Review this document when:

- the project moves from design to implementation;
- the first Azure v2 resource is created;
- telemetry schema is validated;
- `AR-001` is created or revised;
- the first controlled alert is generated;
- incident-object behavior is observed;
- an unauthorized success or mandatory stop occurs;
- teardown completes;
- cost data settles;
- a README, report, resume bullet, or interview claim is drafted;
- evidence is prepared for publication;
- project scope changes.

---

## 16. Definition of done

This credibility standard is implemented when:

- [ ] The project lifecycle state is accurate.
- [ ] Design, implementation, validation, and completion are separated.
- [ ] Approved descriptions exist for each lifecycle stage.
- [ ] Event, alert, incident, and compromise terminology is bounded.
- [ ] Brute-force and spraying terminology require pattern evidence.
- [ ] Enterprise SOC and production claims are restricted.
- [ ] Framework mapping is separated from compliance.
- [ ] GeoIP and IP enrichment are separated from attribution.
- [ ] Organic activity is separated from controlled testing.
- [ ] Resume and interview claims require evidence.
- [ ] Public evidence restrictions are defined.
- [ ] Claim promotion and regression rules are defined.
- [ ] Current public claims remain design-stage only.
- [ ] No unexecuted v2 capability is described as validated or completed.

---

## 17. Related documents

| Document | Purpose |
|---|---|
| [`threat-model.md`](threat-model.md) | Authoritative system boundary, risks, controls, and stop conditions |
| [`architecture-overview.md`](architecture-overview.md) | Concise architecture summary |
| [`production-safe-design-contrast.md`](production-safe-design-contrast.md) | Lab-versus-production comparison |
| [`theory-to-lab-mapping.md`](theory-to-lab-mapping.md) | Framework and theory relationships |
| [`checklist-to-project-roadmap.md`](checklist-to-project-roadmap.md) | Scope and definition of done |
| `../evidence/README.md` | Evidence governance |
| `../evidence/claim-evidence-matrix.md` | Authoritative public claim approval |
| `../runbooks/deployment-runbook.md` | Deployment and controlled validation |
| `../runbooks/rdp-authentication-triage.md` | Authentication triage and disposition |
| `../runbooks/teardown-runbook.md` | Normal and emergency cleanup |
| `../sentinel/analytics-rules-catalog.md` | Analytics-rule lifecycle |
