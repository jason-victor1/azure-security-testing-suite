# Scope and Credibility Notes

## Project

Sentinel RDP Honeypot v2

## Purpose

This project is a self-built Azure cloud security lab designed to support cloud SOC readiness, Microsoft Sentinel practice, KQL hunting, incident triage, and cloud security engineering documentation.

The lab intentionally exposes a Windows virtual machine to the public internet over RDP in order to observe real-world brute-force authentication attempts and validate detection workflows in Microsoft Sentinel.

## Project Type

This is a controlled security lab, not a production enterprise environment.

## What This Project Demonstrates

This project demonstrates:

- Azure resource deployment and lab isolation
- Microsoft Sentinel enablement
- Log Analytics telemetry collection
- Windows Security Event ingestion
- RDP brute-force observation
- Event ID 4625 failed logon analysis
- KQL-based hunting and investigation
- Basic detection engineering workflow
- Incident triage documentation
- Security findings and remediation recommendations
- Evidence capture and redaction discipline
- Cost-control and teardown discipline

## What This Project Does Not Claim

This project does not claim:

- Production SOC ownership
- Enterprise incident response authority
- Management of regulated production workloads
- Full-scale blue team operations
- Formal employment experience
- Unauthorized access, exploitation, retaliation, or attacker interaction

## Lab Exposure Rationale

The Windows VM is intentionally exposed over RDP to generate observable authentication attack telemetry. This exposure is not a production recommendation.

In a production environment, public RDP should generally be avoided. Safer alternatives include Azure Bastion, VPN/private access, Just-in-Time VM access, privileged access workflows, Conditional Access, and strict network restrictions.

## Data Handling

The lab must not contain:

- Production data
- Personal data
- Business data
- Real secrets
- Reused passwords
- Customer information
- Sensitive identity data

Evidence must be reviewed and redacted before being committed to GitHub.

## Safety Boundaries

The lab is limited to passive observation and defensive analysis. The project does not include hack-back activity, retaliation, malware deployment, credential harvesting, or interaction with attacking systems.

## Portfolio Framing

This project should be described as a self-built Azure SOC and detection engineering lab. It is valid as portfolio evidence for cloud security engineering, SOC analysis, detection engineering, and Microsoft Sentinel practice when described honestly and with clear scope boundaries.
