# Theory-to-Lab Mapping

## Purpose

This document maps cybersecurity theory concepts to the Sentinel RDP Honeypot v2 lab so the project demonstrates more than tool usage.

The goal is to connect the Azure SOC build to risk management, security controls, SIEM operations, and incident response concepts.

## CIA Triad Mapping

| CIA Area | Lab Mapping |
|---|---|
| Confidentiality | The honeypot must not contain production, customer, personal, payment, health, or business data. |
| Integrity | Logs, screenshots, and findings must be reviewed and preserved accurately. |
| Availability | Cost controls, runtime limits, teardown, and cleanup verification prevent runaway lab exposure and cost. |

## Risk Management Mapping

Public RDP exposure is intentional in this lab to generate telemetry.

In the lab:

- Likelihood of brute-force attempts is expected to be high.
- Impact should remain low because the VM is isolated and contains no real data.

In production:

- Public RDP exposure could create high risk because a successful compromise could affect business systems, sensitive data, or identity infrastructure.

## Security Control Mapping

| Control Type | Lab Example |
|---|---|
| Administrative | Scope notes, threat model, runbooks, evidence handling rules |
| Technical | NSG, Windows VM, Log Analytics, Microsoft Sentinel, Data Collection Rule |
| Operational | Triage workflow, issue tracking, teardown process |
| Detective | SecurityEvent ingestion, KQL hunting, Sentinel analytics |
| Preventive | No production data, no VNet peering, unique credentials, cost controls |
| Corrective | VM deallocation, resource group deletion, remediation recommendations |

## NIST CSF Mapping

| Function | Lab Mapping |
|---|---|
| Identify | Define assets, risks, resource group, VM, logs, and exposure. |
| Protect | Use isolation, no production data, scoped resources, cost controls, and documented guardrails. |
| Detect | Collect Windows Security Events and analyze Event ID 4625 activity in Sentinel. |
| Respond | Triage brute-force activity using KQL and documented investigation steps. |
| Recover | Tear down lab resources, verify cleanup, and document lessons learned. |

## SOC and SIEM Mapping

| SIEM Function | Lab Implementation |
|---|---|
| Data Collection | Windows Security Events from the honeypot VM |
| Log Management | Log Analytics Workspace |
| Correlation | KQL queries and Sentinel analytics logic |
| Alerting | Sentinel analytics rules or documented rule concepts |
| Dashboards | Sentinel workbooks or visualizations |
| Incident Management | RDP brute-force triage runbook and incident timeline |

## Incident Response Mapping

| NIST 800-61 Phase | Lab Mapping |
|---|---|
| Preparation | Scope notes, cost checklist, threat model, deployment runbook, teardown runbook |
| Detection and Analysis | SecurityEvent ingestion, Event ID 4625 hunting, failed login analysis |
| Containment, Eradication, and Recovery | Stop/deallocate VM, delete resource group, verify cleanup |
| Post-Incident Activity | Findings report, remediation recommendations, lessons learned, rubric scoring |

## Production-Safe Remote Access Contrast

The honeypot intentionally exposes RDP to collect telemetry.

A production environment should avoid public RDP and use safer patterns such as:

- Azure Bastion
- VPN or private connectivity
- Just-in-Time VM access
- Conditional Access
- Privileged Identity Management
- Network restrictions
- Strong monitoring and alerting

## Interview Framing

This project should be explained as a controlled Azure SOC and detection engineering lab that connects cybersecurity theory to practical cloud security operations.

It demonstrates how public remote access exposure can generate brute-force telemetry, how that telemetry can be collected and analyzed in Microsoft Sentinel, and how findings can be translated into production-safe recommendations.
