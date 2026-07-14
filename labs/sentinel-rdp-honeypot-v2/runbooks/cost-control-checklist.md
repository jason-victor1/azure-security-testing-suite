# Sentinel RDP Honeypot v2 Cost-Control Checklist

## 1. Purpose

This checklist defines the cost-governance, runtime, ingestion, resource-inventory, and delayed-reconciliation controls for the Azure Sentinel RDP Honeypot v2 lab.

It is designed to:

- establish an approved cost boundary before Azure side effects;
- treat runtime and teardown as primary containment controls;
- use budgets and alerts as notifications rather than guaranteed spending caps;
- constrain Log Analytics ingestion without treating telemetry loss as normal operation;
- identify compute, storage, networking, and monitoring cost drivers;
- require named cost and teardown owners;
- distinguish preliminary cost observations from delayed or settled data;
- preserve failed or incomplete closure checks; and
- prevent unsupported claims about cost or cleanup.

This checklist does not authorize Azure deployment or public exposure.

## 2. Current status and authorization boundary

At this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Teardown and cost closure have not started.
- No v2 cost evidence exists.
- No claim of zero cost, final cost, or completed cleanup is authorized.

Static validation of this file does not satisfy the predeployment authorization gate.

## 3. Governing documents

Use this checklist with:

1. [`../docs/threat-model.md`](../docs/threat-model.md);
2. [`../docs/scope-and-credibility-notes.md`](../docs/scope-and-credibility-notes.md);
3. [`../docs/checklist-to-project-roadmap.md`](../docs/checklist-to-project-roadmap.md);
4. [`deployment-runbook.md`](deployment-runbook.md);
5. [`teardown-runbook.md`](teardown-runbook.md);
6. [`rdp-authentication-triage.md`](rdp-authentication-triage.md);
7. [`../evidence/README.md`](../evidence/README.md);
8. [`../evidence/redaction-notes.md`](../evidence/redaction-notes.md); and
9. [`../evidence/templates/evidence-record-template.md`](../evidence/templates/evidence-record-template.md).

When instructions conflict, authorization, safety, cost containment, telemetry integrity, teardown readiness, and evidence governance take precedence.

## 4. Cost-control principles

The following principles are mandatory:

1. **Budgets and alerts are notifications, not guaranteed hard stops.**
2. **Maximum runtime and explicit teardown ownership are primary cost boundaries.**
3. **Public-exposure duration is shorter than total VM runtime and requires separate authorization.**
4. **A Log Analytics daily cap is a last-resort ingestion guard, not a normal optimization strategy.**
5. **Reaching an ingestion cap is a telemetry failure and ends exposure-dependent activity.**
6. **Deallocating a VM stops compute usage but is not proof that all related charges ended.**
7. **Disks, public IPs, snapshots, workspaces, and other retained resources require explicit disposition.**
8. **Immediate cost data is preliminary and can be incomplete.**
9. **The 24-hour and 72-hour reviews are mandatory delayed-reconciliation checkpoints.**
10. **If cost data remains incomplete at 72 hours, closure remains open and another review is scheduled.**
11. **No zero-cost or final-cost claim is made before the relevant data has appeared and been reviewed.**
12. **Safety actions take priority over evidence capture.**
13. **Optional paid services remain disabled unless separately approved.**
14. **Failed, blocked, and inconclusive cost checks remain traceable.**

## 5. Operating modes

### 5.1 Normal mode

Normal mode applies only when:

- the applicable deployment phase has a recorded `GO`;
- tenant and subscription context are confirmed;
- the maximum approved cost is recorded;
- VM and public-exposure time limits are recorded;
- budget or alert disposition is documented;
- Log Analytics cap and retention decisions are recorded;
- the cost owner and teardown owner are available;
- private evidence storage is ready; and
- no mandatory cost stop condition is active.

Normal mode permits only the resources and services explicitly approved for the current phase.

### 5.2 Emergency mode

Emergency mode begins immediately when:

- a runtime or exposure deadline is reached;
- an unexpected paid service or SKU appears;
- a budget or ingestion threshold is reached;
- telemetry stops or the daily cap is reached;
- the cost or teardown owner becomes unavailable;
- an unknown residual resource appears;
- subscription context becomes uncertain; or
- the operator cannot confirm the cost state.

Emergency priorities are:

1. stop the current action;
2. remove or restrict TCP/3389 exposure;
3. deallocate the VM when appropriate;
4. begin emergency teardown;
5. preserve only evidence that does not delay containment;
6. record the trigger and response privately; and
7. schedule the required delayed cost checks.

## 6. Required cost-control record

Record the following privately before any Azure side effect:

| Field | Requirement |
|---|---|
| Decision timestamp | UTC |
| Operator | Authorized operator or private alias |
| Branch and commit | Exact repository state |
| Tenant alias | Private alias, not a public tenant ID |
| Subscription alias | Private alias, not a public subscription ID |
| Resource-group alias | Approved dedicated lab scope |
| Maximum approved total cost | Currency and amount |
| Budget scope | Subscription or resource group |
| Budget name | Private record |
| Budget amount | Currency and amount |
| Budget thresholds | Actual and forecast notification thresholds |
| VM size | Approved smallest-suitable SKU |
| Maximum VM runtime | Minutes or hours |
| Maximum public-exposure duration | Minutes |
| Runtime end | UTC |
| Public-exposure end | UTC |
| Log Analytics daily cap | Approved GB value or documented exception |
| Log Analytics retention | Approved duration |
| Optional paid services | Disabled or separately approved |
| Cost owner | Named and available |
| Teardown owner | Named and available |
| Immediate review owner | Named |
| 24-hour review owner | Named |
| 72-hour review owner | Named |
| Private evidence location | Approved nonrepository path |
| Decision | `GO` or `NO-GO` |

A `GO` authorizes only the named phase. It does not authorize public exposure or optional paid services.

## 7. Predeployment cost gate

Every mandatory item must pass:

### 7.1 Scope and ownership

- [ ] Correct tenant and subscription are confirmed.
- [ ] Dedicated resource group is approved.
- [ ] No production resource is in scope.
- [ ] Cost owner is named and available.
- [ ] Teardown owner is named and available.
- [ ] Immediate, 24-hour, and 72-hour review owners are named.
- [ ] Private evidence location exists outside the repository.

### 7.2 Approved limits

- [ ] Maximum approved total cost is recorded.
- [ ] Maximum VM runtime is recorded.
- [ ] Maximum public-exposure duration is recorded.
- [ ] Runtime end is recorded in UTC.
- [ ] Public-exposure end is recorded in UTC.
- [ ] VM size is approved.
- [ ] Disk type and size are approved.
- [ ] Public IP requirement is approved.
- [ ] Log Analytics daily cap is approved or an exception is documented.
- [ ] Retention duration is approved.

### 7.3 Budget and notification controls

- [ ] Budget scope is confirmed.
- [ ] Budget amount is recorded.
- [ ] At least two pre-limit notification thresholds are configured or intentionally documented.
- [ ] An at-limit threshold is configured or intentionally documented.
- [ ] Actual-cost notifications are configured.
- [ ] Forecast notifications are configured when supported and useful.
- [ ] Notification recipients or action groups are confirmed.
- [ ] Budget alerts are not described as hard spending stops.

### 7.4 Service restrictions

- [ ] Only required data sources are enabled.
- [ ] Broad Defender for Cloud plans remain disabled unless approved.
- [ ] Azure Bastion remains disabled unless approved.
- [ ] VPN Gateway remains disabled unless approved.
- [ ] Azure Firewall remains disabled unless approved.
- [ ] Flow-log expansion remains out of scope.
- [ ] No premium disk or unnecessary high-cost SKU is selected.
- [ ] No unreviewed marketplace image or paid extension is planned.

### 7.5 Teardown readiness

- [ ] VM deallocation command is ready.
- [ ] Resource-group deletion procedure is ready.
- [ ] Residual-resource inventory procedure is ready.
- [ ] Workspace retention or deletion decision process is understood.
- [ ] Emergency teardown is executable.
- [ ] Operator understands that deallocation alone is not full cost closure.

If any mandatory item fails, record `NO-GO` or `Blocked` and stop.

## 8. Command preflight and read-only validation

Define explicit nonsecret values:

    export AZ_TENANT_ID="<approved-tenant-guid>"
    export AZ_SUBSCRIPTION_ID="<approved-subscription-guid>"
    export RG_NAME="<approved-resource-group-name>"
    export VM_NAME="<approved-vm-name>"
    export LAW_NAME="<approved-log-analytics-workspace-name>"
    export MAX_APPROVED_COST_USD="<approved-decimal-amount>"
    export MAX_VM_RUNTIME_MINUTES="<approved-positive-integer>"
    export MAX_PUBLIC_EXPOSURE_MINUTES="<approved-positive-integer>"
    export LAW_DAILY_CAP_GB="<approved-positive-decimal>"
    export RUNTIME_END_UTC="<YYYY-MM-DDTHH:MM:SSZ>"
    export PUBLIC_EXPOSURE_END_UTC="<YYYY-MM-DDTHH:MM:SSZ>"
    export COST_OWNER="<private-owner-alias>"
    export TEARDOWN_OWNER="<private-owner-alias>"
    export PRIVATE_EVIDENCE_DIR="<approved-nonrepository-path>"

Run strict mode and preflight before any Azure command:

    set -Eeuo pipefail

    require_command() {
      command -v "$1" >/dev/null 2>&1 || {
        printf 'Missing required command: %s\n' "$1" >&2
        exit 1
      }
    }

    require_var() {
      local name="$1"

      if [[ -z "${!name:-}" ]]; then
        printf 'Missing required variable: %s\n' "$name" >&2
        exit 1
      fi
    }

    require_command az
    require_command date
    require_command python3
    require_command shasum

    for name in \
      AZ_TENANT_ID \
      AZ_SUBSCRIPTION_ID \
      RG_NAME \
      VM_NAME \
      LAW_NAME \
      MAX_APPROVED_COST_USD \
      MAX_VM_RUNTIME_MINUTES \
      MAX_PUBLIC_EXPOSURE_MINUTES \
      LAW_DAILY_CAP_GB \
      RUNTIME_END_UTC \
      PUBLIC_EXPOSURE_END_UTC \
      COST_OWNER \
      TEARDOWN_OWNER \
      PRIVATE_EVIDENCE_DIR
    do
      require_var "$name"
    done

    python3 - \
      "$MAX_APPROVED_COST_USD" \
      "$MAX_VM_RUNTIME_MINUTES" \
      "$MAX_PUBLIC_EXPOSURE_MINUTES" \
      "$LAW_DAILY_CAP_GB" \
      "$RUNTIME_END_UTC" \
      "$PUBLIC_EXPOSURE_END_UTC" <<'PY'
    from datetime import datetime
    from decimal import Decimal, InvalidOperation
    import sys

    cost, vm_minutes, exposure_minutes, cap, runtime_end, exposure_end = sys.argv[1:]

    try:
        if Decimal(cost) <= 0:
            raise ValueError("maximum cost must be positive")
        if int(vm_minutes) <= 0:
            raise ValueError("VM runtime must be positive")
        if int(exposure_minutes) <= 0:
            raise ValueError("exposure duration must be positive")
        if int(exposure_minutes) > int(vm_minutes):
            raise ValueError("exposure duration exceeds VM runtime")
        if Decimal(cap) <= 0:
            raise ValueError("daily cap must be positive")
        runtime_dt = datetime.strptime(runtime_end, "%Y-%m-%dT%H:%M:%SZ")
        exposure_dt = datetime.strptime(exposure_end, "%Y-%m-%dT%H:%M:%SZ")
        if exposure_dt > runtime_dt:
            raise ValueError("exposure end occurs after runtime end")
    except (InvalidOperation, ValueError) as exc:
        raise SystemExit(f"Invalid cost-control input: {exc}")

    print("PASS: Numeric limits and UTC deadlines are internally consistent.")
    PY

    CURRENT_TENANT_ID="$(
      az account show \
        --query tenantId \
        --output tsv
    )"

    CURRENT_SUBSCRIPTION_ID="$(
      az account show \
        --query id \
        --output tsv
    )"

    [[ "$CURRENT_TENANT_ID" == "$AZ_TENANT_ID" ]]
    [[ "$CURRENT_SUBSCRIPTION_ID" == "$AZ_SUBSCRIPTION_ID" ]]

    az account show \
      --query '{tenantId:tenantId,subscriptionId:id,subscriptionName:name}' \
      --output json

    az group exists \
      --name "$RG_NAME"

These commands confirm context and inputs. They do not authorize resource creation.

## 9. Budget and alert controls

### 9.1 Required behavior

The budget record must identify:

- scope;
- amount;
- reset period;
- actual-cost thresholds;
- forecast thresholds where used;
- recipients or action groups;
- creation or verification timestamp; and
- reviewer.

Budget notifications do not guarantee shutdown, deletion, or prevention of additional charges.

### 9.2 Required response to an alert

When any budget alert fires:

1. confirm the scope and threshold;
2. compare current runtime against the approved boundary;
3. review active resources;
4. stop optional activity;
5. remove public exposure when present;
6. deallocate or teardown when the remaining cost margin is uncertain;
7. record the alert privately; and
8. keep delayed cost reviews scheduled.

Do not wait for a 100-percent notification when the remaining approved margin is insufficient.

### 9.3 Automation boundary

Core v2 does not implement automated destructive actions based on budget alerts.

Any future automation requires separate design for:

- authorization;
- false-positive handling;
- service-specific shutdown safety;
- telemetry preservation;
- evidence impact;
- rollback;
- test coverage; and
- ownership.

## 10. Log Analytics and Sentinel cost controls

### 10.1 Before collection

- [ ] Required Windows Security Event scope is defined.
- [ ] Unnecessary connectors remain disabled.
- [ ] Workspace region is confirmed.
- [ ] Pricing tier is reviewed.
- [ ] Daily cap is recorded.
- [ ] Retention is purpose-limited.
- [ ] Daily-cap notification or monitoring approach is defined.
- [ ] Expected ingestion volume and uncertainty are recorded.
- [ ] Cost owner understands that ingestion and retention are separate cost drivers.

### 10.2 Daily-cap boundary

The daily cap is a protective boundary against unexpected billable ingestion. It is not permission to continue operating until collection stops.

When the daily cap is reached or collection stops:

1. classify the condition as telemetry failure;
2. end exposure-dependent activity;
3. remove TCP/3389 exposure;
4. stop controlled testing;
5. preserve only safe private evidence;
6. investigate the ingestion source and cap state;
7. record the test as `Failed`, `Blocked`, or `Inconclusive`; and
8. require a new go/no-go decision before resuming.

Do not increase or disable the cap during an active observation window without a separately approved change record.

### 10.3 Retention and workspace disposition

At teardown, record one disposition:

- delete the workspace with the resource group;
- retain temporarily for an approved evidence or validation need; or
- retain under a separately approved scope.

A retained workspace must have:

- an owner;
- a purpose;
- an approved retention duration;
- a cost boundary;
- a deletion date; and
- a follow-up record.

## 11. VM and compute controls

### 11.1 Before VM creation

- [ ] Smallest suitable VM size is selected.
- [ ] Premium features are excluded unless approved.
- [ ] OS and data-disk requirements are documented.
- [ ] Auto-shutdown is configured or intentionally omitted with justification.
- [ ] Maximum runtime timer is active.
- [ ] Runtime owner is present.
- [ ] Deallocation and teardown commands are ready.

### 11.2 Deallocate when compute must stop

    az vm deallocate \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --no-wait

    az vm wait \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --deallocated

    az vm get-instance-view \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" \
      --output tsv

A VM is not considered contained until the expected deallocated state is observed.

Deallocation is not complete cost closure. Continue to storage, networking, workspace, and residual-resource review.

## 12. Storage, networking, and residual-resource controls

Review and disposition:

- OS disks;
- data disks;
- snapshots;
- images;
- network interfaces;
- public IP addresses;
- load balancers when present;
- network security groups;
- Log Analytics workspaces;
- data collection rules and associations;
- automation artifacts;
- role assignments;
- temporary identities; and
- any unexpected resource type.

Use read-only inventory commands:

    az resource list \
      --resource-group "$RG_NAME" \
      --query '[].{name:name,type:type,location:location,id:id}' \
      --output table

    az disk list \
      --resource-group "$RG_NAME" \
      --query '[].{name:name,diskState:diskState,sku:sku.name,sizeGb:diskSizeGb}' \
      --output table

    az network public-ip list \
      --resource-group "$RG_NAME" \
      --query '[].{name:name,sku:sku.name,allocation:publicIPAllocationMethod,ipAddress:ipAddress}' \
      --output table

    az network nic list \
      --resource-group "$RG_NAME" \
      --query '[].{name:name,provisioningState:provisioningState}' \
      --output table

Raw command output remains private because it can contain resource IDs and addresses.

## 13. Runtime and mandatory cost stop conditions

| Stop condition | Immediate response |
|---|---|
| Maximum VM runtime reached | Remove exposure, deallocate the VM, begin teardown |
| Maximum public-exposure duration reached | Remove exposure immediately; continue only approved nonexposed closure work |
| Maximum approved cost is reached or cannot be bounded | Stop the lab and begin teardown |
| Budget alert indicates insufficient remaining margin | Stop optional activity; deallocate or teardown |
| Log Analytics daily cap is reached | End exposure-dependent activity and enter emergency review |
| Unexpected paid service, tier, SKU, or marketplace item appears | Stop activity and remove or correct the resource |
| Cost or usage data is unavailable and runtime risk is uncertain | Use runtime boundary; stop and teardown rather than assume safety |
| Cost owner or teardown owner becomes unavailable | Remove exposure and stop compute |
| Unknown residual resource appears | Keep closure open and investigate or delete through the approved runbook |
| Resource-group deletion fails | Preserve failure evidence and remediate; do not claim cleanup |
| Subscription or tenant context is uncertain | Stop all Azure actions until context is revalidated |
| Operator cannot determine whether continued operation is affordable | Stop and classify the result as `Blocked` or `Inconclusive` |

## 14. Immediate closure review

Perform immediately after observation, testing, or emergency stop:

- [ ] Public or restricted TCP/3389 exposure is removed.
- [ ] VM is deallocated or deleted according to the approved plan.
- [ ] Resource-group deletion is initiated when required.
- [ ] Resource-group deletion reaches the expected state or failure is recorded.
- [ ] Residual-resource inventory is captured privately.
- [ ] Workspace disposition is recorded.
- [ ] Preliminary cost and usage view is reviewed.
- [ ] Budget-alert state is reviewed.
- [ ] Daily-cap state is reviewed.
- [ ] Unexpected services or resources are investigated.
- [ ] Immediate result is labeled `Preliminary`, not final.
- [ ] 24-hour and 72-hour reviews remain scheduled.

The immediate review does not prove settled cost or complete billing reconciliation.

## 15. Twenty-four-hour reconciliation

At approximately 24 hours:

- [ ] Recheck Cost Management at the same approved scope.
- [ ] Record the displayed cost period and data timestamp when available.
- [ ] Compare observed cost with the preliminary record.
- [ ] Recheck resource-group existence.
- [ ] Recheck residual resources.
- [ ] Confirm no VM, disk, public IP, NIC, snapshot, or workspace remains unexpectedly.
- [ ] Review ingestion and retention charges that have appeared.
- [ ] Review alerts and notification delivery.
- [ ] Record missing or delayed data.
- [ ] Remediate any residual resource.
- [ ] Keep closure open when data is incomplete.
- [ ] Preserve the result as `Passed`, `Failed`, `Blocked`, or `Inconclusive`.

## 16. Seventy-two-hour reconciliation

At approximately 72 hours:

- [ ] Recheck Cost Management at the same approved scope.
- [ ] Record actual displayed cost and the covered period.
- [ ] Compare against the maximum approved cost.
- [ ] Confirm final planned residual-resource state.
- [ ] Confirm workspace and telemetry disposition.
- [ ] Resolve or document billing lag.
- [ ] Record credits, taxes, or excluded charges only when relevant and understood.
- [ ] Do not claim zero cost merely because no charge is displayed.
- [ ] Do not call the cost final when the provider data remains incomplete.
- [ ] Schedule another review when reconciliation remains incomplete.
- [ ] Record the final planned closure decision and reviewer.

The 72-hour checkpoint is the final planned review for core v2, not a guarantee that every billing model has fully settled.

## 17. Emergency cost-response procedure

1. Record the trigger and UTC time privately when safe.
2. Stop the current operation.
3. Remove or restrict TCP/3389 exposure.
4. Stop controlled authentication testing.
5. Deallocate the VM when appropriate.
6. Invoke the emergency path in [`teardown-runbook.md`](teardown-runbook.md).
7. Inventory active and residual resources.
8. Preserve only evidence that does not delay containment.
9. Review budget, cap, runtime, and current cost state.
10. Record unknowns and provider-data latency.
11. Keep 24-hour and 72-hour reviews active.
12. Do not resume without a new go/no-go decision.

## 18. Evidence and claim handling

### 18.1 Private cost evidence

Private evidence may include:

- Cost Management screenshots or exports;
- budget configuration;
- alert history;
- resource IDs;
- subscription and tenant identifiers;
- exact resource names;
- pricing or SKU selections;
- usage quantities;
- ingestion volume;
- retention settings;
- timestamps;
- deletion output; and
- billing-period details.

Store raw evidence outside the repository.

### 18.2 Public cost evidence

Public evidence requires:

1. sanitization;
2. redaction review;
3. technical review;
4. claim-boundary review;
5. publication approval;
6. manifest registration; and
7. SHA-256 recording when required.

### 18.3 Prohibited claims

Do not claim:

- zero cost before relevant data appears and is reviewed;
- final cost while data remains delayed or incomplete;
- complete cleanup before residual-resource checks;
- cost containment merely because a budget exists;
- complete compute shutdown before deallocation is confirmed;
- no remaining charges merely because the VM is deallocated; or
- successful cost governance without evidence from all required checkpoints.

## 19. Cost-control record template

    Cost-control record:
    Operator:
    Branch:
    Commit:
    Decision timestamp UTC:
    Authorized phase:
    Tenant alias:
    Subscription alias:
    Resource-group alias:
    Maximum approved total cost:
    Budget scope:
    Budget name:
    Budget amount:
    Budget thresholds:
    Notification recipients:
    VM size:
    Disk selection:
    Maximum VM runtime:
    Maximum public-exposure duration:
    Runtime end UTC:
    Public-exposure end UTC:
    Log Analytics daily cap:
    Log Analytics retention:
    Optional paid services:
    Cost owner:
    Teardown owner:
    Immediate review owner:
    24-hour review owner:
    72-hour review owner:
    Private evidence location:
    Immediate result:
    24-hour result:
    72-hour result:
    Residual-resource state:
    Workspace disposition:
    Billing-latency note:
    Maximum-cost comparison:
    Test outcome:
    Evidence lifecycle:
    Reviewer:
    Closure decision:
    Additional review required:

Do not place tenant IDs, subscription IDs, resource IDs, public IP addresses, email addresses, secrets, or private evidence paths in a public copy.

## 20. Completion criteria

This checklist is operationally complete when it:

- separates normal and emergency modes;
- records maximum cost, runtime, and public-exposure boundaries;
- requires named cost and teardown owners;
- treats budgets as notifications rather than guaranteed hard stops;
- defines Log Analytics cap and retention controls;
- maps every cost stop condition to an immediate response;
- uses strict command preflight and explicit variables;
- distinguishes VM deallocation from full resource cleanup;
- reviews compute, storage, networking, and monitoring resources;
- requires immediate, 24-hour, and 72-hour checks;
- preserves failed, blocked, and inconclusive closure results;
- prevents premature zero-cost or final-cost claims;
- integrates deployment, teardown, triage, and evidence governance; and
- does not claim that v2 execution or cost validation has occurred.

## 21. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Teardown and cost closure have not started.
- No v2 cost evidence exists.
- No immediate, 24-hour, or 72-hour cost review has occurred.
- No zero-cost, final-cost, or completed-cleanup claim is authorized.
- This checklist is design-stage operational documentation only.
