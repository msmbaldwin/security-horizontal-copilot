---
description: Audit Azure service docsets for security compliance against "Secure your <Service>" articles. Use when: auditing docset security compliance, checking if procedural docs model security best practices, finding documentation gaps, identifying anti-patterns in quickstarts/tutorials/how-tos. Extracts policy rules from security articles and validates all docset content against them.
---

# Azure Docset Compliance Scanner

Scan an entire service docset to ensure procedural content (quickstarts, tutorials, how-tos, code samples) consistently models the security best practices defined in the security horizontal article.

## When to Use This Skill

Use this skill when asked to:
- "Scan the Key Vault docset for security compliance"
- "Check if quickstarts follow security best practices"
- "Find CLI commands missing security flags"
- "Are there any insecure patterns in the tutorials?"
- "Does the docset model the security article correctly?"

## The Problem This Solves

Security articles define **what** should be secured, but procedural content often:
- Shows `az keyvault create` without `--enable-rbac-authorization`
- Demonstrates access policies when RBAC is recommended
- Uses `ClientSecretCredential` instead of managed identities
- Shows public endpoints without mentioning private alternatives

This creates **anti-pattern propagation** — users copy-paste insecure examples into production.

## Core Concept: Security Article as Policy

The security article becomes a **machine-readable policy**. Every recommendation translates to audit rules:

| Security Recommendation | Audit Rule |
|------------------------|------------|
| "Use RBAC for access control" | `az keyvault create` must include `--enable-rbac-authorization` |
| "Enable purge protection" | Creation commands must enable purge protection |
| "Do not use access policies" | Flag any `az keyvault set-policy` command |
| "Use managed identities" | Code samples should use `DefaultAzureCredential` |
| "Use private endpoints" | Public endpoint demos should mention PE alternative |

## Prerequisites

**The user must have both:**
1. The service docset in the workspace
2. The security horizontal article (either in the docset or workspace)

---

## Phase 1: Policy Extraction

Parse the security article into structured rules. For each recommendation, extract:

### Rule Types

**1. Command Flag Rules** — Required parameters for CLI/PowerShell
```yaml
- id: KV-RBAC-001
  type: command_flag
  recommendation: "Use Azure RBAC for access control"
  commands:
    - pattern: "az keyvault create"
      required_flags: ["--enable-rbac-authorization"]
    - pattern: "New-AzKeyVault"
      required_flags: ["-EnableRbacAuthorization"]
  severity: high
```

**2. Anti-Pattern Rules** — Patterns that should never appear
```yaml
- id: KV-ANTI-001
  type: anti_pattern
  recommendation: "Do not use access policies"
  prohibited_patterns:
    - "az keyvault set-policy"
    - "Set-AzKeyVaultAccessPolicy"
    - "accessPolicies:"
  severity: critical
```

**3. Authentication Rules** — How code samples should authenticate
```yaml
- id: KV-AUTH-001
  type: authentication
  recommendation: "Use managed identities"
  preferred: ["DefaultAzureCredential", "ManagedIdentityCredential"]
  discouraged: ["ClientSecretCredential", "connection string"]
  severity: high
```

**4. Configuration Rules** — Required settings in ARM/Bicep/Portal
```yaml
- id: KV-DATA-001
  type: configuration
  recommendation: "Enable purge protection"
  required_properties:
    - "enablePurgeProtection: true"
  severity: critical
```

---

## Phase 2: Docset Scan

Scan every article in the docset to find auditable content:

### Content to Detect

| Content Type | How to Find |
|--------------|-------------|
| **Azure CLI** | Code blocks with `azurecli` or starting with `az ` |
| **PowerShell** | Code blocks with `powershell` or cmdlet patterns |
| **ARM Templates** | JSON with deployment template schema |
| **Bicep** | `.bicep` blocks or files |
| **SDK Code** | Python, .NET, Java, JavaScript with Azure imports |
| **Portal Instructions** | "In the Azure portal..." prose patterns |

### Article Priority

| Type | Priority | Rationale |
|------|----------|-----------|
| **Quickstart** | Critical | First thing users copy; must model security |
| **How-to** | Critical | Procedural guidance users follow exactly |
| **Tutorial** | High | Learning content that shapes habits |
| **Sample** | High | Code users copy directly |
| **Concept** | Medium | May include example configurations |

---

## Phase 3: Compliance Check

For each auditable instance, evaluate against extracted rules:

### Violation Severity

| Finding | Severity | Example |
|---------|----------|---------|
| **Prohibited pattern used** | CRITICAL | Article demonstrates `az keyvault set-policy` |
| **Missing required flag** | HIGH | `az keyvault create` without `--enable-rbac-authorization` |
| **Insecure authentication** | HIGH | Code uses `ClientSecretCredential` without security note |
| **Missing security context** | MEDIUM | Public endpoint demo without private endpoint mention |

---

## Phase 4: Generate Report

### Output Options

**Option A: Terminal Summary**
Print a concise summary with top violations.

**Option B: Markdown Report File**
Create `security-compliance-audit.md` in the docset or workspace root.

### Report Format

```markdown
# Security Compliance Audit Report

**Service:** Azure Key Vault
**Security Policy:** secure-key-vault.md
**Docset:** /azure/key-vault/
**Audit Date:** <today>
**Articles Scanned:** 127
**Auditable Instances:** 342

## Executive Summary

| Metric | Value |
|--------|-------|
| Compliance Score | 73% |
| Critical Violations | 4 |
| High Violations | 12 |
| Medium Violations | 23 |

## Critical Violations

### 1. quickstarts/create-key-vault-cli.md (line 45)

**Rule:** KV-RBAC-001 — Use RBAC for access control
**Found:**
```azurecli
az keyvault create --name $KV_NAME --resource-group $RG_NAME
```

**Issue:** Missing `--enable-rbac-authorization`
**Fix:**
```azurecli
az keyvault create --name $KV_NAME --resource-group $RG_NAME --enable-rbac-authorization
```

---

### 2. how-to/manage-keys.md (line 112)

**Rule:** KV-ANTI-001 — Do not use access policies
**Found:**
```azurecli
az keyvault set-policy --name $KV_NAME --object-id $USER_ID
```

**Issue:** Demonstrates deprecated access policy pattern
**Fix:** Replace with RBAC role assignment

---

## High Violations

| Article | Line | Rule | Issue |
|---------|------|------|-------|
| tutorials/key-rotation.md | 67 | KV-RBAC-001 | Missing RBAC flag |
| samples/dotnet/Program.cs | 23 | KV-AUTH-001 | Uses ClientSecretCredential |

## Compliance by Article Type

| Type | Articles | Compliant | Score |
|------|----------|-----------|-------|
| Quickstarts | 8 | 5 | 62% |
| How-tos | 45 | 38 | 84% |
| Tutorials | 12 | 9 | 75% |
```

---

## Execution Process

1. **Find security article**: Locate `secure-*.md` in the workspace
2. **Extract rules**: Parse each recommendation into structured rules
3. **Scan docset**: Find all CLI, PowerShell, ARM, Bicep, SDK code
4. **Check compliance**: Evaluate each instance against rules
5. **Generate report**: Output to terminal or markdown file
6. **Summarize**: Provide actionable next steps

---

## Limitations and Confidence

### High Confidence Rules
- CLI/PowerShell flag requirements (clear pattern matching)
- Prohibited command patterns (exact string matching)
- ARM/Bicep property requirements (JSON/YAML parsing)

### Medium Confidence Rules
- SDK authentication patterns (requires understanding code context)
- Portal instruction compliance (prose is ambiguous)

### Low Confidence Rules
- Architecture guidance (e.g., "separate vaults per environment")
- Prose-only recommendations without concrete patterns

Report confidence level for each rule when generating the audit.

---

## Example Invocation

```
Audit the Key Vault docset for security compliance
```

The skill will:
1. Find `secure-key-vault.md` and parse it into rules
2. Scan all articles under `/azure/key-vault/`
3. Check every CLI command, code sample, ARM template
4. Generate compliance report

**Output location**: Ask user preference (terminal or file), default to `security-compliance-audit.md` in docset root.
