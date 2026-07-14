# Checklist-to-Project Roadmap

| Field | Value |
|---|---|
| Project | Sentinel RDP Honeypot v2 |
| Document role | Modernized implementation roadmap and definition of done |
| Source basis | Private backup checklists and transcripts |
| Design status | Approved baseline |
| Execution status | Documentation implementation in progress |
| Azure deployment status | Paused |
| Public exposure authorization | Not granted |
| Authoritative risk source | [`threat-model.md`](threat-model.md) |
| Last updated | 2026-07-13 |

> This roadmap translates private historical course checklists into an original, current, safety-bounded project plan. The private source material remains outside Git. Checklist completion does not prove control effectiveness, successful detection, compromise, compliance, or project completion.

---

## 1. Purpose

This document converts the reconstructed course workflow into a controlled engineering roadmap for Sentinel RDP Honeypot v2.

It establishes:

- the disposition of historical checklist items;
- the project implementation sequence;
- the relationship between checklist tasks and repository artifacts;
- safety gates that block premature Azure execution or public exposure;
- test and evidence expectations;
- change-control requirements;
- milestone commit boundaries;
- closure and definition-of-done criteria.

This is not a verbatim reproduction of course material.

The public repository contains original project documentation derived from:

- current platform behavior;
- modern security-engineering practice;
- the project threat model;
- current primary-source guidance;
- independently developed controls, tests, runbooks, and evidence requirements.

---

## 2. Authority hierarchy

When documents disagree, use this order:

1. [`threat-model.md`](threat-model.md)
2. [`scope-and-credibility-notes.md`](scope-and-credibility-notes.md)
3. [`architecture-overview.md`](architecture-overview.md)
4. this roadmap
5. operational runbooks
6. KQL and analytics-rule catalogs
7. report and portfolio templates
8. historical private checklists

Historical checklist instructions never override current safety, cost, platform, evidence, or claim controls.

---

## 3. Disposition model

Each historical checklist item receives one primary disposition.

| Disposition | Meaning | Required treatment |
|---|---|---|
| **RETAIN** | Current, relevant, and acceptably scoped | Implement with normal validation |
| **ADAPT** | Useful objective but outdated, ambiguous, incomplete, or unsafe as written | Rewrite before implementation |
| **DEFER** | Potentially valuable but not required for core v2 | Record as optional expansion; do not block core completion |
| **REJECT** | Unsafe, obsolete, unnecessary, unsupported, or outside scope | Do not implement |
| **REPLACE** | Superseded by a safer or current mechanism | Implement only the named replacement |

Disposition describes the instruction, not its execution status.

---

## 4. Execution-status model

| Status | Meaning |
|---|---|
| **Not started** | No implementation work has begun |
| **In progress** | Work has begun but acceptance criteria are incomplete |
| **Blocked** | A prerequisite or authorization is missing |
| **Ready for review** | Implementation exists and awaits validation |
| **Validated** | Defined tests passed and evidence was reviewed |
| **Failed** | A defined test did not meet expectations |
| **Deferred** | Removed from the current execution path |
| **Rejected** | Explicitly prohibited or superseded |
| **Complete** | All applicable implementation, validation, evidence, and closure criteria passed |

A task is not complete merely because a portal screen, command, or file exists.

---

## 5. Current milestone state

| Milestone | State |
|---|---|
| Repository and branch verification | Complete |
| Private-source exclusion check | Complete |
| Commit 1 architecture and scope documents | In progress |
| Commit 2 evidence-governance documents | Not started |
| Commit 3 operational runbooks | Not started |
| Commit 4 KQL and analytics-rule catalog | Not started |
| Commit 5 reporting and portfolio templates | Not started |
| Cross-document integration review | Not started |
| Predeployment authorization gate | Blocked |
| Azure v2 deployment | Blocked |
| Public TCP/3389 observation | Blocked |
| Controlled authentication testing | Blocked |
| Detection and incident validation | Blocked |
| Teardown and cost closure | Blocked |
| Portfolio-claim promotion | Blocked |

Azure remains paused until the documentation and predeployment gates pass.

---

## 6. Core project objective

The core-v2 objective is to design, implement, and validate a bounded Azure and Microsoft Sentinel workflow for Windows remote-interactive authentication telemetry.

The intended workflow includes:

1. safe Azure scope confirmation;
2. isolated disposable infrastructure;
3. Windows Security Event collection through AMA and a DCR;
4. Log Analytics schema and telemetry-health validation;
5. controlled failed- and successful-authentication tests;
6. KQL investigation;
7. one scheduled analytics rule, `AR-001`;
8. alert and incident-object triage;
9. conditional, time-boxed organic observation;
10. evidence preservation and sanitization;
11. normal and emergency teardown;
12. delayed cost reconciliation;
13. evidence-gated portfolio claims.

---

## 7. Explicit non-objectives

Core v2 does not attempt to implement:

- an enterprise SOC;
- 24×7 monitoring;
- production incident authority;
- a production-safe public RDP administration model;
- a mature deception platform;
- malware analysis;
- attacker interaction;
- credential capture;
- formal penetration testing;
- APT attribution;
- broad threat-intelligence operations;
- regulatory compliance;
- formal RMF authorization;
- an Authority to Operate;
- enterprise identity governance;
- multi-cloud detection;
- Linux or SQL detections;
- a complete Defender for Cloud deployment;
- destructive automated response;
- permanent public exposure;
- long-term telemetry retention.

---

## 8. Source-material handling

The private source checklists and transcripts must remain outside the public repository.

Public artifacts must not contain:

- copied course transcripts;
- copied proprietary explanations;
- private platform screenshots;
- private access links;
- instructor credentials or names where unnecessary;
- hidden course metadata;
- unreviewed scripts from the original material;
- unsupported claims inherited from the original material.

Public project documents must be original engineering artifacts that:

- describe the current project;
- use current terminology;
- identify limitations;
- define tests and evidence;
- separate design from execution;
- remain independently understandable.

---

## 9. Azure introduction disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| AZ-001 | Create or access an Azure account | ADAPT | Use an approved existing subscription and verify tenant, subscription, identity, MFA, and cost boundaries |
| AZ-002 | Create a resource group | RETAIN | Use one dedicated project resource group with explicit naming and tags |
| AZ-003 | Create a virtual network | RETAIN | Use a dedicated isolated VNet |
| AZ-004 | Create a subnet | RETAIN | Use one dedicated workload subnet |
| AZ-005 | Create a network security group | RETAIN | Use explicit inbound and outbound review |
| AZ-006 | Create a public IP | ADAPT | Create only for the approved bounded observation model |
| AZ-007 | Create a Windows VM | RETAIN | Use a disposable, smallest-suitable Windows VM |
| AZ-008 | Use a reusable personal password | REJECT | Use unique synthetic credentials that are never reused |
| AZ-009 | Hardcode credentials in scripts or templates | REJECT | Use secure interactive input or protected variables |
| AZ-010 | Assign broad cloud permissions | REJECT | Use minimum required operator privilege |
| AZ-011 | Attach a managed identity by default | REJECT | Do not attach an identity without a defined need |
| AZ-012 | Peer the lab VNet to another network | REJECT | Preserve isolation from trusted environments |
| AZ-013 | Connect the lab to a VPN or home network | REJECT | No trusted path to personal, family, employer, or customer networks |
| AZ-014 | Enable broad inbound access | REJECT | Allow only explicitly approved TCP/3389 exposure |
| AZ-015 | Leave public RDP open indefinitely | REJECT | Use attended, time-boxed, conditionally authorized exposure |
| AZ-016 | Use the default subscription without checking context | REJECT | Require subscription preflight before every side-effecting action |
| AZ-017 | Add ownership and project tags | RETAIN | Apply purpose, owner, environment, and cleanup metadata |
| AZ-018 | Record resource identifiers | ADAPT | Keep full identifiers private and publish sanitized summaries |

---

## 10. Network and host-security disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| NET-001 | Allow inbound TCP/3389 | ADAPT | Permit only after the pre-exposure gate passes |
| NET-002 | Allow RDP from any source during setup | REJECT | Use a restricted operator source for setup where possible |
| NET-003 | Allow all inbound traffic | REJECT | No broad inbound rule |
| NET-004 | Disable Windows Firewall | REJECT | Windows Firewall must remain enabled |
| NET-005 | Disable security controls to increase telemetry | REJECT | Do not weaken unrelated controls merely to attract traffic |
| NET-006 | Use one isolated VNet | RETAIN | Dedicated VNet and subnet |
| NET-007 | Permit unrestricted trusted-network connectivity | REJECT | No peering, VPN, ExpressRoute, or shared trusted route |
| NET-008 | Deny all outbound traffic | ADAPT | Preserve required Azure monitoring connectivity and restrict unrelated egress where feasible |
| NET-009 | Ignore unexpected outbound activity | REJECT | Treat suspicious egress as a mandatory stop condition |
| NET-010 | Preserve the VM after suspected compromise | ADAPT | Preserve only safely obtainable evidence, then deallocate or destroy |
| NET-011 | Use real or sensitive data on the VM | REJECT | No production, personal, customer, or regulated data |
| NET-012 | Keep the endpoint unattended | REJECT | Operator presence is required during public exposure |
| NET-013 | Patch and record the initial host state | RETAIN | Record image, OS state, update state, and material deviations |
| NET-014 | Install unrelated software | REJECT | Minimize packages and attack surface |
| NET-015 | Create a permanent privileged test account | REJECT | Use temporary synthetic accounts and remove them during teardown |

---

## 11. Logging and monitoring disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| LOG-001 | Create a Log Analytics workspace | RETAIN | Use a dedicated or explicitly approved workspace |
| LOG-002 | Enable Microsoft Sentinel | RETAIN | Enable only on the approved workspace |
| LOG-003 | Install the legacy Log Analytics agent | REPLACE | Use Azure Monitor Agent |
| LOG-004 | Use OMS or MMA collection instructions | REPLACE | Use AMA and DCR-based collection |
| LOG-005 | Create a Data Collection Rule | RETAIN | Define source, stream, filtering, and destination |
| LOG-006 | Associate the DCR with the VM | RETAIN | Validate the actual association |
| LOG-007 | Treat the DCR as a network transit hop | REJECT | DCR configures collection and destination; AMA sends telemetry |
| LOG-008 | Collect all Windows events by default | ADAPT | Collect the minimum events needed for the defined objective |
| LOG-009 | Assume the destination table | REJECT | Validate the actual destination table |
| LOG-010 | Expect Windows Security Events in `SecurityEvent` | ADAPT | Treat as the expected route and validate actual behavior |
| LOG-011 | Use a generic Windows Event DCR and assume `SecurityEvent` | REJECT | Generic event routes may populate different tables and require separate validation |
| LOG-012 | Validate Event ID 4625 | RETAIN | Confirm field population and remote-interactive context |
| LOG-013 | Validate Event ID 4624 | RETAIN | Use an approved controlled success test |
| LOG-014 | Validate heartbeat or equivalent health | RETAIN | Confirm agent and collection health before exposure |
| LOG-015 | Measure freshness and latency | RETAIN | Record event time, ingestion time, and observed delay |
| LOG-016 | Continue exposure during telemetry loss | REJECT | Telemetry failure ends exposure |
| LOG-017 | Create new NSG flow logs | REJECT | New NSG flow-log deployment is unavailable and not part of core v2 |
| LOG-018 | Use VNet flow logs | DEFER | Optional expansion requiring separate scope, cost, and privacy review |
| LOG-019 | Enable every available data connector | REJECT | Enable only connectors required by the project |
| LOG-020 | Retain logs indefinitely | REJECT | Use purpose-limited retention and cleanup decisions |

---

## 12. Microsoft Sentinel disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| SEN-001 | Run KQL queries | RETAIN | Use a versioned query catalog with IDs and status |
| SEN-002 | Inspect failed logons | RETAIN | Validate Event ID 4625 and remote-interactive context |
| SEN-003 | Treat Event ID 4625 as intrinsically RDP | REJECT | Require logon-type and schema evidence |
| SEN-004 | Treat repeated failures as confirmed brute force | REJECT | Qualify the pattern based on source, account, host, and time |
| SEN-005 | Create a scheduled analytics rule | RETAIN | Implement `AR-001` after query validation |
| SEN-006 | Enable the rule before data validation | REJECT | Keep the rule disabled until schema and query tests pass |
| SEN-007 | Test below-threshold behavior | RETAIN | Confirm expected absence of alert |
| SEN-008 | Test threshold-matching behavior | RETAIN | Confirm expected detection result |
| SEN-009 | Create an alert and incident object | RETAIN | Validate platform behavior and correlation |
| SEN-010 | Treat the incident object as confirmed compromise | REJECT | Conduct evidence-based triage |
| SEN-011 | Map entities without validating field quality | REJECT | Map only stable, populated fields |
| SEN-012 | Map the account entity in the first rule version | DEFER | Add after schema and account-quality review |
| SEN-013 | Map source IP and host entities | ADAPT | Use only after validating field presence |
| SEN-014 | Use generic MITRE ATT&CK mapping | ADAPT | Use a conservative technique mapping with stated limitations |
| SEN-015 | Add a workbook or attack map | DEFER | Optional visualization after core validation |
| SEN-016 | Add GeoIP enrichment | DEFER | Use only with provenance, privacy, accuracy, and attribution limitations |
| SEN-017 | Use automated destructive response | REJECT | Core v2 uses manual or approval-gated containment |
| SEN-018 | Use automation rules for non-destructive routing | DEFER | Optional future expansion |
| SEN-019 | Preserve failed tests | RETAIN | Failed tests remain traceable |
| SEN-020 | Tune the rule after testing | RETAIN | Record each query and configuration version |

---

## 13. Security-operations disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| OPS-001 | Review alerts | RETAIN | Use the authentication-triage runbook |
| OPS-002 | Review underlying events | RETAIN | Alert evidence alone is insufficient |
| OPS-003 | Distinguish controlled and organic activity | RETAIN | Use test-session IDs, source, account, host, and time |
| OPS-004 | Classify true and false positives | ADAPT | Define the exact behavior the rule was intended to detect |
| OPS-005 | Determine whether access succeeded | RETAIN | Correlate 4624 and relevant follow-on activity |
| OPS-006 | Infer compromise from failed logons | REJECT | Failed authentication alone does not establish compromise |
| OPS-007 | Infer identity from source IP | REJECT | IP and GeoIP are investigative context only |
| OPS-008 | Claim APT activity | REJECT | No threat-actor attribution objective |
| OPS-009 | Document an incident timeline | RETAIN | Use event, alert, decision, and response timestamps |
| OPS-010 | Produce findings and recommendations | RETAIN | Separate facts, interpretations, limitations, and remediation |
| OPS-011 | Preserve raw evidence publicly | REJECT | Raw evidence remains private |
| OPS-012 | Publish sanitized evidence | RETAIN | Require review, redaction, and claim traceability |
| OPS-013 | Continue observing after an unexplained successful logon | REJECT | Trigger immediate stop and emergency response |
| OPS-014 | Perform hack-back or retaliation | REJECT | Prohibited |
| OPS-015 | Interact with source infrastructure | REJECT | No counterattack or engagement |
| OPS-016 | Claim production incident authority | REJECT | Personal lab decisions only |

---

## 14. Secure cloud-configuration disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| SEC-001 | Use least privilege | RETAIN | Apply to Azure operator and workload identity |
| SEC-002 | Require MFA for the operator | RETAIN | Confirm before deployment |
| SEC-003 | Use dedicated project scope | RETAIN | Dedicated resource group and naming |
| SEC-004 | Review effective NSG rules | RETAIN | Validate actual effective exposure |
| SEC-005 | Review Windows Firewall | RETAIN | Confirm enabled state and relevant rule |
| SEC-006 | Review managed identities | RETAIN | No unnecessary identity |
| SEC-007 | Review role assignments | RETAIN | Identify unexpected or excessive access |
| SEC-008 | Enable Defender for Servers automatically | DEFER | Requires named use case and cost approval |
| SEC-009 | Enable broad Defender for Cloud plans | REJECT | No broad paid-plan activation without authorization |
| SEC-010 | Use just-in-time VM access | DEFER | Production comparison or future expansion |
| SEC-011 | Deploy Azure Bastion | DEFER | Changes the observation model and adds cost |
| SEC-012 | Deploy a VPN Gateway | DEFER | Not required for the isolated public-observation lab |
| SEC-013 | Deploy Azure Firewall | DEFER | Disproportionate scope and cost for core v2 |
| SEC-014 | Use policy-as-code | DEFER | Future guardrail expansion |
| SEC-015 | Add infrastructure as code | DEFER | Valuable follow-on after manual behavior is validated |
| SEC-016 | Add CI/CD deployment | DEFER | Future automation phase |
| SEC-017 | Add hard spending shutdown automation | DEFER | Requires safe service-specific response design |
| SEC-018 | Treat budget alerts as hard stops | REJECT | Budgets are notifications, not guaranteed containment |
| SEC-019 | Use runtime as the primary cost boundary | RETAIN | Explicit maximum runtime and cleanup owner |
| SEC-020 | Review optional-service cost before enablement | RETAIN | Required before paid services are added |

---

## 15. Third-party code and data disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| EXT-001 | Download an enrichment script and execute it directly | REJECT | Treat all external code as untrusted |
| EXT-002 | Use third-party GeoIP data without review | REJECT | Require provenance, license, integrity, and privacy review |
| EXT-003 | Review source code before execution | RETAIN | Required |
| EXT-004 | Record source URL and version | RETAIN | Required for any approved dependency |
| EXT-005 | Record a cryptographic hash | RETAIN | Required for downloaded immutable artifacts |
| EXT-006 | Scan downloaded content | RETAIN | Use malware and secret scanning where applicable |
| EXT-007 | Execute unknown setup scripts with cloud credentials exposed | REJECT | Use quarantine and disposable environments |
| EXT-008 | Permit AI agents to auto-execute unknown repositories | REJECT | Manual review and authorization required |
| EXT-009 | Add optional enrichment after core validation | DEFER | Separate change request and test plan |
| EXT-010 | Publish unlicensed third-party content | REJECT | Do not redistribute without permission |

---

## 16. Environment-cleanup disposition

| ID | Historical objective | Disposition | Core-v2 treatment |
|---|---|---|---|
| CLN-001 | Stop or deallocate the VM | RETAIN | Immediate containment option |
| CLN-002 | Remove public TCP/3389 exposure | RETAIN | First emergency action |
| CLN-003 | Delete the resource group | RETAIN | Primary cleanup mechanism |
| CLN-004 | Assume deletion request equals completion | REJECT | Poll and verify final state |
| CLN-005 | Search for orphaned resources | RETAIN | Subscription-wide review required |
| CLN-006 | Review unattached disks and public IPs | RETAIN | Required |
| CLN-007 | Review NICs, NSGs, snapshots, and images | RETAIN | Required |
| CLN-008 | Remove temporary identities and assignments | RETAIN | Required |
| CLN-009 | Remove or disable analytics rules | RETAIN | Required according to retention decision |
| CLN-010 | Remove DCR associations | RETAIN | Required |
| CLN-011 | Decide workspace retention or deletion | ADAPT | Record telemetry and soft-delete decision |
| CLN-012 | Preserve evidence before safety actions | ADAPT | Preserve only when safe; safety has priority |
| CLN-013 | Review immediate cost | RETAIN | Preliminary view only |
| CLN-014 | Review cost after 24 hours | RETAIN | Delayed reconciliation |
| CLN-015 | Review cost after 72 hours | RETAIN | Final planned closure review |
| CLN-016 | Claim zero cost before settlement | REJECT | Use actual settled data |
| CLN-017 | Close the project with active resources | REJECT | No active unapproved resources may remain |
| CLN-018 | Document cleanup failures | RETAIN | Record and remediate exceptions |

---

## 17. Core artifact roadmap

| Artifact | Commit | Required result |
|---|---:|---|
| `docs/threat-model.md` | 1 | Authoritative boundary, risks, controls, gates, and stop conditions |
| `docs/architecture-overview.md` | 1 | Concise derivative architecture |
| `docs/scope-and-credibility-notes.md` | 1 | Status and claim boundaries |
| `docs/production-safe-design-contrast.md` | 1 | Lab-versus-production comparison |
| `docs/theory-to-lab-mapping.md` | 1 | Modernized theory mapping |
| `docs/checklist-to-project-roadmap.md` | 1 | Implementation roadmap and dispositions |
| `evidence/README.md` | 2 | Evidence-governance standard |
| `evidence/redaction-notes.md` | 2 | Operational redaction rules |
| `evidence/manifest.md` | 2 | Evidence index and metadata |
| `evidence/claim-evidence-matrix.md` | 2 | Public claim authorization |
| `evidence/templates/evidence-record-template.md` | 2 | Standard evidence record |
| `runbooks/cost-control-checklist.md` | 3 | Runtime, budget, ingestion, and cost closure |
| `runbooks/teardown-runbook.md` | 3 | Normal and emergency cleanup |
| `runbooks/rdp-authentication-triage.md` | 3 | Event-to-disposition investigation |
| `runbooks/deployment-runbook.md` | 3 | Phased deployment and validation |
| `kql/hunting-queries.md` | 4 | Versioned KQL catalog |
| `sentinel/analytics-rules-catalog.md` | 4 | `AR-001` specification and lifecycle |
| Root lab `README.md` | 5 | Status-aware public project overview |
| `reports/incident-timeline.md` | 5 | Incident chronology template |
| `reports/findings-report.md` | 5 | Findings template |
| `reports/remediation-recommendations.md` | 5 | Remediation template |
| `resume-assets/interview-talking-points.md` | 5 | Evidence-gated interview material |
| `resume-assets/resume-bullets.md` | 5 | Evidence-gated resume placeholders |
| `rubric/scoring-worksheet.md` | 5 | Validation and quality rubric |

---

## 18. Git milestone plan

### Commit 1 — Architecture, scope, and planning

Files:

- `docs/threat-model.md`
- `docs/architecture-overview.md`
- `docs/scope-and-credibility-notes.md`
- `docs/production-safe-design-contrast.md`
- `docs/theory-to-lab-mapping.md`
- `docs/checklist-to-project-roadmap.md`

Acceptance criteria:

- authoritative and derivative documents are clearly distinguished;
- design and execution status are accurate;
- Azure execution remains paused;
- public exposure remains unauthorized;
- no unsupported implementation claim exists;
- links and terminology are consistent;
- Markdown structure is valid;
- secret and privacy scans pass.

### Commit 2 — Evidence governance

Files:

- evidence governance standard;
- redaction notes;
- manifest;
- claim-evidence matrix;
- evidence-record template.

Acceptance criteria:

- public and private evidence boundaries are explicit;
- evidence IDs and statuses are defined;
- hashes and provenance are supported;
- failed tests remain traceable;
- claims cannot be promoted without evidence;
- secrets and identifiers are prohibited from public artifacts.

### Commit 3 — Operational runbooks

Files:

- cost control;
- teardown;
- renamed authentication-triage runbook;
- deployment runbook.

Acceptance criteria:

- normal and emergency modes are distinct;
- telemetry validation precedes exposure;
- safety has priority over evidence preservation;
- stop conditions map to response actions;
- cost closure includes immediate, 24-hour, and 72-hour checks;
- commands use preflight and explicit variables;
- no weak credentials or unsafe firewall instructions exist.

### Commit 4 — Detection engineering

Files:

- KQL query catalog;
- analytics-rule catalog.

Acceptance criteria:

- queries have stable IDs and lifecycle status;
- schema and health queries precede behavioral detections;
- Event IDs 4625 and 4624 are interpreted correctly;
- controlled-test queries exist;
- `AR-001` remains disabled until prerequisites pass;
- entity mappings depend on validated fields;
- no destructive automation exists.

### Commit 5 — Reporting and portfolio templates

Files:

- root lab README;
- report templates;
- resume and interview templates;
- scoring worksheet.

Acceptance criteria:

- unexecuted capabilities remain clearly labeled;
- templates do not fabricate results;
- resume bullets remain blocked;
- reports separate facts, interpretations, limitations, and recommendations;
- project status is consistent across all artifacts.

---

## 19. Static integration review

After Commit 5 content is ready, perform a repository-wide review.

Required checks include:

- Markdown whitespace validation;
- broken relative-link review;
- empty-file inventory;
- heading and table review;
- code-fence balance;
- duplicate stable-ID review;
- prohibited terminology scan;
- design-versus-validation terminology scan;
- path and renamed-file review;
- stale `rdp-bruteforce-triage.md` reference review;
- `rdp-authentication-triage.md` reference confirmation;
- secret scanning;
- private-course-content review;
- tenant, subscription, IP, email, and resource-ID review;
- Git status and intended-diff review.

No Azure work begins merely because static checks pass.

---

## 20. Predeployment authorization gate

Azure deployment remains blocked until every mandatory item is satisfied.

### 20.1 Repository state

- [ ] Correct branch is checked out.
- [ ] Working tree contains only intended changes.
- [ ] Commit milestones are complete.
- [ ] No private source files are tracked.
- [ ] No empty mandatory core artifact remains.
- [ ] Relative links are valid.
- [ ] Secret scans pass.
- [ ] Evidence and redaction controls exist.
- [ ] Public claims remain design-stage only.

### 20.2 Identity and subscription

- [ ] Intended tenant is confirmed.
- [ ] Intended subscription ID is confirmed privately.
- [ ] Operator identity is confirmed.
- [ ] MFA is enabled.
- [ ] Required privilege is documented.
- [ ] Excessive role assignments are not required.
- [ ] No cloud credential will be stored on the VM.
- [ ] No production credential will be used.

### 20.3 Cost

- [ ] Maximum approved total cost is recorded.
- [ ] Maximum VM runtime is recorded.
- [ ] Maximum public-exposure duration is recorded.
- [ ] Budget notifications are configured or intentionally documented.
- [ ] Budget alerts are not treated as hard stops.
- [ ] Log-ingestion cost assumptions are recorded.
- [ ] Optional paid services remain disabled unless approved.
- [ ] Teardown owner is available.
- [ ] Immediate, 24-hour, and 72-hour cost reviews are scheduled.

### 20.4 Architecture and risk

- [ ] Threat model is approved.
- [ ] Current risk remains accurately recorded.
- [ ] Public exposure authorization remains separate from deployment authorization.
- [ ] No VNet peering is planned.
- [ ] No VPN or trusted route is planned.
- [ ] No sensitive data is planned.
- [ ] No unnecessary managed identity is planned.
- [ ] Stop conditions are understood.
- [ ] Emergency teardown is executable.

### 20.5 Operational readiness

- [ ] Deployment runbook is complete.
- [ ] Triage runbook is complete.
- [ ] Teardown runbook is complete.
- [ ] Cost checklist is complete.
- [ ] KQL schema and health queries are ready.
- [ ] Controlled-test procedure is ready.
- [ ] Evidence-record template is ready.
- [ ] Private evidence storage location is ready.
- [ ] Public sanitization workflow is ready.

### 20.6 Go/no-go record

The deployment decision must record:

- date and time;
- operator;
- branch and commit;
- intended subscription;
- intended region;
- planned runtime;
- maximum approved cost;
- approved resource scope;
- outstanding limitations;
- decision: GO or NO-GO.

A GO decision authorizes only initial deployment without public organic exposure.

---

## 21. Deployment phases

### Phase A — Azure context and project scope

Objectives:

- authenticate safely;
- confirm tenant and subscription;
- establish explicit variables;
- confirm naming and region;
- review cost and quotas;
- create the dedicated resource group.

Exit criteria:

- scope is correct;
- no unrelated resources are affected;
- evidence record exists;
- cost and teardown ownership remain valid.

### Phase B — Isolated infrastructure

Objectives:

- create the VNet and subnet;
- create the NSG without broad public RDP;
- create the public IP and NIC only where required;
- create the disposable Windows VM;
- confirm no peering, VPN, or trusted path;
- confirm no unnecessary managed identity.

Exit criteria:

- VM exists;
- exposure remains restricted;
- Windows Firewall is enabled;
- no sensitive data exists;
- architecture matches the approved design.

### Phase C — Monitoring foundation

Objectives:

- create or confirm the workspace;
- enable Sentinel on the intended workspace;
- install AMA;
- create the DCR;
- associate the DCR with the VM;
- validate collection health.

Exit criteria:

- agent and association are confirmed;
- expected telemetry arrives;
- health and freshness are acceptable;
- the actual destination table is known.

### Phase D — Schema validation

Objectives:

- generate controlled failed authentication;
- validate Event ID 4625;
- validate available logon-type fields;
- generate an approved successful test;
- validate Event ID 4624;
- record actual field names and values.

Exit criteria:

- remote-interactive context can be identified;
- source, account, host, and time fields are understood;
- query assumptions are corrected;
- no exposure beyond the controlled source is required.

### Phase E — Detection validation

Objectives:

- validate KQL query behavior;
- run a below-threshold test;
- run a threshold-matching test;
- create `AR-001`;
- verify alert behavior;
- verify incident-object behavior;
- record limitations and entity quality.

Exit criteria:

- expected and actual results are recorded;
- failed tests are preserved;
- the rule version is known;
- the rule is tuned conservatively;
- no compromise claim is made.

### Phase F — Conditional organic observation

Objectives:

- conduct a final exposure gate review;
- time-box public TCP/3389 exposure;
- keep the operator present;
- monitor telemetry, host state, cost, and stop conditions;
- distinguish organic activity from controlled testing.

Exit criteria:

- approved runtime ends, or
- a stop condition occurs, or
- the defined observation objective is met.

Organic activity is not guaranteed and is not required for core validation.

### Phase G — Triage and response

Objectives:

- review alerts and underlying events;
- classify controlled, benign, suspicious, undetermined, or unauthorized activity;
- correlate successful logons and follow-on activity;
- invoke containment when required;
- preserve evidence only when safe.

Exit criteria:

- each relevant alert has a disposition;
- response decisions are recorded;
- mandatory stop events are escalated to emergency teardown.

### Phase H — Closure

Objectives:

- remove public exposure;
- deallocate or delete the VM;
- remove temporary identities and monitoring objects;
- delete the resource group;
- verify orphaned resources;
- decide workspace and telemetry disposition;
- review immediate cost;
- complete 24-hour and 72-hour cost reviews;
- sanitize public evidence.

Exit criteria:

- no unapproved active resource remains;
- cost is reconciled;
- evidence is reviewed;
- claims are updated to the strongest supported level only.

---

## 22. Public-exposure gate

Public organic exposure requires a separate explicit GO decision after deployment and telemetry validation.

All conditions must pass:

- [ ] Correct subscription and resource group are confirmed.
- [ ] VM is disposable.
- [ ] No sensitive data exists.
- [ ] No trusted network connection exists.
- [ ] No unnecessary managed identity exists.
- [ ] Synthetic credentials are unique and strong.
- [ ] Windows Firewall is enabled.
- [ ] NSG contains only the approved TCP/3389 exposure.
- [ ] AMA is healthy.
- [ ] DCR association is correct.
- [ ] Expected table and fields are validated.
- [ ] Event ID 4625 controlled test passed.
- [ ] Event ID 4624 controlled test passed or has an approved documented exception.
- [ ] Telemetry freshness is acceptable.
- [ ] Health queries are running.
- [ ] Emergency RDP-rule removal is ready.
- [ ] VM deallocation or deletion is ready.
- [ ] Operator is present.
- [ ] Teardown owner is available.
- [ ] Runtime limit is recorded.
- [ ] Cost limit is recorded.
- [ ] Evidence workflow is ready.
- [ ] No unresolved high-risk deviation exists.

Isolation alone does not authorize exposure.

---

## 23. Mandatory stop conditions

Public exposure ends immediately when any applicable stop condition occurs.

| Condition | Immediate action |
|---|---|
| Unexplained successful remote-interactive access | Remove TCP/3389 exposure and deallocate the VM |
| Suspicious persistence or process execution | Remove exposure and deallocate or delete |
| Unexpected privilege or account change | Remove exposure and review identity state |
| Windows Firewall disabled or altered unexpectedly | Remove exposure |
| NSG differs from approved design | Remove or correct exposure |
| AMA or telemetry failure | Remove exposure |
| DCR association failure | Remove exposure |
| Unexpected outbound activity | Remove exposure and deallocate |
| Loss of operator control | Deallocate or delete |
| Unapproved identity or role assignment | Stop and review control-plane activity |
| Runtime limit reached | Begin normal teardown |
| Cost or ingestion limit reached | Stop compute and begin teardown |
| Operator or teardown owner unavailable | Remove exposure and stop compute |
| Evidence leakage suspected | Stop publication and begin exposure review |
| Trusted connectivity discovered | Remove exposure and isolate immediately |

Safety actions take priority over obtaining additional screenshots or logs.

---

## 24. Controlled-test catalog

| Test ID | Test objective | Expected result |
|---|---|---|
| TST-001 | Confirm tenant and subscription context | Intended scope is recorded |
| TST-002 | Confirm resource-group isolation | Only project resources are present |
| TST-003 | Confirm no peering or trusted route | No trusted connectivity exists |
| TST-004 | Confirm NSG setup state | No broad public exposure exists |
| TST-005 | Confirm Windows Firewall | Firewall is enabled |
| TST-006 | Confirm AMA installation | Intended VM reports healthy state |
| TST-007 | Confirm DCR association | Intended DCR is associated with the VM |
| TST-008 | Confirm telemetry health | Current events or heartbeat are visible |
| TST-009 | Confirm destination table | Actual table is recorded |
| TST-010 | Validate controlled Event ID 4625 | Failed event and required fields appear |
| TST-011 | Validate remote-interactive context | Logon type or equivalent context is confirmed |
| TST-012 | Validate controlled Event ID 4624 | Successful event and required fields appear |
| TST-013 | Run below-threshold detection test | No `AR-001` alert is expected |
| TST-014 | Run threshold-matching detection test | Detection result is expected |
| TST-015 | Validate alert creation | Alert links to underlying events |
| TST-016 | Validate incident-object behavior | Case object is created or correlated as configured |
| TST-017 | Validate controlled-versus-organic separation | Test-session metadata supports classification |
| TST-018 | Validate exposure-removal procedure | RDP exposure can be removed promptly |
| TST-019 | Validate VM containment procedure | VM can be deallocated or deleted promptly |
| TST-020 | Validate evidence sanitization | Public artifact passes review and scanning |
| TST-021 | Validate resource-group deletion | Deletion reaches terminal state |
| TST-022 | Validate orphan search | No unapproved residual resource remains |
| TST-023 | Validate immediate cost review | Preliminary cost is recorded |
| TST-024 | Validate 24-hour cost review | Delayed cost is recorded |
| TST-025 | Validate 72-hour cost closure | Final planned cost review is recorded |

---

## 25. Evidence requirements

Every material implementation or validation claim requires:

- a stable evidence ID;
- date and time;
- operator;
- environment and scope;
- artifact or control under test;
- configuration or query version;
- expected result;
- actual result;
- pass, fail, partial, or blocked status;
- evidence location;
- sanitization status;
- limitations;
- related claim IDs.

Evidence may include:

- command output;
- sanitized portal screenshots;
- KQL result exports;
- configuration summaries;
- test-session records;
- alert and incident summaries;
- teardown output;
- resource inventories;
- cost records;
- hashes of immutable artifacts.

A screenshot without context is not sufficient evidence.

---

## 26. Change control

A change request is required when proposing:

- a new Azure service;
- a paid security plan;
- Bastion, VPN, JIT, Firewall, or private endpoints;
- VNet peering or trusted connectivity;
- a managed identity;
- broader data collection;
- longer retention;
- VNet flow logs;
- third-party enrichment;
- an external script;
- automated response;
- infrastructure as code;
- CI/CD deployment;
- multiple VMs;
- another operating system;
- a second cloud;
- production use;
- sensitive data;
- a longer exposure period.

The change review must address:

1. purpose;
2. architecture impact;
3. threat-model impact;
4. privilege impact;
5. network impact;
6. telemetry impact;
7. privacy impact;
8. evidence impact;
9. cost impact;
10. teardown impact;
11. test plan;
12. approval decision.

---

## 27. Deferred expansion backlog

The following may be evaluated after core-v2 completion:

- Azure Bastion comparison deployment;
- just-in-time VM access;
- VNet flow logs;
- Defender for Servers;
- Microsoft Defender for Endpoint integration;
- Sentinel workbook;
- sanitized geolocation visualization;
- automation rules;
- approval-gated Logic App response;
- Terraform implementation;
- Bicep implementation;
- GitHub Actions validation;
- policy-as-code;
- multi-VM credential-spraying simulation;
- Linux SSH authentication variant;
- Azure identity-detection expansion;
- multi-cloud detection integration.

Deferred work must not delay the core objective or be presented as already implemented.

---

## 28. Rejected implementation patterns

The following patterns remain prohibited:

- allow-all inbound NSG rules;
- disabling Windows Firewall;
- weak or reused credentials;
- hardcoded secrets;
- production credentials on the VM;
- sensitive data on the VM;
- trusted-network connectivity;
- unattended public exposure;
- broad paid-plan enablement;
- unknown third-party scripts executed without review;
- destructive automated response;
- unsupported malware detonation;
- hack-back;
- source-infrastructure interaction;
- APT attribution;
- GeoIP-based identity claims;
- using Event ID 4625 alone as proof of RDP attack;
- using an alert as proof of compromise;
- copying private course material into Git;
- claiming compliance or authorization;
- claiming cleanup before orphan and delayed-cost review.

---

## 29. Project completion gate

The project may be described as **Completed** only when every applicable requirement passes.

### Architecture and documentation

- [ ] All five milestone commits are complete.
- [ ] Cross-document terminology is consistent.
- [ ] Threat model reflects final implementation.
- [ ] Architecture reflects final implementation.
- [ ] Claim boundaries reflect final evidence.
- [ ] No mandatory artifact remains empty.
- [ ] No broken relative link remains.

### Implementation

- [ ] Intended Azure resources were created.
- [ ] Resource scope was correct.
- [ ] No trusted connectivity was introduced.
- [ ] No unnecessary identity was introduced.
- [ ] Windows Firewall remained enabled.
- [ ] AMA and DCR were implemented.
- [ ] Actual telemetry destination was documented.
- [ ] Sentinel was configured as intended.

### Validation

- [ ] Telemetry health was validated.
- [ ] Schema was validated.
- [ ] Controlled Event ID 4625 test was completed.
- [ ] Remote-interactive context was validated.
- [ ] Controlled Event ID 4624 test was completed or exception documented.
- [ ] Below-threshold test was completed.
- [ ] Threshold-matching test was completed.
- [ ] Alert behavior was validated.
- [ ] Incident-object behavior was validated.
- [ ] Triage disposition was documented.
- [ ] Failed tests remain traceable.

### Safety and response

- [ ] Public exposure was explicitly authorized.
- [ ] Exposure remained within the approved duration.
- [ ] Operator presence was maintained.
- [ ] No unresolved stop condition remained.
- [ ] Exposure removal was validated.
- [ ] VM containment was validated.
- [ ] Emergency actions were documented where applicable.

### Evidence and reporting

- [ ] Evidence manifest is complete.
- [ ] Claim-evidence matrix is complete.
- [ ] Public evidence is sanitized.
- [ ] Secret scans pass.
- [ ] Private identifiers are excluded.
- [ ] Findings report is complete.
- [ ] Timeline is complete where applicable.
- [ ] Remediation recommendations are complete.
- [ ] Limitations and failed tests are disclosed.

### Cleanup and cost

- [ ] Public exposure was removed.
- [ ] VM was deallocated or deleted.
- [ ] Resource-group deletion completed.
- [ ] Orphan search completed.
- [ ] Temporary identities and assignments were removed.
- [ ] DCR and analytics objects were dispositioned.
- [ ] Workspace and telemetry disposition was documented.
- [ ] Immediate cost review completed.
- [ ] 24-hour cost review completed.
- [ ] 72-hour cost review completed.
- [ ] No unapproved active resource remains.

### Claims

- [ ] README reflects actual status.
- [ ] No design claim is presented as implementation.
- [ ] No implementation claim is presented as validation.
- [ ] No alert is presented as compromise.
- [ ] No framework mapping is presented as compliance.
- [ ] No IP or GeoIP result is presented as human attribution.
- [ ] Resume bullets are supported by evidence.
- [ ] Interview talking points are supported by evidence.

---

## 30. Current roadmap decision

Current decision:

| Decision area | State |
|---|---|
| Continue Commit 1 documentation | GO |
| Stage Commit 1 files | Not yet |
| Begin Commit 2 after Commit 1 review | Conditional GO |
| Begin Azure deployment | NO-GO |
| Authorize public TCP/3389 exposure | NO-GO |
| Claim v2 implementation | Not permitted |
| Claim v2 validation | Not permitted |
| Claim project completion | Not permitted |

The immediate next action after this file is validated is a cross-file review of all six Commit 1 documents before staging.

---

## 31. Related documents

| Document | Purpose |
|---|---|
| [`threat-model.md`](threat-model.md) | Authoritative risks, controls, gates, and stop conditions |
| [`architecture-overview.md`](architecture-overview.md) | Concise architecture |
| [`scope-and-credibility-notes.md`](scope-and-credibility-notes.md) | Status and public-claim boundaries |
| [`production-safe-design-contrast.md`](production-safe-design-contrast.md) | Lab-versus-production comparison |
| [`theory-to-lab-mapping.md`](theory-to-lab-mapping.md) | Theory and framework mapping |
| `../evidence/README.md` | Evidence-governance standard |
| `../evidence/claim-evidence-matrix.md` | Claim authorization |
| `../runbooks/deployment-runbook.md` | Deployment procedure |
| `../runbooks/rdp-authentication-triage.md` | Authentication investigation |
| `../runbooks/teardown-runbook.md` | Normal and emergency cleanup |
| `../runbooks/cost-control-checklist.md` | Cost controls |
| `../kql/hunting-queries.md` | Query catalog |
| `../sentinel/analytics-rules-catalog.md` | Analytics-rule lifecycle |
