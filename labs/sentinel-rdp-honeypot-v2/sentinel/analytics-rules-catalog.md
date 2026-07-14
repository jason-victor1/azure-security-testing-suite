# Sentinel RDP Honeypot v2 Analytics-Rule Catalog

## 1. Purpose

This catalog defines the initial Microsoft Sentinel scheduled analytics-rule specification for Sentinel RDP Honeypot v2.

The core detection artifact is `AR-001`, a disabled design-stage scheduled rule that uses the canonical `Q-020` query to identify repeated failed remote-interactive authentication from one source to one Windows host.

This catalog separates:

- query design from live execution;
- rule lifecycle from deployment state;
- a detection result from an alert;
- an alert from a Microsoft Sentinel incident object;
- a true-positive detection from successful compromise;
- entity-mapping design from validated field quality; and
- rule implementation from rule validation.

This file does not prove that `AR-001` was created, enabled, executed, tuned, or validated.

## 2. Current status and authorization boundary

At this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection and incident validation have not started.
- `SecurityEvent` routing and field population remain unvalidated.
- `Q-001` through `Q-020` are reviewed but unexecuted.
- `AR-001` is draft, disabled, undeployed, and unverified.
- No v2 alert or incident object exists.
- No automated response is configured.
- No implementation, validation, compromise, or completion claim is authorized.

Static review of this catalog does not authorize rule creation or Azure execution.

## 3. Governing documents

Use this catalog with:

1. [`../docs/threat-model.md`](../docs/threat-model.md);
2. [`../docs/scope-and-credibility-notes.md`](../docs/scope-and-credibility-notes.md);
3. [`../docs/checklist-to-project-roadmap.md`](../docs/checklist-to-project-roadmap.md);
4. [`../kql/hunting-queries.md`](../kql/hunting-queries.md);
5. [`../runbooks/deployment-runbook.md`](../runbooks/deployment-runbook.md);
6. [`../runbooks/rdp-authentication-triage.md`](../runbooks/rdp-authentication-triage.md);
7. [`../runbooks/teardown-runbook.md`](../runbooks/teardown-runbook.md);
8. [`../runbooks/cost-control-checklist.md`](../runbooks/cost-control-checklist.md);
9. [`../evidence/README.md`](../evidence/README.md);
10. [`../evidence/redaction-notes.md`](../evidence/redaction-notes.md);
11. [`../evidence/claim-evidence-matrix.md`](../evidence/claim-evidence-matrix.md); and
12. [`../evidence/templates/evidence-record-template.md`](../evidence/templates/evidence-record-template.md).

When this specification conflicts with observed schema, test results, platform behavior, or evidence, keep the rule disabled and use the weaker supported claim.

## 4. Technical reference basis

The catalog is designed around the following Microsoft Sentinel behavior:

- Scheduled analytics rules run KQL at a configured frequency over a configured lookback period.
- Query frequency must not exceed the query period.
- Query results can create alerts, and alert settings can create Microsoft Sentinel incident objects.
- Event grouping determines whether results are grouped into one alert or represented as separate alerts.
- Entity mapping enriches alerts and incidents but depends on reliable projected query fields.
- Incident grouping, suppression, automation rules, and playbooks are separate design decisions.
- Scheduled-rule execution and health can be monitored through Microsoft Sentinel health and audit features when those features are enabled.

Official references:

- [Create scheduled analytics rules](https://learn.microsoft.com/en-us/azure/sentinel/create-analytics-rules)
- [Scheduled analytics-rule configuration](https://learn.microsoft.com/en-us/azure/sentinel/scheduled-rules-overview)
- [Map data fields to entities](https://learn.microsoft.com/en-us/azure/sentinel/map-data-fields-to-entities)
- [Entities in Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/entities)
- [Monitor analytics-rule integrity](https://learn.microsoft.com/en-us/azure/sentinel/monitor-analytics-rule-integrity)
- [Troubleshoot analytics rules](https://learn.microsoft.com/en-us/azure/sentinel/troubleshoot-analytics-rules)

## 5. Lifecycle and deployment-state model

Lifecycle status and deployment state are separate.

| Field | Allowed values | Meaning |
|---|---|---|
| Lifecycle status | `Draft`, `Reviewed`, `Validated`, `Tuned`, `Retired` | Maturity of the rule specification |
| Deployment state | `Blocked`, `Disabled`, `Enabled`, `Deleted`, `Unknown` | State of a live rule in the approved workspace |
| Test outcome | `Not Run`, `Passed`, `Failed`, `Blocked`, `Inconclusive` | Result of a defined rule test |

Current values:

| Field | Current value |
|---|---|
| Lifecycle status | `Reviewed` |
| Deployment state | `Blocked` |
| Test outcome | `Not Run` |
| Configuration version | `1.0.0` |
| Query ID | `Q-020` |
| Query version | `1.0.0` |

`Reviewed` means the specification was statically reviewed. It does not mean the rule exists or works.

## 6. Rule catalog index

| ID | Name | Type | Lifecycle | Deployment | Enabled |
|---|---|---|---|---|---|
| `AR-001` | Repeated failed remote-interactive authentication | Scheduled | Reviewed | Blocked | No |

Only one scheduled analytics rule is in core v2.

---

## 7. AR-001 — Repeated failed remote-interactive authentication

### 7.1 Identity

| Field | Value |
|---|---|
| Rule ID | `AR-001` |
| Display name | `Sentinel RDP Honeypot v2 - Repeated Failed Remote-Interactive Authentication` |
| Description | Detects repeated Event ID 4625 remote-interactive authentication failures from one source IP to one Windows host within a bounded period. |
| Rule type | Scheduled |
| Configuration version | `1.0.0` |
| Canonical query | `Q-020` version `1.0.0` |
| Lifecycle status | `Reviewed` |
| Deployment state | `Blocked` |
| Enabled state | `Disabled` |
| Owner | Project operator |
| Intended scope | Dedicated Sentinel RDP Honeypot v2 workspace only |

### 7.2 Detection objective

`AR-001` is intended to identify a bounded authentication pattern:

- Windows Security Event ID 4625;
- validated logon type `10`;
- one populated source IP;
- one populated destination computer;
- at least five failed events;
- within a five-minute aggregation bucket;
- evaluated over a fifteen-minute lookback period.

A matching result means the configured query condition was satisfied.

It does not independently establish:

- malicious intent;
- password guessing by a human;
- credential compromise;
- successful access;
- host compromise;
- source identity;
- threat-actor attribution; or
- a confirmed security incident.

### 7.3 Data source

| Field | Required value |
|---|---|
| Primary table | `SecurityEvent` |
| Event ID | `4625` |
| Remote-interactive discriminator | `LogonType == 10` or a separately validated equivalent |
| Source field | `IpAddress`, normalized to `SourceIp` |
| Host field | `Computer` |
| Account context | `TargetAccount`, `TargetUserName`, or `Account`, normalized to `TargetAccountNormalized` |
| Time field | `TimeGenerated` |
| Collection path | Windows Security Events through AMA and the approved DCR |

If authentication events arrive only in the generic `Event` table, `AR-001` remains blocked until the route and query are redesigned and reviewed.

### 7.4 Scheduling

| Setting | Draft value | Status |
|---|---|---|
| Query frequency | `5m` | Reviewed |
| Query period / lookback | `15m` | Reviewed |
| Aggregation window inside query | `5m` | Reviewed |
| Failure threshold | `5` | Reviewed |
| Start running | Not applicable while blocked | Blocked |
| Rule enabled | `false` | Required |
| Suppression | Disabled | Required |
| Suppression duration | Not configured | Required |

The query frequency is less than or equal to the query period.

The fifteen-minute lookback overlaps successive five-minute executions. This improves late-arrival tolerance but can repeat qualifying groups. Alert and incident behavior must therefore be tested for duplicates before operational use.

### 7.5 Severity and ATT&CK mapping

| Setting | Draft value |
|---|---|
| Severity | Medium |
| Tactic | Credential Access |
| Technique | `T1110.001` Password Guessing |
| Mapping confidence | Design-stage and conservative |
| Limitation | Repeated failures can be controlled, benign, misconfigured, scanner-generated, or suspicious. |

The ATT&CK mapping describes the behavior the rule is designed to review. It does not attribute intent or prove password guessing.

### 7.6 Canonical query contract

The rule query must match `Q-020` version `1.0.0` in [`../kql/hunting-queries.md`](../kql/hunting-queries.md).

```kql
// Q-020 | version 1.0.0 | AR-001 canonical detection query
let RuleLookback = 15m;
let AggregationWindow = 5m;
let FailureThreshold = 5;
SecurityEvent
| where TimeGenerated >= ago(RuleLookback)
| where EventID == 4625
| extend
    SourceIp = tostring(column_ifexists("IpAddress", "")),
    RawTargetAccount = tostring(column_ifexists("TargetAccount", "")),
    RawTargetUserName = tostring(column_ifexists("TargetUserName", "")),
    RawAccount = tostring(column_ifexists("Account", "")),
    LogonTypeValue = toint(column_ifexists("LogonType", int(null)))
| extend TargetAccountNormalized = case(
    isnotempty(RawTargetAccount), RawTargetAccount,
    isnotempty(RawTargetUserName), RawTargetUserName,
    RawAccount
)
| where LogonTypeValue == 10
| where isnotempty(SourceIp) and SourceIp != "-"
| where isnotempty(Computer)
| summarize
    StartTime = min(TimeGenerated),
    EndTime = max(TimeGenerated),
    FailedLogonCount = count(),
    DistinctAccounts = dcount(TargetAccountNormalized),
    Accounts = make_set(TargetAccountNormalized, 20),
    FailureReasons = make_set(FailureReason, 10),
    Statuses = make_set(Status, 10),
    SubStatuses = make_set(SubStatus, 10)
    by SourceIp, Computer, bin(TimeGenerated, AggregationWindow)
| where FailedLogonCount >= FailureThreshold
| extend
    DetectionThreshold = FailureThreshold,
    QueryVersion = "1.0.0",
    TimeGenerated = EndTime
| project
    TimeGenerated,
    StartTime,
    EndTime,
    SourceIp,
    Computer,
    FailedLogonCount,
    DetectionThreshold,
    DistinctAccounts,
    Accounts,
    FailureReasons,
    Statuses,
    SubStatuses,
    QueryVersion
| order by FailedLogonCount desc, EndTime desc
```

Any change to the query requires:

1. a new query version;
2. review of threshold and scheduling assumptions;
3. review of projected fields;
4. review of entity mappings and custom details;
5. below-threshold and threshold-matching retesting;
6. alert and incident behavior retesting; and
7. an updated rule configuration version.

### 7.7 Result contract

Each qualifying query row must project:

| Field | Purpose |
|---|---|
| `TimeGenerated` | Alert event time derived from the group end time |
| `StartTime` | Earliest failed event in the qualifying group |
| `EndTime` | Latest failed event in the qualifying group |
| `SourceIp` | Candidate IP entity field |
| `Computer` | Candidate host entity field |
| `FailedLogonCount` | Number of failed remote-interactive events |
| `DetectionThreshold` | Threshold applied by the query |
| `DistinctAccounts` | Number of distinct normalized account values |
| `Accounts` | Bounded contextual account set |
| `FailureReasons` | Bounded failure-reason set |
| `Statuses` | Bounded status-code set |
| `SubStatuses` | Bounded substatus-code set |
| `QueryVersion` | Canonical query version |

If a required projected field is missing, renamed, or materially unpopulated, the rule remains disabled.

### 7.8 Event grouping

Initial design:

| Setting | Draft value |
|---|---|
| Event grouping | Trigger one alert for each query-result row |
| Query-result row meaning | One source, one host, one five-minute aggregation bucket |
| Maximum intended alert granularity | One alert per qualifying source-host-window result |

The query already aggregates raw events. Grouping all query results into one alert would combine unrelated source-host groups and reduce investigative clarity.

### 7.9 Alert details

Initial design:

| Setting | Draft value |
|---|---|
| Alert name | Use the static rule display name |
| Alert description | State that repeated failed remote-interactive authentication met the configured threshold |
| Dynamic alert-name override | Deferred |
| Dynamic severity override | Not configured |
| Dynamic tactics override | Not configured |

Dynamic alert properties remain deferred until their projected fields and operational value are validated.

### 7.10 Entity mappings

Entity mappings are conditional.

#### IP entity

| Field | Draft value |
|---|---|
| Entity type | IP |
| Identifier | Address |
| Query field | `SourceIp` |
| Initial state | Deferred until field validation |

Activation requirements:

- `Q-004` confirms the source field exists;
- `Q-005` confirms acceptable population;
- `Q-011` confirms the controlled source value;
- the value is not empty or `-`;
- public evidence handling preserves source-IP privacy; and
- the mapping is tested in the resulting alert.

#### Host entity

| Field | Draft value |
|---|---|
| Entity type | Host |
| Identifier | FullName |
| Query field | `Computer` |
| Initial state | Deferred until format validation |

Activation requirements:

- the intended source computer is confirmed;
- the actual `Computer` format is recorded;
- the selected Host identifier is appropriate for that format;
- the host resolves correctly in the alert or incident; and
- no unrelated host is mapped.

#### Account entity

Account mapping is deferred from rule version `1.0.0`.

Reasons:

- multiple account source fields may be present;
- normalized account context can contain local names, domains, service accounts, machine accounts, or incomplete values;
- `Q-020` can aggregate multiple accounts into one result;
- an account entity requires one stable identifier per mapping;
- incorrect mapping would degrade investigation quality.

Account values remain contextual custom details until a later query version and schema-quality review support a stable mapping.

### 7.11 Custom details

Planned custom details:

| Custom-detail key | Query field | Status |
|---|---|---|
| `FailedLogonCount` | `FailedLogonCount` | Reviewed |
| `DetectionThreshold` | `DetectionThreshold` | Reviewed |
| `DistinctAccounts` | `DistinctAccounts` | Reviewed |
| `Accounts` | `Accounts` | Reviewed |
| `QueryVersion` | `QueryVersion` | Reviewed |
| `GroupStartTime` | `StartTime` | Reviewed |
| `GroupEndTime` | `EndTime` | Reviewed |

Custom details must not expose raw identifiers in public evidence.

### 7.12 Incident settings

Initial controlled-test design:

| Setting | Draft value |
|---|---|
| Create incidents from alerts | Enabled only during the approved validation phase |
| Group related alerts into incidents | Disabled for initial validation |
| Initial relationship | One alert produces one incident object |
| Reopen closed matching incidents | Not applicable while grouping is disabled |
| Grouping lookback | Not applicable while grouping is disabled |

Disabling incident grouping makes `TST-016` deterministic: the test verifies whether the expected alert creates one investigation case object.

Incident grouping can be evaluated later after duplicate-alert and entity-quality testing.

### 7.13 Suppression

Suppression is disabled in version `1.0.0`.

Rationale:

- below-threshold and threshold-matching tests require observable behavior;
- suppression could hide duplicate or repeated results during validation;
- tuning decisions require actual alert evidence; and
- suppression is not a substitute for correcting query cadence or grouping.

A later version may add suppression only with documented evidence and retesting.

### 7.14 Automation and response

`AR-001` has no destructive or automatic response.

Prohibited actions include:

- disabling or deleting the VM automatically;
- modifying NSG or firewall rules automatically;
- blocking a source automatically;
- changing credentials automatically;
- isolating a host automatically;
- deleting evidence automatically;
- executing a playbook without explicit approval; or
- treating an alert as authorization for containment.

Any future non-destructive routing automation remains deferred and separately governed.

### 7.15 False-positive considerations

Potential false-positive or expected-positive sources include:

- approved controlled tests;
- operator typing errors;
- stale saved credentials;
- misconfigured services or scheduled tasks;
- vulnerability scanners;
- monitoring or inventory software;
- shared NAT or proxy addresses;
- internet-wide opportunistic scanning;
- repeated attempts against a disabled or nonexistent account; and
- duplicated results caused by overlapping query periods.

A true-positive detection means the rule correctly identified the behavior it was designed to detect. It does not automatically mean compromise occurred.

### 7.16 False-negative considerations

Potential false-negative conditions include:

- missing or delayed `SecurityEvent` ingestion;
- authentication events arriving in `Event` instead of `SecurityEvent`;
- unpopulated `IpAddress`, `Computer`, or `LogonType`;
- a remote-interactive equivalent not represented as `LogonType == 10`;
- distributed low-volume attempts below threshold;
- activity split across aggregation buckets;
- source-address rotation;
- missing events caused by DCR or AMA failure;
- threshold or lookback values that are too restrictive; and
- query execution failure.

Telemetry health and rule execution health must be monitored separately.

### 7.17 Prerequisites to create the disabled rule

Before creating `AR-001` in a disabled state:

- [ ] Predeployment authorization gate passes.
- [ ] Correct tenant, subscription, and workspace are recorded.
- [ ] Dedicated project scope is confirmed.
- [ ] AMA and DCR implementation is evidenced.
- [ ] `Q-001` heartbeat result is acceptable.
- [ ] `Q-002` confirms the intended destination table.
- [ ] `Q-003` freshness and latency are acceptable.
- [ ] `Q-004` confirms required schema fields.
- [ ] `Q-005` records field-population quality.
- [ ] `Q-006` confirms the intended source host.
- [ ] `Q-007` through `Q-012` assumptions are reviewed against actual fields.
- [ ] `Q-020` version and `AR-001` query match exactly.
- [ ] Rule creation method is reviewed.
- [ ] Teardown procedure includes exact rule disposition.
- [ ] Private evidence location is ready.
- [ ] No unresolved high-risk deviation exists.

Creation of a disabled rule is an implementation action and requires explicit Azure authorization.

### 7.18 Prerequisites to enable for controlled validation

Before enabling `AR-001`, even temporarily:

- [ ] The disabled deployed configuration is captured and reviewed.
- [ ] Entity mappings include only validated fields.
- [ ] Account mapping remains absent in version `1.0.0`.
- [ ] Custom details are reviewed.
- [ ] `Q-013` below-threshold input behavior passes.
- [ ] `Q-014` threshold-matching input behavior passes.
- [ ] Exact controlled-test window, source, account, host, and owner are recorded.
- [ ] Operator and teardown owner are present.
- [ ] Public exposure authorization is separately granted when needed.
- [ ] Runtime and cost limits are active.
- [ ] Emergency exposure-removal and VM-containment procedures are ready.
- [ ] Failed tests will be preserved.
- [ ] A new `GO` decision authorizes temporary enablement.

The rule remains disabled when any prerequisite is missing, failed, blocked, or inconclusive.

### 7.19 Controlled validation plan

#### TST-013 — Below-threshold behavior

1. Keep `AR-001` disabled while validating `Q-013`.
2. Generate fewer than five approved controlled failed remote-interactive events.
3. Confirm the test events appear in `Q-011`.
4. Confirm `Q-013` reports `BelowThreshold`.
5. When rule validation is authorized, enable `AR-001` only for the bounded test.
6. Verify no rule alert is created for the below-threshold group.
7. Record actual result, rule version, query version, time window, and evidence.
8. Disable the rule after the test unless the next approved test begins immediately.

Expected outcome: no `AR-001` alert.

#### TST-014 — Threshold-matching behavior

1. Confirm the exact rule configuration and state.
2. Generate at least five approved controlled failed remote-interactive events within the intended window.
3. Confirm the events appear in `Q-011`.
4. Confirm `Q-014` and `Q-020` return the expected qualifying group.
5. Verify the rule evaluates the same query version.
6. Record whether the expected alert is created.
7. Preserve failed or delayed behavior.
8. Disable the rule after the bounded test.

Expected outcome: one qualifying detection result and the configured alert behavior.

#### TST-015 — Alert creation

Validate:

- alert name;
- alert time;
- rule and query version;
- severity;
- source event linkage;
- query-result fields;
- custom details;
- mapped entities;
- duplicate behavior; and
- false-positive classification.

An alert does not prove compromise.

#### TST-016 — Incident-object behavior

Validate:

- whether one alert creates one incident object;
- incident title and severity;
- alert membership;
- mapped entities;
- timeline;
- owner and status behavior;
- investigation linkage;
- disposition;
- closure decision; and
- absence of unauthorized automation.

An incident object is an investigation case, not proof of a confirmed security incident.

### 7.20 Expected-versus-actual test matrix

| Test | Expected | Actual | Outcome |
|---|---|---|---|
| `TST-013` below threshold | No `AR-001` alert | Not run | Blocked |
| `TST-014` threshold matching | Qualifying result and configured alert behavior | Not run | Blocked |
| `TST-015` alert creation | Alert links to underlying controlled events | Not run | Blocked |
| `TST-016` incident behavior | One alert creates one incident object as configured | Not run | Blocked |
| `TST-017` activity separation | Controlled activity is distinguishable from organic or unclassified activity | Not run | Blocked |

### 7.21 Rule health and execution monitoring

After rule implementation, rule health review should record:

- rule enabled or disabled state;
- last execution time;
- execution success or failure;
- query-result count;
- alert count;
- execution duration when available;
- query or schema errors;
- disabled or modified configuration;
- unexpected duplicate behavior; and
- unresolved health findings.

Microsoft Sentinel health and audit data must be explicitly enabled before relying on related health tables or execution insights.

### 7.22 Tuning rules

A tuning change requires:

- observed evidence;
- a documented false-positive or false-negative problem;
- query and rule version increments;
- expected impact;
- rollback plan;
- repeated `TST-013` through `TST-016` as applicable; and
- claim and evidence review.

Permitted tuning areas include:

- failure threshold;
- query period;
- query frequency;
- aggregation window;
- required field filters;
- exclusions with defensible provenance;
- severity;
- entity mappings;
- custom details;
- incident grouping; and
- suppression.

Do not tune by hiding unexplained failures or deleting failed-test evidence.

### 7.23 Rollback and disablement

The first rollback action is to disable `AR-001`.

Disable the rule when:

- telemetry health fails;
- schema assumptions fail;
- entity mappings are incorrect;
- duplicate behavior is excessive;
- the query produces unexpected scope;
- an unauthorized change occurs;
- cost or runtime boundaries require closure;
- testing ends;
- evidence cannot be preserved safely; or
- any mandatory project stop condition occurs.

After disablement:

1. record UTC time and trigger;
2. verify the rule is disabled;
3. preserve configuration and failure evidence privately;
4. review alerts and incidents already created;
5. update test outcomes;
6. correct or retire the configuration;
7. retest before any re-enable decision; and
8. do not resume without a new authorization decision.

### 7.24 Deletion and teardown disposition

During teardown, record one disposition:

- not created;
- disabled and retained temporarily;
- deleted;
- retained under separate approval; or
- blocked or inconclusive.

Deletion requires:

- exact workspace and rule identity;
- configuration evidence;
- a reviewed deletion method;
- proof the intended rule no longer exists;
- preservation of failed deletion output; and
- residual-object review.

A deletion request is not proof of completed deletion.

### 7.25 Evidence requirements

An `AR-001` implementation or validation record must include:

- evidence ID;
- test ID;
- UTC date and time;
- operator;
- branch and commit;
- tenant, subscription, and workspace aliases;
- exact rule ID and configuration version;
- exact query ID and version;
- enabled or disabled state;
- frequency and period;
- threshold and aggregation window;
- severity and ATT&CK mapping;
- entity mappings;
- custom details;
- incident and grouping settings;
- suppression settings;
- expected result;
- actual result;
- alert and incident identifiers kept private;
- test outcome;
- limitations;
- reviewer; and
- claim impact.

Raw platform identifiers and event data remain outside Git.

### 7.26 Claim boundaries

Currently supportable:

- designed and reviewed `AR-001`;
- specified a disabled scheduled rule;
- linked `AR-001` to canonical query `Q-020`;
- defined field-quality-dependent entity mappings;
- defined below-threshold and threshold-matching tests;
- defined alert, incident, rollback, teardown, and evidence requirements.

Not currently supportable:

- implemented `AR-001`;
- enabled or executed `AR-001`;
- validated threshold behavior;
- generated a v2 Sentinel alert;
- generated or triaged a v2 incident object;
- validated entity mappings;
- detected an attack;
- confirmed compromise;
- completed detection engineering; or
- demonstrated an operational Sentinel workflow.

## 8. AR-001 configuration record template

    Rule ID:
    Rule display name:
    Configuration version:
    Lifecycle status:
    Deployment state:
    Enabled state:
    Rule resource identifier:
    Workspace alias:
    Query ID:
    Query version:
    Query hash:
    Query frequency:
    Query period:
    Aggregation window:
    Threshold:
    Severity:
    Tactic:
    Technique:
    Event grouping:
    Alert details:
    Entity mappings:
    Custom details:
    Incident creation:
    Incident grouping:
    Suppression:
    Automation:
    Expected behavior:
    Actual behavior:
    Test IDs:
    Test outcomes:
    Evidence IDs:
    Private evidence location:
    Reviewer:
    Rollback decision:
    Teardown disposition:
    Claim impact:

Do not place raw resource IDs, tenant IDs, subscription IDs, workspace IDs, alert IDs, incident IDs, IP addresses, account names, email addresses, credentials, secrets, or private evidence paths in a public copy.

## 9. Change-control requirements

A change request is required for:

- query modification;
- query version change;
- threshold change;
- frequency or period change;
- aggregation-window change;
- severity change;
- ATT&CK mapping change;
- entity mapping addition or removal;
- account mapping;
- custom-detail change;
- dynamic alert-property change;
- incident grouping;
- suppression;
- automation;
- rule enablement outside an approved test;
- retention after testing; or
- deletion-method change.

Each approved change requires a new configuration version.

## 10. Catalog completion criteria

This catalog is documentation-complete when it:

- defines one stable `AR-001` identifier;
- records lifecycle, deployment, and test state separately;
- links exactly to `Q-020` version `1.0.0`;
- specifies a five-minute frequency and fifteen-minute period;
- records threshold and aggregation assumptions;
- keeps the rule disabled until prerequisites pass;
- treats Event ID 4625 and logon type `10` correctly;
- conditions IP and Host mappings on field validation;
- defers Account mapping;
- defines custom details without public disclosure;
- makes event grouping explicit;
- defines deterministic initial incident behavior;
- disables suppression during initial validation;
- contains no destructive or automatic response;
- defines below-threshold and threshold-matching tests;
- defines alert and incident validation;
- preserves failed, blocked, and inconclusive results;
- defines rollback and teardown disposition;
- preserves design-stage claim boundaries; and
- does not claim that `AR-001` exists or has run.

## 11. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection and incident validation have not started.
- `SecurityEvent` routing and field population remain unvalidated.
- `Q-020` is reviewed but unexecuted.
- `AR-001` is reviewed, disabled by design, undeployed, and unverified.
- Entity mappings remain conditional and unvalidated.
- No alert or incident object exists.
- No automation is configured.
- No implementation, validation, compromise, completion, or operational claim is authorized.
- This catalog is design-stage detection-engineering documentation only.
