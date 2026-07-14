# Evidence Redaction and Public-Disclosure Notes

## Purpose

This document defines the sanitization, redaction, review, and public-disclosure
requirements for Sentinel RDP Honeypot v2 evidence.

It supplements the governing evidence model in `README.md`. When an evidence
artifact is technically useful but unsafe for public release, the private raw
artifact remains outside Git and only an approved sanitized derivative may be
considered for the repository.

This document does not establish that Azure execution has occurred or that any
v2 evidence has been captured.

## Current project state

At the creation of this redaction baseline:

- Azure execution is paused.
- Azure v2 resources have not been deployed.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- No v2 execution evidence has been captured.
- No v2 evidence artifact has been sanitized or approved for public release.
- Public implementation and validation claims remain design-stage only.

## Core disclosure rule

Raw evidence is private by default.

An artifact may enter Git only when it has:

1. a defined evidence purpose;
2. an assigned evidence ID;
3. a documented relationship to a test, control, finding, query, rule, or claim;
4. a sanitized public derivative;
5. a completed technical-accuracy review;
6. a completed sensitive-data review;
7. any required provenance and license review;
8. an evidence lifecycle status of `Approved-Public`;
9. public wording that does not exceed what the artifact proves.

Redaction is not approval. Sanitization is not validation. A technically correct
artifact may still be rejected for public release.

## Private raw-evidence boundary

Raw evidence must remain outside Git, including:

- original screenshots;
- original screen recordings;
- original Azure Portal exports;
- original Azure CLI or PowerShell output;
- original Windows event records;
- original Log Analytics or Microsoft Sentinel query results;
- unredacted configuration exports;
- raw billing, usage, or budget records;
- raw alert and incident-object exports;
- raw teardown output;
- private course material;
- third-party material pending provenance or license review;
- artifacts containing unknown or unreviewed data.

The public repository must not record private storage paths when those paths
would reveal usernames, device names, home directories, synchronized-drive
locations, or other sensitive local metadata.

## Information prohibited from public release

The following information must not appear in public repository artifacts unless
an explicit project decision documents a safe exception.

### Credentials and secrets

Prohibited material includes:

- passwords;
- password hints;
- authentication cookies;
- browser session data;
- bearer tokens;
- API keys;
- client secrets;
- private keys;
- recovery codes;
- connection strings;
- signed URLs;
- shared-access signatures;
- access tokens;
- refresh tokens;
- credential-manager output;
- command history containing credentials;
- environment-variable values containing secrets.

Partial masking is insufficient when the remaining characters could still aid
identification, correlation, or secret reconstruction.

### Personal and account information

Prohibited material includes:

- personal email addresses;
- personal usernames;
- account display names;
- phone numbers;
- personal profile images;
- billing contacts;
- notification recipients;
- browser account indicators;
- unrelated names visible in portals, terminals, or desktop interfaces.

Synthetic lab-account labels may be disclosed only after review confirms that
they are not reused personal, employer, customer, or production identifiers.

### Azure identifiers

Prohibited or restricted Azure information includes:

- tenant IDs;
- subscription IDs;
- full resource IDs;
- object IDs;
- principal IDs;
- application IDs when they expose an active identity relationship;
- workspace IDs;
- Data Collection Rule immutable IDs;
- deployment correlation IDs;
- support-request identifiers;
- complete portal URLs containing sensitive identifiers.

Sanitized public evidence should retain only the minimum identifier context
needed to explain the control or result.

### Network and endpoint information

Prohibited or restricted information includes:

- active public IP addresses;
- private IP addresses when they reveal unnecessary topology;
- public DNS names;
- active hostnames;
- complete endpoint URLs;
- MAC addresses;
- virtual-network and subnet identifiers that reveal live architecture;
- network-interface identifiers;
- exact source addresses used for controlled testing;
- unrelated source addresses observed in telemetry.

Public evidence must not make a still-active lab endpoint easier to discover or
target.

### Local-system metadata

Prohibited or restricted information includes:

- local usernames;
- device names;
- home-directory paths;
- shell-history paths;
- mounted-volume names;
- synchronized-drive locations;
- terminal window titles containing private data;
- browser bookmarks;
- unrelated desktop files;
- notification banners;
- clipboard contents;
- recent-file lists.

### Billing and financial information

Prohibited or restricted information includes:

- payment methods;
- invoice identifiers;
- billing-account identifiers;
- billing-profile identifiers;
- credit balances;
- promotional-credit details;
- tax information;
- complete cost-management exports;
- personally identifying budget-alert recipients.

Public cost evidence should use the minimum sanitized amount, range, or summary
required to support the stated cost-control conclusion.

### Security-sensitive telemetry

Security telemetry requires context-sensitive review. Restricted fields may
include:

- account names;
- domain names;
- workstation names;
- source and destination addresses;
- complete process command lines;
- file paths;
- session identifiers;
- security identifiers;
- correlation identifiers;
- raw event XML;
- free-text fields containing unexpected personal or system data.

Aggregation does not automatically make telemetry safe. Low-volume results may
still identify a person, host, account, or endpoint.

### Private course and third-party material

Private course material must not be copied, lightly rewritten, screenshotted, or
published as project evidence.

Third-party scripts, screenshots, diagrams, datasets, query packs, or templates
require:

- source identification;
- provenance review;
- license review;
- integrity verification where appropriate;
- malware and secret scanning where applicable;
- clear attribution when redistribution is permitted;
- rejection when public-use rights cannot be established.

## Sanitization method

### Preserve the raw artifact

The raw artifact must remain unchanged in approved private storage.

Do not crop, annotate, overwrite, or redact the only copy of raw evidence. Create
a derivative specifically for public review.

### Create a public derivative

The sanitized derivative should:

- retain only the content needed for the evidence purpose;
- remove unrelated interface elements and records;
- replace restricted values with explicit placeholders;
- preserve technical meaning;
- preserve relevant timestamps or state when safe;
- retain enough context for an independent reviewer;
- document all material sanitization actions;
- receive a separate integrity hash where appropriate.

### Use explicit placeholders

Use placeholders that identify the type of removed value, such as:

- `<REDACTED-TENANT-ID>`;
- `<REDACTED-SUBSCRIPTION-ID>`;
- `<REDACTED-RESOURCE-ID>`;
- `<LAB-VM>`;
- `<LAB-WORKSPACE>`;
- `<LAB-RESOURCE-GROUP>`;
- `<CONTROLLED-TEST-SOURCE-IP>`;
- `<OBSERVED-EXTERNAL-IP>`;
- `<REDACTED-ACCOUNT>`;
- `<REDACTED-LOCAL-PATH>`;
- `<REDACTED-COST-VALUE>`;
- `<REDACTED-TIMESTAMP>` when the timestamp itself is unsafe to release.

Do not replace restricted values with realistic-looking secrets, routable IP
addresses, personal names, or identifiers that could be mistaken for live data.

### Preserve meaning

A redaction must not alter the technical conclusion.

For example:

- do not remove failed rows to make a test appear successful;
- do not crop out warnings that qualify the result;
- do not change timestamps to imply a different sequence;
- do not relabel uncontrolled activity as controlled testing;
- do not remove schema fields that reveal a query assumption failed;
- do not hide cleanup failures or residual resources;
- do not convert an inconclusive result into a passing result.

When redaction makes an artifact ambiguous or misleading, reject the artifact
and create a different public representation.

## Evidence-type redaction requirements

### Screenshots

Before approving a screenshot, review:

- browser tabs;
- bookmarks and favorites;
- address bars;
- account avatars and account selectors;
- tenant and subscription selectors;
- portal breadcrumbs;
- resource names and IDs;
- public IP addresses and DNS names;
- terminal prompts;
- local paths;
- timestamps;
- notifications;
- unrelated applications;
- desktop files and folders;
- billing or credit information;
- visible command history.

Crop only after preserving the private raw screenshot. Use opaque redaction
rather than blur when underlying text must be irrecoverable.

A screenshot should include enough surrounding context to support its stated
purpose without exposing unrelated information.

### Command-line output

Before approving command output:

- remove secrets and authentication material;
- remove sensitive shell prompts and local paths;
- sanitize tenant, subscription, resource, principal, and workspace IDs;
- sanitize active addresses and endpoints;
- preserve the command or provide a safe equivalent;
- preserve exit status when relevant;
- preserve warnings and failures;
- retain enough output to support the conclusion;
- document omitted sections when omission affects interpretation.

Do not publish shell-history output that may contain earlier sensitive commands.

### Azure Portal and configuration exports

Before approving portal or configuration evidence:

- remove tenant and subscription identifiers;
- remove complete resource IDs;
- remove active public endpoints;
- remove unrelated resource names;
- remove identity-object details;
- remove deployment-history identifiers;
- remove personally identifying notification recipients;
- retain the configuration fields necessary for the control claim;
- document whether the artifact is a screenshot, export, or reconstructed
  summary.

A reconstructed summary must be labeled as a summary rather than represented as
a raw export.

### KQL queries and query results

Before approving KQL evidence:

- review query text for embedded identifiers or hardcoded addresses;
- replace controlled-test values with explicit placeholders where appropriate;
- sanitize result fields that identify accounts, hosts, or endpoints;
- prefer aggregated results when row-level data is unnecessary;
- retain the query time range;
- retain relevant filters and schema assumptions;
- retain the result count when safe;
- document fields removed from public results;
- preserve failed or empty query outcomes when they are part of the test record.

A KQL result must not be described as proving RDP activity unless the required
remote-interactive context was actually validated.

### Windows authentication events

For Event ID 4625 evidence:

- preserve that the event represents a failed logon;
- preserve the observed logon type when it supports the test;
- sanitize account, workstation, domain, address, and identifier fields;
- do not describe the event as RDP solely because the event ID is 4625.

For Event ID 4624 evidence:

- preserve that the event represents a successful logon;
- sanitize account and endpoint information;
- do not label the event malicious without supporting correlation and analysis.

### AMA and DCR evidence

Before approving Azure Monitor Agent or Data Collection Rule evidence:

- sanitize resource and association identifiers;
- preserve the selected collection source;
- preserve the configured destination in sanitized form;
- preserve observed table and field names;
- preserve telemetry-freshness results;
- preserve ingestion failures, delays, or gaps;
- identify whether the evidence shows configuration, association, ingestion, or
  end-to-end validation.

Configuration evidence alone must not be presented as proof of successful
telemetry ingestion.

### Analytics-rule, alert, and incident evidence

Before approving analytics-rule evidence:

- sanitize rule-resource identifiers;
- retain rule ID and project version;
- retain cadence, lookback, threshold, severity, and rule state;
- retain query-version references;
- retain entity mappings only after field validation;
- retain whether alert or incident creation occurred;
- sanitize account, host, and address values;
- retain known limitations.

An alert or incident object may show that detection logic generated an object.
It must not be presented as independent proof of compromise.

### Geolocation context

IP geolocation must be described as approximate network-location context.

Public evidence must not:

- attribute activity to a specific person;
- claim the observed network location is physically exact;
- treat a country, region, city, ISP, or organization label as conclusive
  attribution;
- expose an active source address merely to support a map or location label.

Prefer aggregated or generalized geographic context when the precise value is
not necessary.

### Teardown and orphan checks

Before approving teardown evidence:

- sanitize resource and subscription identifiers;
- retain the deletion scope;
- retain orphan-check commands or safe equivalents;
- retain residual-resource findings;
- retain active-endpoint disposition;
- retain telemetry-retention or deletion decisions;
- retain immediate, 24-hour, and 72-hour review status;
- preserve failures and delayed findings.

Do not crop teardown output in a way that conceals remaining resources or
errors.

### Cost evidence

Before approving cost evidence:

- remove billing-account and payment information;
- remove personally identifying recipients;
- remove unrelated account-wide costs;
- preserve the observation window;
- preserve runtime where relevant;
- preserve the service scope;
- preserve immediate, 24-hour, and 72-hour review status;
- preserve delayed-charge limitations;
- preserve unexplained residual cost or usage;
- state that budgets and alerts are not guaranteed hard stops.

Where exact values are unnecessary, use a reviewed range or summarized amount
that still supports the conclusion.

## Metadata and file-property review

Public artifacts must also be reviewed for hidden or embedded metadata,
including:

- image EXIF data;
- author and organization fields;
- document revision history;
- comments and annotations;
- embedded filenames;
- creation-tool metadata;
- geographic metadata;
- hidden spreadsheet cells or sheets;
- hidden slide objects;
- attachment metadata;
- alternate data streams where applicable.

Exporting or copying a file does not guarantee that private metadata has been
removed.

## Integrity and derivation records

Where appropriate, the private evidence record should retain:

- the SHA-256 hash of the raw artifact;
- the SHA-256 hash of the sanitized derivative;
- the tool or method used for sanitization;
- the sanitization timestamp in UTC;
- the reviewer;
- the review date;
- the approval or rejection decision.

A public manifest may include an approved derivative hash. It must not expose a
private path or restricted metadata.

A matching hash proves file consistency only. It does not prove accuracy,
authenticity, completeness, or sufficiency.

## Failed, blocked, and inconclusive evidence

Redaction rules apply equally to passing, failed, blocked, and inconclusive
tests.

Public sanitization must not:

- remove the failed acceptance criterion;
- remove the observed error;
- remove a blocked authorization or dependency condition;
- remove uncertainty from an inconclusive result;
- imply that a later retest erases the earlier outcome.

A rejected public artifact does not erase the underlying private engineering
record.

## Redaction record requirements

Each sanitized derivative should record, when applicable:

- evidence ID;
- raw-artifact hash;
- sanitized-derivative hash;
- evidence purpose;
- related test ID;
- related claim ID;
- original artifact type;
- sanitization method;
- values or regions removed;
- placeholders introduced;
- aggregation performed;
- technical-meaning assessment;
- provenance and license assessment;
- reviewer;
- review date;
- approval decision;
- limitations;
- supersession relationship.

The public record must not include the private artifact path when that path
contains sensitive metadata.

## Public-release review checklist

Before an artifact receives `Approved-Public` status, confirm:

- the raw artifact remains private and unchanged;
- the public derivative has a distinct filename or record;
- secrets and credentials are absent;
- personal and account information is absent;
- restricted Azure identifiers are absent;
- active endpoints are absent unless explicitly approved;
- local paths and device metadata are absent;
- billing and financial details are minimized;
- unrelated records are removed;
- private course material is absent;
- third-party provenance and licensing are resolved;
- hidden metadata has been reviewed;
- the artifact retains necessary technical context;
- the artifact preserves warnings, failures, and limitations;
- the artifact supports its linked claim;
- the claim wording does not exceed the evidence;
- the derivative hash is recorded when appropriate;
- technical and redaction review are complete;
- the approval decision is recorded.

If any required check fails, the artifact must remain private, return for
sanitization, or receive `Rejected` status.

## Prohibited transformations

The following transformations are prohibited:

- altering evidence to manufacture a passing result;
- deleting failed attempts from the traceable record;
- changing dates or times to imply a different sequence;
- substituting uncontrolled activity for controlled testing;
- relabeling design output as implementation evidence;
- relabeling implementation evidence as validation evidence;
- presenting synthetic data as live telemetry;
- presenting an alert or incident as proof of compromise;
- presenting geolocation as human attribution;
- removing cost or teardown findings that qualify completion;
- using blur when the underlying sensitive text may remain recoverable;
- publishing unreviewed third-party or private course material.

## Current public-evidence declaration

No Sentinel RDP Honeypot v2 execution evidence currently qualifies for public
release because no v2 execution evidence has been captured.

This document defines future redaction and disclosure requirements only. It does
not authorize Azure deployment, public TCP/3389 exposure, controlled
authentication testing, evidence capture, or advancement of any public claim
beyond its verified design-stage state.
