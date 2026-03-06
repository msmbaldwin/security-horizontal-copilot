# Context Pack: Security Horizontal Copilot

**Purpose:** This document provides complete context for AI assistants working with this project.

---

## Project Overview

The **security-horizontal-copilot** project generates and validates "Secure your \<Service\>" articles for Microsoft Learn's Security Horizontal initiative. These articles consolidate actionable, service-specific security best practices for Azure services following Zero Trust principles.

**Repository:** `msmbaldwin/security-horizontal-copilot`  
**Primary branch:** `fy26q4` (feature branch), `main` (default)

---

## Core Workflow

### 1. Generation Phase
User provides service name + docset URL → Copilot researches → Generates article → Saves to `output/secure-<service-slug>.md`

**Input:** `input/prompt-generate.txt` (modify service variables, then execute)
**Output:** Markdown article following `input/template.txt` structure

### 2. Validation Phase (Fresh Session)
Open generated article → Execute `input/prompt-validate.txt` → Systematic review for accuracy and service-specificity → Direct edits applied

**Key principle:** Validate in a fresh chat session to avoid confirmation bias.

---

## Project Structure

```
security-horizontal-copilot/
├── input/
│   ├── prompt-generate.txt   # Generation instructions + research strategy
│   ├── prompt-validate.txt   # Validation checklist + severity classification
│   ├── template.txt          # Article structure + YAML frontmatter
│   └── guide.txt             # Writing style + formatting rules (CRITICAL)
├── output/                   # Generated articles
├── ideas/                    # Future feature specs
├── security-review/          # Manual review feedback examples
├── .copilot-instructions.md  # WSL2/CRLF workaround notes
└── README.md
```

---

## Article Structure & Section Ordering

### CRITICAL: Section order depends on service category

| Service Category | Section Order |
|------------------|---------------|
| **Key/Secret Management** (Key Vault, Managed HSM) | Service-specific → Data protection → Identity → Network → TLS/HTTPS → Logging → Compliance → Backup |
| **Identity Services** (Entra ID, B2C) | Identity → Network → Data protection → Logging → Compliance → Backup → Service-specific |
| **Storage Services** (Blob, Files, Data Lake) | Data protection → Network → Identity → Logging → Compliance → Backup → Service-specific |
| **Compute Services** (VMs, App Service, Functions) | Network → Identity → Data protection → Logging → Compliance → Backup → Service-specific |
| **Database Services** (SQL, Cosmos DB, PostgreSQL) | Data protection → Network → Identity → Logging → Compliance → Backup → Service-specific |
| **Default (all other)** | Network → Identity → Data protection → Logging → Compliance → Backup → Service-specific |

---

## Writing Standards Summary

### Voice & Tone
- **Active voice:** "Enable private endpoints" (not "Private endpoints should be enabled")
- **Direct address:** Use "you" to speak to reader
- **Imperative mood:** Give clear commands
- **Professional but accessible**

### Bullet Format (MANDATORY)
```markdown
- **Action verb + specific control**: Clear explanation with security benefit. See [Feature documentation](/azure/service/feature).
```

### Anti-Pattern Format (when documentation supports)
```markdown
- **Do not use <Service> for <inappropriate use>**: Why it's inappropriate + alternative. See [Alternative](/azure/alternative).
```

### Conditional Recommendations (with exceptions)
```markdown
- **Primary recommendation**: Explanation. See [Link](/path).
    - Some scenarios require <exception>. In such cases, <alternative>. See [Exception guidance](/path).
```

### Link Standards
- **Format:** Site-relative paths only (`/azure/service/feature`)
- **Never include:** `https://learn.microsoft.com` or `/en-us/`
- **Service-specific links preferred** over generic guidance
- **If both needed:** Service-specific in main sentence, general as "See also:" second sentence

### Required YAML Frontmatter Tag
```yaml
ms.custom: horz-security
```

---

## Research Strategy (Generation Phase)

### Required Research (all services)
1. Workspace documentation review (semantic_search, grep_search, file_search)
2. Service documentation search: `mcp_microsoft_doc_microsoft_docs_search`
3. Security baseline check: `/security/benchmark/azure/baselines/<service>-security-baseline`
4. WAF guide check: `/azure/well-architected/service-guides/<service>`
5. Anti-pattern research: what NOT to do with the service

### Conditional Research (when applicable)
- Reliability guide (for backup/DR)
- TLS/protocol security (if service exposes endpoints)
- Sub-resource security (if service has components like keys, secrets, certificates)
- Regulatory compliance (if compliance guidance exists)

---

## Validation Checklist (Key Points)

For each recommendation:
- [ ] Architecture compatibility (does service actually support this?)
- [ ] Feature verification (does feature exist AND work as described?)
- [ ] Link validation (resolves AND supports the claim?)
- [ ] Specificity check (service-specific, not generic Azure advice?)
- [ ] Link format correct (site-relative path, no full URLs)

### Issue Severity
- **CRITICAL:** Wrong architecture, hallucinated features → Remove/rewrite
- **HIGH:** Misleading specificity, broken links → Fix immediately
- **MEDIUM:** Incomplete context, weak links → Improve
- **LOW:** Style issues, minor improvements → Optional

---

## Anti-Pattern Rules

- Include **only when official Microsoft documentation describes them**
- Do NOT fabricate anti-patterns
- Place in "Appropriate use of \<Service\>" subsection within Service-Specific Security
- Must explain WHY it's inappropriate + provide alternative with link

---

## TLS/HTTPS Section

Include when service has:
- Specific TLS version requirements
- Protocol negotiation options
- Client compatibility considerations
- Multitenant architecture affecting TLS behavior

---

## Future Features (ideas/ folder)

### Security Coverage Auditor (Proposed)
Uses security articles as "policy" to audit entire docsets for consistency:
- Verifies procedural docs model security best practices
- Flags anti-patterns in quickstarts/tutorials
- Extracts machine-readable rules from security articles

---

## WSL2 Environment Notes

- Workspace runs on WSL2 (Linux) with VS Code on Windows
- Watch for CRLF line ending issues (causes `replace_string_in_file` failures)
- Check with: `head -3 <file> | cat -A` (look for `^M$`)
- Fix with: `sed -i 's/\r$//' <file>`

---

## Example Output Files

- `output/secure-azure-key-vault.md` — Key/Secret Management category (Service-specific first)
- `output/secure-azure-app-service.md` — Compute category (Network first)

---

## Quick Reference: Service Slug Rule

Convert `<Service>` to `<ServiceSlug>`:
1. Lowercase
2. Replace spaces with `-`
3. Remove non-alphanumeric except `-`

Example: "Azure Key Vault" → `azure-key-vault`  
Output file: `output/secure-azure-key-vault.md`
