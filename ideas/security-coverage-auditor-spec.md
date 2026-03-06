# Security Coverage Auditor - Feature Specification

**Status:** Proposal (Validated via Key Vault System Test)  
**Author:** Matthew Baldwin  
**Date:** February 20, 2026  
**Integration Target:** Content Mentor (MCP)
**Last Updated:** February 20, 2026 (post-system-test)

---

## Executive Summary

A Copilot-powered tool that uses "Secure your \<Service\>" articles as the **source of truth** for security best practices, then audits **all articles in the service docset** to ensure security recommendations are consistently modeled throughout procedural content—CLI commands, PowerShell scripts, Portal instructions, and prose guidance.

---

## Problem Statement

Security horizontal articles define **what** should be secured, but there's no systematic way to verify that:
1. Procedural documentation exists for each security recommendation (the **how**)
2. All procedural content across the docset **consistently models** those best practices

This creates:

- **Inconsistent guidance:** Quickstarts show `az keyvault create` without `--enable-rbac-authorization`, contradicting security article
- **Anti-pattern propagation:** Tutorials teach insecure defaults that users copy-paste into production
- **Security recommendation isolation:** Best practices live in security article but aren't reflected in day-to-day how-to content
- **Documentation drift:** Security practices evolve but procedural articles don't get updated
- **No visibility:** No way to know which articles model security correctly vs. which need updates

**Current state:** Security articles and procedural content are disconnected silos.  
**Desired state:** Security article serves as policy; all procedural content is audited for compliance.

---

## Core Concept: Security Article as Policy

The security horizontal article becomes a **machine-readable security policy** that defines:

| Policy Element | Example | Audit Application |
|----------------|---------|-------------------|
| **Required flags/parameters** | "Use RBAC for access control" | Every `az keyvault create` must include `--enable-rbac-authorization` |
| **Required configurations** | "Enable soft delete and purge protection" | Creation commands/steps must enable these features |
| **Prohibited patterns** | "Do not use access policies" | Flag any article demonstrating access policy configuration |
| **Recommended architectures** | "Use private endpoints" | Articles showing public access should note private endpoint alternative |
| **Authentication patterns** | "Use managed identities" | Code samples should use `DefaultAzureCredential`, not connection strings |

---

## Solution Overview

```
┌─────────────────────────────────────────────────────────────┐
│  INPUT                                                      │
│  - Security article: secure-azure-key-vault.md (THE POLICY) │
│  - Docset path: /azure/key-vault/ (ALL CONTENT TO AUDIT)    │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 1: POLICY EXTRACTION                                 │
│  Parse security article into structured rules:              │
│  - Required CLI/PowerShell flags per command                │
│  - Required Portal configurations                           │
│  - Prohibited patterns/anti-patterns                        │
│  - Authentication requirements                              │
│  - Network security requirements                            │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 2: DOCSET SCAN                                       │
│  Analyze every article in the docset:                       │
│  - Find CLI commands (az keyvault ...)                      │
│  - Find PowerShell commands (New-AzKeyVault ...)            │
│  - Find code samples (SDK usage)                            │
│  - Find Portal instructions (prose patterns)                │
│  - Find architecture descriptions                           │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 3: COMPLIANCE CHECK                                  │
│  For each procedural instance found:                        │
│  - Does it include required security flags?                 │
│  - Does it avoid prohibited patterns?                       │
│  - Does it mention security considerations?                 │
│  - Is there a cross-reference to security article?          │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  PHASE 4: GAP ANALYSIS                                      │
│  - Which security recommendations lack procedural docs?     │
│  - Which articles model insecure patterns?                  │
│  - Which articles are missing security context?             │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│  OUTPUT                                                     │
│  - Compliance report (articles modeling security correctly) │
│  - Violation report (articles needing security updates)     │
│  - Gap report (missing procedural documentation)            │
│  - Suggested fixes (specific edits to make)                 │
│  - Work items (optional ADO integration)                    │
└─────────────────────────────────────────────────────────────┘
```

---

## Detailed Requirements

### Input Specification

| Parameter | Required | Description |
|-----------|----------|-------------|
| `security_article_path` | Yes | Path to the security horizontal article (the policy) |
| `docset_path` | Yes | Root path of the service docset to audit |
| `output_path` | No | Where to write reports (default: `./audit-reports/`) |
| `create_work_items` | No | Boolean - create ADO work items for violations (default: false) |
| `severity_threshold` | No | Minimum severity to report: `critical`, `high`, `medium`, `all` (default: `all`) |
| `fix_suggestions` | No | Boolean - generate specific edit suggestions (default: true) |

### Phase 1: Policy Extraction

Parse the security article into machine-actionable rules:

#### Rule Types

**1. Command Flag Rules** - Required parameters for CLI/PowerShell commands
```yaml
rule:
  type: command_flag
  security_recommendation: "Use Azure RBAC for access control"
  commands:
    - pattern: "az keyvault create"
      required_flags:
        - "--enable-rbac-authorization"
      prohibited_flags:
        - "--enable-soft-delete false"  # would disable soft delete
    - pattern: "New-AzKeyVault"
      required_flags:
        - "-EnableRbacAuthorization"
  source_line: 45
```

**2. Configuration Rules** - Required settings in Portal/ARM/Bicep
```yaml
rule:
  type: configuration
  security_recommendation: "Enable soft delete and purge protection"
  required_properties:
    - property: "enableSoftDelete"
      value: true
    - property: "enablePurgeProtection"  
      value: true
  applies_to:
    - "ARM templates"
    - "Bicep files"
    - "Portal instructions"
  source_line: 52
```

**3. Anti-Pattern Rules** - Patterns that should never appear
```yaml
rule:
  type: anti_pattern
  security_recommendation: "Do not use access policies"
  prohibited_patterns:
    - "az keyvault set-policy"
    - "Set-AzKeyVaultAccessPolicy"
    - "accessPolicies:" # in ARM/Bicep
    - "Access policies" # in Portal instructions
  severity: high
  source_line: 67
```

**4. Authentication Rules** - How code samples should authenticate
```yaml
rule:
  type: authentication
  security_recommendation: "Use managed identities"
  preferred_patterns:
    - "DefaultAzureCredential"
    - "ManagedIdentityCredential"
    - "managed identity"
  discouraged_patterns:
    - "ClientSecretCredential"
    - "connection string"
    - "access key"
  source_line: 78
```

**5. Network Rules** - Network security patterns
```yaml
rule:
  type: network
  security_recommendation: "Use private endpoints"
  when_showing: "public endpoint access"
  must_mention: "private endpoint alternative"
  cross_reference: "/azure/key-vault/general/private-link-service"
  source_line: 34
```

**6. Prose Rules** - Text patterns in instructions
```yaml
rule:
  type: prose
  security_recommendation: "Enable diagnostic logging"
  when_topic: "monitoring OR logging OR troubleshooting"
  should_mention: "diagnostic settings"
  source_line: 112
```

### Phase 2: Docset Scan

Analyze every file in the docset to find auditable content:

#### Content Detection Patterns

| Content Type | Detection Method | Examples |
|--------------|------------------|----------|
| **Azure CLI** | Code blocks with `azurecli` or `az ` prefix | `az keyvault create --name myvault` |
| **PowerShell** | Code blocks with `powershell` or cmdlet patterns | `New-AzKeyVault -Name myvault` |
| **ARM Templates** | JSON with `$schema` containing `deploymentTemplate` | Resource definitions |
| **Bicep** | `.bicep` files or `bicep` code blocks | `resource kv 'Microsoft.KeyVault/vaults@...'` |
| **REST API** | HTTP methods with Azure URLs | `PUT https://management.azure.com/...` |
| **SDK Code** | Language-tagged code blocks with Azure SDK imports | Python, .NET, Java, JavaScript |
| **Portal Instructions** | Prose patterns like "In the Azure portal..." | Step-by-step Portal guidance |

#### File Classification

| Article Type | Audit Priority | Rationale |
|--------------|----------------|-----------|
| **Quickstart** | Critical | First thing users copy; must model security |
| **How-to** | Critical | Procedural guidance users follow exactly |
| **Tutorial** | High | Learning content that shapes habits |
| **Sample/Example** | High | Code users copy directly |
| **Concept** | Medium | May include example configurations |
| **Reference** | Low | Usually auto-generated; less copy-paste risk |
| **Overview** | Low | Rarely contains executable content |

### Phase 3: Compliance Check

For each auditable content instance, evaluate against extracted rules:

#### Compliance Evaluation Matrix

| Finding | Severity | Example |
|---------|----------|---------|
| **Missing required flag** | High | `az keyvault create` without `--enable-rbac-authorization` |
| **Prohibited pattern used** | Critical | Article demonstrates `az keyvault set-policy` |
| **Insecure authentication** | High | Code sample uses `ClientSecretCredential` without security note |
| **Missing security context** | Medium | Public endpoint demo without private endpoint mention |
| **Anti-pattern in prose** | Medium | "Configure access policies for your vault" |
| **Outdated security config** | Medium | Uses deprecated security parameter |

#### Compliance Scoring

```
Article Compliance Score = (Compliant Instances / Total Auditable Instances) × 100

Docset Compliance Score = Weighted average across all articles
  - Quickstarts/How-tos weighted 3x
  - Tutorials weighted 2x  
  - Other content weighted 1x
```

### Phase 4: Gap Analysis

Beyond compliance violations, identify documentation gaps:

| Gap Type | Detection | Example |
|----------|-----------|---------|
| **Missing how-to** | Security rec has no procedural article | "Configure autorotation" → no how-to exists |
| **Incomplete coverage** | How-to exists but missing variants | Portal-only, no CLI/PowerShell |
| **Missing cross-reference** | Procedural article doesn't link to security article | Quickstart doesn't mention security best practices |
| **Stale content** | `ms.date` > 18 months on security-relevant article | Old quickstart with outdated security patterns |

### Output Specification

#### Comprehensive Audit Report Format

```markdown
# Security Compliance Audit Report

**Service:** Azure Key Vault  
**Security Policy:** /azure/key-vault/general/secure-key-vault.md  
**Docset Audited:** /azure/key-vault/  
**Audit Date:** 2026-02-20  
**Articles Scanned:** 127  
**Auditable Instances Found:** 342

---

## Executive Summary

| Metric | Value |
|--------|-------|
| **Docset Compliance Score** | 73% |
| **Critical Violations** | 4 |
| **High Violations** | 12 |
| **Medium Violations** | 23 |
| **Compliant Instances** | 249 |
| **Documentation Gaps** | 3 |

### Compliance by Article Type

| Type | Articles | Compliant | Violations | Score |
|------|----------|-----------|------------|-------|
| Quickstarts | 8 | 5 | 3 | 62% |
| How-tos | 45 | 38 | 7 | 84% |
| Tutorials | 12 | 9 | 3 | 75% |
| Concepts | 32 | 28 | 4 | 88% |
| Reference | 30 | 27 | 3 | 90% |

---

## Critical Violations

These articles demonstrate anti-patterns that directly contradict security recommendations.

### 1. quickstarts/create-key-vault-cli.md

**Violation:** Missing required security flag  
**Security Policy:** "Use Azure RBAC for access control" (line 67)  
**Found on line 45:**
```azurecli
az keyvault create --name $KV_NAME --resource-group $RG_NAME --location eastus
```

**Issue:** Command creates vault without `--enable-rbac-authorization`, defaulting to access policies (deprecated pattern).

**Suggested Fix:**
```azurecli
az keyvault create --name $KV_NAME --resource-group $RG_NAME --location eastus --enable-rbac-authorization
```

**Work Item:** Update Key Vault CLI quickstart to use RBAC

---

### 2. how-to/manage-keys.md

**Violation:** Prohibited pattern used  
**Security Policy:** "Do not use access policies" (line 89)  
**Found on line 112:**
```azurecli
az keyvault set-policy --name $KV_NAME --object-id $USER_ID --key-permissions get list
```

**Issue:** Article demonstrates access policy configuration, which contradicts RBAC-first guidance.

**Suggested Fix:** Replace with Azure RBAC role assignment:
```azurecli
az role assignment create --role "Key Vault Crypto User" \
  --assignee $USER_ID \
  --scope "/subscriptions/$SUB_ID/resourceGroups/$RG_NAME/providers/Microsoft.KeyVault/vaults/$KV_NAME"
```

**Work Item:** Migrate manage-keys.md from access policies to RBAC

---

### 3. samples/dotnet-key-vault-sample/Program.cs

**Violation:** Insecure authentication pattern  
**Security Policy:** "Use managed identities for application authentication" (line 78)  
**Found on line 23:**
```csharp
var credential = new ClientSecretCredential(tenantId, clientId, clientSecret);
```

**Issue:** Sample uses client secret authentication without noting managed identity as preferred approach.

**Suggested Fix:** Either replace with managed identity:
```csharp
var credential = new DefaultAzureCredential();
```
Or add security note:
```markdown
> [!NOTE]
> For production workloads, use [managed identities](/azure/key-vault/general/authentication#authentication-with-managed-identities) 
> instead of client secrets. This sample uses client secrets for local development only.
```

**Work Item:** Update .NET sample to demonstrate managed identity authentication

---

## High Severity Violations

### Summary Table

| Article | Line | Violation | Policy Reference |
|---------|------|-----------|------------------|
| tutorials/key-rotation.md | 67 | Missing `--enable-rbac-authorization` | Use RBAC |
| how-to/backup-restore.md | 34 | Uses `Set-AzKeyVaultAccessPolicy` | Do not use access policies |
| quickstarts/create-vault-portal.md | 89 | No mention of RBAC in Portal steps | Use RBAC |
| ... | ... | ... | ... |

[Detailed findings for each in expandable sections]

---

## Medium Severity Violations

### Missing Security Context

These articles demonstrate features correctly but don't mention relevant security considerations.

| Article | Missing Context | Should Reference |
|---------|-----------------|------------------|
| how-to/network-config.md | Private endpoint discussion | Network security section |
| tutorials/secrets-management.md | Soft delete/purge protection | Data protection section |
| quickstarts/create-secret.md | Key Vault security overview | Security article |

---

## Compliant Articles ✅

These articles correctly model security best practices.

| Article | Auditable Instances | All Compliant |
|---------|---------------------|---------------|
| how-to/configure-private-endpoints.md | 8 | ✅ |
| how-to/rbac-guide.md | 12 | ✅ |
| how-to/managed-identity.md | 6 | ✅ |
| security/secure-key-vault.md | N/A | Policy Source |
| ... | ... | ... |

---

## Documentation Gaps

Security recommendations without adequate procedural documentation.

### 1. "Configure autorotation for secrets"

**Policy Location:** Data protection section, line 67  
**Current State:** Links to concept article only  
**Gap:** No step-by-step how-to for configuring secret autorotation  
**Recommendation:** Create `how-to/configure-secret-autorotation.md`  
**Priority:** High

### 2. "Use HSM-protected keys for high-security scenarios"

**Policy Location:** Service-specific section, line 134  
**Current State:** Brief mention in keys overview  
**Gap:** No procedural guide for creating/managing HSM keys  
**Recommendation:** Create `how-to/create-hsm-protected-keys.md`  
**Priority:** Medium

---

## Recommended Work Items

### Critical (Fix Immediately)
1. **[Security]** Update CLI quickstart to use RBAC - quickstarts/create-key-vault-cli.md
2. **[Security]** Migrate manage-keys.md from access policies to RBAC
3. **[Security]** Update .NET sample to use managed identity
4. **[Security]** Update PowerShell quickstart to use RBAC

### High (Fix This Sprint)
5. **[Security]** Add RBAC steps to Portal quickstart
6. **[Security]** Update backup-restore.md authentication pattern
7. **[Security]** Add security context to key-rotation tutorial
... [additional items]

### Medium (Backlog)
12. **[Security]** Add private endpoint mentions to network-config.md
13. **[Docs Gap]** Create secret autorotation how-to
14. **[Docs Gap]** Create HSM-protected keys how-to
... [additional items]

---

## Appendix: Policy Rules Applied

| Rule ID | Type | Security Recommendation | Instances Found | Violations |
|---------|------|-------------------------|-----------------|------------|
| R001 | command_flag | Use RBAC | 45 | 8 |
| R002 | anti_pattern | Do not use access policies | 12 | 4 |
| R003 | authentication | Use managed identities | 23 | 3 |
| R004 | configuration | Enable soft delete | 18 | 2 |
| R005 | network | Use private endpoints | 15 | 6 |
| ... | ... | ... | ... | ... |
```

### Integration Points

#### Content Mentor MCP Tool

```
mcp_content-devel_audit_security_compliance

Parameters:
  security_article: string (required) - Path to security article (the policy)
  docset: string (required) - Path to service docset to audit
  output: string (optional) - Output path for report
  severity: string (optional) - Minimum severity: critical|high|medium|all
  create_work_items: boolean (optional) - Create ADO work items for violations
  fix_suggestions: boolean (optional) - Generate specific edit suggestions
  
Returns:
  report_path: string - Path to generated report
  summary: object
    compliance_score: number
    critical_violations: number
    high_violations: number
    medium_violations: number
    compliant_instances: number
    documentation_gaps: number
  work_items_created: array - ADO work item IDs (if requested)
```

#### ADO Work Item Integration

When `create_work_items: true`, generate work items with:

**For Violations:**
- **Title:** `[Security] <brief description> - <article path>`
- **Description:** Violation details, policy reference, suggested fix
- **Tags:** `security-compliance`, `security-horizontal`, `<service-name>`
- **Priority:** Mapped from violation severity
- **Acceptance Criteria:** 
  - [ ] Security violation addressed
  - [ ] Article models security best practice correctly
  - [ ] Cross-reference to security article added (if applicable)

**For Documentation Gaps:**
- **Title:** `[Docs Gap] Create <article type>: <topic>`
- **Description:** Gap details, policy reference, content outline
- **Tags:** `security-horizontal`, `doc-gap`, `<service-name>`
- **Priority:** Mapped from gap severity

#### Automation Integration

**Scheduled Audits:**
- Run weekly/monthly against all service docsets with security articles
- Generate trend reports showing compliance improvement over time
- Alert on new critical violations

**PR Integration:**
- Run audit on changed files in PR
- Block merge if PR introduces new security violations
- Provide inline suggestions for fixes

---

## Implementation Phases

### Phase 1: Policy Extraction Engine (MVP)
- Parse security article bullet structure
- Extract explicit command patterns (CLI, PowerShell)
- Identify anti-pattern rules from "Do not" recommendations
- Basic authentication pattern detection
- **Deliverable:** Policy extraction for one service (Key Vault)

### Phase 2: Docset Scanner
- Scan all markdown files in docset
- Detect code blocks by language type
- Extract Azure CLI commands
- Extract PowerShell commands
- Detect Portal instruction patterns
- **Deliverable:** Content inventory for docset

### Phase 3: Compliance Checker
- Match found content against extracted rules
- Classify violations by severity
- Generate compliance scores
- Produce basic audit report
- **Deliverable:** Functional audit for Key Vault docset

### Phase 4: Fix Suggestion Engine
- Analyze violation context
- Generate specific code replacements
- Suggest prose additions for missing context
- Format as copy-pasteable fixes
- **Deliverable:** Actionable fix suggestions

### Phase 5: Gap Analysis
- Cross-reference security recommendations with docset coverage
- Identify missing how-to content
- Detect incomplete procedural guides
- **Deliverable:** Documentation gap report

### Phase 6: CM Integration
- MCP tool implementation
- ADO work item creation
- Batch processing across services
- Trend reporting
- **Deliverable:** Full CM integration

### Phase 7: CI/CD Integration
- PR-level audit checks
- Automated compliance gates
- Scheduled docset audits
- **Deliverable:** Continuous compliance monitoring

---

## Technical Considerations

### Policy Extraction Challenges

| Challenge | Approach |
|-----------|----------|
| Security articles vary in structure | Define canonical structure; normalize during parsing |
| Implicit security requirements | Support manual rule additions to policy file |
| Cross-service patterns | Share common rules across services (e.g., managed identity) |
| Rule evolution | Version policy files; track changes over time |

### Content Detection Accuracy

| Content Type | Detection Confidence | Notes |
|--------------|---------------------|-------|
| Azure CLI | High | Clear `az` prefix pattern |
| PowerShell | High | Cmdlet naming patterns |
| ARM/Bicep | High | Schema detection |
| Portal prose | Medium | Heuristic-based; may need tuning |
| SDK code | Medium | Depends on import detection |
| Conceptual mentions | Low | May require NLP; start with keyword matching |

### False Positive Mitigation

- **Contextual analysis:** A command in a "Don't do this" warning shouldn't trigger violation
- **Comment/note detection:** Code in `> [!NOTE]` blocks may be demonstrating what not to do
- **Versioning awareness:** Old API versions in migration guides are intentional
- **Suppression mechanism:** Allow `<!-- security-audit-ignore: R001 -->` comments

---

## Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Policy extraction accuracy | >90% | Rules correctly extracted from security article |
| Content detection recall | >95% | Auditable instances found |
| Violation detection precision | >85% | True violations vs. false positives |
| Fix suggestion acceptance | >70% | Suggestions used without modification |
| Compliance score improvement | +10% per quarter | Docset scores over time |
| Time to audit one docset | <10 minutes | End-to-end execution |

---

## Open Questions

1. **Rule customization:** Should service teams be able to add custom rules beyond what's in the security article?

2. **Severity calibration:** How do we weight different violation types? Is missing RBAC flag worse than missing security note?

3. **Historical content:** How do we handle intentionally outdated content (migration guides, deprecation notices)?

4. **Cross-docset patterns:** Should common patterns (managed identity, private endpoints) be shared rules?

5. **Incremental adoption:** Can we run in "report-only" mode before enforcing in CI/CD?

6. **External references:** How do we handle articles that link to other docsets (Entra, Defender)?

7. **Bicep/ARM modules:** How do we audit infrastructure-as-code templates that may live outside the main docset?

---

## Example Policy Rules for Key Vault

Based on the generated `secure-azure-key-vault.md`:

```yaml
# Sample policy extraction output
service: azure-key-vault
security_article: /azure/key-vault/general/secure-key-vault.md
extracted_rules:

  - id: KV-001
    type: command_flag
    recommendation: "Use Azure RBAC for access control"
    section: "Identity and access management"
    commands:
      - pattern: "az keyvault create"
        required: ["--enable-rbac-authorization"]
      - pattern: "New-AzKeyVault"
        required: ["-EnableRbacAuthorization"]
    severity: critical

  - id: KV-002
    type: anti_pattern
    recommendation: "Do not use legacy access policies"
    section: "Identity and access management"  
    prohibited:
      - "az keyvault set-policy"
      - "Set-AzKeyVaultAccessPolicy"
      - "accessPolicies:"
    severity: critical

  - id: KV-003
    type: configuration
    recommendation: "Enable soft delete"
    section: "Data protection"
    required_properties:
      - name: "enableSoftDelete"
        value: true
    severity: high

  - id: KV-004
    type: configuration
    recommendation: "Enable purge protection"
    section: "Data protection"
    required_properties:
      - name: "enablePurgeProtection"
        value: true
    severity: high

  - id: KV-005
    type: authentication
    recommendation: "Use managed identities for application authentication"
    section: "Identity and access management"
    preferred:
      - "DefaultAzureCredential"
      - "ManagedIdentityCredential"
      - "managed identity"
    discouraged:
      - "ClientSecretCredential"
      - "client secret"
      - "connection string"
    severity: high

  - id: KV-006
    type: network
    recommendation: "Disable public network access and use private endpoints"
    section: "Network security"
    when_showing: "public endpoint"
    must_mention: "private endpoint"
    cross_reference: "/azure/key-vault/general/private-link-service"
    severity: medium

  - id: KV-007
    type: prose
    recommendation: "Enable Microsoft Defender for Key Vault"
    section: "Logging and monitoring"
    when_topic: "monitoring|security|threat"
    should_mention: "Defender for Key Vault"
    severity: low
```

---

## Appendix A: Security Article Structure Reference

Expected structure based on `template.txt` and `guide.txt`:

```markdown
---
title: Secure your <Service> deployment
ms.custom: horz-security
---

# Secure your <Service> deployment

<Intro paragraph>

[!INCLUDE [Zero Trust statement]]

## Network security
- **Action**: Description. See [Link](/path).
    - Exception/sub-recommendation. See [Link](/path).

## Identity and access management
- **Do not <anti-pattern>**: Why this is wrong. See [Alternative](/path).
...

## Data protection
...

## Logging and monitoring
...

## Compliance and governance
...

## Backup and recovery
...

## Service-specific security (position varies by service category)
...

## Next steps
- [Security baseline](/security/benchmark/azure/baselines/...)
- [Well-Architected guidance](/azure/well-architected/...)
```

---

## Appendix B: Common Security Patterns Across Services

These patterns apply to most Azure services and can be shared rules:

| Pattern | CLI | PowerShell | Portal |
|---------|-----|------------|--------|
| **RBAC over access policies** | `--enable-rbac-authorization` | `-EnableRbacAuthorization` | "Permission model: Azure RBAC" |
| **Managed identity** | `DefaultAzureCredential` | `Get-AzAccessToken` | "Identity: System assigned" |
| **Private endpoints** | `--private-link-resource` | `-PrivateLinkResource` | "Private endpoint" |
| **Soft delete** | `--enable-soft-delete` | `-EnableSoftDelete` | "Soft delete: Enabled" |
| **Purge protection** | `--enable-purge-protection` | `-EnablePurgeProtection` | "Purge protection: Enabled" |
| **Diagnostic settings** | `az monitor diagnostic-settings create` | `Set-AzDiagnosticSetting` | "Diagnostic settings" |
| **Customer-managed keys** | `--key-uri` | `-KeyVaultUri` | "Encryption: Customer-managed" |

---

## Appendix C: Violation Severity Guidelines

| Severity | Criteria | Response Time |
|----------|----------|---------------|
| **Critical** | Anti-pattern actively demonstrated; users would copy insecure code | Immediate fix |
| **High** | Missing required security flag; insecure default used | Fix within sprint |
| **Medium** | Missing security context; no cross-reference to security article | Fix within quarter |
| **Low** | Could be more explicit about security; nice-to-have improvement | Backlog |

---

## Appendix D: Related Initiatives

| Initiative | Relationship |
|------------|--------------|
| **Security Horizontal** | Creates the policy (security articles); this tool enforces it |
| **Content Mentor** | Integration target for workflow automation |
| **MCSB v2 Baselines** | Additional policy source; can cross-reference |
| **WAF Service Guides** | Complementary guidance; may inform rules |
| **Defender for Cloud** | Runtime enforcement; this is documentation enforcement |


---

## Appendix E: System Test Results - Azure Key Vault Docset

**Test Date:** February 20, 2026  
**Docset:** /azure/key-vault/  
**Security Article:** secure-key-vault.md (26 recommendations)

### Summary Metrics

| Metric | Value |
|--------|-------|
| Articles scanned | 189 |
| Total lines | 25,964 |
| Recommendations in security article | 26 |
| Recommendations with how-to | 16 (62%) |
| Code compliance score | 100% |
| Topic coverage score | 62% |
| **Overall compliance score** | **78%** |

### Code Compliance Results

| Rule Type | Instances Found | Compliant | Violations | Exceptions |
|-----------|-----------------|-----------|------------|------------|
| `az keyvault create` with RBAC | 7 | 5 | 0 | 2 (legacy) |
| `New-AzKeyVault` with RBAC | 4 | 3 | 0 | 1 (legacy) |
| `az keyvault set-policy` (prohibited) | 3 | 0 | 0 | 3 (legacy docs) |
| `DefaultAzureCredential` | 40+ | 40+ | 0 | 0 |
| `ClientSecretCredential` | 0 | N/A | 0 | 0 |

### Topic Coverage Results

**Best Covered:**
| Topic | Files | Rating |
|-------|-------|--------|
| RBAC | 84 | ⭐⭐⭐⭐⭐ Excellent |
| Key expiration | 50 | ⭐⭐⭐⭐⭐ Excellent |
| Managed identity | 47 | ⭐⭐⭐⭐⭐ Excellent |
| Diagnostic logging | 31 | ⭐⭐⭐⭐ Good |
| Soft delete | 30 | ⭐⭐⭐⭐ Good |

**Documentation Gaps (Worst Covered):**
| Topic | Files | Rating | Gap |
|-------|-------|--------|-----|
| Defender for Key Vault | 1 | ⭐ Minimal | No how-to |
| Conditional Access | 1 | ⭐ Minimal | No how-to |
| Least privilege | 2 | ⭐ Minimal | Limited coverage |
| PIM/JIT | 7 | ⭐⭐ Limited | No how-to |

### External Dependencies Identified

| Include Path | Used By | Security Impact |
|--------------|---------|-----------------|
| `~/reusable-content/.../create-key-vault-cli.md` | 4 quickstarts | Contains RBAC flags |
| `~/reusable-content/.../create-key-vault-ps.md` | 3 quickstarts | Contains RBAC flags |

**Action Required:** Audit external includes at `ce-skilling/azure/includes/key-vault/` to verify security compliance.

### Technical Validation

| Technique | Result |
|-----------|--------|
| grep-based CLI command detection | ✅ 100% accuracy |
| grep-based PowerShell detection | ✅ 100% accuracy |
| grep-based SDK pattern detection | ✅ 100% accuracy |
| Title-based exception detection | ✅ 100% accuracy (4/4 legacy articles identified) |
| File count for topic coverage | ✅ Useful quality signal |

---

## Appendix F: Lessons Learned from System Test

### What Worked Well

1. **grep-based detection** - Simple pattern matching achieved 100% accuracy for code compliance
2. **File count as coverage signal** - Counting files mentioning a topic provides useful quality indicator
3. **Exception detection via title** - "(legacy)" marker reliably identifies intentional anti-pattern content
4. **Separation of concerns** - Code compliance and topic coverage are distinct, both valuable

### What Needs Improvement

1. **External include handling** - Cannot audit content in other repositories; need dependency tracking
2. **Prose recommendations** - Rules like "separate vaults per environment" are hard to audit automatically
3. **Cross-docset content** - Security features documented in Defender/Entra repos create gaps in service docsets

### Unexpected Findings

1. **Good code, bad coverage** - Key Vault has excellent code compliance but significant topic coverage gaps
2. **Legacy content is well-marked** - Zero false positives because legacy docs use consistent naming
3. **Shared includes are pervasive** - 35 of 35 quickstarts use external includes for vault creation

### Key Insight

> A docset can score 100% on code compliance while having critical topic coverage gaps. **Both dimensions must be measured separately.**

### Recommended Next Steps

1. Audit the `ce-skilling/azure/includes/key-vault/` external includes
2. Create how-tos for Defender for Key Vault, PIM/JIT, and Conditional Access
3. Run system test on a second service (e.g., Azure Storage) to validate cross-service patterns
4. Build prototype MCP tool based on validated patterns
