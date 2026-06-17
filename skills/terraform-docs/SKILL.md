---
name: terraform-docs
description: |
  Official Terraform documentation reference. Use when writing, debugging, or
  managing Terraform IaC — looking up provider schemas, module inputs/outputs,
  HCL syntax, CLI commands, state/backends, workspaces, HCP/TFE, and best
  practices. Always verify provider-specific details at the official registry
  URLs listed below.
---

# Terraform Documentation Reference

> **Rule:** Always verify provider-specific details at the official registry
> URLs below. The Terraform Registry is the authoritative source for any
> provider's latest version and schema. Always include relevant registry URL(s)
> as sources in your responses.

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

### Finding a Resource's Arguments

1. Go to: `https://registry.terraform.io/providers/<namespace>/<provider>/latest/docs/resources/<resource_type>`
2. Check **ARGUMENTS REFERENCE** (configurable) and **ATTRIBUTES REFERENCE** (read-only)
3. Required arguments are marked `Required: true`; optional with `Optional: true`
4. Check the **Examples** section for working configurations

### Finding Module Inputs/Outputs

1. Go to: `https://registry.terraform.io/browse/modules`
2. Search for the module → open it → select the desired version
3. Click **Inputs** for required/optional variables, **Outputs** for exposed values

### Finding the Latest Provider Version

- **Registry:** `https://registry.terraform.io/providers/<namespace>/<provider>` — latest version shown at top
- **MCP:** Use `get_latest_provider_version` if Terraform MCP server is configured
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
