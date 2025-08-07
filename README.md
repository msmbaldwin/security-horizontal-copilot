# GitHub Copilot – Security Horizontal Draft Generator

This project generates **publication-ready drafts** for Microsoft Learn's [Security Horizontal initiative](/help/contribute/contribute-security-horizontal). The output is a structured "Secure your <Service>" article that consolidates actionable, service-specific security guidance for Azure services.

> **Note:** Generated drafts follow Microsoft Learn standards but should be reviewed with the service PM team to ensure completeness and technical accuracy.

## 🧰 Prerequisites

- **GitHub Copilot** (Agent Mode enabled)
- **Microsoft Docs Plugin** (`mcp_microsoft_doc_microsoft_docs_search` access)
- **VS Code** with Copilot extension

**Install the Docs plugin:** See [MicrosoftDocs/mcp](https://github.com/MicrosoftDocs/mcp)

---

## 📁 Project Structure

```
security-horizontal-copilot/
├── inputs/
│   ├── prompt.txt       # Generation instructions and research strategy
│   ├── template.txt     # Article structure and Markdown template
│   └── guide.txt        # Writing standards and formatting requirements
├── outputs/             # Generated security articles
└── README.md
```

### File Roles and Functions

**Each file has a distinct, focused purpose:**

- **`prompt.txt`**: 
  - Core generation instructions and research methodology
  - Service configuration variables
  - Quality standards and execution process
  - Research strategy using Microsoft Docs plugin

- **`template.txt`**: 
  - Exact article structure and Markdown formatting
  - Placeholder patterns and usage guidelines  
  - Section templates with proper frontmatter
  - Link formatting and organizational structure

- **`guide.txt`**: 
  - Writing style, tone, and voice requirements
  - Content quality standards and specificity rules
  - Formatting requirements for bullets and links
  - Domain ordering logic and best practices

---

## 🚀 How to Generate Articles

### 1. Setup Environment

Ensure you have:
- GitHub Copilot enabled in VS Code with Agent Mode
- Microsoft Docs plugin installed and functional
- This repository open as your workspace

### 2. Load Context Files

Open these files in VS Code tabs to provide Copilot with full context:
```
inputs/prompt.txt    # Generation instructions and research strategy
inputs/template.txt  # Article structure and formatting template
inputs/guide.txt     # Writing standards and quality requirements
```

**Critical:** Copilot reads from open tabs, so having all three files loaded provides the complete generation context.

### 3. Configure Target Service

Edit `prompt.txt` and replace the service configuration variables:

```txt
**Service Configuration:**
- `<Service>` = "Azure App Service" (Replace with actual Azure service name)
- `<Docset>` = https://learn.microsoft.com/en-us/azure/app-service (Replace with service docset URL)
```

**Examples:**
- `<Service>` = "Azure Kubernetes Service"
- `<Service>` = "Azure SQL Database"  
- `<Service>` = "Azure IoT Hub"

### 4. Create Output File

Create a new file in the `outputs/` folder:
```bash
touch outputs/secure-<service-name>.md
```

**Naming convention:** Use lowercase with hyphens
- `secure-azure-iot-hub.md`
- `secure-app-service.md` 
- `secure-kubernetes-service.md`

**Important:** Open this file in VS Code - Copilot will write the article directly into the currently open file.

### 5. Generate Article

Copy the entire contents of `prompt.txt` and paste into Copilot Chat.

**Copilot will automatically:**
- Search Microsoft Learn for service-specific security guidance using the research strategy
- Follow the exact template structure from `template.txt`
- Apply writing standards and formatting from `guide.txt`
- Generate a complete, properly formatted article in your open file

### 6. Review and Validate

After generation, verify:
- **Link accuracy**: All links use relative paths (`/azure/...`) and point to current docs
- **Service specificity**: All recommendations apply directly to the target service
- **Evidence support**: Every recommendation links to authoritative Microsoft Learn documentation
- **Format consistency**: Bullet points follow the exact pattern from `guide.txt`
- **Domain coverage**: All applicable security domains are included and properly ordered

---

## ✅ Output Quality Standards

Generated articles automatically include:

### Content Quality
- **Service-specific focus**: Only recommendations that apply to the target service
- **Evidence-based guidance**: Every bullet point links to official Microsoft documentation  
- **Actionable instructions**: Clear implementation steps with security justifications
- **Current information**: Based on up-to-date Microsoft Learn content

### Structure and Formatting
- **Template compliance**: Follows exact structure from `template.txt`
- **Writing standards**: Applies tone, voice, and formatting from `guide.txt`
- **Microsoft Learn style**: Consistent with platform standards and conventions
- **AI-optimized organization**: Structured for optimal parsing by future AI systems

### Technical Standards
- **Authoritative sources**: Links only to Microsoft Learn documentation
- **Relative path formatting**: Uses `/azure/service/feature` link format
- **Verified recommendations**: All guidance supported by current Azure capabilities
- **Domain-appropriate ordering**: Security sections ordered by foundational importance

---

## 🔧 Process Optimization Features

### Intelligent Research Strategy
- **Systematic search**: Automated Microsoft Learn searches using specific query patterns
- **Source prioritization**: Service docs → Security baselines → Architecture guides
- **Content validation**: Verification against current Microsoft documentation

### Multi-Purpose Design
- **Human-readable**: Clear, actionable guidance for security engineers
- **AI-consumable**: Structured for optimal parsing by language models  
- **Search-optimized**: Includes relevant keywords and semantic markup
- **Standards-compliant**: Follows Microsoft Learn formatting and style requirements

### Quality Assurance Built-In
- **Evidence requirement**: Every recommendation must link to authoritative sources
- **Specificity filter**: Excludes generic security advice in favor of service-focused guidance
- **Currency validation**: Uses only current, active Microsoft Learn documentation
- **Implementation focus**: Provides actionable steps rather than theoretical concepts

---

## 💡 Best Practices for Success

### Context Management
- **Load all files**: Always open the three prompt files before generating
- **Service specificity**: Use exact Azure service names for better research results
- **Documentation verification**: Let Copilot search thoroughly using the Docs plugin

### Generation Process
- **Single service focus**: Generate one service article at a time for best results
- **Iterative refinement**: Review and refine output for optimal quality
- **Template adherence**: Ensure generated content follows the exact template structure

### Quality Validation
- **Link verification**: Check that all documentation links are current and relevant
- **Specificity check**: Verify recommendations are service-specific, not generic
- **Implementation testing**: Ensure recommendations are technically feasible

---

## 📞 Support and Contributions

- **Primary contact**: Matthew Baldwin (`msmbaldwin`)
- **Contributions**: Pull requests welcome for enhancements to prompts or process
- **Documentation**: Updates to templates or guides should maintain distinct file roles
