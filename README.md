# GitHub Copilot – Security Horizontal Draft Generator

This project generates **initial draft content** for Microsoft Learn's [Security Horizontal initiative](/help/contribute/contribute-security-horizontal). The output is a structured "Secure your <Service>" article that consolidates actionable, service-specific security guidance for Azure services.

> **Note:** Generated drafts follow Microsoft Learn standards but should be reviewed with the service PM team to ensure completeness and technical accuracy.

## 🧰 Prerequisites

- **GitHub Copilot** (Agent Mode enabled)
- **Microsoft Docs Plugin** (`mcp_microsoft_doc_microsoft_docs_search` access)
- **VS Code** with Copilot extension
- **Access to service docset** (optional but recommended for workspace analysis)

**Install the Docs plugin:** See [MicrosoftDocs/mcp](https://github.com/MicrosoftDocs/mcp)

---

## 📁 Project Structure

```
security-horizontal-copilot/
├── input/
│   ├── prompt.txt       # Generation instructions and research strategy
│   ├── template.txt     # Article structure and Markdown template
│   └── guide.txt        # Writing standards and formatting requirements
├── output/              # Generated security articles
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

**Important workspace setup:**
1. Open the root folder of this repository in VS Code (`File > Open Folder...` and select the `security-horizontal-copilot` folder). This ensures Copilot can correctly find the `input` and `output` directories.

2. **Optional but recommended**: Add the service docset to your workspace for enhanced local analysis:
   - If you have access to a local copy of the service documentation (e.g., cloned from `azure-docs-pr` or similar repository)
   - Use `File > Add Folder to Workspace...` to add the docset folder
   - This enables Copilot to analyze existing security documentation and configurations locally before searching external sources

### 2. Generate Article

1. Open GitHub Copilot Chat in VS Code
2. Paste the contents of `prompt.txt` into the chat window
3. Modify the Service Configuration variables directly in the chat window before sending:
  ```txt
  **Service Configuration:**
  - `<Service>` = "Azure App Service" (Replace with actual Azure service name)
  - `<Docset>` = https://learn.microsoft.com/en-us/azure/app-service (Replace with service docset URL)
  ```
4. Send your modified prompt to GitHub Copilot

**Copilot will automatically:**
- Explore workspace folders for local documentation and security content
- Analyze existing service documentation and configurations in the workspace
- Search Microsoft Learn for additional service-specific security guidance
- Follow the template structure from `template.txt`
- Apply writing standards from `guide.txt`
- Generate a properly formatted article saved to `output/secure-<ServiceSlug>.md` (e.g., `secure-app-service.md`)

> **Note:** I use Claude Sonnet 3.7 to generate my security articles, but you can use any AI model that works well for you. If you find a model that produces especially good results, please share that feedback.

### 3. Review and Validate

After generation, verify:

- **Link accuracy**: All links work and point to real, live articles on https://learn.microsoft.com.
- **Service specificity**: All recommendations apply directly to the target service
- **Evidence support**: Every recommendation links to authoritative Microsoft Learn documentation
- **Format consistency**: Bullet points follow the exact pattern from `guide.txt`
- **Domain coverage**: All applicable security domains are included and properly ordered

### 4. Convert Links to Relative Paths

For better compatibility with Microsoft Learn standards, convert all full URLs to relative paths using GitHub Copilot:

1. Open the generated file in VS Code
2. Open GitHub Copilot Chat
3. Use this prompt:

```markdown
Please convert all full Microsoft Learn URLs in this document to relative paths. 
For example, change:
- https://learn.microsoft.com/azure/service/feature to /azure/service/feature
- https://learn.microsoft.com/security/benchmark to /security/benchmark
- https://learn.microsoft.com/entra/identity to /entra/identity

Make sure to preserve all link text and maintain the exact same markdown formatting.
```

This step ensures the links follow Microsoft Learn's recommended format for internal references.

### 5. Loop in the service PM team

Once you have a quality draft, collaborate with the service PM team to finalize the content:

**IMPORTANT:** The generated article is only a **rough, first draft**. Collaborate with the service PM team to finalize the content:

- **Combine expertise**: Merge your security knowledge with the PM team's deep service expertise
- **Validate technical accuracy**: Confirm all implementation guidance is correct and follows current best practices
- **Identify gaps**: Discover any missing security recommendations specific to the service
- **Verify capabilities**: Ensure each security recommendation is actually supported by the service (not hallucinated)
- **Refine technical content**: Improve explanations based on service team feedback
- **Eliminate inaccuracies**: Address any AI-generated errors while maintaining quality standards
- **Prepare for publication**: Finalize the article according to Microsoft Learn requirements

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
- **Link formatting**: Initially uses full URLs (`https://learn.microsoft.com/azure/service/feature`) which are later converted to relative paths (`/azure/service/feature`)
- **Verified recommendations**: All guidance supported by current Azure capabilities
- **Domain-appropriate ordering**: Security sections ordered by foundational importance

---

## 🔧 Process Optimization Features

### Intelligent Research Strategy
- **Workspace-first approach**: Analyzes local documentation and configurations before external searches
- **Systematic search**: Automated Microsoft Learn searches using specific query patterns
- **Source prioritization**: Local workspace docs → Service docs → Security baselines → Architecture guides
- **Content validation**: Verification against current Microsoft documentation and workspace content

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
- **Workspace setup**: Add service docset folders to workspace for enhanced local analysis
- **Service specificity**: Use exact Azure service names for better research results
- **Documentation verification**: Let Copilot analyze workspace content first, then search external sources

### Generation Process
- **Single service focus**: Generate one service article at a time for best results
- **Iterative refinement**: Review and refine output for optimal quality
- **Template adherence**: Ensure generated content follows the exact template structure

### Quality Validation
- **Link verification**: Check that all documentation links are current and relevant
- **Specificity check**: Verify recommendations are service-specific, not generic
- **Workspace alignment**: Ensure recommendations align with existing configurations in workspace
- **Implementation testing**: Ensure recommendations are technically feasible

---

## 📞 Support and Contributions

- **Primary contact**: Matthew Baldwin (`mbaldwin`)
- **Contributions**: Pull requests welcome for enhancements to prompts or process
- **Documentation**: Updates to templates or guides should maintain distinct file roles
