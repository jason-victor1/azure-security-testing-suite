# Threat Model

## Project

Sentinel RDP Honeypot v2

## Purpose

This threat model defines the intended exposure, assets, expected attacker behavior, risks, and defensive controls for the Sentinel RDP Honeypot v2 lab.

The goal is to document the lab before deployment so the project has a clear security boundary, risk model, and production-safe contrast.

## System Overview

The lab contains a Windows virtual machine intentionally exposed to the public internet over RDP. Windows Security Events are collected and sent to Log Analytics, then analyzed in Microsoft Sentinel using KQL hunting queries, detection logic, and incident triage workflows.

## High-Level Data Flow

    Internet
      ↓
    Public IP / NSG RDP Rule
      ↓
    Windows Honeypot VM
      ↓
    Windows Security Events
      ↓
    Azure Monitor Agent / Data Collection Rule
      ↓
    Log Analytics Workspace
      ↓
    Microsoft Sentinel
      ↓
    KQL Hunting / Analytics / Triage / Evidence

## Primary Assets

- Azure subscription used for the lab
- Lab resource group
- Windows honeypot VM
- Virtual network and subnet
- Network security group
- Public IP address
- Log Analytics Workspace
- Microsoft Sentinel workspace
- Security event telemetry
- Evidence screenshots and reports
- GitHub repository documentation

## Intended Exposure

The Windows VM is intentionally exposed over RDP for controlled telemetry collection.

This exposure is expected to attract automated authentication attempts from the public internet.

This exposure is acceptable only because the environment is isolated, contains no production data, and is used for defensive monitoring practice.

## Expected Adversary Behavior

Expected activity includes:

- RDP brute-force attempts
- Password spraying
- Repeated failed logons
- Common username attempts
- Automated scanning
- Repeated attempts from the same source IPs
- Distributed attempts from multiple geographies

## Key Event Focus

The primary Windows Security Event for this lab is:

    Event ID 4625: Failed logon

Supporting events may include:

    Event ID 4624: Successful logon

Successful logons should be investigated carefully because they may indicate unauthorized access or lab misconfiguration.

## Main Risks Introduced by the Lab

| Risk | Description | Control |
|---|---|---|
| Unauthorized access | Public RDP may attract brute-force attempts | Strong credentials, no real data, isolation, monitoring |
| Unexpected cost | Logs, VM runtime, Sentinel, and storage may generate cost | Budget, daily cap, runtime limit, teardown |
| Sensitive evidence exposure | Screenshots or logs may expose IPs, usernames, or Azure identifiers | Redaction review before commit |
| Scope creep | Lab could accidentally connect to non-lab resources | Dedicated resource group, no peering, no production data |
| Misleading portfolio claims | Lab could be overstated as production experience | Scope and credibility notes |

## Isolation Controls

- Dedicated lab resource group
- No production workloads
- No real business or personal data
- No VNet peering to production or personal workloads
- No reused passwords or secrets
- No customer data
- No privileged enterprise identity workflows

## Monitoring Controls

- Windows Security Event collection
- Log Analytics Workspace
- Microsoft Sentinel
- KQL hunting queries
- Evidence capture
- Incident triage notes
- Findings and remediation documentation

## Cost Controls

- Azure budget or cost alert
- Log Analytics daily cap
- VM runtime limit
- Stop/deallocate procedure
- Resource group teardown procedure
- Post-lab cleanup verification

## Evidence Controls

- Screenshots reviewed before commit
- Sensitive values redacted
- Redaction notes maintained
- Raw logs not committed unless sanitized
- Public IPs, tenant IDs, subscription IDs, usernames, and emails reviewed before publication

## Out of Scope

This project does not include:

- Exploitation of attacker systems
- Hack-back activity
- Malware execution
- Credential harvesting
- Production incident response
- Enterprise SOC operations
- Real customer or business data
- Unauthorized access to third-party systems

## Production-Safe Contrast

The honeypot intentionally exposes RDP to collect telemetry. A production environment should not use this design for normal administration.

Production-safe alternatives include:

- Azure Bastion
- Private IP-only administration
- VPN or private connectivity
- Just-in-Time VM access
- Conditional Access
- Privileged Identity Management
- Network restrictions
- Strong logging and alerting
- Removal of public management ports

## Threat Model Completion Criteria

This threat model is complete when the lab scope, assets, intended exposure, expected attacker behavior, risks, controls, and production-safe contrast are documented before deployment.
