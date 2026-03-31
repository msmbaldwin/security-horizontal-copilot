---
description: Generate "Secure your <Service>" security checklist articles for Azure services. Use when: creating security horizontal articles, writing security best practices for Azure services, generating MCSB-aligned security documentation. Follows Microsoft Learn standards, Zero Trust principles, and standard section ordering.
---

# Azure Security Article Generator

Generate comprehensive "Secure your \<Service>" security checklist articles for Microsoft Learn's Security Horizontal initiative.

## When to Use This Skill

Use this skill when asked to:
- "Generate a security article for Azure Storage"
- "Create security documentation for Key Vault"
- "Write a secure your deployment article for App Service"
- "Make a security checklist for Cosmos DB"

## Prerequisites

**The user should have the service docset open in their workspace.** This allows the skill to:
- Research existing security content in the docset
- Place the generated article in the correct location
- Link to existing procedural documentation

## Invocation

Extract the **service name** from the user's request. Examples:
- "Generate a security article for **Azure Blob Storage**" → Service = "Azure Blob Storage"
- "Create security docs for **Key Vault**" → Service = "Azure Key Vault"

## Output Location

**Find the appropriate location in the docset:**
1. Look for an existing `security/` folder in the docset
2. If no security folder, look for existing `secure-*.md` or `security-*.md` files
3. If unclear, ask the user where to place the article
4. Create the file as `secure-<service-slug>.md` (e.g., `secure-key-vault.md`)

**ServiceSlug derivation:**
1. Convert to lowercase
2. Replace spaces with `-`
3. Remove "azure-" prefix if present
4. Remove non-alphanumeric characters except `-`
- Example: "Azure Key Vault" → `key-vault`

---

## Generation Instructions

### Step 1: Classify the Service

Research and classify the service into ONE category to understand its primary security concerns:

| Category | Examples | Primary Security Concern |
|----------|----------|-------------------------|
| **Key/Secret Management** | Key Vault, Managed HSM | Cryptographic material protection |
| **Identity Services** | Entra ID, B2C | Authentication and authorization |
| **Storage Services** | Blob Storage, Files, Data Lake | Data at rest protection |
| **Compute Services** | VMs, App Service, Functions | Network isolation and runtime |
| **Database Services** | SQL, Cosmos DB, PostgreSQL | Data access and encryption |
| **Networking Services** | VNet, Firewall, Front Door | Traffic control and filtering |
| **Analytics Services** | Synapse, Databricks, HDInsight | Data processing security |
| **AI/ML Services** | OpenAI, ML, Cognitive Services | Model and data protection |
| **Integration Services** | Logic Apps, Event Grid, Service Bus | Message and workflow security |
| **General PaaS** | Any service not fitting above | Default concerns |

### Step 2: Apply Standard Section Order

Use a **consistent section order** across all SCI articles:

1. **Service-specific security** (optional — include only when needed)
2. **Network security**
3. **Identity and access management**
4. **Data protection**
5. **Logging and monitoring**
6. **Compliance and governance**
7. **Backup and recovery**

**When to include Service-specific security:**
- The service has unique architectural considerations
- Official documentation describes anti-patterns or inappropriate uses
- The service has distinct security fundamentals not covered by standard domains

When present, Service-specific security comes first to establish context.

### Step 3: Research

**Required Research (Execute All):**
1. **Workspace Documentation**: Use `semantic_search`, `grep_search`, `file_search` to find security content in the docset
2. **Local File Analysis**: Read existing security-related files in the workspace
3. **Service Documentation**: Use `mcp_microsoft_doc_microsoft_docs_search` with query: `<Service> security`
4. **Security Baseline**: Search for MCSB v2 baseline at `/security/benchmark/azure/baselines/<service>-security-baseline`
5. **WAF Guide**: Search for `/azure/well-architected/service-guides/<service>`
6. **Anti-Patterns**: Search for what NOT to do: `<Service> best practices`

**Conditional Research (when applicable):**
- **Reliability Guide** (backup/DR): `/azure/reliability/<service>`
- **TLS/Protocol** (if service exposes endpoints): `<Service> TLS HTTPS protocol security`
- **Multitenancy** (SaaS scenarios): `<Service> multitenant isolation`
- **Sub-Resource Security** (if service has components): `<Service> secure keys OR secrets OR certificates`
- **Regulatory Compliance**: `<Service> compliance HIPAA OR PCI`
- **DDoS Protection** (internet-facing): `<Service> DDoS protection OR rate limiting`

### Step 4: Generate Content

Follow the template structure and writing standards below.

---

## Article Template

```markdown
---
title: Secure your <Azure service name> deployment
description: Learn how to secure <service>, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: <service>
ms.topic: conceptual
ms.custom: horz-security
ms.date: <today's date in MM/DD/YYYY format>
ai-usage: ai-assisted
---

# Secure your <Azure service name> deployment

<Azure service> provides capabilities to <briefly describe what the service does>. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides security recommendations to help protect your Azure <service name> deployment.

[!INCLUDE [Security horizontal Zero Trust statement](~/reusable-content/ce-skilling/azure/includes/security/zero-trust-security-horizontal.md)]

## Service-specific security

<!-- 
Include this section only when:
- The service has unique architectural considerations
- Official documentation describes anti-patterns or inappropriate uses
- The service has distinct security fundamentals not covered by standard domains
-->

<Brief paragraph explaining unique security considerations>

### <Service> architecture

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).

### Appropriate use of <Service>

<!-- Include anti-patterns ONLY when official Microsoft documentation describes them -->

- **Do not use <Service> for <inappropriate use>**: Why inappropriate + alternative. See [Alternative](/azure/alternative).

## Network security

<Brief paragraph explaining network security importance>

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).
    - Some scenarios require <exception>. In such cases, <alternative>. See [Exception guidance](/path).

## TLS and HTTPS

<!-- Include if service has specific TLS/protocol requirements -->

<Service> supports TLS <versions> to ensure secure communication.

- **Enforce TLS version control**: Explanation. See [Documentation link](/azure/service/feature).

## Identity and access management

<Brief paragraph explaining identity and access risks>

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).

## Data protection

<Brief paragraph explaining data protection importance>

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).

## Logging and monitoring

<Brief paragraph explaining monitoring importance>

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).

## Compliance and governance

<Brief paragraph explaining compliance importance>

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).

## Backup and recovery

<Brief paragraph explaining backup importance>

- **Action verb + specific control**: Explanation and security benefit. See [Documentation link](/azure/service/feature).

## Related security articles

<!-- Include if service has sub-resource security articles -->

For security best practices specific to <sub-resources>, see:

- [Secure your <Service> <sub-resource>](/azure/<service>/<sub-resource>/secure-<sub-resource>)

## Next steps

- [Microsoft Cloud Security Benchmark v2 – <Service> security baseline](/security/benchmark/azure/baselines/<service>-security-baseline)
- [Well-Architected Framework – <Service> guide](/azure/well-architected/service-guides/<service>)
- [Zero Trust guidance center](/security/zero-trust/zero-trust-overview)
```

---

## Writing Standards

### Voice and Tone
- **Active voice**: "Enable private endpoints" not "Private endpoints should be enabled"
- **Direct address**: Use "you" to address the reader
- **Imperative mood**: Give clear commands
- **Professional but accessible**

### Bullet Format (MANDATORY)

```markdown
- **Action verb + specific control**: Clear explanation with security benefit. See [Feature documentation](/azure/service/feature).
```

**Examples:**
- **Enable private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network. See [Azure Private Link](/azure/private-link/private-link-overview).
- **Require multi-factor authentication**: Reduce account compromise risk by enforcing MFA for all administrative access. See [Microsoft Entra MFA](/entra/identity/authentication/concept-mfa-howitworks).

### Anti-Pattern Format

```markdown
- **Do not use <Service> for <inappropriate use>**: Why inappropriate + alternative. See [Alternative](/azure/alternative).
```

**Include anti-patterns ONLY when official Microsoft documentation describes them. Do not fabricate.**

### Conditional Recommendations

```markdown
- **Primary recommendation**: Explanation. See [Link](/path).
    - Some scenarios require <exception>. In such cases, <alternative>. See [Exception guidance](/path).
```

### Link Standards
- **Format**: Site-relative paths only (`/azure/service/feature`)
- **Never include**: `https://learn.microsoft.com` or `/en-us/`
- **Service-specific preferred** over generic guidance
- **If both needed**: Service-specific in main sentence, general as "See also:" second sentence

### Granular Control Separation

Separate distinct sub-controls into their own bullets:

**Incorrect (combined):**
- **Assign JIT privileged roles**: Use PIM with approvals and MFA for activation.

**Correct (separated):**
- **Assign just-in-time (JIT) privileged roles**: Use Azure PIM to assign eligible roles. See [PIM overview](/entra/...).
    - **Require approvals for privileged role activation**: Ensure at least one approver is required.
    - **Enforce multi-factor authentication for role activation**: Require MFA to activate JIT roles.

---

## Quality Standards

### Content Rules
- Every recommendation must link to authoritative Microsoft documentation
- Use site-relative paths for all links
- No placeholder content or generic boilerplate
- Include `ms.custom: horz-security` in YAML frontmatter
- **Do NOT use `[!IMPORTANT]`, `[!NOTE]`, or other callout formatting**
- **Only include actionable recommendations representing customer choices**:
  - Don't present enforced features as best practices (e.g., "Use TLS 1.2" when only TLS 1.2+ is supported)
  - Don't include non-actionable items like "Understand the shared responsibility model"
  - Focus on configurations customers can actually enable, disable, or configure

### Zero Trust Banner Placement
Place the Zero Trust include **after all intro content, immediately before the first H2**:
- Don't split intro paragraphs or paragraph+list combinations
- The banner creates a visual break marking the transition from "what this is" to "what to do"
- This placement is consistent regardless of whether the intro is 1 paragraph, 2 paragraphs, or paragraphs + lists

### Specificity Filter
- Include only recommendations that apply directly to the target service
- Avoid generic security advice that applies to all Azure services
- Each recommendation should address service-specific risks or capabilities

---

## Execution Process

1. **Locate Docset**: Find the service docset in the workspace
2. **Classify Service**: Determine category to understand primary security concerns
3. **Research**: Execute Required + applicable Conditional queries
4. **Generate Content**: Follow template structure and writing standards
5. **Verify Specificity**: Ensure each recommendation is service-specific
6. **Check Links**: Verify site-relative paths to valid articles
7. **Write File**: Create article in appropriate location within the docset

**After generation**: Use the security-article-validator skill in a fresh session to validate.
