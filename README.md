# GitHub Copilot Security Horizontal draft generator

This process is used to generate a **rough draft** for the [Security Horizontal initiative](https://learn.microsoft.com/en-us/help/contribute/contribute-security-horizontal). The generated draft should be reviewed and refined—ideally in collaboration with the product manager (PM) team—to ensure all security best practices are correct, none are missing, and the documentation fully describes how to implement each best practice.

For full details, see [Writing for the Security Horizontal](https://learn.microsoft.com/en-us/help/contribute/contribute-security-horizontal).

## 🧰 Prerequisites

- GitHub Copilot (agent mode)
- Microsoft Docs plugin (with `mcp_microsoft_doc2_microsoft_docs_search` access)
- VS Code

For details on installing the Microsoft Docs MCP plugin, see [MicrosoftDocs/mcp](https://github.com/MicrosoftDocs/mcp).

## 📁 Project Structure

```
security-horizontal-copilot/
├── inputs/
│   ├── guide.txt        # Writing guidelines
│   ├── template.txt     # Article structure
│   └── prompt.txt       # Generation instructions
├── github-copilot-prompt.txt  # Starter Copilot prompt (you edit this per service)
├── outputs/             # Generated articles (you must create/touch these manually)
└── README.md
```

## 🚀 How to Generate a "Secure your <Service>" Article

### 1. Open the repo in VS Code

Make sure:
- GitHub Copilot is enabled
- You’re using **Copilot agent mode**
- You have the **Microsoft Docs plugin** installed and working

### 2. Load context files into tabs

Open the following files in your editor to give Copilot the right context:

- `inputs/guide.txt` – Tone, structure, and writing tips
- `inputs/template.txt` – Markdown structure for the article
- `inputs/prompt.txt` – Copilot instructions for generating the article

These files help Copilot maintain structure and style.

### 3. Create an empty output file

Before generating the article, **create (touch) an empty file** in the `outputs/` folder with this format:

```bash
touch outputs/secure-<service>.md
```

Replace `<service>` with the lowercase, dash-separated name of your Azure service. For example:

```bash
touch outputs/secure-azure-iot-hub.md
```

Then open this file in the editor — this is where Copilot will write the generated article.

### 4. Customize the Copilot prompt

Open `github-copilot-prompt.txt` and update it with the actual service name. For example:

```
<Service> = "Azure IoT Hub"

You are generating a "Secure your <Service>" article. Use the following input files from the workspace:

- inputs/prompt.txt (task instructions and generation steps)
- inputs/guide.txt (writing guidance and tone)
- inputs/template.txt (article structure)

Start by calling the Microsoft Docs plugin using:
- Plugin: `microsoft.docs.mcp`
- Function: `mcp_microsoft_doc2_microsoft_docs_search`
- Query: "<Service> security site:learn.microsoft.com"

Also check for a security baseline at:
https://learn.microsoft.com/en-us/security/benchmark/azure/

Use **relative Learn links** in the format `/azure/<service>/<article>` (not full URLs).

Search thoroughly across the entire docset — not just the “Security” node — to identify **every actionable, customer-relevant security recommendation**.

Generate the full article in Markdown and write it directly into this file. Complete all sections fully, including any service-specific guidance.
```

Paste that into the Copilot chat.

### 5. Review and refine

Once the draft is complete:

- Read it carefully
- Check that links are relative (`/azure/…`) and not full URLs
- Confirm it follows the structure and tone guidelines
- Optionally, ask Copilot to verify it against the Azure Security Baseline for that service

## ✅ Output Format

The generated file will be located in:

```bash
outputs/secure-<service>.md
```

This file will be formatted for Microsoft Learn, include consistent headers, and contain all actionable security recommendations found in the docset.

## 🧠 Tips

- Copilot can access all files in the workspace for context.
- The Microsoft Docs MCP plugin can help pull in deep security recommendations not immediately visible from the main overview or “Security” pages.
- The guidance in `guide.txt` helps make content AI-consumable for tools like Azure Copilot.

## 📬 Feedback

For feedback or improvements, please contact Matthew Baldwin (mbaldwin) directly, or create a pull request (PR) against this project.
