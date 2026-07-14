# Production-Safe Design Contrast

| Field | Value |
|---|---|
| Project | Sentinel RDP Honeypot v2 |
| Document role | Lab-versus-production architecture comparison |
| Authoritative boundary and risk source | [`threat-model.md`](threat-model.md) |
| Design status | Approved baseline |
| Execution status | Not started |
| Azure deployment status | Paused |
| Production implementation claim | None |
| Last updated | 2026-07-13 |

> This document compares the intentionally exposed lab design with safer production administration patterns. It does not assert that the production alternatives have been deployed, validated, purchased, or formally approved.

---

## 1. Purpose and authority

Sentinel RDP Honeypot v2 intentionally uses a bounded public-exposure pattern to produce authentication telemetry for a controlled Microsoft Sentinel detection-engineering exercise.

That pattern is not the recommended default for production administration.

This document:

- explains why the lab accepts limited exposure;
- identifies safeguards that bound the lab risk;
- contrasts the lab with production-oriented access, identity, network, monitoring, response, and governance practices;
- prevents the project from being represented as a production-ready public RDP architecture;
- identifies production alternatives without expanding the core-v2 deployment scope.

The authoritative system boundary, risk register, exposure prerequisites, and mandatory stop conditions remain in [`threat-model.md`](threat-model.md).

---

## 2. Current project status

| Capability | Status |
|---|---|
| Lab architecture | Approved design |
| Production comparison | Approved design |
| Azure v2 deployment | Not started |
| Public RDP exposure | Not authorized |
| Azure Bastion | Not planned for core v2 |
| VPN or private administration path | Not planned for core v2 |
| Just-in-time VM access | Deferred |
| Enterprise identity integration | Out of scope |
| Production monitoring | Out of scope |
| Production continuity design | Out of scope |

No statement in this file should be interpreted as evidence that a production alternative has been implemented.

---

## 3. Core distinction

### Intentionally exposed detection lab

The lab temporarily permits public TCP/3389 traffic to one disposable Windows VM so that controlled and potentially unsolicited authentication telemetry can be collected and analyzed.

The lab is acceptable only when:

- exposure is time limited;
- an operator is present;
- the VM contains no production or sensitive data;
- credentials are synthetic and nonreused;
- there is no trusted-network connectivity;
- monitoring is validated before exposure;
- mandatory stop and teardown controls are ready;
- the environment can be destroyed without business impact.

### Production administration

A production administration design should ordinarily minimize or eliminate direct public management-port exposure.

Administration should instead use one or more governed access paths such as:

- Azure Bastion;
- a VPN or other private network connection;
- an approved administrative jump path;
- tightly restricted source networks;
- just-in-time access;
- organization-managed privileged identity;
- formal authorization, monitoring, and change controls.

These are comparison options, not core-v2 deployment requirements.

---

## 4. Side-by-side design comparison

| Design area | Sentinel RDP Honeypot v2 | Production-oriented administration |
|---|---|---|
| Primary objective | Generate and analyze bounded authentication telemetry | Administer workloads while minimizing attack surface |
| Workload value | Disposable lab VM | Business or operational workload |
| Public exposure | Time-boxed TCP/3389 after explicit approval | Avoid routine direct public RDP |
| Public IP | Required by the approved lab pattern | Prefer no public IP on administered VM |
| Exposure duration | Hours or other explicitly approved short interval | Closed by default; opened only when justified |
| Operator presence | Required throughout exposure | Managed through operational ownership and support coverage |
| Inbound rules | Only approved TCP/3389 | Private path, Bastion, VPN, JIT, or tightly restricted sources |
| Source scope | May allow unsolicited internet traffic for observation | Named administrative sources or managed access service |
| Network trust | No peering, VPN, or trusted route | Deliberately designed network zones and approved connectivity |
| Outbound access | Required monitoring paths plus documented residual egress | Explicit egress policy, firewalling, inspection, and service dependencies |
| Credentials | Unique synthetic local credentials | Organization-managed identity and privileged access |
| Credential reuse | Prohibited | Prohibited and governed through enterprise controls |
| Managed identity | Not required for core v2 | Used only where the workload requires it and with least privilege |
| Privilege | Minimum needed for the controlled test | Role-based and task-specific privileged administration |
| MFA | Protects the Azure operator identity | Expected for administrative access and privileged workflows |
| Host contents | No production, personal, or regulated data | Business data handled according to classification and policy |
| Availability | Destruction is acceptable | Availability, recovery, and continuity requirements apply |
| Patch state | Recorded and reviewed for the lab interval | Governed patch, vulnerability, and lifecycle management |
| Windows Firewall | Must remain enabled | Centrally governed host firewall policy |
| Telemetry | Selected Windows Security Events through AMA/DCR | Broader validated logging based on production risks |
| Detection | One bounded scheduled rule plus hunting queries | Multiple tested detections with ownership and lifecycle management |
| Alert coverage | Limited to the documented lab objective | Coverage aligned to organizational threat scenarios |
| Triage | One-person bounded runbook | Assigned roles, escalation, handoffs, and service-level expectations |
| Incident authority | Personal lab decision | Formal incident-management authority |
| Response | Manual or approval-gated containment | Governed automated and manual response procedures |
| Compromise recovery | Deallocate or destroy disposable environment | Containment, forensics, recovery, continuity, and lessons learned |
| Evidence | Private raw evidence and sanitized public artifacts | Protected audit, incident, legal, and operational records |
| Retention | Short and purpose limited | Policy, legal, operational, and regulatory requirements |
| Cost control | Small budget, strict runtime, teardown | Forecasting, allocation, optimization, and ongoing ownership |
| Change management | Project-specific gates and milestone review | Formal change, approval, testing, rollback, and audit processes |
| Compliance | No compliance claim | Compliance assessed through organizational governance |
| Authorization | Personal conditional go/no-go decision | Formal organizational risk acceptance where required |
| Cleanup | Resource-group deletion plus orphan and cost checks | Controlled decommissioning and records-management process |

---

## 5. Public RDP exposure

### Lab decision

Public TCP/3389 exposure exists solely to support the bounded telemetry objective.

It is permitted only after:

- the subscription and resource scope are confirmed;
- the VM has no trusted connectivity;
- Windows Firewall remains enabled;
- no unnecessary identity exists;
- AMA and DCR collection are validated;
- `SecurityEvent` schema and freshness are validated;
- emergency exposure removal and deallocation are ready;
- runtime and cost limits are approved;
- the operator and teardown owner are available.

### Production decision

Routine direct RDP exposure to the internet should not be treated as the preferred production administration model.

A production design should reduce exposure through:

- removal of the VM public IP where feasible;
- Azure Bastion or another approved management gateway;
- VPN or private-network access;
- source-restricted administrative access;
- just-in-time access;
- centralized identity and privileged-access controls;
- monitored and approved administrative sessions.

The lab does not demonstrate that public RDP has been made production safe.

---

## 6. Administrative access alternatives

### 6.1 Azure Bastion

Production use case:

- connect to Azure VMs without assigning a public IP to each target VM;
- provide RDP or SSH through a managed access service;
- reduce direct exposure of workload management ports.

Core-v2 disposition:

> Deferred and not required for the intentionally exposed detection experiment.

Reason:

- Bastion would materially change the lab’s public authentication-observation objective;
- it may introduce unnecessary cost and scope;
- it belongs in the production comparison, not the core deployment.

### 6.2 VPN or private network access

Production use case:

- establish encrypted administrative connectivity into the Azure virtual network;
- keep target management interfaces off the public internet;
- connect approved administrative networks to private VM addresses.

Core-v2 disposition:

> Rejected for the exposed observation path and deferred as an optional comparison design.

Reason:

- trusted or private connectivity would contradict the isolation requirement;
- it could create an unintended pivot path from the disposable VM;
- the core lab must not connect to personal, family, employer, or customer networks.

### 6.3 Just-in-time VM access

Production use case:

- keep management ports closed by default;
- authorize a defined source, port, and duration only when administration is required;
- reduce standing exposure.

Core-v2 disposition:

> Deferred.

Reason:

- core v2 already uses a separately governed short exposure window;
- JIT may require a paid Defender for Cloud plan or added configuration;
- the project should not enable paid services without explicit approval and cost review.

### 6.4 Source-IP restriction

Production use case:

- limit management traffic to approved administrative sources;
- reduce unsolicited internet exposure.

Core-v2 disposition:

> Used for controlled-test periods where appropriate, but not as the sole organic-observation model.

Limitation:

- residential or dynamic IP addresses can change;
- source restriction does not replace identity, host, logging, and response controls;
- unrestricted organic observation must remain separately time boxed and approved.

### 6.5 Administrative jump host

Production use case:

- centralize administrative entry;
- monitor privileged sessions;
- separate administrative tooling from ordinary user workstations.

Core-v2 disposition:

> Out of scope.

A jump host would add another workload, identity surface, cost, patching requirement, and cleanup path.

---

## 7. Identity and privileged access

### Lab identity model

Core v2 uses:

- one MFA-protected Azure operator identity;
- one or more synthetic Windows accounts explicitly created for testing;
- no reusable production credentials;
- no unnecessary VM managed identity;
- no cloud credentials stored on the VM;
- no permanent privileged test identity.

### Production identity model

A production design should use:

- organization-managed identities;
- MFA for privileged access;
- role-based authorization;
- time-bound or approval-based elevation where appropriate;
- separate administrative and ordinary user contexts;
- privileged-access review;
- credential rotation and revocation;
- auditable ownership.

The lab does not demonstrate enterprise IAM, Conditional Access, Privileged Identity Management, or production account governance.

---

## 8. Network segmentation and pivot prevention

### Lab network

The disposable VM must have:

- a dedicated VNet and subnet;
- no VNet peering;
- no VPN or ExpressRoute connectivity;
- no trusted route;
- no access to other cloud projects;
- no shared credentials;
- no production service dependency.

The lab prioritizes isolation from valuable assets over production connectivity.

### Production network

A production network may use:

- segmented workload and management subnets;
- hub-and-spoke or other governed topology;
- controlled peering;
- centralized firewalls;
- private DNS;
- approved service endpoints or private endpoints;
- inspected egress;
- governed routes.

Those production mechanisms require their own architecture and risk analysis. They must not be added casually to the disposable lab.

---

## 9. Outbound connectivity

### Lab requirement

AMA requires access to the relevant Azure monitoring endpoints. Therefore, the lab must not claim that all outbound traffic is denied while monitoring operates.

The lab must:

- document required monitoring connectivity;
- restrict unrelated egress where feasible;
- maintain no trusted-network path;
- treat unexpected outbound activity as a mandatory stop condition;
- deallocate or delete the VM when harmful outbound behavior is suspected.

### Production requirement

A production design should identify:

- approved destinations;
- required service tags or private access paths;
- firewall and proxy policy;
- DNS controls;
- threat inspection;
- logging;
- exception ownership;
- response procedures.

“Outbound access works” is not equivalent to an approved egress design.

---

## 10. Monitoring and detection

### Lab monitoring

Core v2 is limited to:

- Windows Security Events selected through AMA and a DCR;
- `SecurityEvent` schema and field validation;
- `Heartbeat`, where available;
- repeated failed remote-interactive authentication;
- successful-logon correlation during triage;
- selected post-authentication hunting where telemetry permits;
- one scheduled analytics rule, `AR-001`.

### Production monitoring

A production design normally requires broader coverage, potentially including:

- endpoint detection and response;
- host health and configuration telemetry;
- identity events;
- network and egress telemetry;
- vulnerability and patch status;
- control-plane activity;
- application and workload logs;
- rule-health monitoring;
- on-call or assigned operational ownership;
- detection tuning and lifecycle management.

Core v2 does not claim complete endpoint, identity, network, or cloud control-plane detection.

---

## 11. Response and recovery

### Lab response

When a mandatory stop condition occurs:

1. remove TCP/3389 exposure;
2. deallocate or delete the VM;
3. preserve only safely obtainable evidence;
4. review Azure identities and control-plane changes;
5. execute emergency teardown;
6. verify orphan-resource and cost closure.

Destruction is an acceptable recovery method because the workload is disposable.

### Production response

A production response may require:

- business-impact assessment;
- coordinated containment;
- forensic preservation;
- identity revocation;
- legal or compliance notification;
- recovery from trusted images or backups;
- service restoration;
- continuity management;
- post-incident review;
- corrective-action tracking.

The lab does not simulate the full organizational authority or consequence of production incident response.

---

## 12. Availability and continuity

### Lab

- no availability commitment;
- no production dependency;
- no high-availability architecture;
- no load balancing;
- no zone redundancy;
- no disaster-recovery requirement;
- deletion is expected.

### Production

Availability decisions may require:

- defined recovery-time and recovery-point objectives;
- backup and restoration;
- redundancy;
- zone or regional architecture;
- maintenance windows;
- failover;
- continuity testing;
- capacity planning.

These controls are intentionally excluded from core v2.

---

## 13. Evidence and records

### Lab evidence

The project separates:

- private raw security and infrastructure evidence;
- working redaction copies;
- sanitized public portfolio artifacts.

Public evidence must not expose:

- credentials;
- tenant or subscription IDs;
- full source IP addresses;
- active endpoints;
- unnecessary resource IDs;
- personal information;
- private course materials.

### Production records

Production records may additionally require:

- legal holds;
- formal chain of custody;
- audit retention;
- access logging;
- records classification;
- evidence-handling procedures;
- regulatory or contractual retention.

Core v2 evidence governance is designed for trustworthy portfolio validation, not legal forensic certification.

---

## 14. Cost and service selection

### Lab

Cost controls include:

- a project budget;
- actual and forecast alerts;
- maximum approved cost;
- absolute runtime;
- smallest suitable resources;
- limited log collection;
- explicit approval for optional paid services;
- normal and emergency teardown;
- delayed cost reconciliation.

### Production

Production cost management may include:

- forecasting;
- chargeback or allocation;
- reserved capacity;
- service-level tradeoffs;
- licensing;
- security-plan coverage;
- long-term retention;
- optimization;
- capacity and continuity costs.

The cheapest lab design is not automatically the safest or most appropriate production design.

---

## 15. Core-v2 deferred production controls

The following are intentionally not required for core v2:

| Control or service | Core-v2 disposition | Reason |
|---|---|---|
| Azure Bastion | Deferred | Changes the public-observation path and adds scope or cost |
| VPN Gateway | Deferred | Creates a trusted administration path unnecessary for the lab |
| ExpressRoute | Out of scope | Enterprise connectivity not relevant to a personal disposable lab |
| Just-in-time VM access | Deferred | Optional production comparison and possible paid dependency |
| Privileged Identity Management | Out of scope | Enterprise identity governance beyond core lab scope |
| Conditional Access design | Out of scope | Enterprise identity policy beyond synthetic local accounts |
| Defender for Servers paid plan | Deferred | Enable only with a named use case and cost approval |
| Production EDR deployment | Deferred | Broader endpoint project |
| Central Azure Firewall | Out of scope | Disproportionate scope and cost for one disposable VM |
| Hub-and-spoke networking | Out of scope | Enterprise topology beyond the isolated lab |
| Long-term telemetry retention | Rejected for core v2 | Unnecessary cost and privacy exposure |
| Automated destructive response | Rejected | Requires additional authorization and safety engineering |
| High availability | Rejected | Disposable workload has no availability requirement |
| Production backup | Rejected | VM destruction is the intended closure path |

Deferred does not mean unimportant. It means the capability is not required to validate the core-v2 objective.

---

## 16. Controls retained in both designs

Despite their different objectives, both the lab and a production design should retain these principles:

- explicit ownership;
- least privilege;
- MFA for administrative access;
- no shared or reused credentials;
- network segmentation;
- host firewall enforcement;
- validated logging;
- defined monitoring ownership;
- response procedures;
- evidence integrity;
- cost ownership;
- controlled changes;
- cleanup or decommissioning;
- documented residual risk;
- honest claims about control effectiveness.

The implementation depth differs, but the principles remain applicable.

---

## 17. Migration thought experiment

Converting the lab into a safer production administration pattern would require a new design—not merely leaving the VM running.

At minimum, the redesign would need to consider:

1. removing routine public TCP/3389 access;
2. removing the target VM public IP;
3. selecting Bastion, VPN, or another governed private access method;
4. replacing synthetic local credentials with managed organizational identity;
5. defining privileged-access and MFA requirements;
6. implementing approved segmentation and egress controls;
7. expanding endpoint, identity, network, and control-plane monitoring;
8. defining patching, vulnerability, backup, availability, and recovery requirements;
9. establishing operational ownership and escalation;
10. defining retention, privacy, and compliance obligations;
11. conducting production risk acceptance and authorization;
12. completing performance, security, failure, and recovery testing.

The current lab should never be promoted directly into production.

---

## 18. Credibility boundaries

Approved statement:

> The project contrasts an intentionally exposed, disposable authentication-monitoring lab with safer production administration patterns.

Not approved:

- implemented a production Azure RDP architecture;
- hardened public RDP for production;
- deployed enterprise Bastion and JIT controls;
- built production privileged-access management;
- created a compliant administration environment;
- proved public RDP is safe;
- demonstrated enterprise incident response.

Production alternatives may be discussed as architecture reasoning, not claimed as implemented controls.

---

## 19. Review triggers

Review this comparison when:

- the public exposure model changes;
- Bastion, VPN, JIT, Defender for Cloud, or a jump host is proposed;
- managed identity or Entra identity is added;
- VNet peering or trusted connectivity is proposed;
- egress controls change;
- production deployment is suggested;
- the lab becomes long lived;
- sensitive data or a business dependency is proposed;
- availability or backup requirements appear;
- automated response is proposed;
- a public portfolio claim references production readiness.

Any such change also requires a threat-model review.

---

## 20. Definition of done

This comparison is complete when:

- [ ] The lab objective is distinguished from production administration.
- [ ] Direct public RDP is not represented as the production default.
- [ ] Bastion, VPN, JIT, and source restriction are described as options, not core-v2 requirements.
- [ ] Identity and privileged-access differences are explicit.
- [ ] Network and egress differences are explicit.
- [ ] Monitoring, response, evidence, availability, and cost differences are explicit.
- [ ] Deferred production controls are identified.
- [ ] The lab cannot be mistaken for a production-ready architecture.
- [ ] No production alternative is described as implemented.
- [ ] The authoritative threat model remains controlling.

---

## 21. Related documents

| Document | Purpose |
|---|---|
| [`threat-model.md`](threat-model.md) | Authoritative system boundary, risk, controls, and stop conditions |
| [`architecture-overview.md`](architecture-overview.md) | Concise approved lab architecture |
| [`scope-and-credibility-notes.md`](scope-and-credibility-notes.md) | Public claim boundaries |
| [`theory-to-lab-mapping.md`](theory-to-lab-mapping.md) | Theory and framework relationships |
| [`checklist-to-project-roadmap.md`](checklist-to-project-roadmap.md) | Scope and definition of done |
| `../runbooks/deployment-runbook.md` | Deployment and controlled testing |
| `../runbooks/rdp-authentication-triage.md` | Authentication investigation and response |
| `../runbooks/teardown-runbook.md` | Normal and emergency cleanup |
| `../runbooks/cost-control-checklist.md` | Runtime, budget, and cost closure |

---

## 22. Reference context

This comparison is informed by current Microsoft guidance covering:

- production risk from direct public RDP exposure;
- Azure Bastion connectivity without a public IP on the target VM;
- just-in-time VM access for reducing standing management-port exposure;
- private connectivity through Azure VPN Gateway;
- Azure shared-responsibility principles.

The project-specific decisions and deferred-scope choices remain owned by this repository.
