# Deployment Runbook

## Purpose

This runbook documents the deployment workflow for the Sentinel RDP Honeypot v2 lab.

The goal is to build the lab in a controlled, repeatable way while preserving evidence, limiting cost exposure, and maintaining clear security boundaries.

## Project

Sentinel RDP Honeypot v2

## Deployment Status

This runbook starts as a pre-deployment plan and should be updated as the lab is built.

## Pre-Deployment Requirements

Before creating Azure resources, confirm:

- [ ] Correct Azure subscription is selected
- [ ] Cost-control checklist is completed
- [ ] Threat model is completed
- [ ] Teardown runbook is completed
- [ ] Evidence checklist is prepared
- [ ] No production resources are in scope
- [ ] No real credentials or reused passwords are used
- [ ] Lab resource naming is defined
- [ ] Expected runtime window is defined

## Required Variables

Set these values before running Azure CLI validation or teardown commands:

    export RG_NAME="rg-sentinel-rdp-honeypot-v2"
    export LOCATION="eastus"
    export VM_NAME="vm-rdp-honeypot-v2"
    export LAW_NAME="law-sentinel-rdp-honeypot-v2"
    export VNET_NAME="vnet-sentinel-rdp-honeypot-v2"
    export SUBNET_NAME="snet-honeypot"
    export NSG_NAME="nsg-rdp-honeypot-v2"

Update values if the course material or Azure portal uses different names.

## Deployment Phases

## Phase 1: Azure Subscription and Guardrail Validation

Objective:

Confirm the lab is being deployed in the correct Azure subscription with cost controls in place.

Actions:

- Confirm Azure subscription
- Confirm budget or cost alert
- Confirm Log Analytics daily cap plan
- Confirm resource group naming
- Confirm planned region

Evidence to capture:

- Subscription context, redacted if necessary
- Budget or cost alert screenshot
- Daily cap screenshot after Log Analytics Workspace creation

Validation commands:

    az account show --output table

## Phase 2: Resource Group Creation

Objective:

Create or confirm the dedicated resource group for the lab.

Actions:

- Create dedicated lab resource group
- Confirm no production resources are included

Evidence to capture:

- Resource group inventory screenshot
- Azure CLI resource group validation output, redacted if necessary

Validation command:

    az group show \
      --name "$RG_NAME" \
      --output table

## Phase 3: Network Foundation

Objective:

Create the network foundation for the honeypot VM.

Actions:

- Create or confirm virtual network
- Create or confirm subnet
- Create or confirm network security group
- Confirm no VNet peering
- Confirm RDP exposure is intentional and documented

Evidence to capture:

- VNet/subnet screenshot
- NSG screenshot
- RDP inbound rule screenshot

Validation commands:

    az network vnet show \
      --resource-group "$RG_NAME" \
      --name "$VNET_NAME" \
      --output table

    az network nsg show \
      --resource-group "$RG_NAME" \
      --name "$NSG_NAME" \
      --output table

## Phase 4: Windows Honeypot VM Deployment

Objective:

Deploy the Windows VM that will act as the RDP honeypot.

Actions:

- Create Windows VM
- Confirm VM is in the lab resource group
- Confirm VM has no production data
- Confirm RDP exposure is limited to the lab purpose
- Confirm credentials are unique to the lab and not reused

Evidence to capture:

- VM overview screenshot
- VM networking screenshot
- Public IP screenshot if safe/redacted
- NSG RDP rule screenshot

Validation command:

    az vm show \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --output table

## Phase 5: Log Analytics Workspace

Objective:

Create or confirm the Log Analytics Workspace used for Sentinel telemetry.

Actions:

- Create or confirm Log Analytics Workspace
- Configure daily cap
- Review retention settings
- Document cost-control decisions

Evidence to capture:

- Workspace overview screenshot
- Daily cap screenshot
- Retention settings screenshot if used

Validation command:

    az monitor log-analytics workspace show \
      --resource-group "$RG_NAME" \
      --workspace-name "$LAW_NAME" \
      --output table

## Phase 6: Microsoft Sentinel Enablement

Objective:

Enable Microsoft Sentinel on the Log Analytics Workspace.

Actions:

- Enable Sentinel
- Confirm Sentinel is attached to the correct workspace
- Confirm no unrelated workspaces are used

Evidence to capture:

- Sentinel overview screenshot
- Workspace/Sentinel association screenshot

Validation notes:

Document whether Sentinel was enabled through the Azure portal or CLI.

## Phase 7: Azure Monitor Agent and Data Collection

Objective:

Collect Windows Security Events from the honeypot VM.

Actions:

- Install or confirm Azure Monitor Agent
- Create or confirm Data Collection Rule
- Associate DCR with the VM
- Configure Windows Security Event collection
- Confirm SecurityEvent table receives data

Evidence to capture:

- Azure Monitor Agent extension screenshot
- Data Collection Rule screenshot
- DCR association screenshot
- SecurityEvent ingestion query screenshot

Validation query:

    SecurityEvent
    | take 10

## Phase 8: RDP Brute-Force Telemetry Validation

Objective:

Validate that failed RDP authentication attempts are visible in Log Analytics / Sentinel.

Actions:

- Wait for internet-originated RDP attempts
- Query Event ID 4625 failed logons
- Identify source IPs, attempted accounts, and timestamps
- Confirm no successful unauthorized logons

Evidence to capture:

- Event ID 4625 query screenshot
- Failed logon trend screenshot
- Top source IPs screenshot
- Top attempted usernames screenshot
- Successful logon check screenshot

Validation queries to document later:

    SecurityEvent
    | where EventID == 4625
    | take 50

    SecurityEvent
    | where EventID == 4624
    | take 50

## Phase 9: Sentinel Hunting and Detection Documentation

Objective:

Document KQL hunting logic and detection workflow.

Actions:

- Add KQL queries to the lab KQL folder
- Document each query's purpose
- Document triage use cases
- Create or document at least one analytics rule concept
- Link detections to the triage runbook

Evidence to capture:

- Hunting query screenshots
- Analytics rule screenshots if created
- Incident screenshots if generated

Files to update:

    labs/sentinel-rdp-honeypot-v2/kql/hunting-queries.md
    labs/sentinel-rdp-honeypot-v2/sentinel/analytics-rules-catalog.md
    labs/sentinel-rdp-honeypot-v2/runbooks/rdp-bruteforce-triage.md

## Phase 10: Evidence Capture and Redaction

Objective:

Capture portfolio evidence without exposing sensitive details.

Actions:

- Capture screenshots listed in the evidence checklist
- Redact sensitive values
- Update redaction notes
- Avoid committing raw logs or sensitive screenshots

Do not commit:

- Tenant IDs
- Subscription IDs
- Access tokens
- Secrets
- Real usernames or emails
- Unredacted public IPs if not intentionally disclosed
- Raw logs containing sensitive values

Evidence folder:

    labs/sentinel-rdp-honeypot-v2/evidence/sanitized-screenshots/

## Phase 11: Teardown

Objective:

Remove the lab after evidence capture.

Actions:

- Stop and deallocate VM
- Delete resource group
- Verify deletion
- Check for remaining lab resources
- Update GitHub issue

Runbook:

    labs/sentinel-rdp-honeypot-v2/runbooks/teardown-runbook.md

## Phase 12: Post-Deployment Documentation

Objective:

Convert the build into portfolio-grade evidence.

Files to complete:

    labs/sentinel-rdp-honeypot-v2/reports/findings-report.md
    labs/sentinel-rdp-honeypot-v2/reports/incident-timeline.md
    labs/sentinel-rdp-honeypot-v2/reports/remediation-recommendations.md
    labs/sentinel-rdp-honeypot-v2/rubric/scoring-worksheet.md
    labs/sentinel-rdp-honeypot-v2/resume-assets/interview-talking-points.md
    labs/sentinel-rdp-honeypot-v2/resume-assets/resume-bullets.md

## Completion Criteria

Deployment is complete when:

- [ ] Resource group is created
- [ ] Windows honeypot VM is deployed
- [ ] RDP exposure is configured intentionally
- [ ] Log Analytics Workspace is created
- [ ] Microsoft Sentinel is enabled
- [ ] Azure Monitor Agent is installed
- [ ] Data Collection Rule is configured
- [ ] SecurityEvent ingestion is validated
- [ ] Event ID 4625 failed logons are observed
- [ ] At least 5 KQL queries are documented
- [ ] Triage runbook is completed
- [ ] Evidence is captured and redacted
- [ ] Findings report is drafted
- [ ] Lab is torn down
- [ ] Cleanup is verified
