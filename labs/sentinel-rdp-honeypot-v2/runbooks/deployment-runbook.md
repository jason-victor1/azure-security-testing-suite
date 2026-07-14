# Sentinel RDP Honeypot v2 Deployment Runbook

## 1. Purpose

This runbook defines the bounded, authorization-gated deployment workflow for the Azure Sentinel RDP Honeypot v2 lab.

It is designed to:

- confirm Azure scope before side effects;
- create isolated disposable infrastructure;
- establish monitoring before any exposure-dependent activity;
- validate Windows authentication telemetry safely;
- separate controlled testing from organic observation;
- map stop conditions to immediate response actions;
- preserve evidence without delaying containment;
- enforce cost and teardown ownership; and
- prevent unsupported implementation or portfolio claims.

This runbook is an operational design artifact. It does not authorize Azure execution by itself.

## 2. Current status and authorization boundary

At this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing is blocked.
- Detection and incident validation are blocked.
- No v2 execution evidence exists.
- No concrete public evidence record has been approved.

No operator may begin Azure work merely because this file exists or static validation passes.

## 3. Governing documents

Use this runbook with:

1. [`../docs/threat-model.md`](../docs/threat-model.md);
2. [`../docs/scope-and-credibility-notes.md`](../docs/scope-and-credibility-notes.md);
3. [`../docs/architecture-overview.md`](../docs/architecture-overview.md);
4. [`../docs/checklist-to-project-roadmap.md`](../docs/checklist-to-project-roadmap.md);
5. [`cost-control-checklist.md`](cost-control-checklist.md);
6. [`teardown-runbook.md`](teardown-runbook.md);
7. [`rdp-authentication-triage.md`](rdp-authentication-triage.md);
8. [`../kql/hunting-queries.md`](../kql/hunting-queries.md);
9. [`../sentinel/analytics-rules-catalog.md`](../sentinel/analytics-rules-catalog.md);
10. [`../evidence/README.md`](../evidence/README.md);
11. [`../evidence/redaction-notes.md`](../evidence/redaction-notes.md); and
12. [`../evidence/templates/evidence-record-template.md`](../evidence/templates/evidence-record-template.md).

When instructions conflict, authorization, safety, isolation, cost containment, teardown readiness, and evidence governance take precedence.

## 4. Operating principles

The deployment workflow follows these principles:

1. **Authorization precedes side effects.**
2. **Telemetry validation precedes public exposure.**
3. **Normal and emergency modes are distinct.**
4. **Safety has priority over evidence preservation.**
5. **The environment remains isolated from trusted or production networks.**
6. **Credentials are strong, unique, private, and never committed.**
7. **Every phase has explicit entry and exit criteria.**
8. **Every mandatory stop condition maps to an immediate action.**
9. **Failed, blocked, and inconclusive results remain traceable.**
10. **A created resource does not prove successful validation.**
11. **A successful validation does not prove completion of later phases.**
12. **Public claims cannot exceed the evidence actually approved.**

## 5. Operating modes

### 5.1 Normal mode

Normal mode applies only when:

- the relevant go/no-go decision is recorded as `GO`;
- the current branch and commit are known;
- the intended tenant and subscription are confirmed;
- cost controls are active;
- teardown ownership is assigned;
- required variables are defined;
- the private evidence location is ready;
- no mandatory stop condition is active; and
- the next phase has been explicitly authorized.

Normal mode permits only the actions defined for the currently authorized phase.

### 5.2 Emergency mode

Emergency mode begins immediately when a mandatory stop condition occurs.

Emergency priorities are:

1. stop the current action;
2. remove or restrict TCP/3389 exposure when present;
3. stop controlled authentication testing;
4. deallocate the VM when appropriate;
5. begin emergency teardown when required;
6. preserve only evidence that can be captured without delaying containment;
7. record the trigger and actions privately; and
8. reassess credentials, cost, subscription context, and residual resources.

Emergency mode never waits for screenshots or documentation completeness.

## 6. Required variables and command preflight

### 6.1 Required nonsecret variables

Define explicit values before running any Azure CLI command:

    export AZ_TENANT_ID="<approved-tenant-guid>"
    export AZ_SUBSCRIPTION_ID="<approved-subscription-guid>"
    export RG_NAME="<approved-resource-group-name>"
    export LOCATION="<approved-azure-region>"
    export VNET_NAME="<approved-vnet-name>"
    export SUBNET_NAME="<approved-subnet-name>"
    export NSG_NAME="<approved-nsg-name>"
    export PUBLIC_IP_NAME="<approved-public-ip-name>"
    export NIC_NAME="<approved-nic-name>"
    export VM_NAME="<approved-vm-name>"
    export VM_SIZE="<approved-vm-size>"
    export ADMIN_USERNAME="<approved-lab-admin-name>"
    export LAW_NAME="<approved-log-analytics-workspace-name>"
    export DCR_NAME="<approved-data-collection-rule-name>"
    export DCR_ASSOC_NAME="<approved-dcr-association-name>"
    export CONTROLLED_TEST_SOURCE_CIDR="<approved-single-source-cidr>"
    export OBSERVATION_END_UTC="<approved-utc-stop-time>"
    export PRIVATE_EVIDENCE_DIR="<approved-nonrepository-path>"

Do not place passwords, tokens, private keys, or other secrets in this file, Git, shell history, screenshots, or public notes.

### 6.2 Shell preflight

Run shell strict mode and validate commands and variables before side effects:

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

      installed="$(az version --query '\"azure-cli\"' --output tsv)"

      if ! python3 -c \
        'import sys; parse=lambda value: tuple(int(part) for part in value.split(".")[:3]); raise SystemExit(0 if parse(sys.argv[1]) >= parse(sys.argv[2]) else 1)' \
        "$installed" \
        "$minimum"
      then
        printf 'Azure CLI %s or newer is required; found %s\n' \
          "$minimum" \
          "$installed" >&2
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

    require_azure_cli_version "2.61.0"
    require_az_extension "monitor-control-service"

    for name in \
      AZ_TENANT_ID \
      AZ_SUBSCRIPTION_ID \
      RG_NAME \
      LOCATION \
      VNET_NAME \
      SUBNET_NAME \
      NSG_NAME \
      PUBLIC_IP_NAME \
      NIC_NAME \
      VM_NAME \
      VM_SIZE \
      ADMIN_USERNAME \
      LAW_NAME \
      DCR_NAME \
      DCR_ASSOC_NAME \
      CONTROLLED_TEST_SOURCE_CIDR \
      OBSERVATION_END_UTC \
      PRIVATE_EVIDENCE_DIR
    do
      require_var "$name"
    done

### 6.3 Azure CLI extension preparation

The `az monitor data-collection` command group is extension-backed.

Do not permit first-use automatic extension installation during deployment. Review the extension name and version, then install or update it as a separate, explicitly authorized local-tooling action before the deployment window.

The deployment preflight verifies that the reviewed `monitor-control-service` extension is already present. It does not install or update software.

### 6.4 Secret handling

The VM administrator password must be:

- generated or entered privately at deployment time;
- strong and unique to this disposable lab;
- excluded from shell history;
- excluded from screenshots and evidence;
- excluded from Git and issue comments; and
- rotated or invalidated if exposure is suspected.

A secure interactive prompt can be used after authorization:

    read -r -s -p "Enter the strong one-time lab password: " VM_ADMIN_PASSWORD
    printf '\n'

Do not export the password globally. Unset it immediately after the VM creation command completes:

    unset VM_ADMIN_PASSWORD

## 7. Predeployment authorization gate

Azure side effects remain blocked until every mandatory item passes.

### 7.1 Repository state

- [ ] Correct branch is checked out.
- [ ] Intended commit is recorded.
- [ ] Working tree contains only intended changes.
- [ ] No private source files are tracked.
- [ ] Mandatory core artifacts are nonempty.
- [ ] Relative links resolve.
- [ ] Secret scans pass.
- [ ] Evidence and redaction controls exist.
- [ ] Public claims remain design-stage only.

### 7.2 Identity and scope

- [ ] Tenant is confirmed.
- [ ] Subscription is confirmed.
- [ ] Operator authorization is recorded.
- [ ] Resource-group name is approved.
- [ ] Region is approved.
- [ ] No production resource is in scope.
- [ ] No trusted-network connection is planned.
- [ ] No VNet peering, VPN, or private trusted path is planned.
- [ ] No unnecessary managed identity is planned.

### 7.3 Cost and teardown

- [ ] Budget or alert is configured.
- [ ] Log Analytics daily-cap plan is defined.
- [ ] VM size is approved.
- [ ] Runtime boundary is approved.
- [ ] Immediate teardown is executable.
- [ ] Emergency teardown is executable.
- [ ] Immediate, 24-hour, and 72-hour cost owners are assigned.

### 7.4 Evidence readiness

- [ ] Private evidence location exists outside the repository.
- [ ] Evidence-record template is ready.
- [ ] Redaction workflow is understood.
- [ ] Raw logs and screenshots will remain private.
- [ ] No concrete public `EV-###` record will be created without approval.

### 7.5 Go/no-go record

Record privately:

- decision timestamp in UTC;
- operator;
- branch and commit;
- tenant and subscription aliases;
- intended region;
- resource-group alias;
- authorized phase;
- runtime boundary;
- cost owner;
- teardown owner;
- unresolved risks; and
- decision: `GO` or `NO-GO`.

A `GO` applies only to the named phase. It does not authorize later phases or public exposure.

## 8. Phase A — Azure context and project scope

### 8.1 Entry criteria

- Predeployment authorization gate is complete.
- Phase A has an explicit `GO`.
- Required variables are present.
- No stop condition is active.

### 8.2 Actions

1. Authenticate through the approved Azure method.
2. Confirm the current tenant and subscription.
3. Set the approved subscription explicitly.
4. Confirm the approved region and naming plan.
5. Confirm budget, quota, and daily-cap ownership.
6. Create or confirm the dedicated resource group.

### 8.3 Validation commands

    az account show \
      --query '{tenantId:tenantId,subscriptionId:id,subscriptionName:name}' \
      --output json

    az account set \
      --subscription "$AZ_SUBSCRIPTION_ID"

    az account show \
      --query '{tenantId:tenantId,subscriptionId:id}' \
      --output tsv

    az group show \
      --name "$RG_NAME" \
      --query '{name:name,location:location,provisioningState:properties.provisioningState}' \
      --output table

If the resource group is not present and creation is authorized:

    az group create \
      --name "$RG_NAME" \
      --location "$LOCATION" \
      --tags \
        Project="Sentinel-RDP-Honeypot-v2" \
        Environment="Disposable-Lab" \
        Owner="<private-operator-alias>" \
      --output json

### 8.4 Exit criteria

- tenant and subscription match the approved record;
- resource group is dedicated to the lab;
- no unrelated resources are present;
- cost and teardown ownership remain valid;
- evidence record exists privately; and
- no stop condition is active.

## 9. Phase B — Isolated infrastructure

### 9.1 Entry criteria

- Phase A exit criteria passed.
- Phase B has an explicit `GO`.
- Network design matches the approved architecture.
- Public RDP is not yet enabled.

### 9.2 Actions

1. Create the VNet and subnet.
2. Create the NSG with no broad public RDP rule.
3. Create the public IP and NIC only where required.
4. Create the disposable Windows VM.
5. Confirm Windows Firewall remains enabled.
6. Confirm no peering, VPN, trusted path, or production data exists.
7. Confirm no unnecessary managed identity exists.

### 9.3 Network commands

    az network vnet create \
      --resource-group "$RG_NAME" \
      --location "$LOCATION" \
      --name "$VNET_NAME" \
      --subnet-name "$SUBNET_NAME" \
      --output json

    az network nsg create \
      --resource-group "$RG_NAME" \
      --location "$LOCATION" \
      --name "$NSG_NAME" \
      --output json

    az network public-ip create \
      --resource-group "$RG_NAME" \
      --location "$LOCATION" \
      --name "$PUBLIC_IP_NAME" \
      --sku Standard \
      --allocation-method Static \
      --output json

    SUBNET_ID="$(
      az network vnet subnet show \
        --resource-group "$RG_NAME" \
        --vnet-name "$VNET_NAME" \
        --name "$SUBNET_NAME" \
        --query id \
        --output tsv
    )"

    PUBLIC_IP_ID="$(
      az network public-ip show \
        --resource-group "$RG_NAME" \
        --name "$PUBLIC_IP_NAME" \
        --query id \
        --output tsv
    )"

    NSG_ID="$(
      az network nsg show \
        --resource-group "$RG_NAME" \
        --name "$NSG_NAME" \
        --query id \
        --output tsv
    )"

    az network nic create \
      --resource-group "$RG_NAME" \
      --location "$LOCATION" \
      --name "$NIC_NAME" \
      --subnet "$SUBNET_ID" \
      --public-ip-address "$PUBLIC_IP_ID" \
      --network-security-group "$NSG_ID" \
      --output json

### 9.4 VM creation

Prompt for the password privately immediately before creation:

    read -r -s -p "Enter the strong one-time lab password: " VM_ADMIN_PASSWORD
    printf '\n'

    az vm create \
      --resource-group "$RG_NAME" \
      --location "$LOCATION" \
      --name "$VM_NAME" \
      --nics "$NIC_NAME" \
      --image Win2022Datacenter \
      --size "$VM_SIZE" \
      --admin-username "$ADMIN_USERNAME" \
      --admin-password "$VM_ADMIN_PASSWORD" \
      --output json

    unset VM_ADMIN_PASSWORD

This runbook intentionally does not include a reusable command that opens TCP/3389 to all internet sources.

### 9.5 Validation commands

    az network vnet show \
      --resource-group "$RG_NAME" \
      --name "$VNET_NAME" \
      --output json

    az network nsg rule list \
      --resource-group "$RG_NAME" \
      --nsg-name "$NSG_NAME" \
      --output table

    az vm show \
      --resource-group "$RG_NAME" \
      --name "$VM_NAME" \
      --show-details \
      --output json

### 9.6 Exit criteria

- VM exists in the dedicated resource group;
- NSG contains no unauthorized inbound RDP rule;
- Windows Firewall state is documented;
- no sensitive data exists;
- no trusted path exists;
- no unnecessary identity exists;
- architecture matches the approved design; and
- no stop condition is active.

## 10. Phase C — Monitoring foundation

### 10.1 Entry criteria

- Phase B exit criteria passed.
- Phase C has an explicit `GO`.
- Public TCP/3389 remains unauthorized.
- Cost controls remain active.

### 10.2 Actions

1. Create or confirm the Log Analytics workspace.
2. Apply the approved daily cap and retention settings.
3. Enable Microsoft Sentinel on the intended workspace.
4. Install or confirm Azure Monitor Agent.
5. Create or confirm the Data Collection Rule.
6. Associate the DCR with the VM.
7. Confirm the actual destination table.
8. Validate agent and collection health.

### 10.3 Workspace validation

    az monitor log-analytics workspace show \
      --resource-group "$RG_NAME" \
      --workspace-name "$LAW_NAME" \
      --query '{name:name,location:location,customerId:customerId,retentionInDays:retentionInDays}' \
      --output json

### 10.4 VM extension validation

    az vm extension list \
      --resource-group "$RG_NAME" \
      --vm-name "$VM_NAME" \
      --query '[].{name:name,publisher:publisher,type:type,provisioningState:provisioningState}' \
      --output table

### 10.5 DCR and association validation

    az monitor data-collection rule show \
      --resource-group "$RG_NAME" \
      --name "$DCR_NAME" \
      --output json

    az monitor data-collection rule association list \
      --resource "$(
        az vm show \
          --resource-group "$RG_NAME" \
          --name "$VM_NAME" \
          --query id \
          --output tsv
      )" \
      --output json

### 10.6 Exit criteria

- workspace and Sentinel association are confirmed;
- AMA state is known;
- DCR state is known;
- VM association is confirmed;
- expected telemetry is arriving;
- health and freshness are acceptable;
- the actual destination table is known; and
- no stop condition is active.

If telemetry is absent, stale, or uninterpretable, stop. Do not proceed to exposure-dependent activity.

## 11. Phase D — Restricted schema validation

### 11.1 Entry criteria

- Phase C exit criteria passed.
- Phase D has an explicit `GO`.
- Controlled-test source CIDR is a single approved source.
- Public organic exposure remains unauthorized.
- Triage and teardown runbooks are ready.

### 11.2 Restricted access rule

A restricted rule may be created only for the approved controlled-test source:

    case "$CONTROLLED_TEST_SOURCE_CIDR" in
      ""|"*"|"0.0.0.0/0"|"::/0")
        printf 'Rejected unsafe controlled-test source: %s\n' \
          "${CONTROLLED_TEST_SOURCE_CIDR:-missing}" >&2
        exit 1
        ;;
    esac

    if [[ "$CONTROLLED_TEST_SOURCE_CIDR" != */32 &&
          "$CONTROLLED_TEST_SOURCE_CIDR" != */128 ]]
    then
      printf 'Controlled-test source must be a single-host /32 or /128 CIDR.\n' >&2
      exit 1
    fi

    az network nsg rule create \
      --resource-group "$RG_NAME" \
      --nsg-name "$NSG_NAME" \
      --name "Allow-RDP-Controlled-Test" \
      --priority 300 \
      --direction Inbound \
      --access Allow \
      --protocol Tcp \
      --source-address-prefixes "$CONTROLLED_TEST_SOURCE_CIDR" \
      --source-port-ranges '*' \
      --destination-address-prefixes '*' \
      --destination-port-ranges 3389 \
      --description "Temporary controlled-test access; remove after validation" \
      --output json

Reject the action when the source is empty, wildcarded, or broader than the approved controlled-test record.

### 11.3 Failed-authentication validation

1. Record the private test identifier and UTC start time.
2. Perform only the minimum authorized failed authentication attempt.
3. Do not intentionally trigger account lockout.
4. Query for Event ID `4625`.
5. Confirm actual field names, host, timestamp, source, account alias, and logon type where available.
6. Record ingestion delay.
7. Stop after the minimum validation requirement is satisfied.

### 11.4 Successful-authentication validation

A successful test requires separate explicit authorization.

1. Record the private test identifier and UTC start time.
2. Confirm telemetry health immediately before testing.
3. Use only the isolated lab account.
4. Terminate the session after the minimum validation.
5. Query for Event ID `4624`.
6. Confirm remote-interactive context using observed v2 fields.
7. Enter emergency mode if success occurs unexpectedly.

### 11.5 Remove restricted access

Remove the temporary rule after controlled validation:

    az network nsg rule delete \
      --resource-group "$RG_NAME" \
      --nsg-name "$NSG_NAME" \
      --name "Allow-RDP-Controlled-Test"

### 11.6 Exit criteria

- Event ID `4625` is validated or the failure is preserved;
- Event ID `4624` is validated only if separately authorized;
- actual schema and field names are recorded;
- ingestion delay is known;
- restricted RDP access is removed;
- no unexplained successful authentication exists; and
- test outcomes are recorded as `Passed`, `Failed`, `Blocked`, or `Inconclusive`.

## 12. Public-exposure gate

Public organic observation requires a separate explicit `GO` after deployment and telemetry validation.

All conditions must pass:

- [ ] Correct subscription and resource group are reconfirmed.
- [ ] VM is disposable.
- [ ] No sensitive data exists.
- [ ] No trusted network connection exists.
- [ ] No unnecessary managed identity exists.
- [ ] Credentials are unique and strong.
- [ ] Windows Firewall is enabled.
- [ ] NSG design is approved.
- [ ] AMA and DCR health are confirmed.
- [ ] Event ID `4625` schema is validated.
- [ ] Successful-logon check is ready.
- [ ] Triage runbook is ready.
- [ ] Emergency teardown is executable.
- [ ] Cost threshold and stop time are recorded.
- [ ] Private evidence location is ready.
- [ ] Operator is actively monitoring.
- [ ] Public-exposure decision is recorded as `GO`.

Isolation alone does not authorize exposure.

This runbook intentionally omits a generic command that opens TCP/3389 broadly. Any public-exposure change must be generated from the separately approved change record, reviewed immediately before execution, time-boxed, and removed at the end of the authorized window.

## 13. Phase E — Controlled detection validation

### 13.1 Entry criteria

- Phase D exit criteria passed.
- Detection-engineering artifacts are ready.
- Phase E has an explicit `GO`.
- No public organic exposure is required for the controlled tests.

### 13.2 Actions

1. Run schema and freshness queries first.
2. Run the authorized failed-authentication test.
3. Run the separately authorized successful-authentication test when approved.
4. Execute the documented KQL queries.
5. Validate analytics-rule logic only when its prerequisites exist.
6. Use [`rdp-authentication-triage.md`](rdp-authentication-triage.md) for disposition.
7. Preserve failed and inconclusive results.

### 13.3 Exit criteria

- query inputs and time ranges are recorded;
- results are reproducible;
- triage disposition is recorded;
- no unsupported detection claim is made; and
- no stop condition is active.

## 14. Phase F — Conditional organic observation

### 14.1 Entry criteria

- Public-exposure gate passed.
- Separate public-exposure `GO` is recorded.
- Operator is actively monitoring.
- Observation end time is current and explicit.
- Emergency actions are immediately executable.

### 14.2 Actions

1. Apply only the reviewed and approved NSG change.
2. Confirm the effective rule immediately.
3. Record exposure start time privately.
4. Monitor telemetry health continuously during the window.
5. Query for failed and successful remote-interactive authentication.
6. Use the authentication-triage runbook for disposition.
7. End exposure at the approved stop time or earlier.
8. Remove the exposure rule.
9. verify effective NSG state;
10. deallocate or teardown according to the approved plan.

### 14.3 Exit criteria

- exposure is removed;
- successful-authentication review is complete;
- telemetry outcome is recorded;
- stop conditions and responses are documented;
- cost and VM state are known; and
- no active unauthorized resource remains.

## 15. Evidence handling

### 15.1 Private evidence

Raw evidence remains outside the repository and may include:

- portal screenshots;
- raw query exports;
- source addresses;
- account and computer names;
- tenant and subscription identifiers;
- resource IDs;
- precise timestamps;
- command output; and
- incident details.

### 15.2 Public evidence

Public evidence requires:

1. sanitization;
2. redaction review;
3. technical review;
4. claim-boundary review;
5. publication approval;
6. manifest registration; and
7. SHA-256 recording when required.

Redaction is not approval. Sanitization is not validation.

### 15.3 Evidence capture rule

Evidence capture must not delay:

- exposure removal;
- VM deallocation;
- credential protection;
- emergency teardown; or
- cost containment.

A missing screenshot is preferable to extending an unsafe condition.

## 16. Mandatory stop conditions and immediate actions

| Stop condition | Immediate action |
|---|---|
| Tenant or subscription context is uncertain | Stop all Azure actions and revalidate context |
| Unexplained successful remote-interactive access | Remove TCP/3389 exposure, stop testing, deallocate the VM, begin emergency review |
| Suspicious persistence or process execution | Remove exposure and deallocate or delete |
| Unexpected privilege or account change | Remove exposure and review identity state |
| Windows Firewall is disabled or altered unexpectedly | Remove exposure and stop observation |
| NSG differs from approved design | Remove or correct the unauthorized rule |
| AMA or telemetry failure | Remove exposure and stop exposure-dependent activity |
| DCR association failure | Remove exposure and repair collection before resuming |
| Unexpected outbound activity | Remove exposure and deallocate |
| Credential exposure or suspected compromise | Stop testing, remove exposure, invalidate the credential through an approved process |
| Unknown or unauthorized Azure resource appears | Stop activity and begin containment or teardown |
| Cost threshold or runtime boundary is exceeded | Stop the lab and begin cost or teardown closure |
| Evidence capture would delay containment | Skip evidence capture and contain immediately |
| Operator cannot determine whether activity is safe | Stop and classify the result as `Blocked` or `Inconclusive` |

## 17. Emergency procedure

1. Record the trigger time privately when safe.
2. Stop the current action.
3. Remove or restrict TCP/3389 exposure.
4. Stop controlled authentication testing.
5. Deallocate the VM when appropriate.
6. Invoke the emergency path in [`teardown-runbook.md`](teardown-runbook.md).
7. Preserve only immediately available private evidence.
8. Check subscription, identity, NSG, VM, and telemetry state.
9. Invalidate exposed credentials through an approved process.
10. Perform immediate cost review.
11. Record failed, blocked, or inconclusive outcomes.
12. Do not resume without a new go/no-go decision.

## 18. Teardown and cost closure

Follow [`teardown-runbook.md`](teardown-runbook.md) and [`cost-control-checklist.md`](cost-control-checklist.md).

Required closure checkpoints:

### 18.1 Immediate

- remove public or restricted RDP exposure;
- deallocate or delete the VM as approved;
- delete the resource group when required;
- verify no unintended resources remain;
- record preliminary cost state;
- preserve teardown failures privately.

### 18.2 After 24 hours

- review delayed cost and usage data;
- verify no resource reappeared;
- confirm no unapproved workspace, IP, disk, NIC, or rule remains;
- record exceptions and remediation.

### 18.3 After 72 hours

- review settled cost data;
- confirm final residual-resource state;
- resolve or document billing lag;
- avoid claiming zero cost without settled data;
- record final closure decision.

## 19. Resume criteria

Activity can resume only when:

- the stop condition is resolved;
- affected credentials are protected;
- telemetry health is restored;
- architecture matches the approved design;
- cost and teardown ownership remain valid;
- the working tree and evidence record are current;
- a new go/no-go decision is recorded; and
- the resumed phase is explicitly authorized.

## 20. Deployment record template

    Deployment record:
    Operator:
    Branch:
    Commit:
    Decision timestamp UTC:
    Authorized phase:
    Tenant alias:
    Subscription alias:
    Resource-group alias:
    Region:
    Runtime boundary:
    Observation end UTC:
    Cost owner:
    Teardown owner:
    Private evidence location:
    Entry criteria result:
    Commands executed:
    Resources created or changed:
    Validation result:
    Stop condition triggered:
    Immediate response:
    Test outcome:
    Evidence lifecycle:
    Residual-resource state:
    Cost-check state:
    Exit criteria result:
    Resume authorization required:
    Reviewer:
    Closure decision:

Do not place passwords, tokens, tenant IDs, subscription IDs, resource IDs, public IP addresses, email addresses, or other sensitive values in a public copy.

## 21. Completion criteria

This deployment runbook is complete when it:

- separates normal and emergency modes;
- requires explicit phase authorization;
- validates tenant and subscription before side effects;
- uses strict shell preflight and explicit variables;
- creates isolated infrastructure without broad public RDP by default;
- establishes monitoring before exposure-dependent activity;
- validates telemetry before public exposure;
- separates failed and successful controlled tests;
- requires a separate public-exposure `GO`;
- maps every mandatory stop condition to an immediate response;
- makes safety more important than evidence preservation;
- integrates triage, teardown, cost, KQL, analytics, and evidence artifacts;
- includes immediate, 24-hour, and 72-hour closure checks;
- contains no weak-credential or unsafe firewall instruction;
- preserves failed, blocked, and inconclusive outcomes; and
- does not claim that v2 execution has occurred.

## 22. Current execution declaration

As of this documentation checkpoint:

- Azure execution is paused.
- Azure v2 deployment is blocked.
- Public TCP/3389 exposure is unauthorized.
- Controlled authentication testing has not started.
- Detection and incident validation have not started.
- No v2 execution evidence exists.
- No concrete public evidence record has been approved.
- This runbook is design-stage operational documentation only.
