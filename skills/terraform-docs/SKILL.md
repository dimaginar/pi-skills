---
name: terraform-docs
description: |
  Official Terraform documentation reference. Use when writing, debugging, or
  managing Terraform IaC — looking up provider schemas, module inputs/outputs,
  HCL syntax, CLI commands, state/backends, workspaces, HCP/TFE, and best
  practices. Always verify provider-specific details (schemas) using GitHub raw
  source; use registry URLs as human-facing references.
---

# Terraform Documentation Reference

> **Rule:** Always verify provider-specific details (schemas, arguments,
> attribute names) using the **GitHub raw source** method below — registry pages
> are JS-rendered SPAs that cannot be fetched with curl. The GitHub repo is the
> authoritative implementation source. Include registry URLs as human-facing references in responses.

---

## HCL / Configuration Language

| Topic | URL |
|---|---|
| Language overview | https://developer.hashicorp.com/terraform/language |
| Syntax (blocks, attributes, expressions) | https://developer.hashicorp.com/terraform/language/syntax |
| Variables | https://developer.hashicorp.com/terraform/language/variables |
| Outputs | https://developer.hashicorp.com/terraform/language/outputs |
| Locals | https://developer.hashicorp.com/terraform/language/expressions/locals |
| Data sources | https://developer.hashicorp.com/terraform/language/data-sources |
| Modules | https://developer.hashicorp.com/terraform/language/modules |
| Resources | https://developer.hashicorp.com/terraform/language/resources |
| Providers | https://developer.hashicorp.com/terraform/language/providers |
| Functions | https://developer.hashicorp.com/terraform/language/functions |
| Style guide | https://developer.hashicorp.com/terraform/language/style |

## Providers

| Topic | URL |
|---|---|
| Provider requirements | https://developer.hashicorp.com/terraform/language/providers/requirements |
| Provider configuration | https://developer.hashicorp.com/terraform/language/providers/configuration |
| AWS | https://registry.terraform.io/providers/hashicorp/aws/latest/docs |
| Azure | https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs |
| GCP | https://registry.terraform.io/providers/hashicorp/google/latest/docs |
> **Note:** Provider resource/data-source pages on `registry.terraform.io` are JS-rendered SPAs and cannot be fetched with curl/grep. See lookup methods below.

## CLI Commands

| Topic | URL |
|---|---|
| CLI overview | https://developer.hashicorp.com/terraform/cli |
| `init` | https://developer.hashicorp.com/terraform/cli/commands/init |
| `plan` | https://developer.hashicorp.com/terraform/cli/commands/plan |
| `apply` | https://developer.hashicorp.com/terraform/cli/commands/apply |
| `destroy` | https://developer.hashicorp.com/terraform/cli/commands/destroy |
| `validate` | https://developer.hashicorp.com/terraform/cli/commands/validate |
| `fmt` | https://developer.hashicorp.com/terraform/cli/commands/fmt |
| `state` | https://developer.hashicorp.com/terraform/cli/commands/state |
| `workspace` | https://developer.hashicorp.com/terraform/cli/commands/workspace |
| `import` | https://developer.hashicorp.com/terraform/cli/commands/import |

## State & Backends

| Topic | URL |
|---|---|
| State overview | https://developer.hashicorp.com/terraform/language/state |
| State file format | https://developer.hashicorp.com/terraform/language/state/file-format |
| Backends | https://developer.hashicorp.com/terraform/language/settings/backends |
| Workspaces | https://developer.hashicorp.com/terraform/language/state/workspaces |
| State best practices | https://developer.hashicorp.com/terraform/cloud-docs/workspaces/state |

## HCP Terraform / Terraform Enterprise

| Topic | URL |
|---|---|
| HCP Terraform docs | https://developer.hashicorp.com/terraform/cloud-docs |
| Workspaces | https://developer.hashicorp.com/terraform/cloud-docs/workspaces |
| Runs | https://developer.hashicorp.com/terraform/cloud-docs/workspaces/plans-and-applies |
| Variable sets | https://developer.hashicorp.com/terraform/cloud-docs/workspaces/variables/variable-sets |
| Policy checks | https://developer.hashicorp.com/terraform/cloud-docs/workspaces/settings/policy-checks |

## Best Practices

| Topic | URL |
|---|---|
| Module development guide | https://developer.hashicorp.com/terraform/tutorials/modules/module-develop |
| Validated Patterns | https://developer.hashicorp.com/terraform/validated-patterns |
| Well-Architected Framework | https://developer.hashicorp.com/terraform/cloud-docs/well-architected |

## Terraform Registry

| Topic | URL |
|---|---|
| Browse providers | https://registry.terraform.io/browse/providers |
| Browse modules | https://registry.terraform.io/browse/modules |
| Search pattern | `https://registry.terraform.io/browse?provider=<name>&module=<name>` |

---

## How to Look Things Up

### Finding a Resource's Arguments / Schema (Provider Lookup)

> **Important:** Provider docs pages on `registry.terraform.io` are JavaScript-rendered SPAs and cannot be parsed with curl/grep. Use one of these methods:

**Method A — GitHub raw source (most reliable):**
```
curl -s "https://raw.githubusercontent.com/hashicorp/terraform-provider-<provider>/main/internal/services/<service>/<resource>_resource.go" | grep -A 10 '"<attribute_name>"'
```
Example: `curl -sL https://raw.githubusercontent.com/hashicorp/terraform-provider-azurerm/main/internal/services/storage/storage_account_resource.go | grep -B2 -A15 '"account_replication_type"'`

Pros: Always accessible, gives exact schema including validation constraints. Cons: Reads source code (not end-user docs).

**Method B — HashiCorp Registry API:**
```
curl -s "https://registry.terraform.io/v2/providers/<provider_id>"
```
Note: Returns provider metadata, not full resource schemas.

**Method C — Human-facing references (not for programmatic use):**
- `https://registry.terraform.io/providers/hashicorp/<provider>/latest/docs/resources/<resource>`
- `https://developer.hashicorp.com/terraform/tutorials` (tutorials and guides)
- `https://learn.microsoft.com/azure/developer/terraform/` (Azure-specific examples)

### Finding Module Inputs/Outputs

1. Go to: `https://registry.terraform.io/browse/modules`
2. Search for the module → open it → select the desired version
3. Click **Inputs** for required/optional variables, **Outputs** for exposed values
4. Alternatively, check the module's GitHub `variables.tf` / `outputs.tf` files directly

### Finding the Latest Provider Version

- **GitHub releases:** `https://github.com/hashicorp/terraform-provider-<provider>/releases`
- **Registry:** `https://registry.terraform.io/providers/<namespace>/<provider>`
- **CLI:** `terraform providers` shows installed versions

---

## Quick Reference

### Common Debugging

| Symptom | First Step |
|---|---|
| Provider version conflict | `terraform init -verify-plugins=false` |
| Resource type not found | Check provider docs for exact resource type name |
| State not found | `terraform state list` then `terraform plan` |
| Module input not declared | Check module's Inputs tab on the registry |
| Invalid expression | `terraform validate` |
| Cycle detected | `terraform plan` — error shows the cycle |

### S3 Backend Template

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```
