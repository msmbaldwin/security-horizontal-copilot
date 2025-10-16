---
title: Secure your Azure Managed HSM deployment
description: Learn how to secure Azure Managed HSM, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: key-vault
ms.topic: conceptual
ms.custom: horz-security
ms.date: 10/07/2025
ai-usage: ai-assisted
---

# Secure your Azure Managed HSM deployment

Azure Managed HSM is a fully managed, highly available, single-tenant Hardware Security Module (HSM) service that provides FIPS 140-3 Level 3 validated cryptographic key protection for your cloud applications. Because Managed HSM safeguards your most sensitive cryptographic keys and secrets, implementing comprehensive security controls is essential to protect against threats and maintain business continuity.

[!INCLUDE [Security horizontal Zero Trust statement](~/reusable-content/ce-skilling/azure/includes/security/zero-trust-security-horizontal.md)]

This article provides security recommendations to help protect your Azure Managed HSM deployment.

## Service-specific security

Service-specific security controls are unique to Managed HSM and provide foundational protection for cryptographic operations, access isolation, and security domain management that form the core security architecture.

- **Implement multi-person control for security domain**: Configure a security domain quorum with multiple RSA key pairs (minimum 3 recommended) to prevent single-person control over HSM recovery. Specify a quorum threshold that requires multiple key holders to collaborate for security domain decryption, ensuring no single individual can compromise the HSM. The security domain contains all credentials needed to rebuild the HSM partition from scratch and uses Shamir's Secret Sharing algorithm to split key shares across customer-provided RSA public keys. See [Security domain overview](https://learn.microsoft.com/azure/key-vault/managed-hsm/security-domain).

- **Store security domain keys offline in secure locations**: Keep security domain private keys on encrypted, offline storage devices such as encrypted USB drives stored in separate geographical locations within physical safes or lock boxes. Never store security domain keys on internet-connected computers to reduce exposure to cyber threats and ensure air-gapped security. Without the security domain, disaster recovery is not possible and Microsoft cannot recover the security domain or access your keys. See [Security domain overview](https://learn.microsoft.com/azure/key-vault/managed-hsm/security-domain).

- **Establish security domain key management procedures**: Implement policies for periodic review of security domain key custody when personnel changes occur or when keys may be compromised. Document security domain holder responsibilities, maintain accurate records of key locations and custody, and ensure the quorum can be assembled for disaster recovery scenarios. The security domain must be downloaded before the service becomes live by providing RSA encryption keys to secure it. See [Security domain overview](https://learn.microsoft.com/azure/key-vault/managed-hsm/security-domain).

- **Enable purge protection for HSM and keys**: Configure purge protection to prevent permanent deletion of the HSM or individual keys before the retention period expires. This control protects against accidental or malicious deletion and provides a recovery window for critical operations. Note that purge protection cannot be disabled or overridden by anyone, including Microsoft, once enabled. See [Soft-delete overview](https://learn.microsoft.com/azure/key-vault/managed-hsm/soft-delete-overview).

## Network security

Network security protects your Managed HSM through secure connectivity and network access controls. Unlike services that deploy into virtual networks, Managed HSM is accessed through secure endpoints and supports private connectivity via Azure Private Link. Managed HSM does not support Virtual Network integration, Network Security Group rules, or Virtual Network Service Endpoints - network security is achieved through private endpoints and firewall configurations.

- **Deploy private endpoints using Azure Private Link**: Establish private, secured connectivity to your Managed HSM instance by creating a private endpoint in your virtual network. This prevents exposure to the public internet and routes all traffic over the Microsoft backbone network. See [Integrate Managed HSM with Azure Private Link](https://learn.microsoft.com/azure/key-vault/managed-hsm/private-link).

- **Disable public network access**: Prevent access from public IP addresses by configuring your Managed HSM to deny public network access. This setting works with private endpoints to ensure only private network traffic reaches your HSM. See [Integrate Managed HSM with Azure Private Link](https://learn.microsoft.com/azure/key-vault/managed-hsm/private-link).

- **Configure firewall rules for trusted services**: When you cannot disable public access entirely, use Managed HSM firewall rules to deny public internet access while allowing specific trusted Azure services through the `--bypass AzureServices` setting. This restricts the attack surface while maintaining necessary service integrations. Individual entities (such as Azure Storage accounts or Azure SQL Servers) still need specific role assignments to access keys even with trusted services bypass enabled. See [Integrate Managed HSM with Azure Private Link](https://learn.microsoft.com/azure/key-vault/managed-hsm/private-link).

## Identity and access management

Identity and access management secures authentication and authorization to your Managed HSM resources. Managed HSM uses a dual-plane access model with different authorization systems for control plane and data plane operations.

- **Implement Managed HSM local RBAC for data plane access**: Use Managed HSM local RBAC to control access to keys and cryptographic operations within the HSM. This authorization system operates independently from Azure RBAC and provides granular permissions for key operations, role assignments, and security domain management. See [Access control for Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/access-control).

- **Enable managed identities for application access**: Configure system-assigned or user-assigned managed identities for applications to authenticate to Managed HSM without storing credentials in code or configuration files. Managed identities integrate with Microsoft Entra ID and automatically handle credential rotation. See [Access control for Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/access-control).

- **Apply least-privilege access with appropriate scopes**: Grant permissions at the most restrictive scope necessary—either HSM-level (`/` or `/keys`) for broad access or key-level (`/keys/<key-name>`) for specific key access. Use built-in roles like Managed HSM Crypto Officer, Managed HSM Crypto User, or Managed HSM Crypto Auditor based on required operations. See [Access control for Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/access-control).

- **Assign HSM Administrator role to security groups**: Grant the HSM Administrator role to Microsoft Entra security groups instead of individual users to prevent accidental lockout if user accounts are deleted. This approach simplifies permission management and ensures continuity of administrative access during the HSM provisioning process. See [Access control for Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/access-control).

- **Enable Privileged Identity Management for administrative roles**: Use Microsoft Entra Privileged Identity Management (PIM) to enforce just-in-time access for highly privileged roles like Managed HSM Administrator. PIM reduces the risk of standing administrative privileges and provides approval workflows for elevated access. See [Access control for Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/access-control).

- **Separate control plane and data plane access**: Understand that control plane access (Azure RBAC) for managing HSM resources does not grant data plane access to keys. Explicitly assign data plane roles through Managed HSM local RBAC to users who need to perform key operations. See [Access control for Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/access-control).

## Data protection

Data protection safeguards cryptographic keys and sensitive data stored in Managed HSM through encryption, key management policies, and secure storage practices. Proper data protection ensures key material remains confidential and tamper-resistant.

- **Configure appropriate soft-delete retention periods**: Set soft-delete retention periods between 7 to 90 days based on your recovery requirements and compliance needs. The retention period can only be set when creating an HSM and cannot be changed later. The default retention period is 90 days if not specified. Longer retention periods provide more recovery time but may conflict with data residency requirements. See [Soft-delete overview](https://learn.microsoft.com/azure/key-vault/managed-hsm/soft-delete-overview).

- **Use HSM-protected key import (BYOK) for sensitive keys**: Import keys from on-premises FIPS 140-3 Level 3 HSMs using the bring-your-own-key (BYOK) process to maintain hardware-level protection throughout the key lifecycle. The key material never exists outside an HSM in plaintext form during the transfer process. During import, the key material is protected with a key held in the Managed HSM. See [HSM-protected keys BYOK](https://learn.microsoft.com/azure/key-vault/managed-hsm/hsm-protected-keys-byok).

- **Implement key rotation procedures**: Establish regular key rotation schedules based on organizational policies and compliance requirements. Use Managed HSM's key versioning capabilities to maintain access to historical key versions while ensuring current operations use the latest key material. Managed HSM supports automatic key rotation through Azure Policy enforcement of key expiration requirements. See [Key management](https://learn.microsoft.com/azure/key-vault/managed-hsm/key-management).

## Logging and monitoring

Logging and monitoring provide visibility into HSM access patterns and operations, enabling threat detection and compliance reporting. Comprehensive logging helps identify suspicious activity and supports forensic investigations.

- **Enable audit logging with diagnostic settings**: Configure diagnostic settings to capture all authenticated REST API requests, key operations, and security domain actions in the AzureDiagnostics table. Route logs to Azure Storage accounts, Log Analytics workspaces, or Event Hubs based on your retention and analysis requirements. Log information becomes available within 10 minutes (at most) after HSM operations, though typically sooner. See [Managed HSM logging](https://learn.microsoft.com/azure/key-vault/managed-hsm/logging).

- **Analyze logs with Azure Monitor and Log Analytics**: Use Azure Monitor to collect and analyze HSM logs through KQL queries that filter on ResourceProvider "MICROSOFT.KEYVAULT" and ResourceType "MANAGEDHSMS". Managed HSM logs are stored in the AzureDiagnostics table which stores logs for multiple services, so filtering is essential. Create custom dashboards and workbooks for security operations teams to monitor access patterns and key usage. See [Monitor Azure Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/logging-azure-monitor).

- **Configure alerts for critical security events**: Create Azure Monitor alert rules for events such as HSM availability drops below 100%, service API latency exceeding thresholds, unusual error code patterns, or failed authentication attempts. Configure both static threshold and dynamic threshold alerts to reduce false positives while maintaining security visibility. See [Configure Managed HSM alerts](https://learn.microsoft.com/azure/key-vault/managed-hsm/configure-alerts).

- **Integrate with Microsoft Sentinel for advanced threat detection**: Deploy Microsoft Sentinel to automatically detect suspicious activity using machine learning analytics and custom detection rules specific to Managed HSM operations. Create analytic rules for sensitive operations like security domain downloads, bulk key operations, or anomalous access patterns. See [Setting up Microsoft Sentinel for Azure Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/sentinel).

- **Implement proper log retention policies**: Establish log retention periods that meet compliance requirements and support forensic investigations. Use Azure Monitor Log Analytics retention policies to manage storage costs while maintaining adequate investigation capabilities for security incidents. See [Monitor Azure Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/logging-azure-monitor).

## Compliance and governance

Compliance and governance controls ensure your Managed HSM deployment meets regulatory requirements and organizational policies through automated policy enforcement and compliance monitoring.

- **Implement Azure Policy for key governance**: Grant the "Managed HSM Crypto Auditor" role to the "Azure Managed HSM Key Governance Service" (App ID: a1b76039-a76c-499f-a2dd-846b4cc32627) to enable Azure Policy compliance scanning of your complete key inventory. Without this role assignment, Azure Policy can only check compliance for new, updated, imported, and rotated keys, but cannot scan existing inventory keys for compliance reporting. A user with "Managed HSM Administrator" role must execute this role assignment. See [Integrate Azure Managed HSM with Azure Policy](https://learn.microsoft.com/azure/key-vault/managed-hsm/azure-policy).

- **Configure key lifecycle and cryptographic standards**: Use built-in Azure Policy definitions to enforce key expiration dates, mandate minimum RSA key sizes for security compliance, restrict elliptic curve cryptography to approved curve names (P-256, P-256K, P-384, P-521), and ensure keys have sufficient time before expiration for rotation procedures. Policy assignments require up to 30 minutes to enforce "Deny" effects and do not support management group level assignments. See [Integrate Azure Managed HSM with Azure Policy](https://learn.microsoft.com/azure/key-vault/managed-hsm/azure-policy).

- **Monitor compliance through Azure Policy dashboards**: Use Azure Policy compliance dashboards to track adherence to cryptographic security standards and identify non-compliant keys that require remediation. Set up both audit and deny policy effects to provide visibility and enforcement of security baselines. Compliance results appear after 12 hours, and Azure Policy compliance reports filter on the "Key Vault" category. See [Integrate Azure Managed HSM with Azure Policy](https://learn.microsoft.com/azure/key-vault/managed-hsm/azure-policy).

## Backup and recovery

Backup and recovery protects against data loss and enables business continuity through proper backup strategies, disaster recovery procedures, and recovery testing to ensure cryptographic keys remain accessible.

- **Create regular full HSM backups**: Schedule automated full HSM backups that include all keys, versions, attributes, tags, and role assignments to prevent data loss from hardware failures or operational incidents. Use user-assigned managed identities for backup operations to enable secure access to storage accounts without credential management. Full backup operations require "Managed HSM Administrator" or "Managed HSM Backup" role permissions. See [Full backup and restore](https://learn.microsoft.com/azure/key-vault/managed-hsm/backup-restore).

- **Implement individual key-level backups for critical keys**: Create selective backups of high-value keys using the `az keyvault key backup` command to enable granular recovery without full HSM restore operations. Key backups are encrypted and cryptographically tied to the security domain, meaning they can only be restored to HSMs sharing the same security domain. Individual key restore will not succeed if a key with the same name exists in active or deleted state. See [Full backup and restore](https://learn.microsoft.com/azure/key-vault/managed-hsm/backup-restore).

- **Prepare comprehensive disaster recovery procedures**: Develop and test disaster recovery plans that include security domain recovery using RSA key pairs, backup restoration procedures, and application reconfiguration steps. Ensure you maintain secure offline access to security domain files, private keys (minimum quorum), and recent backups stored in geographically separate locations. Full restore is a destructive operation that wipes all current HSM contents and requires a mandatory backup of the destination HSM at least 30 minutes before restore can be performed. See [Disaster recovery guide](https://learn.microsoft.com/azure/key-vault/managed-hsm/disaster-recovery-guide).

- **Test backup and restore procedures regularly**: Conduct periodic disaster recovery drills using non-production HSM instances to validate security domain recovery, backup restoration, and the complete disaster recovery workflow. Testing ensures security domain quorum holders can successfully execute recovery operations and verifies backup integrity. During restore operations, the HSM enters restore mode and all data plane commands (except checking restore status) are disabled. See [Disaster recovery guide](https://learn.microsoft.com/azure/key-vault/managed-hsm/disaster-recovery-guide).

- **Secure backup storage with proper access controls**: Store HSM backups in Azure Storage accounts configured with appropriate RBAC permissions, private endpoints, and customer-managed encryption keys. Configure the user-assigned managed identity with Storage Blob Data Contributor role and implement backup retention policies that balance recovery requirements with storage costs. User-assigned managed identity method works even when storage accounts are behind private endpoints with trusted service bypass enabled. See [Full backup and restore](https://learn.microsoft.com/azure/key-vault/managed-hsm/backup-restore).

## Next steps

- [Microsoft Cloud Security Benchmark – Key Vault](https://learn.microsoft.com/security/benchmark/azure/baselines/key-vault-managed-hsm-security-baseline)
- [Well-Architected Framework – Key Vault guide](https://learn.microsoft.com/azure/well-architected/service-guides/key-vault)
- [Security documentation for Azure Managed HSM](https://learn.microsoft.com/azure/key-vault/managed-hsm/)