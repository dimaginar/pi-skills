A Pi coding agent skill that curates official Terraform documentation URLs so you never hallucinate resource schemas, argument names, or outdated provider APIs.

## Why this exists
Terraform providers evolve constantly — arguments get deprecated, resource types shift between providers, and registry versions drift. Writing IaC from memory or blog posts leads to failed applies, broken state, and hours debugging wrong attribute names.

This skill adds a verified reference index that points you to the authoritative Terraform Registry and HashiCorp docs for every provider, CLI command, and language construct.

## Requirements
- Pi coding agent with web search or browser access (to follow linked docs)
- Terraform Registry access (public, no auth needed)

## Installation
Copy the full skill directory (terraform-docs/) into your (project's) skills folder.

## Usage
Tell Pi to look up Terraform docs:

look up azurerm_storage_account schema
find terraform backend configuration
debug terraform state not found

Pi will respond with authoritative registry URLs and the relevant schema details for the requested topic.

## What it covers
- HCL language: syntax, variables, outputs, locals, data sources, modules, resources, providers, functions, style guide
- Providers: AWS, Azure (azurerm), GCP registry docs and provider configuration
- CLI commands: init, plan, apply, destroy, validate, fmt, state, workspace, import
- State & backends: state file format, backends, workspaces, state best practices
- HCP Terraform / TFE: workspaces, runs, variable sets, policy checks
- Best practices: module development, validated patterns, well-architected framework
- Terraform Registry: provider and module browsing, search patterns

## Working with MCP `learn`
This skill is designed to be used alongside the MCP `learn` tool (Microsoft Learn docs). When writing any Terraform construct — resources, data sources, modules, variables, backends — follow this pattern:

1. Look up the resource schema and argument names via **this skill** (Terraform Registry)
2. Verify Azure-specific details via the **MCP `learn` tool**: API versions, permissions, region limits, GA/beta status
3. Never rely on one source alone or guess from memory or blog posts

The terraform-docs skill gives you the HCL surface; MCP `learn` validates the Azure backend reality. Together they prevent valid-looking Terraform that targets deprecated or unsupported Azure features.
