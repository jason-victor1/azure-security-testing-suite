# Checklist-to-Project Roadmap

## Purpose

This document maps private lab checklist themes into an original project roadmap for Sentinel RDP Honeypot v2.

The goal is to prevent scope creep while preserving a clear expansion path from a focused RDP detection lab into a broader Azure SOC and honeynet project.

This file does not contain raw course transcripts, proprietary checklist text, credentials, screenshots, or copied course material.

## Current Scope: Sentinel RDP Honeypot v2

The current branch focuses on a controlled Azure SOC lab centered on Windows RDP brute-force telemetry.

### Primary Objectives

- Deploy an isolated Azure lab environment.
- Create a Windows VM intentionally exposed for RDP telemetry.
- Send Windows Security Events to a Log Analytics Workspace.
- Enable Microsoft Sentinel on the workspace.
- Configure Azure Monitor Agent and a Data Collection Rule.
- Validate SecurityEvent ingestion.
- Detect failed RDP authentication activity using Event ID 4625.
- Create or document a Sentinel analytics rule for Windows brute-force attempts.
- Capture sanitized evidence.
- Document triage workflow, findings, remediation, and cleanup.
- Tear down the lab and verify cleanup.

### Current Project Artifacts

| Artifact | Purpose |
|---|---|
| docs/scope-and-credibility-notes.md | Defines lab boundaries and prevents overclaiming. |
| docs/threat-model.md | Documents expected threats, assets, risks, and controls. |
| docs/theory-to-lab-mapping.md | Connects cybersecurity theory to the lab. |
| runbooks/cost-control-checklist.md | Documents cost and runtime guardrails. |
| runbooks/deployment-runbook.md | Guides build execution. |
| runbooks/teardown-runbook.md | Guides cleanup and deletion verification. |
| kql/hunting-queries.md | Stores original KQL hunting queries. |
| sentinel/analytics-rules-catalog.md | Documents Sentinel analytics rule logic. |
| runbooks/rdp-bruteforce-triage.md | Documents investigation workflow. |
| evidence/README.md | Defines required screenshots and validation evidence. |
| reports/findings-report.md | Summarizes observations and recommendations. |
| reports/incident-timeline.md | Tracks key security events and lab activity. |

## Current Detection Path

The first detection path should remain narrow and verifiable:

| Detection Area | Current Scope |
|---|---|
| Source system | Windows VM |
| Exposure type | Public RDP for controlled lab telemetry |
| Log source | Windows Security Events |
| Log table | SecurityEvent |
| Primary event | Event ID 4625 failed logon |
| SIEM | Microsoft Sentinel |
| Query layer | KQL |
| Response layer | Triage runbook and findings report |
| Cleanup layer | VM deallocation, resource deletion, and verification |

## Deferred Expansion: Linux SSH Telemetry

Linux SSH detection is a strong follow-on expansion, but it should not block the first RDP-focused build.

Future work may include:

- Ubuntu VM deployment.
- Syslog collection.
- Linux authentication log ingestion.
- SSH failed-authentication analysis.
- SSH world-map workbook.
- SSH brute-force triage workflow.

## Deferred Expansion: MSSQL Authentication Telemetry

MSSQL authentication telemetry is useful for detection engineering, but it adds software installation, service exposure, and extra attack surface.

Future work may include:

- SQL Server installation in a lab VM.
- SQL authentication logging.
- Failed MSSQL login simulation.
- MSSQL-specific KQL queries.
- MSSQL brute-force analytics rules.
- MSSQL findings and remediation recommendations.

## Deferred Expansion: Controlled Attack VM

A dedicated attack VM can help generate controlled telemetry, but it increases cost and complexity.

Future work may include:

- Separate attacker resource group.
- Separate attacker VNet.
- Controlled failed RDP, SSH, and MSSQL attempts.
- Evidence comparing simulated traffic with organic internet traffic.
- Cleanup verification for attacker infrastructure.

## Deferred Expansion: NSG Flow Logs and Network Analytics

NSG flow logs and network analytics would expand the lab from host authentication telemetry into network telemetry.

Future work may include:

- Storage account for NSG flow logs.
- NSG flow log enablement.
- AzureNetworkAnalytics_CL validation.
- Inbound flow analysis.
- Malicious allowed inbound flow workbook.
- Network-layer detection findings.

## Deferred Expansion: Entra ID Logging

Tenant-level logging is valuable for identity detection, but it should be handled separately from the first RDP host telemetry build.

Future work may include:

- Entra ID diagnostic settings.
- AuditLogs ingestion.
- SigninLogs ingestion.
- Failed sign-in analysis.
- Privileged role assignment detection.
- Dummy-user lifecycle testing.
- Identity-focused triage runbook.

## Deferred Expansion: Break-Glass Account Controls

Break-glass account design is important for identity resilience, but it involves privileged account handling and should be documented carefully.

Future work may include:

- Break-glass account design notes.
- MFA and Conditional Access considerations.
- Secure credential storage guidance.
- Monitoring and alerting for break-glass usage.
- Clear distinction between lab demonstration and production identity governance.

## Deferred Expansion: Storage and Key Vault Logging

Storage and Key Vault logging can turn the project into a broader cloud detection lab.

Future work may include:

- Storage diagnostic settings.
- Key Vault diagnostic settings.
- StorageBlobLogs validation.
- Key Vault secret access monitoring.
- Detection logic for sensitive secret reads.
- Remediation guidance for public access, private endpoints, and least privilege.

## Deferred Expansion: Microsoft Defender for Cloud

Defender for Cloud can strengthen the project by adding posture management, recommendations, and continuous export.

Future work may include:

- Defender plan enablement.
- Continuous export to Log Analytics.
- Secure score review.
- Recommendation triage.
- Defender-generated alert review.
- Cost and workspace duplication checks.

## Deferred Expansion: NIST 800-53 and SC-7 Hardening

NIST 800-53 and SC-7 controls are better suited for a secure configuration and governance expansion.

Future work may include:

- Regulatory compliance review.
- Mapping findings to NIST 800-53 control families.
- Private endpoint implementation for cloud resources.
- Public network access restrictions.
- Network boundary hardening.
- Compliance-oriented remediation writeups.

## Detection Engineering Roadmap

The project should mature in stages:

| Stage | Focus |
|---|---|
| Stage 1 | Manual KQL query validation |
| Stage 2 | Manual Sentinel analytics rule creation |
| Stage 3 | Alert triggering and evidence capture |
| Stage 4 | Incident triage runbook |
| Stage 5 | Findings report and remediation mapping |
| Stage 6 | Optional analytics rule import |
| Stage 7 | Optional workbook/map visualization |
| Stage 8 | Expansion into multi-source detection |

## Evidence Strategy

Evidence should prove that the lab was built, monitored, validated, and cleaned up.

Required evidence should include:

- Resource group inventory.
- Cost guardrail or cost review.
- Log Analytics Workspace.
- Microsoft Sentinel enabled.
- Azure Monitor Agent installed.
- Data Collection Rule configured.
- SecurityEvent table populated.
- Event ID 4625 query results.
- Sentinel analytics rule or rule logic.
- Triage notes.
- Findings report.
- Cleanup verification.

Evidence must be sanitized before commit.

Do not commit:

- Tenant IDs.
- Subscription IDs.
- Real usernames.
- Real passwords.
- Public IPs unless intentionally redacted or generalized.
- Raw logs containing sensitive identifiers.
- Screenshots with account, billing, or identity details.
- Raw course transcripts or proprietary checklist material.

## Cleanup Strategy

Cleanup is mandatory for every lab run.

The teardown process should verify:

- VM stopped or deleted.
- Resource group deleted.
- Public IP deleted.
- NSG deleted.
- VNet/subnet deleted.
- DCR deleted.
- Diagnostic settings removed where appropriate.
- Log Analytics/Sentinel resources handled according to project plan.
- Attacker or dummy accounts removed if created.
- Defender for Cloud or compliance settings reviewed if enabled.
- No unexpected resources remain.
- Cost Management reviewed after teardown.

## Portfolio Framing

This project should be described as a controlled Azure SOC and detection engineering lab.

Strong framing:

- Built an isolated Azure lab to collect and analyze Windows RDP brute-force telemetry.
- Configured Microsoft Sentinel and Log Analytics for security monitoring.
- Validated Windows Security Event ingestion and Event ID 4625 failed-logon detection.
- Documented triage workflow, detection logic, findings, remediation, and cleanup.
- Distinguished intentional lab exposure from production-safe remote access patterns.

Avoid overclaiming:

- Do not present this as production SOC ownership.
- Do not imply enterprise incident response authority.
- Do not imply real attacker attribution.
- Do not imply regulated data handling.
- Do not treat public RDP exposure as a production recommendation.

## Decision

The current branch will remain focused on Sentinel RDP Honeypot v2.

The broader checklist themes will be treated as expansion opportunities after the RDP-focused build is complete, validated, documented, and cleaned up.
