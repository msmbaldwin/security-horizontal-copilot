# Security Horizontal Article Audit

**Date:** March 31, 2026  
**Source:** Articles with `ms.custom: horz-security` metadata tag

---

## 1. Articles Following the Rubric ✅

These follow the "Secure your \<Service>" pattern with holistic security coverage across domains (Network, Identity, Data, Logging, Compliance, Backup):

| Service | URL |
|---------|-----|
| Azure Key Vault | https://learn.microsoft.com/en-us/azure/key-vault/general/secure-key-vault |
| Azure Key Vault Keys | https://learn.microsoft.com/en-us/azure/key-vault/keys/secure-keys |
| Azure Key Vault Secrets | https://learn.microsoft.com/en-us/azure/key-vault/secrets/secure-secrets |
| Azure Key Vault Certificates | https://learn.microsoft.com/en-us/azure/key-vault/certificates/secure-certificates |
| Azure Managed HSM | https://learn.microsoft.com/en-us/azure/key-vault/managed-hsm/secure-managed-hsm |
| Azure Cloud HSM | https://learn.microsoft.com/en-us/azure/cloud-hsm/secure-cloud-hsm |
| Azure Dedicated HSM | https://learn.microsoft.com/en-us/azure/dedicated-hsm/secure-dedicated-hsm |
| Azure Payment HSM | https://learn.microsoft.com/en-us/azure/payment-hsm/secure-payment-hsm |
| Azure Cosmos DB (NoSQL) | https://learn.microsoft.com/en-us/azure/cosmos-db/security |
| Azure Cosmos DB (Gremlin) | https://learn.microsoft.com/en-us/azure/cosmos-db/gremlin/security |
| Cosmos DB in Microsoft Fabric | https://learn.microsoft.com/en-us/fabric/database/cosmos-db/security |
| Azure Container Apps | https://learn.microsoft.com/en-us/azure/container-apps/secure-deployment |
| Azure Confidential Ledger | https://learn.microsoft.com/en-us/azure/confidential-ledger/secure-confidential-ledger |
| Azure Monitor | https://learn.microsoft.com/en-us/azure/azure-monitor/best-practices-security |
| Azure App Configuration | https://learn.microsoft.com/en-us/azure/azure-app-configuration/secure-azure-app-configuration |
| Azure Application Gateway | https://learn.microsoft.com/en-us/azure/application-gateway/secure-application-gateway |
| Azure Data Factory | https://learn.microsoft.com/en-us/azure/data-factory/secure-your-azure-data-factory |
| Microsoft Fabric Data Factory | https://learn.microsoft.com/en-us/fabric/data-factory/secure-data-factory-in-microsoft-fabric |
| SQL Server | https://learn.microsoft.com/en-us/sql/relational-databases/security/secure-sql-server |
| Azure SQL Database | https://learn.microsoft.com/en-us/azure/azure-sql/database/secure-database |
| Azure SQL Managed Instance | https://learn.microsoft.com/en-us/azure/sql/managed-instance/secure-managed-instance |
| Azure DevOps (main) | https://learn.microsoft.com/en-us/azure/devops/organizations/security/security-overview |
| Azure Boards | https://learn.microsoft.com/en-us/azure/devops/boards/secure-your-azure-boards |
| Azure DevOps Reporting | https://learn.microsoft.com/en-us/azure/devops/report/secure-your-analytics-reporting |
| Power Query | https://learn.microsoft.com/en-us/power-query/security-best-practices-power-query |
| Data API Builder | https://learn.microsoft.com/en-us/azure/data-api-builder/concept/security/ |
| Azure Managed Grafana | https://learn.microsoft.com/en-us/azure/managed-grafana/secure-azure-managed-grafana |
| Azure IoT Hub | https://learn.microsoft.com/en-us/azure/iot-hub/secure-azure-iot-hub |
| Azure Firewall | https://learn.microsoft.com/en-us/azure/firewall/secure-firewall |
| Azure Front Door | https://learn.microsoft.com/en-us/azure/frontdoor/secure-front-door |
| Azure Load Balancer | https://learn.microsoft.com/en-us/azure/load-balancer/secure-load-balancer |
| Azure Virtual Network | https://learn.microsoft.com/en-us/azure/virtual-network/secure-virtual-network |
| Azure VPN Gateway | https://learn.microsoft.com/en-us/azure/vpn-gateway/secure-vpn-gateway |
| Azure Virtual WAN | https://learn.microsoft.com/en-us/azure/virtual-wan/secure-virtual-wan |
| Azure Web Application Firewall | https://learn.microsoft.com/en-us/azure/web-application-firewall/secure-web-application-firewall |
| Azure DNS | https://learn.microsoft.com/en-us/azure/dns/secure-dns |
| Azure ExpressRoute | https://learn.microsoft.com/en-us/azure/expressroute/secure-expressroute |
| Azure Attestation | https://learn.microsoft.com/en-us/azure/attestation/secure-attestation |
| Azure AI Translator | https://learn.microsoft.com/en-us/azure/ai-services/translator/secure-deployment |
| Azure AI Video Indexer | https://learn.microsoft.com/en-us/azure/azure-video-indexer/security-baseline-video-indexer |
| Azure IoT (solutions) | https://learn.microsoft.com/en-us/azure/iot/iot-overview-security |
| Azure Database for MySQL | https://learn.microsoft.com/en-us/azure/mysql/flexible-server/security-overview |

**Total: ~40 articles**

---

## 2. NOT "Secure Your Service" Articles ❌

These have the `horz-security` tag but are granular topics, how-tos, overviews, or feature-specific documentation — not holistic security checklists:

### PostgreSQL Security Folder
Entire folder is granular security topics, not a holistic checklist:

| Article | Why It Doesn't Fit |
|---------|-------------------|
| security-access-control | Role management how-to |
| security-audit | pgaudit extension setup |
| security-audit-entra | Entra audit specifics |
| security-azure-policy | Azure Policy topic |
| security-configure-data-encryption | Encryption how-to |
| security-connect-scram | SCRAM auth topic |
| security-data-encryption | Encryption concepts |
| security-defender-for-cloud | Defender setup |
| security-managed-database-users | User management |
| security-managed-identity-overview | MSI overview |
| security-entra-concepts | Conceptual (not checklist) |
| security-entra-authentication | Auth topic |
| security-reset-admin-password | How-to |
| security-confidential-computing | Feature topic |
| security-compliance | Compliance overview |

### MySQL Flexible Server Security Folder
Granular security topics:

| Article | Why It Doesn't Fit |
|---------|-------------------|
| security-tls-root-certificate-rotation | Certificate rotation how-to |
| security-how-to-connect | Connection how-to |
| security-customer-managed-key | CMK setup |
| security-tls-how-to-connect | TLS connection how-to |
| security-tls-root-certificate-rotation-faq | FAQ |
| security-how-to-create-users | User creation how-to |
| security-entra-authentication | Entra auth topic |
| security-how-to-data-encryption-cli | CLI how-to |
| security-how-to-data-encryption-portal | Portal how-to |
| security-how-to-entra | Entra how-to |
| security-configure-managed-identities-system-assigned | MSI how-to |
| security-configure-managed-identities-user-assigned | MSI how-to |
| security-tls | TLS concepts |
| security-entra-configure | Config how-to |

**Total: ~30 articles that should NOT have the tag**

---

## 3. Potentially MISSING Articles 🔍

Major Azure services that should have "Secure your \<Service>" articles but didn't appear in the tagged list:

| Service | Priority | Notes |
|---------|----------|-------|
| **Azure Storage** | High | Foundational service, widely used |
| **Azure Kubernetes Service (AKS)** | High | Major compute platform |
| **Azure Container Registry** | High | Often paired with AKS/Container Apps |
| **Azure Service Bus** | High | Core messaging service |
| **Azure Event Hubs** | High | Event streaming |
| **Azure Event Grid** | Medium | Event routing |
| **Azure OpenAI Service** | High | High-profile AI service |
| **Azure Cognitive Services** (most) | Medium | Only Translator is covered |
| **Azure API Management** | High | API gateway service |
| **Azure Logic Apps** | Medium | Integration service |
| **Azure Service Fabric** | Medium | Compute platform |
| **Azure Databricks** | High | Analytics platform |
| **Azure Synapse Analytics** | High | Analytics service |
| **Azure HDInsight** | Medium | Big data service |
| **Microsoft Defender for Cloud** | Medium | Security service itself |
| **Microsoft Sentinel** | Medium | SIEM service |
| **Azure App Service** | High | Already exists? Check tag |
| **Azure Functions** | High | Serverless compute |

**Total: ~15+ major services potentially missing coverage**

---

## Summary

| Category | Count |
|----------|-------|
| ✅ Following rubric | ~39 |
| ❌ Mistagged (granular topics) | ~30 |
| 🔍 Potentially missing | ~15+ |

### Recommendations

1. **Remove `horz-security` tag** from PostgreSQL and MySQL granular security topics, or create a proper "Secure your PostgreSQL/MySQL" umbrella article that links to them.

2. **Prioritize creation** of articles for:
   - Azure Storage (foundational)
   - Azure Kubernetes Service (high adoption)
   - Azure OpenAI Service (high visibility)
   - Azure Service Bus/Event Hubs (messaging backbone)

3. **Audit existing articles** for compliance with the rubric:
   - Standard section order
   - Zero Trust banner placement
   - Actionable recommendations with links
