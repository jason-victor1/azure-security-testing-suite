# Teardown Runbook

## Purpose

This runbook defines the shutdown and cleanup process for the Sentinel RDP Honeypot v2 lab.

The lab must be stopped and deleted after evidence capture to reduce cost, limit exposure, and maintain a clean Azure environment.

## When to Use

Use this runbook after:

- SecurityEvent ingestion is validated
- Event ID 4625 failed logons are observed
- Required screenshots are captured
- KQL queries are documented
- Triage notes are completed
- Findings are drafted

## Pre-Teardown Evidence Checklist

Before deleting resources, confirm the following evidence has been captured and reviewed:

- [ ] Resource group inventory
- [ ] Budget or cost guardrail
- [ ] Log Analytics daily cap
- [ ] Microsoft Sentinel enabled
- [ ] VNet, subnet, and NSG configuration
- [ ] RDP NSG rule
- [ ] Azure Monitor Agent or data collection setup
- [ ] SecurityEvent ingestion
- [ ] Event ID 4625 validation
- [ ] Failed RDP activity query results
- [ ] Workbook or visualization, if used
- [ ] Redaction notes updated

## Required Variables

Set these values before running teardown commands:

    export RG_NAME="rg-sentinel-rdp-honeypot-v2"
    export VM_NAME="vm-rdp-honeypot-v2"

## Step 1: Confirm Current Resources

List resources in the lab resource group:

    az resource list \
      --resource-group "$RG_NAME" \
      --output table

## Step 2: Stop and Deallocate the VM

Stop and deallocate the VM before deleting resources:

    az vm deallocate \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME"

Verify VM power state:

    az vm get-instance-view \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" \
      --output table

Expected result:

    VM deallocated

## Step 3: Delete the Resource Group

Delete the lab resource group after evidence capture is complete:

    az group delete \
      --name "$RG_NAME" \
      --yes

## Step 4: Verify Resource Group Deletion

Confirm the resource group no longer exists:

    az group exists \
      --name "$RG_NAME"

Expected result:

    false

## Step 5: Check for Remaining Lab Resources

If needed, search for remaining lab-named resources:

    az resource list \
      --query "[?contains(name, 'honeypot') || contains(name, 'sentinel') || contains(name, 'rdp')].{Name:name, Type:type, ResourceGroup:resourceGroup}" \
      --output table

## Step 6: Update GitHub Issue

After teardown, add a comment to the project issue with:

- Teardown date
- Resource group deletion result
- Evidence captured
- Any cleanup issues
- Remaining documentation tasks

Example issue comment command:

    gh issue comment 2 --body "Teardown completed. Lab resource group was deleted and cleanup was verified. Evidence review and documentation updates remain in progress."

## Step 7: Final Local Documentation Check

Before committing evidence or documentation, run:

    git status --short

    find labs/sentinel-rdp-honeypot-v2 -maxdepth 3 -type f | sort

    git ls-files | grep -i '.DS_Store'

Do not commit:

- .DS_Store files
- Raw logs
- Secrets
- Tenant IDs
- Subscription IDs
- Unredacted IPs
- Sensitive screenshots
- Credentials
- Access tokens

## Completion Criteria

Teardown is complete when:

- [ ] VM is stopped/deallocated
- [ ] Resource group is deleted
- [ ] Resource group deletion is verified
- [ ] Remaining lab resources are checked
- [ ] GitHub issue is updated
- [ ] Evidence is reviewed before commit
