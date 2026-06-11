# Cost-Control Checklist

## Purpose

This checklist reduces the risk of unexpected Azure costs during the Sentinel RDP Honeypot v2 lab.

The lab intentionally exposes a VM to the internet and collects security telemetry. Because telemetry, compute, storage, and Sentinel usage can create cost risk, cost controls must be checked before deployment and again before teardown.

## Pre-Deployment Cost Controls

Complete these before creating the VM or enabling Sentinel data collection.

- [ ] Confirm correct Azure subscription is selected
- [ ] Confirm lab resource group name is defined
- [ ] Confirm no production resources are in scope
- [ ] Confirm Azure budget or cost alert exists
- [ ] Confirm Log Analytics daily cap is configured
- [ ] Confirm VM size is intentionally selected
- [ ] Confirm expected VM runtime window is defined
- [ ] Confirm teardown runbook is available
- [ ] Confirm evidence plan is defined before resources are created

## Suggested Lab Runtime Boundary

Define the intended runtime window before exposing RDP.

Example:

    Start Date:
    Expected End Date:
    Maximum Runtime:
    Planned Teardown Date:

## Log Analytics Controls

Check the following:

- [ ] Daily ingestion cap configured
- [ ] Retention period reviewed
- [ ] Workspace region confirmed
- [ ] Sentinel enabled intentionally
- [ ] Only required log sources are collected for this lab phase

## VM Cost Controls

Check the following:

- [ ] VM size is appropriate for lab use
- [ ] Auto-shutdown is configured if used
- [ ] VM stop/deallocate command is documented
- [ ] Disk cost is understood
- [ ] Public IP and related networking resources are included in teardown

## Stop/Deallocate Command Template

Set variables first:

    export RG_NAME="rg-sentinel-rdp-honeypot-v2"
    export VM_NAME="vm-rdp-honeypot-v2"

Stop and deallocate the VM:

    az vm deallocate \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME"

Verify VM power state:

    az vm get-instance-view \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" \
      --output table

## Post-Lab Cost Checks

After evidence capture:

- [ ] VM stopped/deallocated
- [ ] Resource group deletion initiated
- [ ] Resource group deletion verified
- [ ] Azure Cost Management checked
- [ ] No unexpected lab resources remain
- [ ] GitHub issue updated with cleanup confirmation

## Completion Criteria

This checklist is complete when cost controls were checked before deployment, the lab was stopped or deleted after evidence capture, and cleanup was verified.
