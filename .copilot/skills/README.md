# Security Horizontal Copilot Skills

AI-assisted generation and auditing of "Secure your \<Service>" security articles for Microsoft Learn.

## Skills

| Skill | Purpose | Example Invocation |
|-------|---------|-------------------|
| **azure-security-article-generator** | Create new security articles | "Generate a security article for Azure Storage" |
| **azure-security-article-auditor** | Audit existing articles for accuracy + completeness | "Audit this security article" |
| **azure-docset-compliance-scanner** | Scan docset for security violations | "Scan the docset for compliance" |

---

## Workflow

### 1. Add the Service Docset to Your Workspace

Open VS Code and add the service docset folder to your workspace:
- `File > Add Folder to Workspace...`
- Select the docset folder (e.g., `azure-docs-pr/articles/key-vault/`)

This gives the skills access to existing documentation for research and linking.

### 2. Generate, Audit, or Scan

**To create a new article:**
```
Generate a security article for Azure Key Vault
```

The skill will:
- Research security content in the docset
- Search Microsoft Learn for additional guidance
- Create `secure-key-vault.md` in the appropriate location

**To audit an existing article:**
```
Audit this security article
```

Run in a **fresh chat session** to avoid bias. Validates accuracy, checks completeness against docset, finds gaps, applies fixes.

**To scan the docset for compliance:**
```
Scan the Key Vault docset for security compliance
```

Parses the security article into rules, scans all CLI commands / code samples / ARM templates in the docset, reports violations (e.g., missing `--enable-rbac-authorization`, use of deprecated access policies). Output goes to terminal or a markdown report file.

---

## Setup

### Option A: Clone This Repository

```bash
git clone https://github.com/msmbaldwin/security-horizontal-copilot.git

# Symlink skills to your global Copilot skills directory
ln -s "$(pwd)/security-horizontal-copilot/.copilot/skills/azure-security-article-generator" ~/.copilot/skills/
ln -s "$(pwd)/security-horizontal-copilot/.copilot/skills/azure-security-article-auditor" ~/.copilot/skills/
ln -s "$(pwd)/security-horizontal-copilot/.copilot/skills/azure-docset-compliance-scanner" ~/.copilot/skills/
```

### Option B: Copy Skills Directly

```bash
cp -r .copilot/skills/azure-* ~/.copilot/skills/
```

### Option C: Use from This Workspace

If you have this repository open in VS Code alongside the service docset, the skills are automatically available.

---

## Prerequisites

- **GitHub Copilot** with Agent Mode enabled
- **Microsoft Docs MCP Server** for documentation search
  - Install from [MicrosoftDocs/mcp](https://github.com/MicrosoftDocs/mcp)
  - Provides `mcp_microsoft_doc_microsoft_docs_search` tool
- **VS Code** with Copilot extension
- **Service docset** in workspace (for generation/auditing/scanning)

---

## Standard Section Order

All security articles follow this structure:

1. **Service-specific security** (optional — include only when needed)
2. **Network security**
3. **Identity and access management**
4. **Data protection**
5. **Logging and monitoring**
6. **Compliance and governance**
7. **Backup and recovery**

---

## Key Principles

### Audit vs Scan

| Skill | Focus | Output |
|-------|-------|--------|
| **auditor** | Is the security article accurate and complete? | Fixes applied to article |
| **scanner** | Does the docset model the security best practices? | Compliance report |

The **auditor** reviews the article itself (accuracy, links, gaps).
The **scanner** reviews procedural content (CLI commands, code samples, ARM templates) for violations.

### Source of Truth
Security articles are an **index**, not a repository. Every recommendation must link to supporting procedural documentation. Don't include guidance that isn't described in detail elsewhere.

### Anti-Patterns
Include "Do not" guidance **only when official Microsoft documentation describes it**. Never fabricate anti-patterns.

### Actionability
Include only recommendations that represent **customer choices**. Don't recommend features that are enforced by default or built into the architecture.

---

## Troubleshooting

**Skill not recognized?**
- Verify skill folders exist in `~/.copilot/skills/` or `.copilot/skills/` in your workspace
- Each skill folder must contain a `SKILL.md` file
- Restart VS Code to reload skills

**Microsoft Docs search not working?**
- Verify the Microsoft Docs MCP server is installed and running
- Check MCP server logs for connection issues

**Article not found?**
- Ensure the docset is added to your VS Code workspace
- Check for `secure-*.md` files in the docset

---

## Related Files

The `input/` directory contains the original prompt files used to create these skills:
- `prompt-generate.txt` — Generation instructions
- `prompt-validate.txt` — Validation checklist
- `prompt-audit.txt` — Audit and refresh process
- `template.txt` — Article structure
- `guide.txt` — Writing standards
