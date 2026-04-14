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

### 8. Validation
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
