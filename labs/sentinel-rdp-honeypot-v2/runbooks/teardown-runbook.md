# Sentinel RDP Honeypot v2 Teardown Runbook

## 1. Purpose

This runbook defines the normal and emergency teardown workflow for the Azure Sentinel RDP Honeypot v2 lab.

It is designed to:

- remove public or restricted TCP/3389 exposure promptly;
- stop controlled authentication activity;
- deallocate or delete disposable compute safely;
- remove monitoring associations and temporary security objects;
- disposition identities, role assignments, analytics objects, and telemetry;
- delete the dedicated resource group only after scope confirmation;
- verify deletion reaches a terminal state;
- search for subscription-level residual resources;
- preserve failed, blocked, and inconclusive cleanup outcomes;
- perform immediate, 24-hour, and 72-hour cost reconciliation; and
- prevent unsupported cleanup, cost, or project-completion claims.

This runbook does not authorize Azure execution by itself.

## 2. Current status and authorization boundary

At this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Teardown and cost closure have not started.
- No v2 teardown evidence exists.
- No resource-group deletion has been attempted.
- No cleanup or cost-closure claim is authorized.

Static validation of this runbook does not satisfy the deployment or teardown authorization gates.

## 3. Governing documents

Use this runbook with:

1. [`../docs/threat-model.md`](../docs/threat-model.md);
2. [`../docs/scope-and-credibility-notes.md`](../docs/scope-and-credibility-notes.md);
3. [`../docs/checklist-to-project-roadmap.md`](../docs/checklist-to-project-roadmap.md);
4. [`deployment-runbook.md`](deployment-runbook.md);
5. [`cost-control-checklist.md`](cost-control-checklist.md);
6. [`rdp-authentication-triage.md`](rdp-authentication-triage.md);
7. [`../sentinel/analytics-rules-catalog.md`](../sentinel/analytics-rules-catalog.md);
8. [`../evidence/README.md`](../evidence/README.md);
9. [`../evidence/redaction-notes.md`](../evidence/redaction-notes.md);
10. [`../evidence/manifest.md`](../evidence/manifest.md);
11. [`../evidence/claim-evidence-matrix.md`](../evidence/claim-evidence-matrix.md); and
12. [`../evidence/templates/evidence-record-template.md`](../evidence/templates/evidence-record-template.md).

When instructions conflict, authorization, safety, exposure removal, identity protection, cost containment, and evidence governance take precedence.

## 4. Teardown principles

The following principles are mandatory:

1. **Safety has priority over evidence preservation.**
2. **Exposure removal precedes optional evidence capture.**
3. **Normal and emergency teardown are distinct.**
4. **Every mandatory stop condition maps to an immediate response.**
5. **A deletion request is not proof of deletion completion.**
6. **VM deallocation is not full resource or cost closure.**
7. **The resource group must be confirmed as dedicated before deletion.**
8. **Force deletion is not a default recovery mechanism.**
9. **Locks, backup dependencies, deny assignments, and retained services must be investigated before overrides.**
10. **DCR associations, analytics objects, temporary identities, and role assignments require explicit disposition.**
11. **Workspace deletion, retention, and permanent deletion are different outcomes.**
12. **Subscription-wide orphan review is required after resource-group deletion.**
13. **Immediate cost data is preliminary.**
14. **The 24-hour and 72-hour reviews remain required after teardown.**
15. **Failed, blocked, and inconclusive teardown steps remain traceable.**
16. **No cleanup or zero-cost claim is promoted without supporting evidence.**

## 5. Operating modes

### 5.1 Normal mode

Normal mode applies when:

- the approved observation or validation objective is complete;
- the authorized runtime boundary is approaching or reached;
- no active compromise or unsafe deviation is known;
- tenant and subscription context are confirmed;
- the dedicated resource group is confirmed;
- teardown ownership is active;
- the private evidence location is available;
- required variables are explicit; and
- the normal teardown decision is recorded as `GO`.

Normal mode follows the planned teardown sequence and still prioritizes exposure removal over screenshots or exports.

### 5.2 Emergency mode

Emergency mode begins immediately when any mandatory stop condition occurs, including:

- unexplained successful remote-interactive access;
- telemetry failure during exposure;
- an unauthorized NSG or firewall change;
- suspicious process, persistence, identity, or privilege activity;
- credential exposure;
- unexpected outbound activity;
- runtime or cost limit breach;
- loss of operator or teardown-owner availability;
- uncertain tenant or subscription context;
- trusted connectivity discovery; or
- inability to determine whether the environment is safe.

Emergency priorities are:

1. stop the current action;
2. remove or restrict TCP/3389 exposure;
3. stop controlled authentication testing;
4. deallocate the VM when appropriate;
5. protect or invalidate affected credentials through an approved process;
6. begin emergency cleanup;
7. preserve only evidence that does not delay containment;
8. record failures and unknowns privately; and
9. keep delayed cost reconciliation scheduled.

Emergency mode never waits for evidence completeness.

## 6. Required variables and command preflight

### 6.1 Required nonsecret variables

Define explicit values before any teardown command:

    export AZ_TENANT_ID="<approved-tenant-guid>"
    export AZ_SUBSCRIPTION_ID="<approved-subscription-guid>"
    export RG_NAME="<approved-resource-group-name>"
    export RG_SCOPE="/subscriptions/$AZ_SUBSCRIPTION_ID/resourceGroups/$RG_NAME"
    export VM_NAME="<approved-vm-name>"
    export NSG_NAME="<approved-nsg-name>"
    export RDP_RULE_NAME="<exact-approved-rdp-rule-name>"
    export LAW_NAME="<approved-log-analytics-workspace-name>"
    export DCR_NAME="<approved-data-collection-rule-name>"
    export DCR_ASSOC_NAME="<approved-dcr-association-name>"
    export PROJECT_TAG_VALUE="Sentinel-RDP-Honeypot-v2"
    export PROJECT_NAME_PREFIX="<approved-project-name-prefix>"
    export WORKSPACE_DISPOSITION="<delete-with-resource-group|retain-temporarily|permanent-delete-separate>"
    export DELETE_POLL_ATTEMPTS="<approved-positive-integer>"
    export DELETE_POLL_INTERVAL_SECONDS="<approved-positive-integer>"
    export COST_OWNER="<private-owner-alias>"
    export TEARDOWN_OWNER="<private-owner-alias>"
    export PRIVATE_EVIDENCE_DIR="<approved-nonrepository-path>"

Optional objects must use explicit values when applicable:

    export TEMP_IDENTITY_NAME="<exact-temporary-identity-name-or-empty>"
    export TEMP_ROLE_ASSIGNMENT_ID="<exact-role-assignment-resource-id-or-empty>"
    export SENTINEL_ALERT_RULE_NAME="<exact-rule-name-or-empty>"

Do not place secrets, credentials, access tokens, raw evidence, or personal identifiers in this file, Git, shell history, screenshots, or public issue comments.

### 6.2 Shell and Azure CLI preflight

Run strict mode and validate tooling before side effects:

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

    require_azure_cli_version() {
      local minimum="$1"
      local installed

      installed="$(az version --query '"azure-cli"' --output tsv)"

      if ! python3 -c 'import sys; parse=lambda value: tuple(int(part) for part in value.split(".")[:3]); raise SystemExit(0 if parse(sys.argv[1]) >= parse(sys.argv[2]) else 1)' "$installed" "$minimum"
      then
        printf 'Azure CLI %s or newer is required; found %s\n' \
          "$minimum" "$installed" >&2
        exit 1
      fi
    }

    require_az_extension() {
      local name="$1"

      if ! az extension show \
        --name "$name" \
        --only-show-errors \
        >/dev/null 2>&1
      then
        printf 'Missing reviewed Azure CLI extension: %s\n' \
          "$name" >&2
        printf 'Do not rely on automatic extension installation.\n' >&2
        exit 1
      fi
    }

    require_command az
    require_command date
    require_command python3
    require_command shasum
    require_command sleep

    require_azure_cli_version "2.61.0"
    require_az_extension "monitor-control-service"

    for name in \
      AZ_TENANT_ID \
      AZ_SUBSCRIPTION_ID \
      RG_NAME \
      RG_SCOPE \
      VM_NAME \
      NSG_NAME \
      RDP_RULE_NAME \
      LAW_NAME \
      DCR_NAME \
      DCR_ASSOC_NAME \
      PROJECT_TAG_VALUE \
      PROJECT_NAME_PREFIX \
      WORKSPACE_DISPOSITION \
      DELETE_POLL_ATTEMPTS \
      DELETE_POLL_INTERVAL_SECONDS \
      COST_OWNER \
      TEARDOWN_OWNER \
      PRIVATE_EVIDENCE_DIR
    do
      require_var "$name"
    done

    case "$WORKSPACE_DISPOSITION" in
      delete-with-resource-group|retain-temporarily|permanent-delete-separate)
        ;;
      *)
        printf 'Invalid workspace disposition: %s\n' \
          "$WORKSPACE_DISPOSITION" >&2
        exit 1
        ;;
    esac

    python3 - \
      "$DELETE_POLL_ATTEMPTS" \
      "$DELETE_POLL_INTERVAL_SECONDS" <<'PY'
    import sys

    attempts, interval = sys.argv[1:]

    try:
        if int(attempts) <= 0:
            raise ValueError("poll attempts must be positive")
        if int(interval) <= 0:
            raise ValueError("poll interval must be positive")
    except ValueError as exc:
        raise SystemExit(f"Invalid teardown input: {exc}")

    print("PASS: Teardown polling inputs are valid.")
    PY

### 6.3 Context verification

Confirm tenant and subscription before any mutation:

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

If either value differs, stop all Azure actions.

### 6.4 Extension boundary

The DCR commands in this runbook require the reviewed `monitor-control-service` extension.

The preflight verifies that the extension already exists. It does not install or update software.

Microsoft Sentinel alert-rule CLI commands are extension-backed and currently experimental. This runbook therefore does not automatically install the Sentinel extension or execute an experimental alert-rule deletion command. Any analytics-rule cleanup must follow the separately reviewed Commit 4 disposition procedure.

## 7. Teardown authorization gate

Every mandatory item must pass before normal teardown:

### 7.1 Scope

- [ ] Correct tenant and subscription are confirmed.
- [ ] Resource-group name and scope are exact.
- [ ] The resource group is dedicated to this lab.
- [ ] No production or unrelated resource is present.
- [ ] Resource locks are listed.
- [ ] Backup or recovery dependencies are reviewed.
- [ ] Deny assignments or policy constraints are understood.
- [ ] Workspace disposition is recorded.
- [ ] Teardown owner is present.

### 7.2 Safety

- [ ] Public or restricted exposure can be removed immediately.
- [ ] VM deallocation is executable.
- [ ] Emergency teardown remains available.
- [ ] Credential response owner is known.
- [ ] No evidence task can delay containment.
- [ ] Stop conditions and immediate actions are understood.

### 7.3 Object disposition

- [ ] DCR association disposition is recorded.
- [ ] DCR disposition is recorded.
- [ ] Analytics-rule disposition is recorded.
- [ ] Temporary identity disposition is recorded.
- [ ] Temporary role-assignment disposition is recorded.
- [ ] Workspace and telemetry disposition is recorded.
- [ ] Retained objects have owners and deletion dates.

### 7.4 Evidence and cost

- [ ] Private evidence location is available.
- [ ] Required test outcomes are recorded.
- [ ] Failed or incomplete steps will be preserved.
- [ ] Immediate cost review owner is present.
- [ ] 24-hour review is scheduled.
- [ ] 72-hour review is scheduled.
- [ ] Public claims remain blocked pending review.

If a normal-teardown item fails, record `NO-GO`, `Blocked`, or `Inconclusive`. Enter emergency mode when safety or cost requires immediate containment.

## 8. Pre-teardown private record

Create a private teardown record containing:

| Field | Requirement |
|---|---|
| Teardown identifier | Private stable identifier |
| Mode | Normal or Emergency |
| Trigger | Objective complete, runtime, cost, stop condition, or other |
| Trigger time | UTC |
| Operator | Authorized operator or alias |
| Teardown owner | Named and available |
| Branch and commit | Exact repository state |
| Tenant alias | Private |
| Subscription alias | Private |
| Resource-group alias | Private |
| Exposure state | Present, removed, absent, or unknown |
| VM state | Running, stopped, deallocated, absent, or unknown |
| DCR state | Present, deleted, retained, or unknown |
| Analytics-rule state | Present, disabled, deleted, not created, or unknown |
| Identity state | Present, removed, not created, or unknown |
| Workspace disposition | Exact approved value |
| Private evidence location | Approved nonrepository path |
| Immediate cost owner | Named |
| 24-hour cost owner | Named |
| 72-hour cost owner | Named |
| Authorization decision | `GO`, `NO-GO`, or emergency trigger |

Do not record passwords, tokens, raw tenant IDs, raw subscription IDs, or public source addresses in a public copy.

## 9. Normal teardown sequence

Normal teardown follows this order:

1. reconfirm tenant, subscription, and resource-group scope;
2. remove public or restricted TCP/3389 exposure;
3. stop controlled authentication activity;
4. deallocate the VM and verify its state;
5. capture only minimum safe private state records;
6. inventory resources, locks, identities, assignments, DCR objects, and monitoring objects;
7. disposition temporary monitoring and identity objects;
8. apply the approved workspace disposition;
9. delete the dedicated resource group when authorized;
10. poll until deletion reaches the expected state;
11. perform a subscription-wide residual-resource search;
12. complete the immediate cost review;
13. schedule or confirm the 24-hour and 72-hour reviews;
14. sanitize only approved public evidence; and
15. update project status without overstating completion.

A later step never blocks an earlier safety action.

## 10. Emergency teardown sequence

Emergency teardown follows this order:

1. record the trigger time privately when safe;
2. stop the current action;
3. remove or restrict TCP/3389 exposure immediately;
4. stop controlled authentication testing;
5. deallocate the VM when appropriate;
6. protect or invalidate affected credentials through an approved process;
7. preserve only immediately available private evidence;
8. inventory the minimum resources required for safe cleanup;
9. delete the dedicated resource group when context and scope are confirmed;
10. preserve any deletion failure;
11. search for residual resources;
12. begin immediate cost review;
13. retain 24-hour and 72-hour reviews; and
14. prohibit resumption without a new go/no-go decision.

If tenant, subscription, or resource-group scope cannot be confirmed, do not issue destructive commands. Remove exposure and stop compute only through the already verified resource context, then escalate.

## 11. Remove TCP/3389 exposure

### 11.1 Inventory the exact rule

    az network nsg rule show \
      --resource-group "$RG_NAME" \
      --nsg-name "$NSG_NAME" \
      --name "$RDP_RULE_NAME" \
      --output json

Confirm that the returned rule is the exact approved lab rule.

### 11.2 Delete the exact rule

    az network nsg rule delete \
      --resource-group "$RG_NAME" \
      --nsg-name "$NSG_NAME" \
      --name "$RDP_RULE_NAME\"

### 11.3 Verify removal

    if az network nsg rule show \
      --resource-group "$RG_NAME" \
      --nsg-name "$NSG_NAME" \
      --name "$RDP_RULE_NAME" \
      --only-show-errors \
      >/dev/null 2>&1
    then
      printf 'FAIL: RDP rule still exists: %s\n' \
        "$RDP_RULE_NAME" >&2
      exit 1
    fi

Do not create a replacement exposure rule during teardown.

## 12. Stop and deallocate the VM

When the VM exists:

    if az vm show \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --only-show-errors \
      >/dev/null 2>&1
    then
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
    else
      printf 'NOTICE: VM is absent or cannot be read: %s\n' \
        "$VM_NAME\"
    fi

Do not treat deallocation as complete cleanup. Disks, NICs, public IPs, workspaces, and other resources can remain billable.

## 13. Pre-deletion inventory

Capture the minimum private inventory needed for verification:

    az resource list \
      --resource-group "$RG_NAME" \
      --query '[].{name:name,type:type,location:location,id:id}' \
      --output json

    az lock list \
      --resource-group "$RG_NAME" \
      --output json

    az role assignment list \
      --scope "$RG_SCOPE" \
      --include-inherited false \
      --all \
      --output json

    az identity list \
      --resource-group "$RG_NAME" \
      --output json

    az monitor data-collection rule list \
      --resource-group "$RG_NAME" \
      --output json

    az monitor log-analytics workspace show \
      --resource-group "$RG_NAME" \
      --workspace-name "$LAW_NAME" \
      --output json

Raw inventory remains private because it can contain identifiers, addresses, principal IDs, and resource IDs.

If unrelated resources appear, block resource-group deletion and resolve scope.

## 14. DCR association and rule disposition

### 14.1 Resolve the VM resource ID

Before deleting the VM or resource group:

    VM_RESOURCE_ID="$(
      az vm show \
        --resource-group "$RG_NAME" \
        --name "$VM_NAME" \
        --query id \
        --output tsv
    )"

    [[ -n "$VM_RESOURCE_ID" ]]

### 14.2 Inventory the association

    az monitor data-collection rule association show \
      --name "$DCR_ASSOC_NAME" \
      --resource "$VM_RESOURCE_ID" \
      --output json

### 14.3 Delete the exact association

When deletion is approved:

    az monitor data-collection rule association delete \
      --name "$DCR_ASSOC_NAME" \
      --resource "$VM_RESOURCE_ID" \
      --yes

### 14.4 Delete the DCR

When the DCR is dedicated to this lab and deletion is approved:

    az monitor data-collection rule delete \
      --resource-group "$RG_NAME" \
      --name "$DCR_NAME" \
      --delete-associations true \
      --yes

Do not delete a shared DCR. A shared or uncertain DCR blocks this command and requires separate disposition.

## 15. Analytics-rule disposition

Record one outcome for each lab analytics rule:

- not created;
- disabled and retained temporarily;
- deleted;
- retained under a separately approved scope; or
- blocked or inconclusive.

The current Azure CLI alert-rule operations are extension-backed and experimental. Do not permit first-use automatic extension installation or run an unreviewed experimental deletion command during teardown.

Commit 4 must define the exact rule identifier, lifecycle, deletion or disablement procedure, validation method, and evidence record before an analytics rule is created.

If a rule exists but the approved cleanup method is unavailable:

1. remove public exposure;
2. stop compute;
3. preserve the rule state privately;
4. keep project closure open;
5. assign an owner and deadline; and
6. complete cleanup through an approved portal, API, or reviewed CLI method.

## 16. Temporary identities and role assignments

### 16.1 Inventory first

If a temporary user-assigned identity was created:

    if [[ -n "${TEMP_IDENTITY_NAME:-}" ]]
    then
      az identity show \
        --resource-group "$RG_NAME" \
        --name "$TEMP_IDENTITY_NAME" \
        --output json
    fi

If a temporary role-assignment resource ID was recorded:

    if [[ -n "${TEMP_ROLE_ASSIGNMENT_ID:-}" ]]
    then
      az role assignment list \
        --scope "$RG_SCOPE" \
        --include-inherited false \
        --all \
        --query "[?id=='$TEMP_ROLE_ASSIGNMENT_ID']" \
        --output json
    fi

### 16.2 Delete only exact approved objects

Delete an exact temporary role assignment only after the preceding list result is reviewed:

    if [[ -n "${TEMP_ROLE_ASSIGNMENT_ID:-}" ]]
    then
      az role assignment delete \
        --ids "$TEMP_ROLE_ASSIGNMENT_ID\"
    fi

Delete an exact temporary identity only after its associated resources and assignments are removed:

    if [[ -n "${TEMP_IDENTITY_NAME:-}" ]]
    then
      az identity delete \
        --resource-group "$RG_NAME" \
        --name "$TEMP_IDENTITY_NAME\"
    fi

Never use a broad role, assignee, or scope filter as a teardown shortcut.

## 17. Workspace and telemetry disposition

Record exactly one workspace disposition.

### 17.1 Delete with the dedicated resource group

This is the default core-v2 path when the workspace is dedicated to the lab and no retention need is approved.

The resource-group deletion removes the workspace resource. Workspace deletion can enter a recoverable soft-delete state, and the workspace name can remain reserved temporarily.

Do not describe resource-group deletion as permanent telemetry destruction.

### 17.2 Retain temporarily

Temporary retention requires:

- a documented purpose;
- an owner;
- an approved duration;
- a cost boundary;
- a deletion date;
- a review schedule; and
- a separately approved resource-scope plan.

Do not delete the resource group while the workspace remains inside it.

### 17.3 Permanent deletion through a separate decision

Permanent deletion is irreversible and requires a separate explicit approval:

    az monitor log-analytics workspace delete \
      --resource-group "$RG_NAME" \
      --workspace-name "$LAW_NAME" \
      --force true \
      --yes

Do not use `--force` as a routine cleanup shortcut. Record why recovery is not required and verify the command applies to the intended dedicated workspace.

## 18. Delete the dedicated resource group

### 18.1 Review deletion blockers

Before deletion:

    az lock list \
      --resource-group "$RG_NAME" \
      --output table

    az resource list \
      --resource-group "$RG_NAME" \
      --output table

Resource locks, backup data, deny assignments, policy constraints, or managed dependencies can block deletion.

Do not remove a lock or override a control unless it is confirmed as a lab-created object and a separate approval authorizes removal.

### 18.2 Initiate deletion

When scope and authorization are confirmed:

    az group delete \
      --name "$RG_NAME" \
      --yes \
      --no-wait

Force-deletion options are not used by default.

### 18.3 Poll for terminal state

    delete_complete="false"

    for ((attempt = 1; attempt <= DELETE_POLL_ATTEMPTS; attempt++))
    do
      group_exists="$(
        az group exists \
          --name "$RG_NAME\"
      )"

      if [[ "$group_exists" == "false" ]]
      then
        delete_complete="true"
        break
      fi

      printf 'Resource group still exists; poll %s of %s.\n' \
        "$attempt" "$DELETE_POLL_ATTEMPTS\"

      sleep "$DELETE_POLL_INTERVAL_SECONDS"
    done

    if [[ "$delete_complete" != "true" ]]
    then
      printf 'FAIL: Resource-group deletion did not reach the expected state.\n' >&2
      exit 1
    fi

A timeout is a failed or inconclusive teardown result. Do not claim cleanup.

## 19. Subscription-wide residual-resource review

A name-only search is insufficient by itself. Use exact project tags, expected prefixes, resource types, and the private deployment record.

### 19.1 Project-tag search

    az resource list \
      --query "[?tags.Project=='$PROJECT_TAG_VALUE'].{name:name,type:type,resourceGroup:resourceGroup,id:id}" \
      --output json

### 19.2 Approved-prefix search

    az resource list \
      --query "[?starts_with(name, '$PROJECT_NAME_PREFIX')].{name:name,type:type,resourceGroup:resourceGroup,id:id}" \
      --output json

### 19.3 High-risk residual categories

    az disk list \
      --query "[?starts_with(name, '$PROJECT_NAME_PREFIX')].{name:name,resourceGroup:resourceGroup,diskState:diskState,id:id}" \
      --output json

    az network public-ip list \
      --query "[?starts_with(name, '$PROJECT_NAME_PREFIX')].{name:name,resourceGroup:resourceGroup,ipAddress:ipAddress,id:id}" \
      --output json

    az network nic list \
      --query "[?starts_with(name, '$PROJECT_NAME_PREFIX')].{name:name,resourceGroup:resourceGroup,id:id}" \
      --output json

    az snapshot list \
      --query "[?starts_with(name, '$PROJECT_NAME_PREFIX')].{name:name,resourceGroup:resourceGroup,id:id}" \
      --output json

Any unexpected result keeps cleanup open. Review exact ownership before deleting a residual object.

## 20. Immediate closure review

Complete immediately after teardown activity:

- [ ] Public or restricted TCP/3389 exposure is removed.
- [ ] Controlled authentication activity is stopped.
- [ ] VM is deallocated, deleted, or confirmed absent.
- [ ] DCR association disposition is recorded.
- [ ] DCR disposition is recorded.
- [ ] Analytics-rule disposition is recorded.
- [ ] Temporary identity disposition is recorded.
- [ ] Temporary role-assignment disposition is recorded.
- [ ] Workspace and telemetry disposition is recorded.
- [ ] Resource-group deletion result is recorded.
- [ ] Deletion reached terminal state or failure is preserved.
- [ ] Subscription-wide residual search is completed.
- [ ] Unexpected resources are assigned for remediation.
- [ ] Preliminary cost and usage state is reviewed.
- [ ] Result is labeled `Preliminary`, not final.
- [ ] 24-hour and 72-hour reviews remain scheduled.

The immediate review does not prove settled cost or complete billing reconciliation.

## 21. Twenty-four-hour reconciliation

At approximately 24 hours:

- [ ] Recheck resource-group existence.
- [ ] Repeat project-tag and approved-prefix searches.
- [ ] Recheck disks, public IPs, NICs, snapshots, and workspaces.
- [ ] Confirm no lab VM is allocated.
- [ ] Confirm no unauthorized exposure rule exists.
- [ ] Review delayed Cost Management data at the approved scope.
- [ ] Record displayed cost period and data timestamp when available.
- [ ] Review Log Analytics ingestion and retention charges.
- [ ] Review budget-alert delivery.
- [ ] Record missing or delayed data.
- [ ] Remediate residual resources.
- [ ] Keep closure open when data is incomplete.
- [ ] Preserve the result as `Passed`, `Failed`, `Blocked`, or `Inconclusive`.

## 22. Seventy-two-hour reconciliation

At approximately 72 hours:

- [ ] Recheck resource-group and residual-resource state.
- [ ] Confirm final planned workspace disposition.
- [ ] Confirm identity and role-assignment disposition.
- [ ] Confirm DCR and analytics-object disposition.
- [ ] Review Cost Management at the same scope.
- [ ] Record actual displayed cost and covered period.
- [ ] Compare against the maximum approved cost.
- [ ] Resolve or document billing latency.
- [ ] Do not claim zero cost merely because no charge is displayed.
- [ ] Do not call the cost final while provider data remains incomplete.
- [ ] Schedule another review when reconciliation remains incomplete.
- [ ] Record the final planned closure decision and reviewer.

The 72-hour checkpoint is the final planned core-v2 review, not a guarantee that all billing systems have fully settled.

## 23. Failure handling and resume criteria

### 23.1 Failure categories

Record each incomplete action as:

- `Failed`;
- `Blocked`;
- `Inconclusive`; or
- `Not Applicable`.

Examples include:

- exposure rule cannot be removed;
- VM cannot be deallocated;
- DCR association cannot be deleted;
- workspace disposition is unresolved;
- resource-group deletion is blocked or times out;
- a lock or deny assignment is unexplained;
- residual resources remain;
- cost data is unavailable; or
- authorization scope becomes uncertain.

### 23.2 Resume criteria

Cleanup activity can resume only when:

- tenant and subscription context are revalidated;
- exact affected resources are known;
- authorization is renewed;
- required credentials are protected;
- the operator and teardown owner are available;
- the planned action has a bounded scope;
- evidence and cost records are current; and
- a new go/no-go decision is recorded.

Public exposure or testing does not resume merely because a cleanup failure was repaired.

## 24. Evidence and claim handling

### 24.1 Private teardown evidence

Private evidence may include:

- resource inventories;
- deletion command output;
- resource IDs;
- tenant and subscription identifiers;
- exact resource names;
- principal and role-assignment IDs;
- public IP addresses;
- workspace and DCR details;
- lock and policy details;
- timestamps;
- Cost Management records;
- failure output; and
- remediation notes.

Store raw evidence outside the repository.

### 24.2 Public evidence

Public evidence requires:

1. sanitization;
2. redaction review;
3. technical review;
4. claim-boundary review;
5. publication approval;
6. manifest registration; and
7. SHA-256 recording when required.

Safety actions must not be delayed to capture a screenshot.

### 24.3 Prohibited claims

Do not claim:

- teardown completed when deletion remains pending;
- cleanup verified before subscription-wide residual review;
- no remaining charges because the VM is deallocated;
- zero cost before relevant data appears and is reviewed;
- permanent workspace deletion after a recoverable deletion;
- identity cleanup before exact assignments are checked;
- detection-object cleanup without a recorded disposition;
- project completion before the 24-hour and 72-hour reviews; or
- successful v2 teardown when no v2 execution occurred.

## 25. Teardown record template

    Teardown record:
    Teardown identifier:
    Mode:
    Trigger:
    Trigger time UTC:
    Operator:
    Teardown owner:
    Branch:
    Commit:
    Tenant alias:
    Subscription alias:
    Resource-group alias:
    Authorization decision:
    Exposure state before:
    Exposure-removal result:
    VM state before:
    VM containment result:
    DCR-association disposition:
    DCR disposition:
    Analytics-rule disposition:
    Temporary identity disposition:
    Temporary role-assignment disposition:
    Workspace disposition:
    Resource-group deletion request:
    Resource-group terminal-state result:
    Residual-resource search result:
    Immediate cost result:
    24-hour result:
    72-hour result:
    Failure or exception:
    Remediation owner:
    Private evidence location:
    Evidence lifecycle:
    Claim-maturity effect:
    Additional review required:
    Reviewer:
    Closure decision:

Do not place tenant IDs, subscription IDs, resource IDs, public IP addresses, email addresses, credentials, secrets, or private evidence paths in a public copy.

## 26. Completion criteria

This runbook is operationally complete when it:

- separates normal and emergency teardown;
- prioritizes safety over evidence preservation;
- requires exact tenant, subscription, and resource-group validation;
- uses strict command preflight and explicit variables;
- removes exposure before optional evidence capture;
- verifies VM deallocation rather than assuming it;
- dispositions DCR associations and rules;
- dispositions analytics objects;
- dispositions temporary identities and role assignments;
- records workspace retention, recoverable deletion, or permanent deletion correctly;
- deletes the dedicated resource group only after scope confirmation;
- polls and verifies the deletion terminal state;
- performs a subscription-wide orphan search;
- maps mandatory stop conditions to immediate actions;
- requires immediate, 24-hour, and 72-hour cost checks;
- preserves failed, blocked, and inconclusive outcomes;
- prevents premature cleanup, zero-cost, and completion claims;
- integrates deployment, cost, triage, and evidence governance; and
- does not claim that v2 execution or teardown has occurred.

## 27. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Teardown and cost closure have not started.
- No v2 teardown evidence exists.
- No resource-group deletion has been attempted.
- No immediate, 24-hour, or 72-hour teardown review has occurred.
- No cleanup, zero-cost, or completed-project claim is authorized.
- This runbook is design-stage operational documentation only.
