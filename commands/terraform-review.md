Review the Terraform codebase against the tesa-blueprints Terraform standards.

## What to Check

### 1. Formatting & Structure
- [ ] Run `terraform fmt -check -recursive` — any unformatted files?
- [ ] File organization follows standard: main.tf, variables.tf, outputs.tf, locals.tf, data.tf, providers.tf, versions.tf, backend.tf
- [ ] Terraform version constraint in versions.tf
- [ ] Provider versions pinned with `~>` constraint

### 2. Naming Conventions
- [ ] Azure resources follow `{prefix}-{env}-{name}` pattern
- [ ] Storage Accounts and Container Registries have no hyphens
- [ ] Terraform resource names are snake_case and descriptive
- [ ] `this` only used when there's exactly one instance of a type

### 3. Variables
- [ ] Every variable has `type` and `description`
- [ ] `validation` blocks for constrained inputs (environment, naming patterns)
- [ ] Secrets marked with `sensitive = true`
- [ ] No default values for environment-specific variables

### 4. Tags
- [ ] ALL resources have mandatory tags: environment, project, owner, cost-center, managed-by
- [ ] Using `local.common_tags` pattern
- [ ] Tags assigned to every resource

### 5. State & Security
- [ ] Remote backend configured (Azure Storage)
- [ ] No .tfstate files in the repo
- [ ] `.terraform.lock.hcl` committed
- [ ] No hardcoded credentials or connection strings
- [ ] `terraform.tfvars` not committed (`.tfvars.example` instead)

### 6. Modules
- [ ] Module versions pinned to Git tags (not `ref=main`)
- [ ] Each module has main.tf, variables.tf, outputs.tf, versions.tf, README.md
- [ ] All module outputs have descriptions
- [ ] Max 2 levels of module nesting

### 7. Azure Best Practices
- [ ] Managed Identities preferred over Service Principals
- [ ] Private Endpoints for PaaS services
- [ ] Diagnostic Settings enabled for ALL resources
- [ ] `prevent_deletion_if_contains_resources = true` on Resource Groups
- [ ] `purge_soft_delete_on_destroy = false` on Key Vaults

### 7b. Resource Locks
- [ ] Production resource groups have `CanNotDelete` lock
- [ ] Databases, Key Vaults, Storage Accounts in prod are locked
- [ ] Hub networking (VNet, DNS) has `ReadOnly` lock in prod

### 7c. RBAC
- [ ] No `Owner` role at subscription level for apps or pipelines
- [ ] Roles assigned at resource group scope (not subscription)
- [ ] Applications use specific data-plane roles (`Key Vault Secrets User`, `Storage Blob Data Reader`), not `Contributor`
- [ ] Separate identities per environment (no shared credentials)

### 7d. Resource Hardening
- [ ] Storage Accounts: TLS 1.2, no public blob access, network deny by default, no shared access keys
- [ ] Key Vaults: purge protection enabled, RBAC auth, network deny by default
- [ ] SQL: TLS 1.2, Azure AD auth (no SQL passwords), TDE, audit logging in prod
- [ ] App Services: HTTPS only, TLS 1.2, FTP disabled, Managed Identity
- [ ] NSGs: Deny all inbound by default

### 8. Environment-Specific Checks
- Identify the target environment (dev/staging/prod)
- If **production**:
  - [ ] Private Endpoints for all PaaS services
  - [ ] Network Security Groups on all subnets
  - [ ] WAF / Firewall configured
  - [ ] Geo-redundancy / DR configured
  - [ ] Log retention set to 90 days minimum
  - [ ] Alerting configured
  - [ ] Key Vault for all secrets (no env vars with credentials)
  - [ ] Managed Identities (no Service Principal secrets)
- If **dev**:
  - [ ] Auto-shutdown configured for VMs
  - [ ] Smaller SKUs used (cost optimization)
  - [ ] No real user data in dev resources

### 9. Validation
- [ ] Run `terraform validate` — any errors?
- [ ] Run `tflint --recursive` — any warnings/errors?
- [ ] Run `terraform plan` — any unexpected changes?

## Output Format

### Terraform Review Report

#### Passed
- ...

#### Issues Found
For each issue:
- **File:Line** — Description
- **Severity:** Critical / Warning / Suggestion
- **Fix:** What should be changed

#### Missing Diagnostic Settings
List resources without Diagnostic Settings configured.

#### Missing Tags
List resources without mandatory tags.
