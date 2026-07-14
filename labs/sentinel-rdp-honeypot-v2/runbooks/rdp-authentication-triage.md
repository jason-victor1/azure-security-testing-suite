# RDP Authentication Triage Runbook

## 1. Purpose

This runbook defines the bounded investigation and response process for Windows remote-interactive authentication telemetry in the Azure Sentinel RDP Honeypot v2 lab.

It supports analysis of:

- failed Windows authentication events;
- successful Windows authentication events;
- expected controlled-test activity;
- organic internet-originated observations, when separately authorized;
- telemetry-health failures;
- unexpected successful remote-interactive access; and
- conditions requiring immediate exposure removal or teardown.

This document is an operational design artifact. Azure execution is paused, public TCP/3389 exposure is unauthorized, and no v2 execution evidence currently exists.

## 2. Authority and governing documents

Use this runbook together with:

1. [`../docs/threat-model.md`](../docs/threat-model.md);
2. [`../docs/scope-and-credibility-notes.md`](../docs/scope-and-credibility-notes.md);
3. [`../docs/checklist-to-project-roadmap.md`](../docs/checklist-to-project-roadmap.md);
4. [`deployment-runbook.md`](deployment-runbook.md);
5. [`teardown-runbook.md`](teardown-runbook.md);
6. [`cost-control-checklist.md`](cost-control-checklist.md);
7. [`../kql/hunting-queries.md`](../kql/hunting-queries.md);
8. [`../evidence/README.md`](../evidence/README.md);
9. [`../evidence/redaction-notes.md`](../evidence/redaction-notes.md); and
10. [`../evidence/templates/evidence-record-template.md`](../evidence/templates/evidence-record-template.md).

When instructions conflict, safety, authorization, cost containment, and evidence-governance controls take precedence.

## 3. Scope

### 3.1 In scope

This runbook covers:

- Windows Security Event ID `4625` failed-logon analysis;
- Windows Security Event ID `4624` successful-logon analysis;
- remote-interactive logon analysis, including Logon Type `10` when present;
- source-address, account, host, timestamp, status, and substatus review;
- comparison against an authorized controlled-test record;
- detection of repeated failures or suspicious source patterns;
- correlation of failures with subsequent successful authentication;
- telemetry-health and schema failures;
- incident disposition;
- stop-condition handling; and
- private evidence capture and public-evidence eligibility review.

### 3.2 Out of scope

This runbook does not authorize:

- Azure deployment;
- public TCP/3389 exposure;
- password spraying;
- credential stuffing;
- account-lockout testing;
- intentionally weak or guessable passwords;
- reuse of personal, production, or previously exposed credentials;
- persistence, malware, exploitation, privilege escalation, or lateral movement;
- production incident response;
- third-party targeting;
- attribution of an observed source to a person or organization; or
- publication of raw identifiers, credentials, tenant data, resource IDs, or source addresses.

## 4. Safety principles

The following principles are mandatory:

1. **Safety takes priority over evidence preservation.**
2. **Telemetry validation must precede any public exposure.**
3. A missing screenshot is preferable to extending an unsafe condition.
4. An unexpected successful remote-interactive logon is an emergency condition.
5. Exposure must not remain open while telemetry is absent, delayed, or materially incomplete.
6. No weak, reused, or intentionally guessable credentials may be used.
7. Raw evidence remains private until sanitization, review, and approval are complete.
8. Uncertainty is resolved by stopping or blocking activity, not by assuming safety.
9. Failed, blocked, and inconclusive tests remain part of the evidence record.
10. A successful portal action does not prove telemetry, detection, response, or teardown effectiveness.

## 5. Operating modes

### 5.1 Normal mode

Normal mode applies only when:

- the applicable authorization gate has passed;
- the lab scope and subscription are confirmed;
- the VM and logging path are known;
- Log Analytics ingestion is healthy;
- expected Windows Security Event fields are available;
- the intended test window is recorded;
- the operator understands all stop conditions;
- teardown is executable; and
- cost controls are active.

Normal mode permits bounded analysis and an explicitly authorized controlled authentication test.

Normal mode does not independently authorize public exposure.

### 5.2 Emergency mode

Emergency mode begins immediately when any mandatory stop condition occurs.

Emergency priorities are:

1. remove or restrict TCP/3389 exposure;
2. stop controlled testing;
3. deallocate the VM when appropriate;
4. begin emergency teardown when required;
5. preserve only evidence that can be collected without delaying containment;
6. record the triggering condition and actions privately; and
7. reassess cost, identity, and residual-resource state.

Emergency mode does not wait for evidence completeness.

## 6. Required case inputs

Create a private case record before analysis.

Use explicit values for:

| Field | Requirement |
|---|---|
| Case identifier | Local private identifier; do not expose sensitive values publicly |
| Operator | Authorized operator name or private alias |
| Review start | UTC timestamp |
| Review end | UTC timestamp |
| Test identifier | Applicable `TST-###` value or `Not Applicable` |
| Expected activity | Controlled failure, controlled success, organic observation, or health validation |
| Expected source | Private source-address record or `Not Applicable` |
| Expected account | Private account alias; never record the password |
| Expected host | Private VM or host alias |
| Expected event IDs | Normally `4625`, `4624`, or both |
| Expected logon type | Logon Type `10` when validating RDP remote-interactive activity |
| Authorization record | Reference to the applicable go/no-go decision |
| Private evidence location | Approved nonrepository location |
| Cost-control state | Confirmed, blocked, or unknown |
| Teardown readiness | Confirmed, blocked, or unknown |

Do not begin analysis when the time range, expected activity, authorization state, or target host is unknown.

## 7. Preconditions and no-go gate

Before any controlled test or exposure-dependent analysis, confirm:

- [ ] The correct repository branch and commit are recorded.
- [ ] The working tree contains only intended changes.
- [ ] Azure execution has been separately authorized.
- [ ] The correct subscription and resource group are confirmed.
- [ ] The lab contains no production data.
- [ ] The account credential is strong, unique, and private.
- [ ] The credential is not stored in Git, shell history, screenshots, tickets, or public notes.
- [ ] The Log Analytics workspace is receiving current telemetry.
- [ ] The expected `SecurityEvent` schema has been validated.
- [ ] Event timestamps are interpretable in UTC.
- [ ] The VM identity and expected computer field are known.
- [ ] Teardown and emergency response steps are executable.
- [ ] Cost controls and observation-time boundaries are active.
- [ ] The private evidence location is ready.
- [ ] Public sanitization and approval rules are understood.

If any mandatory item fails, record the activity as `Blocked` and stop.

## 8. Telemetry-validation gate

Telemetry validation must be completed before any separately authorized public TCP/3389 observation.

### 8.1 Validate the collection path

Confirm that:

1. Azure Monitor Agent health is known;
2. the applicable Data Collection Rule association is known;
3. the Log Analytics workspace is receiving recent `SecurityEvent` records;
4. Event ID `4625` can be retrieved from an authorized restricted test;
5. expected fields are populated sufficiently for investigation;
6. timestamps align with the private test record;
7. the expected host can be distinguished;
8. ingestion delay is understood; and
9. the result is recorded privately.

### 8.2 Minimum useful fields

Review the available equivalents of:

- `TimeGenerated`;
- `EventID`;
- `Computer`;
- `TargetUserName`;
- `SubjectUserName`;
- `IpAddress`;
- `WorkstationName`;
- `LogonType`;
- `Status`;
- `SubStatus`;
- `Activity`; and
- event-rendered description fields when required.

Field availability can vary by collection method and event shape. Do not claim a field was validated unless it was observed in v2 telemetry.

### 8.3 Telemetry no-go conditions

Do not proceed to exposure-dependent activity when:

- no current events are arriving;
- ingestion delay cannot be bounded;
- the expected host cannot be identified;
- the event schema is materially incomplete;
- timestamps cannot be correlated;
- the test event cannot be distinguished from unrelated activity; or
- workspace or DCR state is uncertain.

Record the outcome as `Failed`, `Blocked`, or `Inconclusive` as appropriate.

## 9. Controlled authentication-test procedure

A controlled test requires separate authorization.

### 9.1 Failed-authentication test

1. Record the UTC start time.
2. Confirm the expected source, account alias, host alias, and test identifier.
3. Confirm the account uses a strong private credential.
4. Perform only the minimum authorized failed authentication attempt.
5. Do not intentionally trigger an account lockout.
6. Record the UTC completion time.
7. Query for the expected Event ID `4625`.
8. Confirm the source, account alias, host, timestamp, and Logon Type where available.
9. Record ingestion delay.
10. Mark the test `Passed`, `Failed`, `Blocked`, or `Inconclusive`.
11. Stop testing after the minimum evidence requirement is satisfied.

### 9.2 Successful-authentication test

A successful-authentication test is not implied by authorization for a failed-authentication test.

Before a controlled success:

- obtain explicit authorization;
- confirm the credential remains private;
- confirm the expected source and time window;
- confirm telemetry health;
- confirm no unexplained successful authentication exists;
- record the expected Event ID `4624`;
- use only the isolated lab account; and
- terminate the session after the minimum authorized validation.

If success occurs unexpectedly, enter emergency mode.

## 10. Normal triage workflow

### Step 1 — Open the private case record

Record:

- purpose;
- time range;
- expected activity;
- applicable test identifier;
- authorization state;
- operator;
- expected source, account, and host aliases; and
- private evidence location.

### Step 2 — Confirm telemetry integrity

Verify:

- recent event ingestion;
- host identity;
- usable timestamps;
- expected event IDs;
- expected fields;
- known ingestion delay; and
- absence of a telemetry-health stop condition.

Do not interpret activity until telemetry integrity is sufficient.

### Step 3 — Identify the event set

Retrieve the minimum required time range and isolate:

- Event ID `4625`;
- Event ID `4624`;
- Logon Type `10` where present;
- the intended host;
- the expected account alias;
- the expected source; and
- nearby events needed for correlation.

Avoid exporting unrelated records.

### Step 4 — Establish the pattern

Group or summarize events by:

- source address;
- target account;
- computer;
- event ID;
- logon type;
- status and substatus;
- time interval; and
- expected versus unexpected origin.

Identify:

- isolated failures;
- repeated failures from one source;
- failures distributed across accounts;
- failures distributed across sources;
- changes in rate;
- activity outside the authorized window; and
- gaps suggesting ingestion or collection failure.

### Step 5 — Pivot from failures to success

For suspicious failures, check for related Event ID `4624` activity involving:

- the same source;
- the same account;
- the same host;
- the same time window; or
- an unexplained remote-interactive logon.

A failure-to-success sequence is not automatically proof of compromise, but it requires heightened review.

An unexpected successful remote-interactive logon triggers emergency mode.

### Step 6 — Compare against the controlled-test record

Determine whether the observed activity matches:

- expected source;
- expected account alias;
- expected host;
- expected timestamp;
- expected event ID;
- expected logon type; and
- expected number of attempts.

Do not label unmatched activity as controlled testing.

### Step 7 — Assign a disposition

Use one primary disposition:

| Disposition | Meaning |
|---|---|
| Expected controlled test | Event matches the authorized private test record |
| Organic failed-authentication observation | Unplanned failure activity with no observed success |
| Suspected scanning or brute-force pattern | Repeated or distributed failures warrant further analysis |
| Unexpected successful authentication | Unplanned success requiring emergency response |
| Telemetry failure | Collection, schema, timing, or ingestion is insufficient |
| Benign or explained administrative activity | Activity is supported by an authorized record |
| Blocked | Required authorization or precondition was absent |
| Inconclusive | Available evidence cannot support a reliable conclusion |
| Not Applicable | The test or analysis does not apply |

### Step 8 — Determine response

Apply the response mapped to the disposition and stop conditions.

### Step 9 — Record evidence privately

Preserve:

- query or test identifier;
- UTC time range;
- summarized result;
- test outcome;
- disposition;
- response actions;
- evidence-location reference;
- redaction status;
- reviewer state; and
- claim-maturity effect.

Do not add a concrete public `EV-###` record until approval requirements are satisfied.

### Step 10 — Close or escalate

A case can close only when:

- the disposition is recorded;
- required containment is complete;
- cost state is known;
- residual-resource state is known when teardown occurred;
- evidence remains private or has completed approval;
- failed or inconclusive outcomes are preserved; and
- no unsupported claim was promoted.

## 11. Investigation decision matrix

| Condition | Initial disposition | Required action |
|---|---|---|
| Expected `4625` matches the controlled-test record | Expected controlled test | Record test outcome and stop further attempts |
| Unexpected `4625` with no observed success | Organic observation | Analyze rate, source, account, and time window |
| Repeated `4625` events from one source | Suspected brute-force pattern | Continue bounded analysis; do not attribute identity |
| Distributed failures across accounts or sources | Suspected scanning or password-spray pattern | Escalate analysis without performing counter-testing |
| Expected `4624` matches separately authorized success test | Expected controlled test | End the session and record the result |
| Unexpected `4624`, especially Logon Type `10` | Unexpected successful authentication | Enter emergency mode immediately |
| Failures followed by unexplained success | Potential compromise condition | Enter emergency mode and preserve safe private evidence |
| Expected test has no corresponding telemetry | Telemetry failure | Stop testing; repair collection before proceeding |
| Event timestamps or fields cannot be correlated | Inconclusive | Stop exposure-dependent activity |
| Source, account, or host differs from the test record | Unexplained activity | Treat as organic or suspicious, not controlled |
| Authorization cannot be verified | Blocked | Do not continue |
| Cost or residual-resource state is unknown | Operational risk | Stop activity and begin cost or teardown review |

## 12. Mandatory stop conditions and response actions

| Stop condition | Immediate response |
|---|---|
| Unexpected successful remote-interactive authentication | Remove or restrict TCP/3389 exposure, stop testing, deallocate when appropriate, begin emergency teardown review |
| Telemetry stops, becomes stale, or cannot be interpreted | Stop testing and exposure-dependent activity; repair telemetry before resuming |
| Unknown or unauthorized resource appears | Stop activity; identify ownership; begin emergency containment or teardown |
| Subscription or tenant context is uncertain | Stop all Azure actions until context is revalidated |
| Credential exposure or suspected credential compromise | Stop testing; remove exposure; rotate or invalidate affected credentials through an approved process |
| Account-lockout behavior occurs unexpectedly | Stop authentication testing and preserve the failed-test record |
| VM configuration changes unexpectedly | Stop observation; assess integrity; deallocate or teardown as required |
| NSG permits access beyond the approved rule | Remove the unsafe rule and enter emergency review |
| Evidence capture would delay containment | Skip or stop capture and contain the environment |
| Cost threshold, runtime boundary, or daily-cap condition is exceeded | Stop the lab and follow the cost-control and teardown runbooks |
| Unexplained administrative or successful login occurs | Enter emergency mode and treat the activity as untrusted |
| Operator cannot determine whether activity is safe | Stop and classify the result as blocked or inconclusive |

## 13. Emergency-mode procedure

1. Record the trigger time privately when doing so does not delay containment.
2. Stop controlled authentication activity.
3. Remove or restrict public TCP/3389 exposure.
4. Deallocate the VM when appropriate.
5. Initiate the emergency path in [`teardown-runbook.md`](teardown-runbook.md).
6. Preserve only immediately available private evidence that does not delay containment.
7. Record the known source, account, host, event ID, and time window privately.
8. Do not publish screenshots or raw logs.
9. Check for unknown Azure resources or configuration changes.
10. Reassess subscription context and credential state.
11. Perform the applicable cost-control checks.
12. Record the event outcome as `Failed`, `Blocked`, or `Inconclusive` unless validated evidence supports another outcome.
13. Do not resume activity without a new go/no-go decision.

## 14. Evidence and claim handling

### 14.1 Private evidence

Private evidence may include:

- raw query exports;
- raw Windows events;
- source addresses;
- account names;
- computer names;
- subscription or tenant identifiers;
- resource IDs;
- portal screenshots;
- timestamps;
- incident details; and
- test-operation notes.

Store raw evidence outside the repository.

### 14.2 Public evidence

Public evidence requires:

1. sanitization;
2. redaction review;
3. technical review;
4. claim-boundary review;
5. approval for publication;
6. manifest registration; and
7. a SHA-256 integrity record when required.

Redaction is not approval. Sanitization is not validation.

### 14.3 Test outcomes and claim maturity

Keep these dimensions separate:

- evidence lifecycle;
- test outcome;
- incident disposition; and
- claim maturity.

A completed runbook does not demonstrate that Azure deployment, telemetry, detection, triage, or teardown succeeded.

## 15. Case-record template

    Case identifier:
    Operator:
    Review start UTC:
    Review end UTC:
    Authorization record:
    Test identifier:
    Expected activity:
    Expected source alias:
    Expected account alias:
    Expected host alias:
    Expected event IDs:
    Expected logon type:
    Observed event count:
    Observed source summary:
    Observed account summary:
    Observed host summary:
    Telemetry-health state:
    Ingestion-delay observation:
    Disposition:
    Stop condition triggered:
    Response actions:
    Test outcome:
    Private evidence location:
    Evidence lifecycle:
    Claim-maturity effect:
    Reviewer:
    Closure decision:

Do not enter passwords, secrets, raw tokens, tenant IDs, subscription IDs, globally routable source addresses, or other sensitive values in a public copy.

## 16. Handoff requirements

A handoff must state:

- what activity was expected;
- what was observed;
- whether telemetry was healthy;
- whether any successful authentication occurred;
- whether a stop condition triggered;
- which response actions were completed;
- whether the VM remains allocated;
- whether TCP/3389 remains exposed;
- current cost-control state;
- current teardown state;
- evidence location and lifecycle;
- unresolved questions; and
- whether another authorization decision is required.

## 17. Completion criteria

This runbook is operationally complete when it:

- distinguishes normal and emergency modes;
- requires telemetry validation before exposure;
- makes safety more important than evidence preservation;
- separates controlled testing from organic activity;
- prohibits weak, reused, or intentionally guessable credentials;
- defines failed- and successful-authentication triage;
- maps every mandatory stop condition to a response;
- preserves failed, blocked, and inconclusive outcomes;
- integrates evidence-governance requirements;
- references deployment, cost-control, teardown, KQL, and evidence artifacts;
- contains no secrets or live environment identifiers; and
- does not claim that v2 execution has occurred.

## 18. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- No v2 execution evidence exists.
- No concrete public evidence record has been approved.
- This runbook is design-stage operational documentation only.
