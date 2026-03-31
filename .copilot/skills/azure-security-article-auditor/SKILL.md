---
description: Audit "Secure your <Service>" security articles for accuracy, completeness, and currency. Use when: reviewing security articles, validating recommendations, checking for new features, refreshing stale articles, verifying source-of-truth compliance. Combines technical validation with docset gap analysis.
---

# Azure Security Article Auditor

Comprehensively audit security horizontal articles for technical accuracy, service specificity, completeness, and currency against the service docset.

## When to Use This Skill

Use this skill when asked to:
- "Audit this security article"
- "Validate the security article"
- "Review the security article for accuracy"
- "Check if the security article is up to date"
- "Refresh the security article against the docset"
- "Find gaps in the security documentation"

**IMPORTANT**: Run audits in a **fresh chat session** to avoid confirmation bias from generation.

## Prerequisites

**The user should have the service docset open in their workspace** containing the security article to audit.

## Core Principle: Source of Truth

Security articles are an **INDEX**, not a repository. They summarize and link to guidance that exists in the technical docs. No best practice should appear in a security article that isn't described in detail elsewhere.

## Finding the Article

1. If the user has a file open, audit that file
2. Otherwise, search the workspace for `secure-*.md` or `security-*.md` files
3. If multiple found, ask user which to audit
4. Extract the service name from the article title

---

## Phase 1: Docset Discovery

Before reviewing the article, understand the current state of the docset:

1. **List docset contents**: Use `list_dir` on the service docset folder
2. **Find security-related content**: Use `grep_search` for:
   - `security|encrypt|authenticat|firewall|private endpoint|RBAC|access control`
3. **Review TOC structure**: Read `toc.yml` to understand documentation organization
4. **Find recent additions**: Search for content newer than the security article's `ms.date`

### Document Capability Inventory

Create a checklist of security-relevant capabilities found in the docset:

- [ ] Network security features (private endpoints, firewalls, VNet integration)
- [ ] Identity features (managed identities, RBAC, authentication options)
- [ ] Data protection features (encryption, key management, soft delete)
- [ ] Monitoring features (logging, auditing, threat detection)
- [ ] Compliance features (policy, regulatory certifications)
- [ ] Backup/recovery features (HA, DR, backup procedures)

---

## Phase 2: Structural Validation

### Zero Trust Banner Placement

Verify the Zero Trust include statement is positioned correctly:
- Must appear **after all intro content, immediately before the first H2**
- Should NOT split intro paragraphs or paragraph+list combinations
- The banner marks the transition from introduction to technical guidance

**Checklist:**
- [ ] Zero Trust include is present
- [ ] Positioned right before the first H2 section
- [ ] Does not interrupt intro paragraph flow

### Standard Section Ordering

Verify the article uses the standard section order:

1. **Service-specific security** (optional — include only when needed)
2. **Network security**
3. **Identity and access management**
4. **Data protection**
5. **Logging and monitoring**
6. **Compliance and governance**
7. **Backup and recovery**

**When Service-specific security should be present:**
- The service has unique architectural considerations
- Official documentation describes anti-patterns or inappropriate uses
- The service has distinct security fundamentals not covered by standard domains

**Checklist:**
- [ ] Standard section order is followed
- [ ] Service-specific security section is present only when warranted
- [ ] TLS/HTTPS section included if service has specific protocol requirements
- [ ] Related security articles section included if sub-resource articles exist

### Anti-Pattern Validation

For any "Do not" recommendations in Service-Specific Security:

- [ ] Each anti-pattern is supported by official Microsoft documentation
- [ ] No fabricated anti-patterns (guidance not found in docs)
- [ ] Anti-patterns explain WHY the use is inappropriate
- [ ] Alternative approaches provided with valid links

**If anti-patterns exist without documentation support, REMOVE them.**

---

## Phase 3: Per-Recommendation Validation

For **each** recommendation, verify:

### Technical Accuracy
- [ ] **Architecture compatibility**: Does the service's deployment model support this control as described?
- [ ] **Feature verification**: Does this feature exist and work as described for this service specifically?
- [ ] **Context accuracy**: Are prerequisites, limitations, and implementation details correct?
- [ ] **Specificity check**: Is this service-specific guidance, not generic Azure advice?

### Actionability
- [ ] **Actionable choice**: Is this something the customer can actually choose to do?
  - Remove if feature is enforced by default with no option to disable
  - Remove if capability is built into architecture with no configuration needed
  - Remove if guidance is non-actionable (e.g., "Understand X" instead of "Configure X")

### Link Validation
- [ ] **Link exists**: Does the recommendation link to underlying documentation?
- [ ] **Link works**: Does the link resolve (no 404s)?
- [ ] **Link supports claim**: Does the linked doc actually describe this guidance?
- [ ] **Link format**: Uses site-relative path (`/azure/service/feature`), not full URL

### Factual Accuracy
- [ ] **Default values**: Are claims about defaults correct? (e.g., "enabled by default")
- [ ] **Behavior descriptions**: Does the feature work as described?
- [ ] **Version/protocol claims**: Are version numbers accurate? (e.g., TLS versions)
- [ ] **Role/permission claims**: Are role names and required permissions correct?

---

## Issue Severity Classification

### CRITICAL (Remove or completely rewrite)
- Wrong architecture assumptions (e.g., VNet deployment when service doesn't support it)
- Hallucinated features that don't exist
- Fundamentally incorrect implementation guidance
- Fabricated anti-patterns without documentation support
- Wrong section ordering

### HIGH (Significant rewrite needed)
- Vague or generic guidance that could apply to any Azure service
- Missing critical context about prerequisites or limitations
- Misleading descriptions of how the feature works
- Anti-patterns that overstate or misrepresent documentation

### MEDIUM (Fix required)
- Broken or incorrect documentation link
- Minor technical inaccuracies
- Link uses full URL instead of site-relative path
- Missing service-specific context

### LOW (Style/formatting)
- Formatting inconsistencies
- Minor wording improvements
- Style guide deviations

---

## Phase 4: Completeness & Gap Analysis

### Compare Article Against Docset

For each capability found in Phase 1:

1. **Is it covered?** Does the security article mention this capability?
2. **Is it accurate?** Does the description match the underlying documentation?
3. **Is it linked?** Does it link to the procedural/reference documentation?

### Missing Capabilities to Check

- [ ] New features added since article was last updated
- [ ] Features documented elsewhere but not in security article
- [ ] Sub-resource security (keys, secrets, certificates, partitions, etc.)
- [ ] Integration security (connections to other Azure services)
- [ ] Compliance certifications and regulatory guidance

### Security Domain Completeness

#### Network Security
- [ ] Inbound protection (private endpoints, IP restrictions, service endpoints)
- [ ] Outbound/backend protection (VNet integration, secure downstream connections)
- [ ] DDoS protection (for internet-facing services)

#### Identity
- [ ] Service authentication (managed identities for accessing other resources)
- [ ] User authentication (built-in auth providers, if applicable)
- [ ] Delegated/on-behalf-of authentication (if service supports it)
- [ ] Management plane RBAC (who can configure the service)

#### Data Protection
- [ ] Data at rest encryption
- [ ] Data in transit encryption
- [ ] Secrets management (Key Vault integration)
- [ ] Secure connections to backend/remote resources

#### Compliance
- [ ] Azure Policy enforcement
- [ ] Security posture assessment (Defender for Cloud)
- [ ] Penetration testing guidance (if applicable)
- [ ] Regulatory compliance (HIPAA, PCI, SOC if service has certifications)

#### Platform Security Context
- [ ] Shared responsibility clarity (Microsoft vs. customer)
- [ ] Built-in security features (auto-patching, isolation, encryption, threat protection)

**If any applicable area is missing, research and add it.**

---

## Phase 5: Common Issues to Look For

- **Networking recommendations assuming VNet deployment** when service doesn't support it
- **Security controls only available via specific mechanisms** (e.g., only via private endpoints) without stating this
- **Generic Azure features incorrectly described** as applying directly when they require specific patterns
- **Authentication/authorization methods** the service doesn't support or supports with limitations
- **Monitoring/logging capabilities** unavailable or working differently than described
- **Backup/disaster recovery features** that don't exist or have service-specific implementations
- **Anti-patterns presented as fact** without documentation support
- **Missing TLS/HTTPS section** when service has protocol requirements
- **Missing Related security articles section** when sub-resource articles exist
- **Stale content**: Article `ms.date` > 12 months without review

---

## Phase 6: Final Quality Checklist

Before completing the audit:

- [ ] No placeholder content remains
- [ ] Formatting is consistent throughout
- [ ] Links use site-relative paths (no `https://learn.microsoft.com`)
- [ ] **No `[!IMPORTANT]`, `[!NOTE]`, or other Markdown callouts**
- [ ] Content optimized for AI parsing
- [ ] Article structure matches template
- [ ] Section ordering follows standard order
- [ ] Anti-patterns included only when documentation supports them
- [ ] All docset capabilities are covered

---

## Execution

1. **Find the article**: Locate the security article in the workspace/docset
2. **Inventory docset**: Catalog all security-relevant content
3. **Verify structure**: Check standard section ordering
4. **Validate anti-patterns**: Confirm each "Do not" has documentation support
5. **Check each recommendation**: Verify accuracy, links, actionability
6. **Compare coverage**: Match docset capabilities to article content
7. **Fix issues in place**: Apply corrections directly to the article
8. **Report findings**: Summarize what was fixed and what needs human review

**Use `mcp_microsoft_doc_microsoft_docs_search`** to verify claims against current documentation.

**Reference MCSB v2 baselines** at `/security/benchmark/azure/baselines/<service>-security-baseline` when validating.
