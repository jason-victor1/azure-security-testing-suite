# Theory-to-Lab Mapping

| Field | Value |
|---|---|
| Project | Sentinel RDP Honeypot v2 |
| Document role | Cybersecurity-theory and framework mapping |
| Design status | Approved baseline |
| Execution status | Not started |
| Mapping status | Approved design; implementation evidence pending |
| Azure deployment status | Paused |
| Compliance or authorization claim | None |
| Authoritative risk source | [`threat-model.md`](threat-model.md) |
| Last updated | 2026-07-13 |

> This document maps the modernized theory curriculum to the project without forcing every course topic into the Azure lab. A mapping explains relevance; it does not establish implementation, control effectiveness, compliance, certification, authorization, or enterprise operational maturity.

---

## 1. Purpose

This document connects the reconstructed cybersecurity-theory curriculum to the design and planned validation of Sentinel RDP Honeypot v2.

It is intended to:

- preserve useful theory from the original internship-style course;
- correct outdated, ambiguous, or overstated concepts;
- distinguish directly implemented concepts from design influences and contextual knowledge;
- identify the project artifacts that embody each relevant concept;
- prevent framework names from being used as unsupported compliance claims;
- define where theory ends and implementation evidence must begin;
- make the project defensible in portfolio reviews and technical interviews.

This document is subordinate to:

1. [`threat-model.md`](threat-model.md) for risk, boundaries, controls, and stop conditions;
2. [`scope-and-credibility-notes.md`](scope-and-credibility-notes.md) for public claims;
3. `../evidence/README.md` and the claim-evidence matrix for validation claims;
4. operational runbooks for actual execution.

---

## 2. Curriculum structure

The reconstructed curriculum contains two parts.

### Part 1 — Cybersecurity Theory

#### Module 1 — Security Refresher

1. Confidentiality, Integrity, and Availability
2. Security Controls
3. Advanced Persistent Threats
4. Risk

#### Module 2 — Security Frameworks

5. NIST publications and frameworks
6. Risk Management Framework
7. Applying the Risk Management Framework
8. NIST SP 800-53
9. NIST SP 800-61
10. NIST Cybersecurity Framework
11. CIS Critical Security Controls

#### Module 3 — Regulations and Standards

12. HIPAA and HITRUST
13. PCI DSS
14. GDPR

#### Module 4 — Security Operations

15. Security Operations Center
16. Security Information and Event Management
17. Indicators of Compromise

### Part 2 — Hands-On Work

1. Azure introduction
2. Logging and monitoring
3. Microsoft Sentinel
4. Secure cloud configuration
5. Environment cleanup

Part 2 is implemented through the architecture, runbooks, KQL, analytics-rule catalog, evidence controls, reporting artifacts, and teardown workflow.

---

## 3. Mapping classifications

Each theory topic receives one primary classification.

| Classification | Meaning | Project expectation |
|---|---|---|
| **Direct** | The concept is operationally represented in the core lab | An artifact, control, test, or evidence relationship must exist |
| **Conceptual** | The concept materially informs design or interpretation | The relationship must be documented, but full implementation is not required |
| **Context** | The topic improves professional understanding but is not a project objective | It must not be forced into the implementation or used to inflate claims |
| **Excluded** | The concept is inappropriate, unsafe, obsolete, or outside scope | It must not be represented as part of core v2 |

A conceptual mapping is not weaker scholarship. It is a deliberate scope decision.

---

## 4. Claim rules

The following rules apply throughout this mapping:

- Framework alignment does not establish compliance.
- Framework mapping does not establish authorization.
- A documented control does not prove that the control operates effectively.
- A deployed tool does not prove that its configuration is correct.
- A collected event does not automatically establish malicious activity.
- An alert does not automatically establish an incident.
- An incident object does not automatically establish compromise.
- An IP address or GeoIP result does not establish human attribution.
- A lab workflow does not establish enterprise SOC capability.
- A personal go/no-go decision is not an Authority to Operate.
- Deleting a resource group does not by itself prove complete cleanup or final cost closure.

The project claim hierarchy is:

> Designed → Implemented → Validated → Demonstrated → Completed

The separate terms **Mapped** and **Informed by** describe relationships to external guidance; they do not replace implementation evidence.

---

## 5. Top-level theory-to-project matrix

| ID | Theory topic | Classification | Primary project relationship |
|---|---|---|---|
| T-01 | Confidentiality, Integrity, and Availability | Conceptual | Telemetry, credentials, evidence, availability, and cleanup decisions |
| T-02 | Security Controls | Direct | Preventive, detective, containment, recovery, governance, and evidence controls |
| T-03 | Advanced Persistent Threats | Context | Threat-awareness and attribution restraint |
| T-04 | Risk | Direct | Risk register, exposure gates, residual risk, stop conditions |
| T-05 | NIST publications and frameworks | Conceptual | Correct use of guidance according to document purpose |
| T-06 | Risk Management Framework | Conceptual | RMF-inspired lifecycle without formal authorization |
| T-07 | Applying the RMF | Conceptual | Structured preparation, selection, implementation, assessment, and monitoring |
| T-08 | NIST SP 800-53 | Conceptual | Selected control-family reasoning, not control-baseline compliance |
| T-09 | NIST SP 800-61 | Direct | Preparation, detection, response, recovery, and improvement workflow |
| T-10 | NIST Cybersecurity Framework | Conceptual | Six-Function outcome taxonomy |
| T-11 | CIS Critical Security Controls | Conceptual | Prioritization and safeguard context |
| T-12 | HIPAA and HITRUST | Context | Regulated-environment awareness; no health-data or assurance claim |
| T-13 | PCI DSS | Context | Payment-card security awareness; no cardholder-data environment |
| T-14 | GDPR | Conceptual | Data minimization, purpose limitation, and evidence publication restraint |
| T-15 | Security Operations Center | Conceptual | Bounded SecOps capability, not an enterprise SOC |
| T-16 | SIEM | Direct | Collection, schema, health, analytics, triage, and evidence workflow |
| T-17 | Indicators of Compromise | Direct | Observable enrichment, pattern analysis, qualification, and attribution limits |

---

## 6. Module 1 — Security Refresher

### 6.1 T-01 — Confidentiality, Integrity, and Availability

**Classification:** Conceptual

#### Retained concept

The CIA triad remains a foundational model for evaluating security objectives.

#### Modernized interpretation

CIA is useful but incomplete when used by itself. The project also considers:

- authenticity;
- accountability;
- privacy;
- auditability;
- resilience;
- safety;
- evidence quality.

#### Project application

| Objective | Project application |
|---|---|
| Confidentiality | Protect credentials, tenant data, subscription data, raw events, source IPs, and private evidence |
| Integrity | Preserve accurate configurations, query versions, evidence records, timestamps, and failed-test history |
| Availability | Maintain telemetry and administrative control during the approved window; remove exposure when monitoring fails |
| Authenticity | Distinguish controlled tests from organic activity |
| Accountability | Record operator decisions, rule versions, test sessions, and dispositions |
| Privacy | Minimize and sanitize public evidence |
| Auditability | Preserve traceable evidence and claim relationships |
| Resilience | Use stop conditions and teardown rather than attempting to preserve a compromised disposable VM |
| Safety | Prevent pivoting, harmful outbound use, excessive exposure, and unmanaged cost |

#### Relevant artifacts

- `threat-model.md`
- `architecture-overview.md`
- `scope-and-credibility-notes.md`
- `../evidence/README.md`
- `../runbooks/teardown-runbook.md`

#### Claim boundary

The project may state that CIA and related security objectives informed the design.

It must not claim that all confidentiality, integrity, or availability risks have been eliminated.

---

### 6.2 T-02 — Security Controls

**Classification:** Direct

#### Retained concept

Security controls reduce risk through administrative, operational, and technical measures.

#### Modernized interpretation

Control classification has multiple independent dimensions.

| Dimension | Examples |
|---|---|
| Ownership or implementation | Management, operational, technical |
| Security function | Preventive, detective, corrective, deterrent, compensating, recovery |
| Lifecycle role | Governance, preparation, detection, response, recovery, monitoring |
| Evidence state | Designed, implemented, assessed, effective, ineffective |

A technical tool can support several functions. Its presence does not prove effectiveness.

#### Direct project examples

| Control role | Project examples |
|---|---|
| Governance | Scope, threat model, explicit go/no-go decision |
| Preventive | Isolated VNet, no peering, synthetic credentials, Windows Firewall |
| Detective | AMA/DCR collection, KQL queries, `AR-001`, telemetry-health checks |
| Containment | Remove TCP/3389 rule, deallocate or delete VM |
| Corrective | Revise query, DCR, rule, documentation, or configuration |
| Recovery | Teardown, orphan-resource review, cost closure |
| Administrative | Runbooks, evidence rules, approval gates |
| Technical | NSG, Windows Firewall, AMA, DCR, Log Analytics, Sentinel |
| Compensating | Time-boxed exposure and disposable infrastructure where public RDP is intentionally retained |

#### Control-effectiveness rule

A control is not considered validated until:

1. its intended objective is defined;
2. the implementation is identified;
3. a test is executed;
4. the actual result is recorded;
5. the result is evaluated;
6. limitations and residual risk are documented.

#### Relevant artifacts

- `threat-model.md`
- `checklist-to-project-roadmap.md`
- `../runbooks/deployment-runbook.md`
- `../runbooks/rdp-authentication-triage.md`
- `../runbooks/teardown-runbook.md`
- `../evidence/claim-evidence-matrix.md`

---

### 6.3 T-03 — Advanced Persistent Threats

**Classification:** Context

#### Retained concept

Sophisticated threat actors may use extended, adaptive, and multi-stage operations.

#### Project boundary

This project does not attempt to:

- identify an APT;
- attribute activity to a named group;
- infer nation-state activity;
- reconstruct a long-duration campaign;
- perform malware analysis;
- conduct threat-actor engagement;
- infer attacker identity from IP or GeoIP data.

Ordinary scanning, authentication attempts, or a Sentinel alert are not evidence of an APT.

#### Appropriate project use

APT material supports professional caution concerning:

- persistence;
- privilege escalation;
- defense evasion;
- credential access;
- lateral movement;
- command and control;
- attribution uncertainty.

These ideas inform stop conditions and follow-on hunting but do not become project claims.

---

### 6.4 T-04 — Risk

**Classification:** Direct

#### Retained concept

Risk concerns the possibility that a threat event will exploit a vulnerability or precondition and produce harmful consequences.

#### Modernized interpretation

A useful project risk scenario identifies:

- the asset or objective;
- the threat source or event;
- the vulnerability or exposure condition;
- the potential consequence;
- existing controls;
- residual uncertainty;
- the owner;
- the treatment decision;
- evidence required for closure.

A likelihood-times-impact score is a prioritization heuristic, not a precise measurement of future loss.

#### Direct project application

The threat model contains risks involving:

- unauthorized remote access;
- harmful outbound activity;
- pivoting into trusted networks;
- weak or reused credentials;
- missing telemetry;
- incorrect schema assumptions;
- false-positive or false-negative detections;
- evidence leakage;
- excessive privilege;
- cost overrun;
- incomplete teardown;
- unsupported public claims.

#### Risk treatment options

| Treatment | Project use |
|---|---|
| Avoid | Reject trusted-network connectivity, real credentials, sensitive data, or unattended exposure |
| Mitigate | Apply isolation, telemetry, time limits, stop conditions, and teardown |
| Accept | Conditionally accept bounded residual exposure after prerequisites pass |
| Transfer | Generally not applicable to the personal lab; cloud-provider responsibility does not transfer operator duties |

#### Authorization boundary

The operator may make a conditional personal lab go/no-go decision.

The project does not perform formal federal authorization and does not receive an Authority to Operate.

---

## 7. Module 2 — Security Frameworks

### 7.1 T-05 — NIST publications and frameworks

**Classification:** Conceptual

#### Modernized interpretation

“NIST” is not one control framework. Different publications have different purposes.

| Publication or resource | Project use |
|---|---|
| CSF 2.0 | High-level cybersecurity outcome taxonomy |
| SP 800-37 Rev. 2 | RMF lifecycle concepts |
| SP 800-53 Rev. 5 | Security and privacy control catalog |
| SP 800-53B | Control-baseline context |
| SP 800-53A | Control-assessment procedure context |
| SP 800-61 Rev. 3 | Incident-response recommendations integrated with CSF 2.0 |
| Other NIST guidance | Used only when specifically identified and relevant |

The project must identify the publication and its role rather than using “NIST” as an unsupported quality label.

---

### 7.2 T-06 — Risk Management Framework

**Classification:** Conceptual

#### Current RMF structure

The RMF contains seven steps:

1. Prepare
2. Categorize
3. Select
4. Implement
5. Assess
6. Authorize
7. Monitor

#### Project analogy

| RMF step | Project analogy |
|---|---|
| Prepare | Scope, architecture, roles, cost limits, evidence plan, threat model |
| Categorize | Describe assets, data sensitivity, operational consequence, and disposable status |
| Select | Choose project controls based on risks and constraints |
| Implement | Deploy Azure and Sentinel configurations |
| Assess | Execute schema, health, detection, response, teardown, and cost tests |
| Authorize | Make a bounded personal go/no-go decision; not a formal ATO |
| Monitor | Review telemetry, runtime, cost, configuration, and stop conditions |

#### Boundary

The project is **RMF-inspired**.

It is not a complete organizational RMF implementation because it lacks:

- an organizational risk-management hierarchy;
- formal system categorization;
- an approved control baseline;
- independent control assessment;
- an authorizing official;
- formal authorization documentation;
- continuous organizational monitoring.

---

### 7.3 T-07 — Applying the RMF

**Classification:** Conceptual

The useful lesson is disciplined lifecycle execution rather than ceremonial framework language.

#### Applied sequence

1. Define the mission and boundaries.
2. Identify risks and constraints.
3. Choose proportionate controls.
4. Document the expected configuration.
5. Implement only after approval.
6. Assess actual behavior.
7. record failures and residual risk.
8. decide whether exposure may continue.
9. monitor until closure.
10. decommission and reconcile cost.

#### Anti-patterns

- selecting controls only because they appear in a framework;
- claiming implementation before deployment;
- claiming effectiveness without testing;
- treating risk acceptance as permanent;
- treating authorization as a one-time checkbox;
- ignoring cleanup and cost after testing;
- describing a personal lab decision as an ATO.

---

### 7.4 T-08 — NIST SP 800-53

**Classification:** Conceptual

#### Correct role

SP 800-53 is a catalog of security and privacy controls.

Related publications have different roles:

- SP 800-53 provides the control catalog;
- SP 800-53B provides control baselines;
- SP 800-53A provides assessment procedures.

The project may map selected controls or control families where the relationship is explicit.

#### Potentially relevant control families

| Family | Project relationship |
|---|---|
| AC — Access Control | Operator and synthetic-account access |
| AU — Audit and Accountability | Windows events, Log Analytics, evidence records |
| CA — Assessment, Authorization, and Monitoring | Validation and continuous review concepts |
| CM — Configuration Management | Approved architecture and configuration tracking |
| CP — Contingency Planning | Limited teardown and disposable recovery |
| IA — Identification and Authentication | Azure and Windows authentication controls |
| IR — Incident Response | Triage, containment, teardown, lessons |
| RA — Risk Assessment | Threat model and risk register |
| SC — System and Communications Protection | Isolation, NSG, firewall, trusted-path restrictions |
| SI — System and Information Integrity | Telemetry health and suspicious-activity review |

#### Boundary

A family or control mapping does not prove:

- full control implementation;
- organization-defined parameter completion;
- baseline conformance;
- assessment success;
- continuous monitoring;
- authorization;
- compliance.

---

### 7.5 T-09 — NIST SP 800-61

**Classification:** Direct

#### Modernized baseline

SP 800-61 Rev. 3 is the current incident-response publication used by this project.

The older four-phase model from Rev. 2 may be discussed historically, but it is not treated as the current authoritative structure.

Rev. 3 integrates incident response across CSF 2.0 cybersecurity-risk-management outcomes.

#### Direct project workflow

| Incident-response concern | Project implementation |
|---|---|
| Governance and preparation | Scope, roles, stop conditions, evidence and cost plans |
| Detection | AMA/DCR telemetry, KQL, `AR-001`, health checks |
| Analysis | Authentication context, source/account/host/time correlation |
| Response | Exposure removal, deallocation, identity and control-plane review |
| Recovery | Destruction, cleanup, orphan review, cost closure |
| Improvement | Findings, failed tests, tuning, remediation recommendations |

#### Incident threshold

Not every security event is an incident.

The project distinguishes:

1. event;
2. observable;
3. pattern;
4. detection result;
5. alert;
6. incident object;
7. compromise hypothesis;
8. confirmed unauthorized activity.

#### Relevant artifacts

- `../runbooks/rdp-authentication-triage.md`
- `../runbooks/teardown-runbook.md`
- `../reports/incident-timeline.md`
- `../reports/findings-report.md`
- `../reports/remediation-recommendations.md`

---

### 7.6 T-10 — NIST Cybersecurity Framework

**Classification:** Conceptual

#### Current structure

CSF 2.0 uses six Functions:

1. Govern
2. Identify
3. Protect
4. Detect
5. Respond
6. Recover

The Functions organize outcomes. They do not prescribe one required technology implementation.

#### Project mapping

| Function | Project relationship |
|---|---|
| Govern | Scope, claim rules, risk ownership, cost limits, go/no-go decisions |
| Identify | Assets, boundaries, dependencies, telemetry requirements, risks |
| Protect | Identity, isolation, firewall, synthetic credentials, evidence restrictions |
| Detect | Event collection, schema checks, health checks, KQL, analytics rule |
| Respond | Triage, exposure removal, deallocation, identity review |
| Recover | Teardown, cleanup verification, cost closure, lessons learned |

#### Boundary

The project is informed by CSF 2.0.

It does not claim:

- an organizational Current Profile;
- a Target Profile;
- Tier achievement;
- enterprise CSF adoption;
- CSF compliance;
- organizational governance maturity.

---

### 7.7 T-11 — CIS Critical Security Controls

**Classification:** Conceptual

#### Current baseline

CIS Controls v8.1 contains:

- 18 top-level Controls;
- 153 Safeguards;
- three Implementation Groups.

Implementation Groups prioritize safeguards according to organizational resources and risk. They must not be treated as a generic maturity-level score.

#### Relevant control themes

| CIS Control theme | Project relationship |
|---|---|
| Asset inventory | Dedicated resource group and resource inventory |
| Account management | Synthetic accounts and operator identity |
| Access control management | Least privilege and removal of unneeded identities |
| Secure configuration | Windows Firewall, NSG, approved VM configuration |
| Audit-log management | Windows Security Events, workspace validation |
| Network monitoring and defense | Bounded public endpoint and monitoring |
| Security-awareness concepts | Operator decision and evidence discipline |
| Incident-response management | Triage and teardown runbooks |
| Penetration-testing concepts | Controlled authentication tests only |

#### Boundary

The project does not perform a CIS Controls assessment and does not claim an Implementation Group.

---

## 8. Module 3 — Regulations and Standards

### 8.1 T-12 — HIPAA and HITRUST

**Classification:** Context

#### Retained lesson

Regulated environments impose additional obligations concerning data, access, risk, evidence, incident response, third parties, and assurance.

#### Project boundary

Core v2 does not intentionally process:

- protected health information;
- electronic protected health information;
- patient records;
- healthcare-production workloads.

HITRUST is not treated as interchangeable with HIPAA, and neither is treated as a project certification.

The project must not claim:

- HIPAA compliance;
- HITRUST certification;
- healthcare-system authorization;
- completion of a regulated breach analysis.

#### Useful contextual application

- minimize sensitive data;
- restrict public evidence;
- document access;
- preserve accurate records;
- define incident and escalation criteria;
- avoid universal breach-timeline claims without identifying the controlling law or contract.

---

### 8.2 T-13 — PCI DSS

**Classification:** Context

#### Retained lesson

Payment-card environments require defined scoping, segmentation, access control, logging, testing, and evidence.

#### Project boundary

Core v2 contains no intended:

- cardholder data;
- sensitive authentication data;
- payment application;
- cardholder-data environment;
- payment processor integration.

The project must not claim:

- PCI DSS compliance;
- a PCI-scoped architecture;
- a segmentation assessment;
- a Qualified Security Assessor review.

#### Useful contextual application

PCI concepts reinforce:

- accurate scope;
- segmentation;
- least privilege;
- log review;
- controlled testing;
- evidence retention;
- decommissioning.

---

### 8.3 T-14 — GDPR

**Classification:** Conceptual

#### Retained lesson

Security telemetry and evidence can contain information that identifies or helps distinguish people, accounts, devices, or network activity.

#### Project application

The project applies privacy-conscious handling through:

- purpose-limited collection;
- minimum required fields;
- short lab duration;
- private raw evidence;
- source-IP redaction;
- account-name sanitization;
- removal of tenant and subscription identifiers;
- controlled public publication;
- retention and deletion decisions;
- no unsupported human attribution.

#### Boundary

The project does not claim:

- GDPR compliance;
- a formal lawful-basis assessment;
- completion of a data-protection impact assessment;
- organizational controller or processor compliance;
- satisfaction of breach-notification obligations.

Privacy principles inform evidence governance without creating a regulatory certification claim.

---

## 9. Module 4 — Security Operations

### 9.1 T-15 — Security Operations Center

**Classification:** Conceptual

#### Modernized interpretation

A SOC is an operational capability involving people, process, technology, authority, coverage, escalation, and continuous improvement. It is not merely a room, dashboard, or SIEM deployment.

#### Project application

Core v2 demonstrates selected SecOps activities:

- telemetry onboarding;
- detection development;
- alert creation;
- bounded investigation;
- incident-object handling;
- containment;
- evidence;
- reporting;
- remediation recommendations;
- cleanup.

#### Boundary

The project does not demonstrate:

- 24×7 monitoring;
- shift operations;
- enterprise escalation;
- production incident authority;
- service-level management;
- multi-tenant operations;
- broad detection coverage;
- threat-intelligence operations;
- complete SOC maturity.

Preferred wording:

> Bounded Microsoft Sentinel detection-engineering and SecOps workflow.

---

### 9.2 T-16 — Security Information and Event Management

**Classification:** Direct

#### Modernized interpretation

A SIEM capability requires more than log ingestion.

The project evaluates:

1. source configuration;
2. collection path;
3. destination table;
4. schema and field population;
5. freshness and latency;
6. query logic;
7. threshold behavior;
8. entity mapping;
9. alert generation;
10. incident-object behavior;
11. triage and disposition;
12. evidence and retention;
13. health and failure handling;
14. cost.

#### Direct implementation

| SIEM layer | Core-v2 element |
|---|---|
| Source | Windows Security log |
| Agent | Azure Monitor Agent |
| Collection definition | Data Collection Rule |
| Destination | Log Analytics workspace |
| Expected table | `SecurityEvent` |
| Health signal | `Heartbeat`, where available, plus freshness checks |
| Analytics | Versioned KQL catalog |
| Scheduled detection | `AR-001` |
| Case object | Sentinel or Defender incident object |
| Investigation | Authentication-triage runbook |
| Evidence | Private records and sanitized public artifacts |
| Closure | Teardown and cost reconciliation |

#### Failure principle

Public exposure is not authorized when telemetry is unavailable, stale, incorrectly scoped, or unvalidated.

---

### 9.3 T-17 — Indicators of Compromise

**Classification:** Direct

#### Modernized terminology

An observable is a recorded fact or field.

An indicator is an observable or pattern interpreted as having security relevance.

A single observable is not automatically proof of compromise.

#### Project observables

- event ID;
- logon type;
- source IP address;
- destination host;
- account name;
- timestamp;
- authentication status;
- failure reason;
- process or follow-on activity where available;
- alert identifier;
- incident identifier.

#### Project patterns

- repeated failures from one source;
- repeated failures against one account;
- distributed failures across accounts;
- controlled threshold-matching tests;
- failed logons followed by a successful logon;
- successful remote-interactive access followed by suspicious activity;
- telemetry loss during exposure.

#### Qualification requirements

An indicator must be evaluated against:

- controlled-test records;
- approved test source;
- synthetic account;
- time window;
- host;
- event semantics;
- schema quality;
- surrounding activity;
- known platform behavior.

#### GeoIP and attribution boundary

GeoIP may provide approximate network-location context.

It does not prove:

- attacker nationality;
- physical location;
- identity;
- motive;
- organizational affiliation;
- APT membership.

---

## 10. Part 2 hands-on mapping

| Hands-on area | Core-v2 project implementation |
|---|---|
| Azure introduction | Subscription, resource group, VNet, subnet, NSG, public IP, NIC, Windows VM |
| Logging and monitoring | Windows Security log, AMA, DCR, Log Analytics, health and schema validation |
| Microsoft Sentinel | KQL, scheduled analytics rule, alert, incident object, triage |
| Secure cloud configuration | Isolation, identity restrictions, host firewall, conditional exposure, cost controls |
| Environment cleanup | Normal and emergency teardown, orphan checks, telemetry disposition, delayed cost review |

Hands-on implementation remains blocked until the predeployment gate authorizes Azure execution.

---

## 11. Direct mappings and required evidence

| Mapping ID | Direct topic | Required project artifact | Required eventual evidence |
|---|---|---|---|
| D-01 | Security controls | Threat model and runbooks | Control implementation and test results |
| D-02 | Risk | Risk register and stop conditions | Gate decision and residual-risk record |
| D-03 | Incident response | Triage and teardown runbooks | Controlled response or safe response test |
| D-04 | SIEM collection | AMA/DCR and workspace | Association, health, table, source, timestamps |
| D-05 | SIEM schema | KQL schema queries | Actual 4625 and 4624 field population |
| D-06 | Detection | `AR-001` and KQL | Below-threshold and threshold-matching tests |
| D-07 | Alert handling | Sentinel alert | Alert evidence and underlying events |
| D-08 | Incident handling | Incident object and triage | Disposition, closure decision, limitations |
| D-09 | Indicators | KQL and enrichment workflow | Qualified observable and pattern records |
| D-10 | Recovery and closure | Teardown and cost controls | Orphan review and delayed cost reconciliation |

Until the evidence exists, each row remains designed or planned.

---

## 12. Conceptual mappings

| Mapping ID | Conceptual topic | Design influence | Prohibited escalation |
|---|---|---|---|
| C-01 | CIA and complementary objectives | Evidence, telemetry, identity, resilience | “All CIA risks mitigated” |
| C-02 | NIST publication roles | Correct source selection | “NIST compliant” |
| C-03 | RMF | Structured lifecycle | “Formal RMF authorization” |
| C-04 | Applying RMF | Prepare through monitor analogy | “Received an ATO” |
| C-05 | SP 800-53 | Selected control reasoning | “Implemented the baseline” |
| C-06 | CSF 2.0 | Six-Function taxonomy | “CSF compliant” |
| C-07 | CIS Controls | Prioritization context | “Achieved IG1, IG2, or IG3” |
| C-08 | GDPR | Privacy-aware evidence handling | “GDPR compliant” |
| C-09 | SOC | SecOps capability model | “Built an enterprise SOC” |

---

## 13. Context mappings

| Mapping ID | Context topic | Professional value | Core-v2 exclusion |
|---|---|---|---|
| X-01 | Advanced Persistent Threats | Recognize multi-stage threats and attribution limits | No APT identification |
| X-02 | HIPAA and HITRUST | Understand regulated health-data environments | No PHI or certification claim |
| X-03 | PCI DSS | Understand scoped payment-card security | No cardholder-data environment |

Context topics may appear in educational discussion but should not drive unnecessary Azure services or portfolio inflation.

---

## 14. Theory-to-artifact traceability

| Project artifact | Principal theory relationships |
|---|---|
| `threat-model.md` | Risk, controls, CIA, RMF, CSF Govern/Identify |
| `architecture-overview.md` | CIA, controls, SIEM, secure configuration |
| `scope-and-credibility-notes.md` | Risk communication, attribution restraint, compliance boundaries |
| `production-safe-design-contrast.md` | Controls, risk treatment, secure architecture |
| `checklist-to-project-roadmap.md` | RMF-inspired lifecycle, control implementation planning |
| `../runbooks/deployment-runbook.md` | Prepare, implement, assess, monitor |
| `../runbooks/rdp-authentication-triage.md` | SP 800-61, SIEM, indicators, incident analysis |
| `../runbooks/teardown-runbook.md` | Respond, recover, risk containment |
| `../runbooks/cost-control-checklist.md` | Governance, risk, monitoring |
| `../kql/hunting-queries.md` | SIEM, observables, indicators, detection |
| `../sentinel/analytics-rules-catalog.md` | Detection engineering and rule lifecycle |
| `../evidence/README.md` | CIA, privacy, accountability, auditability |
| `../reports/findings-report.md` | Risk communication and improvement |
| `../reports/incident-timeline.md` | Incident-response chronology |
| `../reports/remediation-recommendations.md` | Corrective controls and improvement |

---

## 15. Control-versus-evidence distinction

The project must keep these concepts separate:

| Concept | Question |
|---|---|
| Control objective | What risk reduction is intended? |
| Control design | How should the control work? |
| Implementation | Was the control configured or performed? |
| Assessment | Was the control tested? |
| Result | What actually happened? |
| Effectiveness | Did the control achieve its objective within scope? |
| Evidence | What supports the conclusion? |
| Residual risk | What remains uncertain or accepted? |

Examples:

- An NSG rule is not evidence that Windows Firewall is enabled.
- AMA installation is not evidence that the correct events reached the workspace.
- A DCR definition is not evidence of a valid VM association.
- A KQL query is not evidence that required fields exist.
- A scheduled rule is not evidence that alerts will fire correctly.
- An alert is not evidence of successful compromise.
- Resource-group deletion is not evidence that no orphaned resource remains.
- A budget alert is not a hard spending stop.

---

## 16. Detection and incident terminology ladder

The project uses the following ladder:

1. **Event** — a recorded occurrence.
2. **Observable** — a validated fact extracted from data.
3. **Pattern** — related observables grouped by relevant dimensions.
4. **Detection result** — query or rule conditions were satisfied.
5. **Alert** — a platform-generated security notification.
6. **Incident object** — a platform case used for investigation.
7. **Investigation finding** — an analyst conclusion supported by evidence.
8. **Compromise hypothesis** — a testable explanation that unauthorized access may have succeeded.
9. **Confirmed unauthorized activity** — evidence satisfies defined confirmation criteria.

The project must not skip levels merely to make a report sound more dramatic.

---

## 17. Excluded or rejected theory applications

The following are excluded from core v2:

- claiming that all unsolicited traffic is malicious;
- treating Event ID 4625 as intrinsically RDP;
- treating all repeated failures as brute force;
- using GeoIP as human attribution;
- claiming an APT based on ordinary internet activity;
- claiming NIST, CIS, HIPAA, HITRUST, PCI DSS, or GDPR compliance;
- representing personal approval as an ATO;
- forcing regulated-data scenarios into a lab with no regulated data;
- deploying unnecessary enterprise services solely to mention them;
- using a framework mapping as proof of control effectiveness;
- treating a SIEM dashboard as a complete SOC;
- presenting course material as original project evidence;
- reproducing private source material in the public repository.

---

## 18. Interview-use guidance

### Before Azure execution

Supported discussion topics include:

- why the theory classifications were chosen;
- why some topics are direct while others are contextual;
- how risk drove architecture and stop conditions;
- why public exposure requires telemetry first;
- how control design differs from control effectiveness;
- why failed authentication is not automatically compromise;
- how CSF, RMF, SP 800-53, SP 800-61, and CIS have different roles;
- why compliance and attribution claims are restricted.

### After validated execution

Additional discussion may include:

- actual AMA/DCR behavior;
- discovered `SecurityEvent` schema;
- query changes;
- rule-threshold results;
- alert and incident behavior;
- controlled versus organic activity;
- failed tests;
- containment decisions;
- cleanup results;
- cost settlement.

The discussion must remain consistent with the claim-evidence matrix.

---

## 19. Review triggers

Review this mapping when:

- a framework or publication is revised or withdrawn;
- course source material is reinterpreted;
- a project capability moves from design to implementation;
- new Azure services are added;
- telemetry schema differs from the design;
- the incident workflow changes;
- a compliance or framework claim is proposed;
- a resume or interview claim cites a framework;
- the project scope expands beyond Windows remote-interactive authentication;
- production deployment is proposed.

Version-sensitive references must be checked against current primary sources before publication.

---

## 20. Definition of done

This mapping is complete when:

- [ ] All 17 theory sections have one primary classification.
- [ ] Direct, conceptual, context, and excluded relationships are distinguished.
- [ ] CIA is treated as foundational but not exhaustive.
- [ ] Control classification dimensions are not conflated.
- [ ] Risk is scenario based and includes residual uncertainty.
- [ ] RMF uses the seven-step structure.
- [ ] The personal lab is not represented as formally authorized.
- [ ] SP 800-53, SP 800-53A, and SP 800-53B have distinct roles.
- [ ] SP 800-61 Rev. 3 is treated as the current baseline.
- [ ] CSF 2.0 uses six Functions, including Govern.
- [ ] CIS Implementation Groups are not represented as maturity levels.
- [ ] Regulated-domain topics do not create compliance claims.
- [ ] SOC is treated as a capability rather than a tool or room.
- [ ] SIEM includes collection health, schema, analytics, triage, and evidence.
- [ ] Observables, indicators, alerts, incidents, and compromise are distinguished.
- [ ] GeoIP is not used for human attribution.
- [ ] Each direct mapping identifies an artifact and eventual evidence.
- [ ] No theory topic is forced into the Azure deployment merely for appearance.

---

## 21. Primary-source baseline

| Source | Role in this mapping |
|---|---|
| NIST Cybersecurity Framework 2.0 | Six-Function cybersecurity outcome taxonomy |
| NIST SP 800-37 Rev. 2 | Seven-step Risk Management Framework |
| NIST SP 800-53 Rev. 5 | Security and privacy control catalog |
| NIST SP 800-53A Rev. 5 | Control-assessment procedures |
| NIST SP 800-53B | Control baselines |
| NIST SP 800-61 Rev. 3 | Current incident-response recommendations and CSF 2.0 Community Profile |
| CIS Critical Security Controls v8.1 | Prioritized Controls, Safeguards, and Implementation Groups |

Any future version change requires review before this document is described as current.

---

## 22. Related documents

| Document | Purpose |
|---|---|
| [`threat-model.md`](threat-model.md) | Authoritative risks, controls, boundaries, and stop conditions |
| [`architecture-overview.md`](architecture-overview.md) | Concise system architecture |
| [`scope-and-credibility-notes.md`](scope-and-credibility-notes.md) | Claim and terminology boundaries |
| [`production-safe-design-contrast.md`](production-safe-design-contrast.md) | Lab-versus-production comparison |
| [`checklist-to-project-roadmap.md`](checklist-to-project-roadmap.md) | Implementation roadmap and definition of done |
| `../runbooks/deployment-runbook.md` | Deployment and controlled validation |
| `../runbooks/rdp-authentication-triage.md` | Authentication analysis and response |
| `../runbooks/teardown-runbook.md` | Normal and emergency cleanup |
| `../kql/hunting-queries.md` | SIEM and indicator analysis |
| `../sentinel/analytics-rules-catalog.md` | Detection lifecycle |
| `../evidence/README.md` | Evidence governance |
| `../evidence/claim-evidence-matrix.md` | Public claim authorization |
