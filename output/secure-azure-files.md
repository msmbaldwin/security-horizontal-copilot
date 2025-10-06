---
title: Secure your Azure Files deployment
description: Learn how to secure Azure Files, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-files
ms.topic: conceptual
ms.custom: horz-security
ms.date: 10/06/2025
ai-usage: ai-assisted
---

# Secure your Azure Files deployment

Azure Files provides fully managed file shares in the cloud, accessible via SMB and NFS protocols, for mounting to clients in the cloud, on-premises, or both. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

[!INCLUDE [Security horizontal Zero Trust statement](~/reusable-content/ce-skilling/azure/includes/security/zero-trust-security-horizontal.md)]

This article provides security recommendations to help protect your Azure Files deployment.

## Network security

Network security controls help prevent unauthorized access to Azure Files and ensure that data is only accessible from trusted networks and endpoints.

- **Enable private endpoints for storage accounts**: Eliminate public internet exposure by routing traffic through your virtual network using Azure Private Link. Private endpoints provide dedicated network interfaces with private IP addresses that connect directly to Azure Files, reducing risk of data exfiltration and ensuring traffic stays within your virtual network boundaries. See [Configuring private endpoints for Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-networking-endpoints#create-a-private-endpoint).

- **Restrict public network access to storage accounts**: Configure the storage account firewall to deny public access by default or limit access to specific virtual networks and IP address ranges. This minimizes the attack surface and enforces network isolation by requiring traffic to originate from approved sources. See [Restrict access to the public endpoint](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-networking-endpoints#restrict-public-endpoint-access).

- **Configure storage account firewall rules**: Enable firewall rules to allow access only from specific virtual networks, subnets, and IP address ranges. Start with "deny all" as the default action and incrementally grant the least required access through virtual network rules or IP network rules. See [Configure Azure Storage firewalls and virtual networks](https://learn.microsoft.com/en-us/azure/storage/common/storage-network-security).

- **Disable SMBv1 on clients**: SMBv1 is insecure, deprecated, and not supported by Azure Files which only accepts SMBv2.1 and higher. Ensure all clients use SMBv2.1 or SMBv3 to connect by disabling SMBv1 on client operating systems. See [Disable SMB 1 on Linux clients](https://learn.microsoft.com/en-us/azure/storage/files/files-remove-smb1-linux).

## Identity and access management

Identity and access management controls ensure only authorized users and applications can access Azure Files, enforcing least privilege and strong authentication.

- **Enable identity-based authentication for SMB shares**: Configure Azure Files to integrate with Microsoft Entra ID, Microsoft Entra Domain Services, or on-premises Active Directory Domain Services to enable Kerberos-based authentication. This reduces reliance on storage account keys, improves auditability, and enables granular access control at both share and directory/file levels. See [Overview of Azure Files identity-based authentication options for SMB access](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-active-directory-overview).

- **Configure Azure RBAC roles for share-level permissions**: Assign built-in Azure RBAC roles such as Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, and Storage File Data SMB Share Elevated Contributor to control share-level access. Apply roles based on least privilege principles to Microsoft Entra users or groups. See [Assign share-level permissions for Azure file shares](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-identity-assign-share-level-permissions).

- **Secure and rotate storage account access keys**: If using access keys is required, store them in Azure Key Vault or another secure key management solution, rotate them regularly, and audit their usage through logging. Minimize key distribution and prefer identity-based authentication over shared keys wherever possible. See [Manage storage account access keys](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage).

## Data protection

Data protection controls safeguard your data from unauthorized access, accidental deletion, and ensure compliance with regulatory requirements.

- **Configure encryption at rest for file shares**: Azure Files automatically encrypts all data at rest using AES-256 encryption with Microsoft-managed keys by default. For enhanced control over key management and rotation, configure customer-managed keys stored in Azure Key Vault, which allows you to control access and audit key usage. See [Azure Storage encryption for data at rest](https://learn.microsoft.com/en-us/azure/storage/common/storage-service-encryption).

- **Enable encryption in transit for NFS shares**: Protect data in transit by configuring TLS encryption for NFS 4.1 file shares using the AZNFS utility package and Stunnel. This prevents interception and tampering during transmission using AES-GCM encryption without requiring Kerberos authentication. See [Encryption in transit for NFS Azure file shares](https://learn.microsoft.com/en-us/azure/storage/files/encryption-in-transit-for-nfs-shares).

- **Enable soft delete for file shares**: Configure soft delete at the storage account level to protect against accidental or malicious deletion of file shares. Deleted shares are retained in a recoverable state for the specified retention period (1-365 days), allowing restoration of both the share and its contents including snapshots. See [Enable soft delete on Azure file shares](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-enable-soft-delete).

## Logging and monitoring

Logging and monitoring controls provide visibility into access, usage, and potential threats, enabling timely detection and response to security incidents.

- **Configure diagnostic settings for Azure Files monitoring**: Create diagnostic settings to collect Azure Files resource logs and metrics, routing them to Log Analytics workspaces for analysis. This enables monitoring of access patterns, performance metrics, authentication events, and error conditions for security investigations. See [Monitor Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-monitoring).

- **Enable Microsoft Defender for Storage**: Activate advanced threat protection to detect malware scanning, activity monitoring, and sensitive data threat detection for storage accounts hosting Azure Files. Defender for Storage provides contextual security alerts for unusual access patterns, potential data exfiltration, and malicious file uploads. See [What is Microsoft Defender for Storage](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-storage-introduction).

- **Create Azure Monitor alerts for critical events**: Configure metric and log-based alerts for security-relevant events such as file share deletion, unauthorized access attempts, unusual access patterns, and storage account key usage. Define appropriate alert rules with notification actions to enable rapid incident response. See [Create monitoring alerts for Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/files-monitoring-alerts).

## Compliance and governance

Compliance and governance controls help ensure your Azure Files deployment meets regulatory requirements and organizational policies.

- **Apply resource locks to storage accounts**: Prevent accidental or malicious deletion of storage accounts by applying Azure Resource Manager locks. This protects critical data and resources. See [Lock a storage account resource](https://learn.microsoft.com/en-us/azure/storage/common/lock-account-resource).

- **Use Azure Policy to enforce security configurations**: Define and assign Azure Policy to enforce required security settings, such as encryption, network rules, and identity configurations. See [Azure Policy for storage security](https://learn.microsoft.com/en-us/azure/governance/policy/samples/storage-security-policy).

- **Review regulatory compliance with Microsoft Defender for Cloud**: Monitor compliance status and remediate issues using built-in regulatory compliance controls. See [Regulatory compliance in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/regulatory-compliance).

## Backup and recovery

Backup and recovery controls ensure business continuity and rapid restoration of data in case of accidental deletion, corruption, or disaster.

- **Configure Azure Backup for file shares**: Protect file shares using Azure Backup with snapshot-based or vaulted backup tiers. Snapshot backups provide local protection against accidental deletion, while vaulted backups offer comprehensive offsite data protection stored in Recovery Services vaults with retention up to 10 years. See [About Azure Files backup](https://learn.microsoft.com/en-us/azure/backup/azure-file-share-backup-overview).

- **Select appropriate storage redundancy for disaster recovery**: Choose storage redundancy options including locally redundant storage (LRS), zone-redundant storage (ZRS), geo-redundant storage (GRS), or geo-zone-redundant storage (GZRS) based on your availability and disaster recovery requirements. Consider cross-region replication for critical workloads. See [Azure Files redundancy](https://learn.microsoft.com/en-us/azure/storage/files/files-redundancy).

- **Establish disaster recovery testing procedures**: Regularly test backup restoration and failover processes, including cross-region failover scenarios for geo-redundant storage accounts. Document recovery time objectives (RTO) and recovery point objectives (RPO), and maintain updated disaster recovery runbooks. See [Disaster recovery and failover for Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/files-disaster-recovery).

## Service-specific security

Azure Files provides unique security features and considerations for both SMB and NFS file shares.

- **Secure NFS shares with network-level controls**: NFS shares require network-based authentication and must use either private endpoints or service endpoints to ensure secure access. Configure storage account firewalls to restrict access to specific virtual networks, as NFS shares cannot be accessed via the public endpoint without service endpoints. See [NFS Azure file shares - Security and networking](https://learn.microsoft.com/en-us/azure/storage/files/files-nfs-protocol#security-and-networking).

- **Implement Azure File Sync security for hybrid deployments**: Configure Azure File Sync to securely extend on-premises file servers to Azure Files, ensuring encrypted synchronization and centralized backup. Follow disaster recovery best practices including cloud-based backup strategies, appropriate data redundancy, and failover procedures for both storage accounts and Storage Sync Services. See [Disaster recovery best practices with Azure File Sync](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-disaster-recovery-best-practices).

## Next steps

- [Microsoft Cloud Security Benchmark – Azure Storage](https://learn.microsoft.com/en-us/security/benchmark/azure/baselines/storage-security-baseline)
- [Well-Architected Framework – Azure Files guide](https://learn.microsoft.com/en-us/azure/well-architected/service-guides/azure-files)
- [Azure Files data protection overview](https://learn.microsoft.com/en-us/azure/storage/files/files-data-protection-overview)
