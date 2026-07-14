# Sentinel RDP Honeypot v2 KQL Query Catalog

## 1. Purpose

This catalog defines the versioned Kusto Query Language queries for Sentinel RDP Honeypot v2.

The query sequence deliberately places telemetry health, destination-table confirmation, freshness, and schema inspection before authentication behavior or scheduled detection. The catalog supports design-stage review now and controlled execution only after the applicable authorization, deployment, telemetry, and evidence gates pass.

This file does not prove that Azure resources exist, that `SecurityEvent` is populated, that Event IDs 4625 or 4624 were collected, or that any detection, alert, incident, compromise, cleanup, or cost-closure result occurred.

## 2. Current status and authorization boundary

At this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection and incident validation have not started.
- The expected `SecurityEvent` route is unvalidated.
- No query in this catalog has been executed against the v2 workspace.
- No query result or execution evidence exists.
- `AR-001` remains draft, disabled, undeployed, and unverified.
- Public claims remain design-stage only.

Static review of this catalog does not authorize Azure execution or query execution.

## 3. Governing documents

Use this catalog with:

1. [`../docs/threat-model.md`](../docs/threat-model.md);
2. [`../docs/scope-and-credibility-notes.md`](../docs/scope-and-credibility-notes.md);
3. [`../docs/checklist-to-project-roadmap.md`](../docs/checklist-to-project-roadmap.md);
4. [`../runbooks/deployment-runbook.md`](../runbooks/deployment-runbook.md);
5. [`../runbooks/rdp-authentication-triage.md`](../runbooks/rdp-authentication-triage.md);
6. [`../runbooks/teardown-runbook.md`](../runbooks/teardown-runbook.md);
7. [`../runbooks/cost-control-checklist.md`](../runbooks/cost-control-checklist.md);
8. [`../sentinel/analytics-rules-catalog.md`](../sentinel/analytics-rules-catalog.md);
9. [`../evidence/README.md`](../evidence/README.md);
10. [`../evidence/redaction-notes.md`](../evidence/redaction-notes.md);
11. [`../evidence/claim-evidence-matrix.md`](../evidence/claim-evidence-matrix.md); and
12. [`../evidence/templates/evidence-record-template.md`](../evidence/templates/evidence-record-template.md).

When query output conflicts with the authoritative project state or evidence record, use the weaker supported interpretation.

## 4. Technical reference basis

The catalog is designed around the following Microsoft platform behavior:

- The Windows Security Events via AMA connector is expected to send Windows security events to `SecurityEvent`; a generic Windows event DCR can instead send events to `Event`. The actual v2 destination must be verified.
- `SecurityEvent` currently documents fields including `TimeGenerated`, `EventID`, `Computer`, `IpAddress`, `LogonType`, `LogonTypeName`, `TargetAccount`, `TargetUserName`, `Status`, `SubStatus`, `FailureReason`, `WorkstationName`, and `EventRecordId`.
- Windows Event ID 4625 records a failed logon on the computer where the attempt occurred. It does not independently prove RDP, malicious intent, or compromise.
- Windows Event ID 4624 records creation of a successful logon session on the accessed computer. It does not independently prove authorization or compromise.
- Logon type `10` represents remote-interactive authentication and must be validated in the actual collected schema.
- Microsoft Sentinel scheduled rules evaluate KQL on a defined cadence and lookback. Alert creation, incident creation, entity mapping, grouping, and automation are separate configuration decisions.

Official references:

- [SecurityEvent table reference](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/securityevent)
- [Heartbeat table reference](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/tables/heartbeat)
- [Collect Windows events with Azure Monitor Agent](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/data-collection-windows-events)
- [Windows Event ID 4625](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625)
- [Windows Event ID 4624](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624)
- [Scheduled analytics rules in Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/scheduled-rules-overview)

## 5. Query lifecycle

Each query records two separate states.

| Field | Allowed values | Meaning |
|---|---|---|
| Lifecycle status | `Draft`, `Reviewed`, `Validated`, `Tuned`, `Retired` | Maturity of the query definition |
| Execution status | `Blocked`, `Not Run`, `Passed`, `Failed`, `Inconclusive` | Result of a specific authorized execution |

For this initial catalog:

- every query lifecycle status is `Reviewed`;
- every execution status is `Blocked`;
- static review means the query structure and claim boundaries were reviewed;
- `Reviewed` does not mean the query ran successfully;
- a live result must be recorded separately with query version, time range, workspace scope, expected result, actual result, test outcome, evidence ID, and limitations.

## 6. Query execution rules

1. Run `Q-001` through `Q-006` before any behavioral query.
2. Confirm the actual destination table before relying on `SecurityEvent`.
3. Confirm telemetry freshness and expected source computer.
4. Inspect actual field presence and population before applying remote-interactive filters.
5. Use exact UTC test windows and controlled selectors for `Q-011` through `Q-014`.
6. Treat Event ID 4625 as a failed logon, not automatically as RDP or attack activity.
7. Treat Event ID 4624 as a successful logon session, not automatically as authorized or unauthorized access.
8. Require logon type `10` or an explicitly validated equivalent before using remote-interactive wording.
9. Treat source IP, account, and host values as investigative context whose quality must be validated.
10. Do not publish raw query output containing identifiers.
11. Preserve failed, blocked, and inconclusive results.
12. Keep `AR-001` disabled until its prerequisites pass.
13. Do not add destructive automation to this catalog.
14. Do not promote a design claim to implementation or validation based only on static KQL.

## 7. Catalog index

| ID | Title | Category | Lifecycle | Execution | Primary mapping |
|---|---|---|---|---|---|
| `Q-001` | AMA heartbeat recency | Health | Reviewed | Blocked | `TST-006`, `TST-008` |
| `Q-002` | Authentication destination-table comparison | Route | Reviewed | Blocked | `TST-009` |
| `Q-003` | SecurityEvent freshness and ingestion latency | Health | Reviewed | Blocked | `TST-008`, `TST-009` |
| `Q-004` | SecurityEvent authentication schema probe | Schema | Reviewed | Blocked | `TST-009` |
| `Q-005` | Authentication-field population profile | Schema | Reviewed | Blocked | `TST-010`, `TST-011`, `TST-012` |
| `Q-006` | SecurityEvent source-host coverage | Health | Reviewed | Blocked | `TST-008`, `TST-009` |
| `Q-007` | Recent failed-logon events | Authentication | Reviewed | Blocked | `TST-010` |
| `Q-008` | Failed remote-interactive events | Authentication | Reviewed | Blocked | `TST-011` |
| `Q-009` | Recent successful-logon events | Authentication | Reviewed | Blocked | `TST-012` |
| `Q-010` | Successful remote-interactive events | Authentication | Reviewed | Blocked | `TST-012` |
| `Q-011` | Controlled failed-logon test window | Controlled test | Reviewed | Blocked | `TST-010`, `TST-011` |
| `Q-012` | Controlled successful-logon test window | Controlled test | Reviewed | Blocked | `TST-012` |
| `Q-013` | Below-threshold detection check | Controlled test | Reviewed | Blocked | `TST-013` |
| `Q-014` | Threshold-matching detection check | Controlled test | Reviewed | Blocked | `TST-014` |
| `Q-015` | Repeated-failure summary | Behavioral | Reviewed | Blocked | Investigation support |
| `Q-016` | Single-account guessing pattern | Behavioral | Reviewed | Blocked | Investigation support |
| `Q-017` | Multi-account spraying pattern | Behavioral | Reviewed | Blocked | Investigation support |
| `Q-018` | Controlled-versus-organic classification | Classification | Reviewed | Blocked | `TST-017` |
| `Q-019` | Failed-to-successful temporal correlation | Correlation | Reviewed | Blocked | Triage support |
| `Q-020` | AR-001 canonical detection query | Detection | Reviewed | Blocked | `AR-001`, `TST-013`–`TST-016` |

---

## Q-001 — AMA heartbeat recency

| Field | Value |
|---|---|
| Category | Health |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-006`, `TST-008` |
| Data source | `Heartbeat` |

**Purpose:** Confirm that the intended Windows host has a recent heartbeat before relying on authentication telemetry.

**Expected result:** One row for the intended host with a recent `LastHeartbeat` and a bounded `MinutesSinceLastHeartbeat` value.

**Interpretation:** A recent heartbeat supports agent connectivity only. It does not prove that the intended DCR is associated or that Windows Security Events are reaching the expected table.

**Limitations:** Heartbeat can remain present while a specific data stream is misconfigured. Confirm DCR association and `SecurityEvent` separately.

```kql
// Q-001 | version 1.0.0 | AMA heartbeat recency
let HealthLookback = 24h;
let ExpectedComputer = "";
Heartbeat
| where TimeGenerated >= ago(HealthLookback)
| where OSType =~ "Windows"
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
| summarize
    LastHeartbeat = max(TimeGenerated),
    HeartbeatCount = count(),
    ResourceIds = make_set(_ResourceId, 5)
    by Computer
| extend MinutesSinceLastHeartbeat = datetime_diff("minute", now(), LastHeartbeat)
| project
    Computer,
    LastHeartbeat,
    MinutesSinceLastHeartbeat,
    HeartbeatCount,
    ResourceIds
| order by LastHeartbeat desc
```

---

## Q-002 — Authentication destination-table comparison

| Field | Value |
|---|---|
| Category | Route |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-009` |
| Data source | `SecurityEvent`, `Event` |

**Purpose:** Determine whether collected authentication events appear in `SecurityEvent`, `Event`, both, or neither.

**Expected result:** A route summary showing the actual table or tables containing Event IDs 4624 and 4625 for the intended host.

**Interpretation:** The expected Sentinel connector route is `SecurityEvent`. A result in `Event` requires the catalog and deployment assumptions to be reviewed before behavioral detection.

**Limitations:** `union isfuzzy=true` tolerates a missing table reference but does not prove connector configuration. Empty results require collection and time-range investigation.

```kql
// Q-002 | version 1.0.0 | Authentication destination-table comparison
let RouteLookback = 24h;
let ExpectedComputer = "";
union isfuzzy=true withsource=SourceTable SecurityEvent, Event
| where TimeGenerated >= ago(RouteLookback)
| extend
    NormalizedEventId = toint(column_ifexists("EventID", int(null))),
    NormalizedComputer = tostring(column_ifexists("Computer", ""))
| where NormalizedEventId in (4624, 4625)
| where isempty(ExpectedComputer) or NormalizedComputer =~ ExpectedComputer
| summarize
    EventCount = count(),
    FirstEvent = min(TimeGenerated),
    LastEvent = max(TimeGenerated),
    EventIds = make_set(NormalizedEventId, 10)
    by SourceTable, NormalizedComputer
| order by LastEvent desc
```

---

## Q-003 — SecurityEvent freshness and ingestion latency

| Field | Value |
|---|---|
| Category | Health |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-008`, `TST-009` |
| Data source | `SecurityEvent` |

**Purpose:** Measure event-generation freshness and observed ingestion latency for the intended `SecurityEvent` source.

**Expected result:** Recent events from the intended host with last-event time and latency statistics appropriate for the approved test window.

**Interpretation:** Positive latency values reflect the observed difference between ingestion time and source event time. Investigate sustained or extreme delay before enabling any scheduled rule.

**Limitations:** Clock skew, late arrival, low event volume, and workspace behavior can affect latency. This query does not establish end-to-end detection readiness by itself.

```kql
// Q-003 | version 1.0.0 | SecurityEvent freshness and ingestion latency
let FreshnessLookback = 24h;
let ExpectedComputer = "";
SecurityEvent
| where TimeGenerated >= ago(FreshnessLookback)
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
| extend IngestionLatencySeconds = datetime_diff("second", ingestion_time(), TimeGenerated)
| summarize
    FirstEvent = min(TimeGenerated),
    LastEvent = max(TimeGenerated),
    EventCount = count(),
    AverageLatencySeconds = avg(IngestionLatencySeconds),
    MaximumLatencySeconds = max(IngestionLatencySeconds)
    by Computer
| extend MinutesSinceLastEvent = datetime_diff("minute", now(), LastEvent)
| project
    Computer,
    FirstEvent,
    LastEvent,
    MinutesSinceLastEvent,
    EventCount,
    AverageLatencySeconds,
    MaximumLatencySeconds
| order by LastEvent desc
```

---

## Q-004 — SecurityEvent authentication schema probe

| Field | Value |
|---|---|
| Category | Schema |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-009` |
| Data source | `SecurityEvent` |

**Purpose:** Inspect the currently available `SecurityEvent` schema for fields required by the authentication and detection queries.

**Expected result:** Schema rows for the required time, event, host, source, account, logon-type, status, and record fields.

**Interpretation:** Only fields present in the actual workspace should be used for validated entity mappings or detection assumptions.

**Limitations:** A field can exist in the table schema while remaining empty for the relevant event type. Run `Q-005` after this query.

```kql
// Q-004 | version 1.0.0 | SecurityEvent authentication schema probe
SecurityEvent
| take 1
| getschema
| where ColumnName in (
    "TimeGenerated",
    "EventID",
    "Computer",
    "IpAddress",
    "LogonType",
    "LogonTypeName",
    "TargetAccount",
    "TargetUserName",
    "Account",
    "Status",
    "SubStatus",
    "FailureReason",
    "WorkstationName",
    "EventRecordId"
)
| project
    ColumnName,
    ColumnOrdinal,
    DataType,
    ColumnType
| order by ColumnName asc
```

---

## Q-005 — Authentication-field population profile

| Field | Value |
|---|---|
| Category | Schema |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-010`, `TST-011`, `TST-012` |
| Data source | `SecurityEvent` |

**Purpose:** Measure field population for Event IDs 4624 and 4625 before applying remote-interactive logic or entity mapping.

**Expected result:** Per-event-ID population rates for source IP, target account, logon type, workstation, status, and event record identifiers.

**Interpretation:** Entity mappings and filters must depend on populated, stable fields. Low population requires query revision or deferred mapping.

**Limitations:** Population does not establish semantic correctness. Validate representative raw events and controlled-test provenance.

```kql
// Q-005 | version 1.0.0 | Authentication-field population profile
let ProfileLookback = 24h;
let ExpectedComputer = "";
SecurityEvent
| where TimeGenerated >= ago(ProfileLookback)
| where EventID in (4624, 4625)
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
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
| extend
    WorkstationValue = tostring(column_ifexists("WorkstationName", "")),
    StatusValue = tostring(column_ifexists("Status", "")),
    SubStatusValue = tostring(column_ifexists("SubStatus", "")),
    RecordIdValue = tostring(column_ifexists("EventRecordId", ""))
| summarize
    EventCount = count(),
    SourceIpPopulated = countif(isnotempty(SourceIp) and SourceIp != "-"),
    AccountPopulated = countif(isnotempty(TargetAccountNormalized) and TargetAccountNormalized != "-"),
    LogonTypePopulated = countif(isnotnull(LogonTypeValue)),
    WorkstationPopulated = countif(isnotempty(WorkstationValue) and WorkstationValue != "-"),
    StatusPopulated = countif(isnotempty(StatusValue)),
    SubStatusPopulated = countif(isnotempty(SubStatusValue)),
    EventRecordIdPopulated = countif(isnotempty(RecordIdValue))
    by EventID, Computer
| extend
    SourceIpPopulationPercent = round(100.0 * SourceIpPopulated / EventCount, 1),
    AccountPopulationPercent = round(100.0 * AccountPopulated / EventCount, 1),
    LogonTypePopulationPercent = round(100.0 * LogonTypePopulated / EventCount, 1),
    WorkstationPopulationPercent = round(100.0 * WorkstationPopulated / EventCount, 1),
    StatusPopulationPercent = round(100.0 * StatusPopulated / EventCount, 1),
    SubStatusPopulationPercent = round(100.0 * SubStatusPopulated / EventCount, 1),
    EventRecordIdPopulationPercent = round(100.0 * EventRecordIdPopulated / EventCount, 1)
| order by EventID asc, Computer asc
```

---

## Q-006 — SecurityEvent source-host coverage

| Field | Value |
|---|---|
| Category | Health |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-008`, `TST-009` |
| Data source | `SecurityEvent` |

**Purpose:** Confirm which computers are contributing authentication events and when each source last reported.

**Expected result:** The intended lab host appears with recent 4624 or 4625 activity and no unexpected project host.

**Interpretation:** Unexpected hosts or missing expected hosts require scope and DCR investigation before behavioral detection.

**Limitations:** Absence of authentication activity can reflect a quiet host rather than collection failure. Correlate with `Q-001` and `Q-003`.

```kql
// Q-006 | version 1.0.0 | SecurityEvent source-host coverage
let CoverageLookback = 24h;
SecurityEvent
| where TimeGenerated >= ago(CoverageLookback)
| where EventID in (4624, 4625)
| summarize
    FirstAuthenticationEvent = min(TimeGenerated),
    LastAuthenticationEvent = max(TimeGenerated),
    AuthenticationEventCount = count(),
    EventIds = make_set(EventID, 10),
    ResourceIds = make_set(_ResourceId, 5)
    by Computer
| extend MinutesSinceLastAuthenticationEvent = datetime_diff(
    "minute",
    now(),
    LastAuthenticationEvent
)
| order by LastAuthenticationEvent desc
```

---

## Q-007 — Recent failed-logon events

| Field | Value |
|---|---|
| Category | Authentication |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-010` |
| Data source | `SecurityEvent` |

**Purpose:** Inspect recent Event ID 4625 records and the fields needed for controlled-test and triage decisions.

**Expected result:** Failed-logon rows with source, target account, host, logon type, status, and timestamp where those fields are populated.

**Interpretation:** Each row represents a failed logon record. It does not by itself establish RDP, malicious intent, repeated behavior, or compromise.

**Limitations:** Field population varies by event and collection route. Keep raw output private and review `Q-005` first.

```kql
// Q-007 | version 1.0.0 | Recent failed-logon events
let QueryLookback = 2h;
let ExpectedComputer = "";
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
| where EventID == 4625
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
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
| project
    TimeGenerated,
    EventID,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    LogonTypeName,
    WorkstationName,
    FailureReason,
    Status,
    SubStatus,
    EventRecordId
| order by TimeGenerated desc
```

---

## Q-008 — Failed remote-interactive events

| Field | Value |
|---|---|
| Category | Authentication |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-011` |
| Data source | `SecurityEvent` |

**Purpose:** Restrict failed-logon analysis to records whose collected logon type is `10`, the Windows remote-interactive logon type.

**Expected result:** Event ID 4625 records from the intended host where the validated `LogonType` value is `10`.

**Interpretation:** This supports remote-interactive wording only after the field and value are validated in the actual collected schema.

**Limitations:** Remote-interactive failure is not proof of password guessing, brute force, compromise, or human attribution.

```kql
// Q-008 | version 1.0.0 | Failed remote-interactive events
let QueryLookback = 2h;
let ExpectedComputer = "";
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
| where EventID == 4625
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
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
| project
    TimeGenerated,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    LogonTypeName,
    WorkstationName,
    FailureReason,
    Status,
    SubStatus,
    EventRecordId
| order by TimeGenerated desc
```

---

## Q-009 — Recent successful-logon events

| Field | Value |
|---|---|
| Category | Authentication |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-012` |
| Data source | `SecurityEvent` |

**Purpose:** Inspect recent Event ID 4624 records for an approved controlled-success validation.

**Expected result:** Successful-logon rows for the exact test window and intended host, with source, account, logon-type, and session fields when populated.

**Interpretation:** Event ID 4624 records creation of a successful logon session. Authorization and user intent require separate evidence.

**Limitations:** Windows generates many benign 4624 events. Do not use this broad query as a compromise detector.

```kql
// Q-009 | version 1.0.0 | Recent successful-logon events
let QueryLookback = 2h;
let ExpectedComputer = "";
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
| where EventID == 4624
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
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
| project
    TimeGenerated,
    EventID,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    LogonTypeName,
    WorkstationName,
    LogonID,
    TargetLogonId,
    EventRecordId
| order by TimeGenerated desc
```

---

## Q-010 — Successful remote-interactive events

| Field | Value |
|---|---|
| Category | Authentication |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-012` |
| Data source | `SecurityEvent` |

**Purpose:** Restrict successful-logon review to Event ID 4624 records with validated logon type `10`.

**Expected result:** A controlled successful remote-interactive event tied to the intended host, source, account, and test window.

**Interpretation:** The result confirms a successful remote-interactive session record, not whether the access was authorized or harmful.

**Limitations:** This query must be run only under an approved controlled-success procedure or during evidence-based triage.

```kql
// Q-010 | version 1.0.0 | Successful remote-interactive events
let QueryLookback = 2h;
let ExpectedComputer = "";
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
| where EventID == 4624
| where isempty(ExpectedComputer) or Computer =~ ExpectedComputer
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
| project
    TimeGenerated,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    LogonTypeName,
    WorkstationName,
    LogonID,
    TargetLogonId,
    EventRecordId
| order by TimeGenerated desc
```

---

## Q-011 — Controlled failed-logon test window

| Field | Value |
|---|---|
| Category | Controlled test |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-010`, `TST-011` |
| Data source | `SecurityEvent` |

**Purpose:** Retrieve only the approved controlled failed-logon activity using exact time and host selectors, with optional source and account selectors.

**Expected result:** The known test events appear in the recorded window with Event ID 4625 and, when applicable, logon type `10`.

**Interpretation:** A matching row supports the specific controlled test only when provenance and expected-versus-actual results are recorded.

**Limitations:** Set `TestComputer` before execution. Empty optional selectors widen only within the exact host and time window.

```kql
// Q-011 | version 1.0.0 | Controlled failed-logon test window
let TestStart = ago(30m);
let TestEnd = now();
let TestComputer = "";
let TestSourceIp = "";
let TestAccount = "";
SecurityEvent
| where TimeGenerated between (TestStart .. TestEnd)
| where EventID == 4625
| where isnotempty(TestComputer)
| where Computer =~ TestComputer
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
| where isempty(TestSourceIp) or SourceIp == TestSourceIp
| where isempty(TestAccount) or TargetAccountNormalized =~ TestAccount
| project
    TimeGenerated,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    LogonTypeName,
    WorkstationName,
    FailureReason,
    Status,
    SubStatus,
    EventRecordId
| order by TimeGenerated asc
```

---

## Q-012 — Controlled successful-logon test window

| Field | Value |
|---|---|
| Category | Controlled test |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-012` |
| Data source | `SecurityEvent` |

**Purpose:** Retrieve only an approved controlled successful-logon event using exact time and host selectors.

**Expected result:** The known controlled Event ID 4624 record appears with the expected remote-interactive context when that test is authorized.

**Interpretation:** The result validates the controlled event and fields only; it does not authorize broader successful-logon monitoring claims.

**Limitations:** Set `TestComputer` before execution. The controlled-success procedure and credential controls remain governed by the runbooks.

```kql
// Q-012 | version 1.0.0 | Controlled successful-logon test window
let TestStart = ago(30m);
let TestEnd = now();
let TestComputer = "";
let TestSourceIp = "";
let TestAccount = "";
SecurityEvent
| where TimeGenerated between (TestStart .. TestEnd)
| where EventID == 4624
| where isnotempty(TestComputer)
| where Computer =~ TestComputer
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
| where isempty(TestSourceIp) or SourceIp == TestSourceIp
| where isempty(TestAccount) or TargetAccountNormalized =~ TestAccount
| project
    TimeGenerated,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    LogonTypeName,
    WorkstationName,
    LogonID,
    TargetLogonId,
    EventRecordId
| order by TimeGenerated asc
```

---

## Q-013 — Below-threshold detection check

| Field | Value |
|---|---|
| Category | Controlled test |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-013` |
| Data source | `SecurityEvent` |

**Purpose:** Count controlled remote-interactive failures and verify that the observed count remains below the proposed `AR-001` threshold.

**Expected result:** `ThresholdState` equals `BelowThreshold` and no `AR-001` alert is expected.

**Interpretation:** This query validates input count and expected logic only. Alert absence must be verified separately after a deployed disabled-to-enabled test plan is approved.

**Limitations:** Set the exact test computer and time window. Use the same threshold and grouping assumptions as `Q-020` and `AR-001`.

```kql
// Q-013 | version 1.0.0 | Below-threshold detection check
let TestStart = ago(30m);
let TestEnd = now();
let TestComputer = "";
let TestSourceIp = "";
let FailureThreshold = 5;
SecurityEvent
| where TimeGenerated between (TestStart .. TestEnd)
| where EventID == 4625
| where isnotempty(TestComputer)
| where Computer =~ TestComputer
| extend
    SourceIp = tostring(column_ifexists("IpAddress", "")),
    LogonTypeValue = toint(column_ifexists("LogonType", int(null)))
| where LogonTypeValue == 10
| where isempty(TestSourceIp) or SourceIp == TestSourceIp
| summarize
    FailedLogonCount = count(),
    FirstFailure = min(TimeGenerated),
    LastFailure = max(TimeGenerated)
    by Computer, SourceIp
| extend
    DetectionThreshold = FailureThreshold,
    ThresholdState = iff(
        FailedLogonCount < FailureThreshold,
        "BelowThreshold",
        "ThresholdMetOrExceeded"
    )
| project
    Computer,
    SourceIp,
    FirstFailure,
    LastFailure,
    FailedLogonCount,
    DetectionThreshold,
    ThresholdState
```

---

## Q-014 — Threshold-matching detection check

| Field | Value |
|---|---|
| Category | Controlled test |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-014` |
| Data source | `SecurityEvent` |

**Purpose:** Count controlled remote-interactive failures and verify that the observed count meets or exceeds the proposed `AR-001` threshold.

**Expected result:** `ThresholdState` equals `ThresholdMetOrExceeded`, producing the same qualifying group as `Q-020`.

**Interpretation:** A qualifying query result is a detection result, not proof that Sentinel created an alert or incident.

**Limitations:** Alert and incident behavior require separate `TST-015` and `TST-016` records after the rule is implemented and explicitly enabled.

```kql
// Q-014 | version 1.0.0 | Threshold-matching detection check
let TestStart = ago(30m);
let TestEnd = now();
let TestComputer = "";
let TestSourceIp = "";
let FailureThreshold = 5;
SecurityEvent
| where TimeGenerated between (TestStart .. TestEnd)
| where EventID == 4625
| where isnotempty(TestComputer)
| where Computer =~ TestComputer
| extend
    SourceIp = tostring(column_ifexists("IpAddress", "")),
    LogonTypeValue = toint(column_ifexists("LogonType", int(null)))
| where LogonTypeValue == 10
| where isempty(TestSourceIp) or SourceIp == TestSourceIp
| summarize
    FailedLogonCount = count(),
    FirstFailure = min(TimeGenerated),
    LastFailure = max(TimeGenerated)
    by Computer, SourceIp
| extend
    DetectionThreshold = FailureThreshold,
    ThresholdState = iff(
        FailedLogonCount >= FailureThreshold,
        "ThresholdMetOrExceeded",
        "BelowThreshold"
    )
| project
    Computer,
    SourceIp,
    FirstFailure,
    LastFailure,
    FailedLogonCount,
    DetectionThreshold,
    ThresholdState
```

---

## Q-015 — Repeated-failure summary

| Field | Value |
|---|---|
| Category | Behavioral |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | Investigation support |
| Data source | `SecurityEvent` |

**Purpose:** Summarize repeated failed remote-interactive authentication by source, target account, host, and time bucket.

**Expected result:** Aggregated groups that expose count, time span, status, and account context without assigning intent.

**Interpretation:** Repeated failures may be controlled, benign, misconfigured, or suspicious. Triage determines disposition.

**Limitations:** The query requires validated source, account, host, and logon-type fields and can undercount events with missing values.

```kql
// Q-015 | version 1.0.0 | Repeated-failure summary
let QueryLookback = 24h;
let AggregationWindow = 5m;
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
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
| summarize
    FirstFailure = min(TimeGenerated),
    LastFailure = max(TimeGenerated),
    FailedLogonCount = count(),
    FailureReasons = make_set(FailureReason, 10),
    Statuses = make_set(Status, 10),
    SubStatuses = make_set(SubStatus, 10)
    by
        SourceIp,
        TargetAccountNormalized,
        Computer,
        bin(TimeGenerated, AggregationWindow)
| order by FailedLogonCount desc, LastFailure desc
```

---

## Q-016 — Single-account guessing pattern

| Field | Value |
|---|---|
| Category | Behavioral |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | Investigation support |
| Data source | `SecurityEvent` |

**Purpose:** Identify one source producing repeated remote-interactive failures against one target account on one host.

**Expected result:** Groups meeting the review threshold with one distinct target account.

**Interpretation:** The pattern may be consistent with password guessing after controlled-test and benign explanations are excluded.

**Limitations:** Do not label the activity brute force or malicious without source, test, account, and host context.

```kql
// Q-016 | version 1.0.0 | Single-account guessing pattern
let QueryLookback = 24h;
let AggregationWindow = 10m;
let ReviewThreshold = 5;
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
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
| where isnotempty(TargetAccountNormalized) and TargetAccountNormalized != "-"
| summarize
    FirstFailure = min(TimeGenerated),
    LastFailure = max(TimeGenerated),
    FailedLogonCount = count(),
    DistinctAccounts = dcount(TargetAccountNormalized),
    Accounts = make_set(TargetAccountNormalized, 10)
    by SourceIp, Computer, bin(TimeGenerated, AggregationWindow)
| where FailedLogonCount >= ReviewThreshold
| where DistinctAccounts == 1
| order by FailedLogonCount desc, LastFailure desc
```

---

## Q-017 — Multi-account spraying pattern

| Field | Value |
|---|---|
| Category | Behavioral |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | Investigation support |
| Data source | `SecurityEvent` |

**Purpose:** Identify one source producing remote-interactive failures across multiple target accounts on one host.

**Expected result:** Groups meeting both the failure-count and distinct-account review thresholds.

**Interpretation:** The pattern may be consistent with password spraying after controlled-test, scanner, and benign explanations are excluded.

**Limitations:** Account normalization and missing source IPs can materially change results. Thresholds require tuning from actual lab data.

```kql
// Q-017 | version 1.0.0 | Multi-account spraying pattern
let QueryLookback = 24h;
let AggregationWindow = 15m;
let FailureReviewThreshold = 5;
let DistinctAccountThreshold = 3;
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
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
| where isnotempty(TargetAccountNormalized) and TargetAccountNormalized != "-"
| summarize
    FirstFailure = min(TimeGenerated),
    LastFailure = max(TimeGenerated),
    FailedLogonCount = count(),
    DistinctAccounts = dcount(TargetAccountNormalized),
    Accounts = make_set(TargetAccountNormalized, 20)
    by SourceIp, Computer, bin(TimeGenerated, AggregationWindow)
| where FailedLogonCount >= FailureReviewThreshold
| where DistinctAccounts >= DistinctAccountThreshold
| order by DistinctAccounts desc, FailedLogonCount desc
```

---

## Q-018 — Controlled-versus-organic classification

| Field | Value |
|---|---|
| Category | Classification |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `TST-017` |
| Data source | `SecurityEvent` |

**Purpose:** Label authentication records as controlled only when they match the recorded test window, computer, and optional source and account selectors.

**Expected result:** Known test records are labeled `Controlled`; all other records remain `OrganicOrUnclassified`.

**Interpretation:** `OrganicOrUnclassified` does not mean malicious. It means the event did not satisfy the recorded controlled-test selectors.

**Limitations:** Incomplete test metadata can misclassify events. Record selectors privately and preserve uncertainty.

```kql
// Q-018 | version 1.0.0 | Controlled-versus-organic classification
let QueryLookback = 24h;
let TestStart = ago(30m);
let TestEnd = now();
let TestComputer = "";
let TestSourceIp = "";
let TestAccount = "";
SecurityEvent
| where TimeGenerated >= ago(QueryLookback)
| where EventID in (4624, 4625)
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
| extend ControlledSelectorReady = isnotempty(TestComputer)
| extend ActivityClass = case(
    ControlledSelectorReady
        and TimeGenerated between (TestStart .. TestEnd)
        and Computer =~ TestComputer
        and (isempty(TestSourceIp) or SourceIp == TestSourceIp)
        and (isempty(TestAccount) or TargetAccountNormalized =~ TestAccount),
    "Controlled",
    "OrganicOrUnclassified"
)
| project
    TimeGenerated,
    EventID,
    Computer,
    SourceIp,
    TargetAccountNormalized,
    LogonTypeValue,
    ActivityClass,
    ControlledSelectorReady,
    EventRecordId
| order by TimeGenerated desc
```

---

## Q-019 — Failed-to-successful temporal correlation

| Field | Value |
|---|---|
| Category | Correlation |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | Triage support |
| Data source | `SecurityEvent` |

**Purpose:** Find a successful remote-interactive logon that follows failed remote-interactive events for the same source, account, and host within a bounded interval.

**Expected result:** Correlated groups with failure count, first and last failure, and first subsequent success.

**Interpretation:** The sequence is an investigation lead. It does not establish that failures and success came from the same human or that the success was unauthorized.

**Limitations:** NAT, shared accounts, missing fields, time skew, and reused source addresses can create false associations.

```kql
// Q-019 | version 1.0.0 | Failed-to-successful temporal correlation
let QueryLookback = 24h;
let CorrelationWindow = 30m;
let Failures = SecurityEvent
    | where TimeGenerated >= ago(QueryLookback)
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
    | where isnotempty(TargetAccountNormalized) and TargetAccountNormalized != "-"
    | summarize
        FirstFailure = min(TimeGenerated),
        LastFailure = max(TimeGenerated),
        FailureCount = count()
        by SourceIp, TargetAccountNormalized, Computer;
let Successes = SecurityEvent
    | where TimeGenerated >= ago(QueryLookback)
    | where EventID == 4624
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
    | where isnotempty(TargetAccountNormalized) and TargetAccountNormalized != "-"
    | summarize FirstSuccess = min(TimeGenerated)
        by SourceIp, TargetAccountNormalized, Computer;
Failures
| join kind=inner Successes
    on SourceIp, TargetAccountNormalized, Computer
| where FirstSuccess between (FirstFailure .. LastFailure + CorrelationWindow)
| project
    SourceIp,
    TargetAccountNormalized,
    Computer,
    FirstFailure,
    LastFailure,
    FailureCount,
    FirstSuccess,
    CorrelationWindow
| order by FirstSuccess desc
```

---

## Q-020 — AR-001 canonical detection query

| Field | Value |
|---|---|
| Category | Detection |
| Query version | `1.0.0` |
| Lifecycle status | `Reviewed` |
| Execution status | `Blocked` |
| Primary mapping | `AR-001`, `TST-013`–`TST-016` |
| Data source | `SecurityEvent` |

**Purpose:** Define the canonical query for repeated failed remote-interactive authentication from one source to one host.

**Expected result:** One row per qualifying source, host, and time bucket when failed logons meet or exceed the configured threshold.

**Interpretation:** A returned row means the query condition was satisfied. It is not proof of compromise, malicious intent, or attacker identity.

**Limitations:** The query must remain disabled in `AR-001` until the `SecurityEvent` route, field population, source IP, host, logon type, test behavior, and rule configuration are validated.

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

---

## 8. Query validation record template

Use one record per authorized query execution:

    Query ID:
    Query version:
    Execution status:
    Test ID:
    UTC execution time:
    Operator:
    Branch:
    Commit:
    Tenant alias:
    Subscription alias:
    Workspace alias:
    Query time range:
    Selectors used:
    Expected result:
    Actual result:
    Field-quality findings:
    Latency findings:
    False-positive considerations:
    False-negative considerations:
    Evidence ID:
    Private evidence location:
    Public derivative:
    Reviewer:
    Follow-up action:
    Claim impact:

Do not place tenant IDs, subscription IDs, workspace IDs, resource IDs, public IP addresses, account names, email addresses, credentials, secrets, or private evidence paths in a public copy.

## 9. Catalog completion criteria

This catalog is documentation-complete when it:

- defines `Q-001` through `Q-020` with stable IDs;
- records lifecycle and execution status separately;
- places health, route, freshness, and schema queries before behavioral detections;
- distinguishes `SecurityEvent` from the generic `Event` route;
- treats Event ID 4625 as failed authentication rather than automatic RDP or attack proof;
- treats Event ID 4624 as a successful logon session rather than automatic authorization or compromise proof;
- requires logon type `10` or a validated equivalent for remote-interactive wording;
- includes controlled failed and successful test queries;
- includes below-threshold and threshold-matching validation helpers;
- preserves controlled-versus-organic classification boundaries;
- defines one canonical `Q-020` query for `AR-001`;
- keeps source IP and host entity use dependent on validated fields;
- defers account entity mapping until account-field quality is validated;
- contains no destructive query or automated response;
- preserves failed, blocked, and inconclusive outcomes; and
- does not claim that live query validation occurred.

## 10. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection and incident validation have not started.
- `SecurityEvent` routing and field population remain unvalidated.
- `Q-001` through `Q-020` are statically reviewed but unexecuted.
- Every query execution status remains `Blocked`.
- `AR-001` remains draft, disabled, undeployed, and unverified.
- No KQL result, alert, incident, compromise, or evidence claim is authorized.
- This catalog is design-stage detection-engineering documentation only.
