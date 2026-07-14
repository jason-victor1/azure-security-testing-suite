# Public Evidence Manifest

## Purpose

This manifest is the public inventory of Sentinel RDP Honeypot v2 evidence
artifacts that have completed sanitization, technical review, redaction review,
and approval for repository publication.

The manifest provides traceability from each approved public artifact to its
evidence ID, related tests, governed claims, detection content, integrity
record, review decision, and limitations.

This manifest does not inventory private raw evidence and must not expose
private evidence-storage paths or restricted metadata.

## Current project state

At the creation of this manifest:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection validation has not started.
- Teardown validation has not started.
- Cost reconciliation has not started.
- No v2 execution evidence has been captured.
- No v2 evidence artifact has reached `Approved-Public`.
- Public implementation and validation claims remain design-stage only.

The current manifest therefore contains no evidence entries.

## Manifest scope

This file inventories only sanitized evidence derivatives that are approved for
public repository use.

It may include approved public artifacts such as:

- sanitized screenshots;
- sanitized command-output excerpts;
- reviewed configuration summaries;
- reviewed KQL queries and sanitized result summaries;
- AMA and DCR validation summaries;
- Windows authentication-event validation summaries;
- analytics-rule validation summaries;
- alert or incident-object summaries;
- failed-test summaries;
- teardown and orphan-check summaries;
- sanitized cost-review summaries;
- other reviewed artifacts that support a governed claim.

An artifact must not be added merely because it was captured, sanitized, or
technically useful.

## Inclusion criteria

An artifact may be entered in this manifest only when all applicable criteria
are satisfied:

1. An `EV-###` evidence ID has been assigned.
2. The raw artifact remains in approved private storage outside Git.
3. The public derivative has been sanitized.
4. Technical accuracy has been reviewed.
5. Sensitive-data and metadata review has been completed.
6. Third-party provenance and licensing have been resolved when applicable.
7. The public derivative has an evidence lifecycle status of
   `Approved-Public`.
8. The associated test outcome is recorded independently.
9. A SHA-256 hash of the approved public derivative is available when
   appropriate.
10. The public repository path is stable and valid.
11. Related test, claim, query, and analytics-rule identifiers are recorded
    when applicable.
12. The artifact's limitations are documented.
13. The artifact does not overstate implementation, validation, compromise,
    attribution, cleanup, cost control, compliance, or completion.

## Exclusion criteria

The following must not be entered in the public manifest:

- private raw evidence;
- private evidence-storage paths;
- raw-artifact hashes that are not approved for disclosure;
- artifacts with lifecycle status `Planned`;
- artifacts with lifecycle status `Captured-Private`;
- artifacts with lifecycle status `Sanitized`;
- artifacts awaiting technical or redaction review;
- artifacts with lifecycle status `Rejected`;
- unreviewed screenshots or command output;
- unredacted Azure exports;
- credentials, secrets, or authentication material;
- tenant IDs, subscription IDs, or complete resource IDs;
- active public IP addresses, DNS names, or endpoints;
- private course material;
- third-party material without resolved provenance and licensing;
- evidence whose public form would be misleading after redaction.

An excluded artifact may remain part of the private engineering record without
appearing in this public manifest.

## Manifest-entry schema

Each approved public-evidence entry must include the following fields when
applicable.

| Field | Requirement |
|---|---|
| Evidence ID | Stable `EV-###` identifier that is not silently reused. |
| Title | Concise description of the artifact and its evidence purpose. |
| Artifact type | Screenshot, command output, KQL, configuration summary, alert summary, teardown record, cost record, or other reviewed type. |
| Public path | Repository-relative path to the approved derivative. |
| Lifecycle status | Must be `Approved-Public` for an active manifest entry. |
| Test outcome | `Passed`, `Failed`, `Blocked`, `Inconclusive`, `Not Run`, or `Not Applicable`, recorded independently from lifecycle status. |
| Related test | Applicable `TST-###` identifier or `N/A`. |
| Related claim | Applicable `CLM-###` identifier or `N/A`. |
| Related query | Applicable `Q-###` identifier or `N/A`. |
| Related analytics rule | Applicable `AR-###` identifier or `N/A`. |
| Capture time | UTC timestamp for the underlying evidence capture when safe to disclose. |
| Approval time | UTC timestamp for public-release approval. |
| Public SHA-256 | SHA-256 hash of the approved public derivative when appropriate. |
| Reviewer | Public-safe reviewer name, role, or identifier. |
| Limitations | Material constraints on interpretation or reuse. |
| Supersession | Prior or replacement evidence ID when applicable. |

A public entry must not contain a private local path, private storage location,
credential, restricted Azure identifier, active endpoint, or unapproved raw
metadata.

## Lifecycle status and test outcome

Evidence lifecycle status and test outcome describe different properties.

Lifecycle status records what has happened to the artifact. This manifest
accepts active entries only when the artifact is `Approved-Public`.

Test outcome records what happened during the associated test. An approved
public artifact may document a test outcome of:

- `Passed`;
- `Failed`;
- `Blocked`;
- `Inconclusive`;
- `Not Run`;
- `Not Applicable`.

For example, a sanitized and approved failure record may legitimately have:

- lifecycle status: `Approved-Public`;
- test outcome: `Failed`.

Approval for publication must never be interpreted as a passing technical
result.

## Identifier relationships

Manifest entries use the project identifier system:

| Identifier | Relationship |
|---|---|
| `EV-###` | Identifies the public evidence record or artifact. |
| `TST-###` | Identifies the validation test associated with the evidence. |
| `CLM-###` | Identifies the governed claim supported or constrained by the evidence. |
| `Q-###` | Identifies the KQL query or query family associated with the evidence. |
| `AR-###` | Identifies the Microsoft Sentinel analytics rule associated with the evidence. |

Not every artifact requires every identifier. Use `N/A` only when the
relationship was reviewed and does not apply.

Missing analysis or an undefined relationship must not be disguised as `N/A`.

## Public-path requirements

The public path must:

- be relative to the repository root;
- point to the approved derivative rather than private raw evidence;
- use a stable and descriptive filename;
- avoid personal names and sensitive identifiers;
- avoid active IP addresses, DNS names, tenant IDs, subscription IDs, or
  complete resource IDs;
- preserve the evidence ID where practical;
- remain synchronized with the manifest entry.

If an artifact is moved, renamed, or removed, the manifest must be updated in
the same change set.

## Integrity requirements

The public manifest may record the SHA-256 hash of an approved public
derivative.

Recommended command:

    shasum -a 256 <public-artifact>

The hash must be calculated after the final approved sanitization and metadata
review.

A hash demonstrates file consistency. It does not independently prove:

- authenticity;
- accuracy;
- completeness;
- successful control operation;
- a passing test;
- compromise;
- attribution;
- compliance;
- authorization;
- project completion.

Private raw-artifact hashes must remain outside the public manifest unless an
explicit review approves their disclosure.

## Review and approval requirements

Before adding an entry, confirm:

- the evidence purpose is defined;
- the evidence ID is unique;
- the public derivative exists;
- the public path is correct;
- the derivative has completed sanitization;
- technical accuracy has been reviewed;
- secrets and restricted identifiers are absent;
- active endpoints are absent unless explicitly approved;
- hidden metadata has been reviewed;
- provenance and licensing are resolved;
- the associated test outcome is accurate;
- failed or inconclusive results have not been concealed;
- the related claim does not exceed the evidence;
- limitations are documented;
- the SHA-256 value was calculated from the final derivative when applicable;
- the approval decision and timestamp are recorded.

An entry must be removed from active use or marked as superseded if its artifact
later fails integrity, redaction, provenance, licensing, or technical review.

## Claim alignment

Manifest inclusion does not automatically authorize a claim to advance.

A claim may advance only when the claim-evidence matrix confirms that the
available evidence satisfies the claim's defined maturity requirements.

The following interpretations are prohibited:

- a design artifact treated as implementation evidence;
- implementation evidence treated as validation evidence;
- an alert or incident object treated as proof of compromise;
- Event ID 4625 treated as intrinsically RDP;
- Event ID 4624 treated as intrinsically malicious;
- IP geolocation treated as human attribution;
- resource deletion treated as complete teardown;
- a budget alert treated as a guaranteed cost hard stop;
- framework mapping treated as compliance or authorization;
- public approval treated as proof that a test passed.

## Failed and superseded evidence

Failed, blocked, and inconclusive tests may have approved public evidence
entries when publication requirements are satisfied.

Such entries must:

- preserve the actual test outcome;
- preserve material warnings and limitations;
- retain their original evidence IDs;
- remain traceable after corrective work;
- link to later retest evidence when applicable.

Superseded evidence must not be silently deleted from the engineering record.

When a public artifact is replaced:

1. retain the original evidence ID;
2. mark its relationship to the replacement;
3. assign the replacement its own evidence ID or documented version;
4. update affected claim mappings;
5. preserve the earlier test outcome;
6. document why the replacement occurred.

## Manifest maintenance

The manifest must be updated in the same milestone or commit that adds, removes,
renames, rejects, or supersedes public evidence.

Before committing a manifest update:

- verify all listed paths;
- verify all listed SHA-256 hashes;
- verify evidence-ID uniqueness;
- verify identifier relationships;
- verify lifecycle and outcome values;
- verify that no private paths are present;
- verify that no restricted identifiers or active endpoints are present;
- verify claim-evidence matrix synchronization;
- run repository secret scanning;
- review the complete diff.

## Current approved public-evidence entries

There are currently no approved public-evidence entries.

No rows, evidence IDs, hashes, test outcomes, or implementation claims have been
pre-populated because no Sentinel RDP Honeypot v2 execution evidence has been
captured.

Future entries must be added only after the complete evidence workflow,
sanitization process, technical review, redaction review, and public-release
approval have occurred.

## Current manifest declaration

This manifest establishes the approved-public evidence inventory structure only.

It does not authorize Azure deployment, public TCP/3389 exposure, controlled
authentication testing, evidence capture, or advancement of any public claim
beyond its verified design-stage state.
