# Evidence Record Template

## Purpose

Use this template to document one Sentinel RDP Honeypot v2 evidence record.

The template separates:

- evidence lifecycle status;
- test outcome;
- technical observations;
- private raw evidence;
- sanitized public derivatives;
- review and approval;
- claim relationships;
- limitations and failed-test history.

Creating or completing a record does not automatically authorize publication or
claim promotion.

A populated record must remain private unless its public derivative has
completed the required technical, sanitization, redaction, provenance, and
approval reviews.

## Current project-state notice

At the creation of this template:

- Azure execution is paused.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- No v2 execution evidence has been captured.
- No populated v2 evidence record is authorized for Git.
- Public implementation and validation claims remain blocked.

This notice describes the project state when the template was created. A future
record must state the actual verified project state at its capture time.

## Template instructions

1. Copy this template into the approved private evidence workspace.
2. Assign a unique `EV-###` identifier.
3. Define the evidence purpose before execution.
4. Link applicable `TST-###`, `CLM-###`, `Q-###`, and `AR-###` identifiers.
5. Confirm authorization and safety prerequisites.
6. Record the actual test outcome independently from evidence lifecycle status.
7. Preserve raw evidence outside Git.
8. Calculate SHA-256 hashes where appropriate.
9. Create a separate sanitized derivative for public review.
10. Preserve failed, blocked, and inconclusive results.
11. Do not mark an artifact `Approved-Public` until all required reviews pass.
12. Add an artifact to `manifest.md` only after public approval.
13. Promote a claim only through `claim-evidence-matrix.md`.
14. Never include credentials, restricted identifiers, active endpoints, or
    private paths in a public derivative.

Remove instructional text only in the private working copy. Do not weaken the
required fields or claim boundaries.

---

# Evidence Record: EV-###

## 1. Record identity

| Field | Value |
|---|---|
| Evidence ID | `EV-###` |
| Record title | `[REQUIRED]` |
| Artifact type | `[Screenshot / command output / configuration / KQL / authentication event / analytics rule / alert / incident object / teardown / cost / other]` |
| Record owner | `[PRIVATE OR PUBLIC-SAFE IDENTIFIER]` |
| Record version | `[REQUIRED]` |
| Created at UTC | `[YYYY-MM-DDTHH:MM:SSZ]` |
| Last updated at UTC | `[YYYY-MM-DDTHH:MM:SSZ]` |
| Supersedes | `[EV-### / N/A]` |
| Superseded by | `[EV-### / N/A]` |

The evidence ID must not be reused for an unrelated artifact.

## 2. Identifier relationships

| Relationship | Identifier or value |
|---|---|
| Related test | `[TST-### / N/A with rationale]` |
| Related claim | `[CLM-### / N/A with rationale]` |
| Related query | `[Q-### / N/A with rationale]` |
| Related analytics rule | `[AR-### / N/A with rationale]` |
| Related control | `[CONTROL ID / N/A]` |
| Related risk | `[RISK ID / N/A]` |
| Related stop condition | `[STOP-## / N/A]` |
| Related change record | `[REFERENCE / N/A]` |

Use `N/A` only after confirming that the relationship does not apply. Do not use
`N/A` to hide incomplete analysis.

## 3. Evidence purpose

### Intended purpose

[State exactly what this record is intended to establish.]

### Supported proposition

[State the narrow factual proposition the artifact may support.]

### Explicitly unsupported propositions

[List conclusions that this artifact cannot establish.]

Examples include:

- successful compromise;
- malicious intent;
- human attribution;
- compliance;
- authorization;
- complete teardown;
- final cost;
- project completion.

## 4. Project and authorization state

| Field | Verified value |
|---|---|
| Azure execution authorized | `[Yes / No]` |
| Public TCP/3389 exposure authorized | `[Yes / No / Not Applicable]` |
| Controlled testing authorized | `[Yes / No / Not Applicable]` |
| Evidence capture authorized | `[Yes / No]` |
| Approved execution window | `[UTC interval / N/A]` |
| Approved maximum runtime | `[VALUE / N/A]` |
| Approved maximum cost | `[PRIVATE VALUE OR PUBLIC-SAFE SUMMARY / N/A]` |
| Operator available | `[Yes / No / N/A]` |
| Teardown owner available | `[Yes / No / N/A]` |
| Mandatory stop conditions reviewed | `[Yes / No / N/A]` |

### Authorization source

[Identify the approved go/no-go record or explain why the record concerns a
non-execution design activity.]

### Safety exceptions

[List approved exceptions. Use `None` when no exception exists.]

An evidence record must not be used to retroactively authorize activity.

## 5. Evidence lifecycle status

Select one value:

- [ ] `Planned`
- [ ] `Captured-Private`
- [ ] `Sanitized`
- [ ] `Reviewed`
- [ ] `Approved-Public`
- [ ] `Rejected`
- [ ] `Superseded`

### Lifecycle rationale

[Explain why the selected lifecycle status is accurate.]

### Lifecycle history

| UTC timestamp | Previous status | New status | Reviewer or owner | Reason |
|---|---|---|---|---|
| `[YYYY-MM-DDTHH:MM:SSZ]` | `[STATUS]` | `[STATUS]` | `[IDENTIFIER]` | `[REASON]` |

Do not mark a planned artifact `Captured-Private`. Do not mark a sanitized
artifact `Approved-Public` without completed review.

## 6. Test outcome

Select one value independently from lifecycle status:

- [ ] `Not Run`
- [ ] `Passed`
- [ ] `Failed`
- [ ] `Blocked`
- [ ] `Inconclusive`
- [ ] `Not Applicable`

### Outcome rationale

[Explain why the selected outcome is accurate.]

A record may be `Approved-Public` while documenting a `Failed`, `Blocked`, or
`Inconclusive` test.

## 7. Test definition

| Field | Value |
|---|---|
| Test ID | `[TST-### / N/A]` |
| Test objective | `[REQUIRED WHEN TEST APPLIES]` |
| Configuration version | `[REQUIRED WHEN TEST APPLIES]` |
| Query version | `[Q-### VERSION / N/A]` |
| Analytics-rule version | `[AR-### VERSION / N/A]` |
| Planned start UTC | `[YYYY-MM-DDTHH:MM:SSZ / N/A]` |
| Planned end UTC | `[YYYY-MM-DDTHH:MM:SSZ / N/A]` |
| Actual start UTC | `[YYYY-MM-DDTHH:MM:SSZ / NOT RUN]` |
| Actual end UTC | `[YYYY-MM-DDTHH:MM:SSZ / NOT RUN]` |
| Execution source | `[CONTROLLED SOURCE / N/A]` |
| Synthetic account | `[PRIVATE REFERENCE / N/A]` |
| Target system | `[PUBLIC-SAFE IDENTIFIER / PRIVATE REFERENCE]` |

### Preconditions

- [ ] Required authorization exists.
- [ ] Expected configuration version is deployed.
- [ ] Monitoring prerequisites are healthy.
- [ ] Required containment actions are available.
- [ ] Cost and runtime controls are active.
- [ ] Evidence storage is ready.
- [ ] No mandatory stop condition is active.
- [ ] Preconditions are not applicable because the test was not run.

### Acceptance criteria

1. `[CRITERION 1]`
2. `[CRITERION 2]`
3. `[CRITERION 3]`

### Expected result

[Document the expected result before execution.]

## 8. Execution record

### Procedure performed

[Record the authorized procedure or safe equivalent.]

Do not include passwords, secrets, tokens, signed URLs, or active sensitive
identifiers.

### Commands or actions

    [SANITIZED COMMAND OR ACTION]
    [SANITIZED COMMAND OR ACTION]

### Tool and version information

| Tool or service | Version or service state |
|---|---|
| `[TOOL]` | `[VERSION]` |
| `[AZURE SERVICE]` | `[CONFIGURATION OR API STATE]` |

### Actual observed result

[Record what actually occurred, including unexpected behavior.]

### Exit status or platform result

`[EXIT CODE / PLATFORM STATUS / N/A]`

### Warnings and errors

[List all material warnings and errors. Use `None` only when none occurred.]

### Mandatory stop-condition evaluation

| Stop condition | Triggered | Action taken |
|---|---|---|
| `[STOP-## / DESCRIPTION]` | `[Yes / No]` | `[ACTION / N/A]` |

Safety and containment take priority over completing evidence capture.

## 9. Technical observations

### Observed facts

- `[FACT 1]`
- `[FACT 2]`
- `[FACT 3]`

### Analyst interpretation

[Separate interpretation from directly observed facts.]

### Alternative explanations

[List plausible alternatives and unresolved hypotheses.]

### Confidence

`[High / Medium / Low]`

### Confidence rationale

[Explain the basis and limits of the confidence assessment.]

## 10. Authentication-event fields

Complete when the record concerns Windows authentication telemetry.

| Field | Observed value or status |
|---|---|
| Event ID | `[4625 / 4624 / OTHER / N/A]` |
| Table | `[VALIDATED TABLE / UNVALIDATED / N/A]` |
| Source computer | `[PUBLIC-SAFE VALUE / PRIVATE REFERENCE / N/A]` |
| Event timestamp UTC | `[VALUE / N/A]` |
| Ingestion timestamp UTC | `[VALUE / N/A]` |
| Logon type | `[OBSERVED VALUE / NOT AVAILABLE / N/A]` |
| Remote-interactive context validated | `[Yes / No / Inconclusive / N/A]` |
| Account field quality | `[ASSESSMENT / N/A]` |
| Source-address field quality | `[ASSESSMENT / N/A]` |
| Host field quality | `[ASSESSMENT / N/A]` |
| Controlled-test correlation | `[CONFIRMED / NOT CONFIRMED / N/A]` |

### Authentication claim boundary

Event ID 4625 means a failed logon. It is not intrinsically RDP.

Event ID 4624 means a successful logon. It is not automatically malicious.

[Document the strongest wording supported by the observed context.]

## 11. AMA and DCR fields

Complete when the record concerns Azure Monitor Agent or a Data Collection Rule.

| Field | Observed value or status |
|---|---|
| AMA installation or extension state | `[VALUE / N/A]` |
| Intended VM association | `[CONFIRMED / NOT CONFIRMED / N/A]` |
| DCR version | `[VALUE / N/A]` |
| DCR association | `[CONFIRMED / NOT CONFIRMED / N/A]` |
| Collection source | `[SANITIZED VALUE / N/A]` |
| Destination | `[SANITIZED VALUE / N/A]` |
| Expected table | `[VALUE / N/A]` |
| Actual table | `[VALUE / UNVALIDATED / N/A]` |
| Telemetry freshness | `[VALUE / UNVALIDATED / N/A]` |
| Ingestion gaps | `[VALUE / NONE OBSERVED / N/A]` |

### Validation boundary

Configuration or association evidence alone does not prove successful telemetry
ingestion.

## 12. KQL record

Complete when the record concerns a KQL query or query result.

| Field | Value |
|---|---|
| Query ID | `[Q-### / N/A]` |
| Query version | `[VERSION / N/A]` |
| Repository path | `[PUBLIC PATH / N/A]` |
| Target table | `[VALUE / N/A]` |
| Query time range | `[UTC RANGE / N/A]` |
| Execution time UTC | `[VALUE / N/A]` |
| Result count | `[VALUE / N/A]` |
| Aggregation used | `[DESCRIPTION / NONE / N/A]` |
| Controlled-test filter | `[DESCRIPTION / NONE / N/A]` |
| Schema assumptions | `[DESCRIPTION / N/A]` |

### Query limitations

[Document known false positives, false negatives, missing fields, or unvalidated
assumptions.]

Rows returned by a query do not independently prove malicious activity,
compromise, or attribution.

## 13. Analytics-rule, alert, and incident fields

Complete when applicable.

| Field | Value |
|---|---|
| Analytics-rule ID | `[AR-### / N/A]` |
| Rule version | `[VERSION / N/A]` |
| Rule state | `[Draft / Disabled / Enabled / N/A]` |
| Severity | `[VALUE / N/A]` |
| Cadence | `[VALUE / N/A]` |
| Lookback | `[VALUE / N/A]` |
| Threshold | `[VALUE / N/A]` |
| Entity mappings | `[VALIDATED MAPPINGS / DEFERRED / N/A]` |
| Alert created | `[Yes / No / Not Tested / N/A]` |
| Incident object created or correlated | `[Yes / No / Not Tested / N/A]` |
| Alert or incident identifier | `[PRIVATE REFERENCE / N/A]` |
| Disposition | `[CONTROLLED / BENIGN / SUSPICIOUS / UNDETERMINED / UNAUTHORIZED / N/A]` |

### Detection boundary

An alert demonstrates that configured detection logic generated an alert under
the observed conditions.

An incident object demonstrates that the platform created or correlated an
investigation object.

Neither independently proves compromise.

## 14. Controlled-versus-organic classification

| Field | Value |
|---|---|
| Test-session ID | `[PRIVATE OR PUBLIC-SAFE IDENTIFIER / N/A]` |
| Approved test source | `[PRIVATE REFERENCE / N/A]` |
| Synthetic account | `[PRIVATE REFERENCE / N/A]` |
| Explicit test window | `[UTC RANGE / N/A]` |
| Source matches approved test source | `[Yes / No / Inconclusive / N/A]` |
| Account matches controlled test | `[Yes / No / Inconclusive / N/A]` |
| Timing matches controlled test | `[Yes / No / Inconclusive / N/A]` |
| Final classification | `[Controlled / Organic / Undetermined / N/A]` |

Organic activity must not be retroactively relabeled as controlled testing.

## 15. Private raw-evidence inventory

This section belongs in the private record only.

| Item | Private reference | Raw SHA-256 | Capture time UTC | Notes |
|---|---|---|---|---|
| `[RAW ITEM 1]` | `[PRIVATE LOCATOR]` | `[SHA-256 / N/A]` | `[UTC]` | `[NOTES]` |
| `[RAW ITEM 2]` | `[PRIVATE LOCATOR]` | `[SHA-256 / N/A]` | `[UTC]` | `[NOTES]` |

Do not copy private locators into a public repository record.

Recommended integrity command:

    shasum -a 256 <private-artifact>

A hash proves file consistency only. It does not prove authenticity, accuracy,
completeness, or claim sufficiency.

## 16. Sanitized public derivative

| Field | Value |
|---|---|
| Public derivative created | `[Yes / No]` |
| Public-safe filename | `[VALUE / N/A]` |
| Proposed repository path | `[RELATIVE PATH / N/A]` |
| Derivative SHA-256 | `[VALUE / N/A]` |
| Sanitization time UTC | `[VALUE / N/A]` |
| Sanitization tool or method | `[VALUE / N/A]` |
| Original artifact type | `[VALUE / N/A]` |
| Aggregation performed | `[DESCRIPTION / NONE / N/A]` |
| Placeholders introduced | `[LIST / NONE / N/A]` |
| Hidden metadata reviewed | `[Yes / No / N/A]` |

### Values or regions removed

- `[REMOVED VALUE TYPE OR REGION]`
- `[REMOVED VALUE TYPE OR REGION]`

### Technical-meaning assessment

[Explain why sanitization preserved or did not preserve the technical meaning.]

### Public derivative limitations

[List limitations introduced by cropping, aggregation, redaction, reconstruction,
or omitted context.]

## 17. Sensitive-data and disclosure review

Confirm the public derivative contains none of the following unless an explicit
reviewed exception exists:

- [ ] Credentials or secrets
- [ ] Authentication tokens or session data
- [ ] Personal email addresses or usernames
- [ ] Tenant IDs
- [ ] Subscription IDs
- [ ] Complete Azure resource IDs
- [ ] Active public IP addresses or DNS names
- [ ] Unnecessary private IP addresses or topology
- [ ] Local usernames or device names
- [ ] Private filesystem paths
- [ ] Billing-account or payment information
- [ ] Private course material
- [ ] Unlicensed third-party material
- [ ] Unsupported attribution
- [ ] Hidden document or image metadata
- [ ] Unrelated desktop, browser, or notification content

### Exceptions

[Document reviewed exceptions or state `None`.]

## 18. Provenance and licensing review

| Field | Value |
|---|---|
| Artifact created entirely by project operator | `[Yes / No]` |
| Third-party content present | `[Yes / No]` |
| Source identified | `[Yes / No / N/A]` |
| License identified | `[Yes / No / N/A]` |
| Redistribution permitted | `[Yes / No / N/A]` |
| Attribution required | `[Yes / No / N/A]` |
| Malware and secret scanning required | `[Yes / No / N/A]` |
| Malware and secret scanning completed | `[Yes / No / N/A]` |
| Provenance decision | `[Approved / Rejected / Pending / N/A]` |

### Provenance notes

[Document source, license, attribution, integrity, and quarantine decisions.]

## 19. Technical and redaction review

| Review | Reviewer | UTC date | Decision | Notes |
|---|---|---|---|---|
| Technical accuracy | `[IDENTIFIER]` | `[UTC]` | `[Pass / Fail / Pending]` | `[NOTES]` |
| Sensitive-data review | `[IDENTIFIER]` | `[UTC]` | `[Pass / Fail / Pending]` | `[NOTES]` |
| Hidden-metadata review | `[IDENTIFIER]` | `[UTC]` | `[Pass / Fail / Pending]` | `[NOTES]` |
| Provenance and license | `[IDENTIFIER]` | `[UTC]` | `[Pass / Fail / N/A / Pending]` | `[NOTES]` |
| Claim alignment | `[IDENTIFIER]` | `[UTC]` | `[Pass / Fail / Pending]` | `[NOTES]` |

## 20. Public-release decision

Select one:

- [ ] Approved for public release
- [ ] Rejected for public release
- [ ] Returned for additional sanitization
- [ ] Pending review
- [ ] Private record only

| Field | Value |
|---|---|
| Final lifecycle status | `[STATUS]` |
| Decision owner | `[PUBLIC-SAFE IDENTIFIER]` |
| Decision time UTC | `[VALUE / PENDING]` |
| Manifest entry required | `[Yes / No]` |
| Manifest updated | `[Yes / No / N/A]` |
| Claim matrix update required | `[Yes / No]` |
| Claim matrix updated | `[Yes / No / N/A]` |

### Decision rationale

[Explain the release, rejection, return, or private-only decision.]

Public approval does not mean the associated technical test passed.

## 21. Claim assessment

| Field | Value |
|---|---|
| Related claim ID | `[CLM-### / N/A]` |
| Existing claim maturity | `[Designed / Implemented / Validated / Demonstrated / Completed / N/A]` |
| Proposed maturity | `[VALUE / NO CHANGE]` |
| Evidence sufficient for promotion | `[Yes / No / Inconclusive]` |
| Exact proposed wording | `[WORDING / N/A]` |
| Limitations included | `[Yes / No / N/A]` |
| Claim-matrix decision | `[Approved / Blocked / Regress / Retired / Superseded / Pending]` |

### Claim assessment rationale

[Explain why the evidence does or does not support the proposed wording.]

A manifest entry does not automatically authorize claim promotion.

## 22. Failed, blocked, or inconclusive result preservation

Complete when the test outcome is `Failed`, `Blocked`, or `Inconclusive`.

### Failed or unmet acceptance criterion

[Record the exact criterion.]

### Observed error or blocking condition

[Record what occurred.]

### Known cause

[State the verified cause or `Unknown`.]

### Unresolved hypotheses

- `[HYPOTHESIS 1]`
- `[HYPOTHESIS 2]`

### Corrective action

[Record the planned or completed corrective action.]

### Retest relationship

| Field | Value |
|---|---|
| Retest planned | `[Yes / No]` |
| Retest ID | `[TST-### / N/A]` |
| Replacement evidence ID | `[EV-### / N/A]` |
| Earlier outcome preserved | `[Yes / No]` |

A successful retest must not erase this record.

## 23. Containment and teardown record

Complete when containment, cleanup, or closure applies.

| Field | Value |
|---|---|
| Public TCP/3389 removed | `[Yes / No / N/A]` |
| Effective exposure rechecked | `[Yes / No / N/A]` |
| VM deallocated | `[Yes / No / N/A]` |
| VM deleted | `[Yes / No / N/A]` |
| Resource-group deletion requested | `[Yes / No / N/A]` |
| Resource-group terminal state verified | `[Yes / No / N/A]` |
| Subscription-wide orphan search completed | `[Yes / No / N/A]` |
| Residual resources found | `[DESCRIPTION / NONE / N/A]` |
| Identities and role assignments dispositioned | `[Yes / No / N/A]` |
| DCR associations dispositioned | `[Yes / No / N/A]` |
| Analytics objects dispositioned | `[Yes / No / N/A]` |
| Telemetry disposition recorded | `[Yes / No / N/A]` |
| Private evidence disposition recorded | `[Yes / No / N/A]` |

A deletion request alone does not establish complete cleanup.

## 24. Cost record

Complete when cost monitoring or closure applies.

| Field | Value |
|---|---|
| Observation period | `[UTC RANGE / N/A]` |
| Runtime | `[VALUE / N/A]` |
| Service scope | `[PUBLIC-SAFE SUMMARY / N/A]` |
| Budget notifications configured | `[Yes / No / N/A]` |
| Runtime limit enforced | `[Yes / No / N/A]` |
| Immediate cost review | `[VALUE OR PRIVATE REFERENCE / PENDING / N/A]` |
| 24-hour cost review | `[VALUE OR PRIVATE REFERENCE / PENDING / N/A]` |
| 72-hour cost review | `[VALUE OR PRIVATE REFERENCE / PENDING / N/A]` |
| Residual usage or cost | `[DESCRIPTION / NONE OBSERVED / PENDING / N/A]` |
| Delayed-charge limitation documented | `[Yes / No / N/A]` |
| Cost closure state | `[Open / Preliminary / Completed / N/A]` |

Budgets and alerts are not guaranteed hard stops.

An immediate cost view must not be presented as final cost closure.

## 25. Limitations and residual uncertainty

### Known limitations

- `[LIMITATION 1]`
- `[LIMITATION 2]`

### Missing evidence

- `[MISSING ITEM 1]`
- `[MISSING ITEM 2]`

### Residual uncertainty

[Document what remains unknown.]

### Applicability boundary

[State the configuration, test conditions, time window, and environment to which
the result applies.]

## 26. Final record summary

| Field | Final value |
|---|---|
| Evidence lifecycle status | `[STATUS]` |
| Test outcome | `[OUTCOME]` |
| Public release | `[APPROVED / REJECTED / PENDING / PRIVATE ONLY]` |
| Manifest status | `[ADDED / NOT ADDED / N/A]` |
| Claim decision | `[APPROVED / BLOCKED / REGRESS / RETIRED / SUPERSEDED / N/A]` |
| Follow-up required | `[Yes / No]` |
| Follow-up owner | `[IDENTIFIER / N/A]` |
| Follow-up due UTC | `[VALUE / N/A]` |

### Final bounded conclusion

[State the narrowest accurate conclusion supported by the record.]

### Required follow-up

[Document the next action or state `None`.]

## 27. Sign-off

| Role | Identifier | UTC date | Decision |
|---|---|---|---|
| Record owner | `[IDENTIFIER]` | `[UTC]` | `[Complete / Incomplete]` |
| Technical reviewer | `[IDENTIFIER]` | `[UTC]` | `[Approve / Reject / Pending]` |
| Redaction reviewer | `[IDENTIFIER]` | `[UTC]` | `[Approve / Reject / Pending]` |
| Claim reviewer | `[IDENTIFIER]` | `[UTC]` | `[Approve / Block / Regress / Pending]` |

## Template closure notice

This template establishes record structure only.

It does not authorize:

- Azure deployment;
- public TCP/3389 exposure;
- controlled authentication testing;
- evidence capture;
- claim promotion;
- publication;
- staging or committing a populated evidence record;
- a pull request.

A populated record must follow the evidence-governance, redaction, manifest, and
claim-approval requirements before any public use.
