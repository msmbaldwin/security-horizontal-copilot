---
title: Secure your Azure SQL Managed Instance deployment
description: Learn how to secure Azure SQL Managed Instance, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: sql-managed-instance
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure SQL Managed Instance deployment

Azure SQL Managed Instance provides a fully managed SQL Server database engine instance in the Azure cloud, with near 100% compatibility with on-premises SQL Server. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure SQL Managed Instance deployment.

## Network security

Azure SQL Managed Instance is deployed within its own virtual network, providing network isolation by default. Properly configuring network access is critical to protect your instance from unauthorized access.

- **Use private endpoints**: Establish secure connectivity to your SQL Managed Instance using Azure Private Link, which creates a private endpoint in your virtual network and eliminates public network exposure. See [Azure Private Link for Azure SQL Managed Instance](/azure/azure-sql/managed-instance/private-endpoint-overview?view=azuresql).

- **Disable public endpoint access**: Limit exposure by disabling public endpoint access and ensuring access is only through the private VNet endpoint. This significantly reduces the attack surface of your SQL Managed Instance. See [Use Azure SQL Managed Instance securely with public endpoints](/azure/azure-sql/managed-instance/public-endpoint-overview?view=azuresql).

- **Implement Network Security Groups**: Configure Network Security Groups (NSGs) to restrict traffic to and from the SQL Managed Instance subnet. Allow only necessary ports and IP ranges to access your instance. See [VNet architecture for managed instances](/azure/azure-sql/managed-instance/connectivity-architecture-overview?view=azuresql).

- **Use ExpressRoute or VPN for on-premises connectivity**: When connecting from on-premises networks, use Azure ExpressRoute or VPN Gateway to establish secure, private connections rather than routing traffic over the public internet. See [Connect your application to a managed instance](/azure/azure-sql/managed-instance/connect-application-instance?view=azuresql).

- **Configure secure network topology**: Implement a hub-and-spoke network topology where SQL Managed Instance resides in a dedicated subnet with proper route tables and NSGs to control traffic flow. See [Security isolation](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview?view=azuresql#security-isolation).

## Identity and access management

Implementing proper authentication and authorization mechanisms is essential to ensure only authorized users and applications can access your SQL Managed Instance.

- **Enable Microsoft Entra authentication**: Use Microsoft Entra ID (formerly Azure AD) for centralized identity management, allowing you to manage database users centrally and enforce multi-factor authentication. See [Configure and manage Microsoft Entra authentication with Azure SQL](/azure/azure-sql/database/authentication-aad-configure?view=azuresql).

- **Assign appropriate permissions**: Follow the principle of least privilege by granting users only the permissions they need to perform their specific tasks. Avoid assigning broad server-level roles when database-level permissions are sufficient. See [Authorization](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview?view=azuresql#authorization).

- **Create Microsoft Entra groups for role assignment**: Use Microsoft Entra groups to manage permissions at scale. Assign database roles to groups rather than individual users to simplify access management. See [Central management for identities](/azure/azure-sql/database/security-best-practice?view=azuresql#central-management-for-identities).

- **Assign Directory Readers role**: Ensure your SQL Managed Instance has the Directory Readers role permissions to read Microsoft Entra ID for scenarios like authorizing users who connect through security group membership. See [Assign Microsoft Graph permissions](/azure/azure-sql/database/authentication-aad-configure?view=azuresql#assign-microsoft-graph-permissions).

- **Use managed identities for applications**: Configure your applications to use managed identities when connecting to SQL Managed Instance, eliminating the need to manage credentials in your code or configuration files. See [Using managed identity authentication](/sql/connect/ado-net/sql/azure-active-directory-authentication?view=sql-server-ver17#using-managed-identity-authentication).

## Data protection

Protecting data at rest, in transit, and during processing is crucial for maintaining confidentiality and integrity of your information in SQL Managed Instance.

- **Enable Transparent Data Encryption (TDE)**: Ensure TDE is enabled to protect your data at rest by encrypting database files, transaction logs, and backups. This protects against unauthorized access to the physical storage media. See [Transparent data encryption for SQL Database, SQL Managed Instance, and Azure Synapse Analytics](/azure/azure-sql/database/transparent-data-encryption-tde-overview?view=azuresql).

- **Use customer-managed keys with Azure Key Vault**: Consider using customer-managed keys (BYOK) in Azure Key Vault for TDE to maintain control over your encryption keys and enable centralized key management. See [Key management with Azure Key Vault](/azure/azure-sql/database/security-overview?view=azuresql#key-management-with-azure-key-vault).

- **Implement Always Encrypted for sensitive data**: Use Always Encrypted to protect sensitive data such as credit card numbers or personal identifiers, ensuring the data remains encrypted even during processing and is only decrypted by authorized client applications. See [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver17).

- **Configure Dynamic Data Masking**: Implement dynamic data masking to limit exposure of sensitive data to non-privileged users by masking the data in query results without changing the underlying stored data. See [Dynamic data masking](/sql/relational-databases/security/dynamic-data-masking).

- **Enable Row-Level Security**: Implement row-level security to control access to rows in database tables based on user characteristics, ensuring users can only access data relevant to their role or department. See [Row-level security](/sql/relational-databases/security/row-level-security).

## Logging and monitoring

Comprehensive logging and monitoring are essential for detecting and responding to security threats, as well as meeting compliance requirements.

- **Configure auditing**: Enable SQL Managed Instance auditing to track database events and write them to an audit log in your Azure storage account. This helps maintain regulatory compliance and detect potential security violations. See [Get started with Azure SQL Managed Instance auditing](/azure/azure-sql/managed-instance/auditing-configure?view=azuresql).

- **Enable Advanced Threat Protection**: Implement Microsoft Defender for SQL to detect unusual behavior and potentially harmful attempts to access your databases. This provides alerts for suspicious activities like SQL injection attacks, credential theft, and anomalous access patterns. See [Advanced Threat Protection](/azure/azure-sql/database/threat-detection-overview?view=azuresql#alerts).

- **Set up diagnostic settings**: Configure diagnostic settings to send logs to Azure Monitor Logs or Event Hubs for advanced analysis and alerting capabilities. This enables comprehensive monitoring of your SQL Managed Instance. See [Set up auditing for your server to Event Hubs or Azure Monitor logs](/azure/azure-sql/managed-instance/auditing-configure?view=azuresql#set-up-auditing-for-your-server-to-event-hubs-or-azure-monitor-logs).

- **Implement custom audit configurations**: Configure auditing for specific types of actions and action groups to control the volume of audit logs and focus on critical security events. See [Audit critical security events](/azure/azure-sql/database/security-best-practice?view=azuresql#audit-critical-security-events).

- **Secure audit logs**: Restrict access to the storage account containing audit logs to support separation of duties between database administrators and auditors. See [Secure audit logs](/azure/azure-sql/database/security-best-practice?view=azuresql#secure-audit-logs).

## Compliance and governance

Implement governance controls and compliance measures to ensure your SQL Managed Instance deployment meets organizational and regulatory requirements.

- **Implement data classification and discovery**: Use data discovery and classification capabilities to identify, classify, and label sensitive data in your databases, helping you meet data privacy standards and regulatory requirements. See [Data discovery and classification](/azure/azure-sql/database/security-overview?view=azuresql#data-discovery-and-classification).

- **Run vulnerability assessments regularly**: Enable vulnerability assessment to discover, track, and remediate potential database vulnerabilities. Schedule regular assessments to maintain a strong security posture. See [Vulnerability assessment](/azure/azure-sql/database/security-overview?view=azuresql#vulnerability-assessment).

- **Apply Azure Policy definitions**: Use Azure Policy to enforce security controls and compliance requirements for your SQL Managed Instance deployments. This ensures consistent security configurations across your environment. See [Azure Policy Regulatory Compliance controls for Azure SQL Database & SQL Managed Instance](/azure/azure-sql/database/security-controls-policy?view=azuresql).

- **Maintain TLS version compliance**: Ensure your SQL Managed Instance is configured to use TLS 1.2 or newer for all connections to protect data in transit. See [Transport Layer Security (Encryption-in-transit)](/azure/azure-sql/database/security-overview?view=azuresql#transport-layer-security-encryption-in-transit).

- **Review compliance certifications**: Regularly review the Azure compliance offerings relevant to SQL Managed Instance to ensure your deployment meets the necessary regulatory requirements. See [Microsoft Azure Trust Center](https://www.microsoft.com/trust-center/compliance/compliance-overview).

## Backup and recovery

Implementing robust backup and recovery strategies is essential for protecting your data and ensuring business continuity.

- **Configure long-term backup retention**: Extend the retention period for automated backups beyond the default to meet your business and compliance requirements. See [Automated backups](/azure/azure-sql/managed-instance/automated-backups-overview?view=azuresql).

- **Implement geo-replication**: For critical workloads, consider setting up geo-replication to protect your databases from regional outages and enable disaster recovery across Azure regions. See [Overview of the Managed Instance link](/azure/azure-sql/managed-instance/managed-instance-link-feature-overview?view=azuresql).

- **Secure TDE certificate backups**: When using customer-managed TDE, ensure you properly back up and secure your TDE certificates. For migrated databases, ensure you import the required TDE certificates before restoration. See [Migrate TDE certificate](/azure/azure-sql/managed-instance/tde-certificate-migrate?view=azuresql).

- **Test backup restoration regularly**: Perform periodic test restorations of your database backups to verify the recovery process and ensure your backups are valid. See [Point-in-time database restore](/azure/azure-sql/database/recovery-using-backups?view=azuresql#point-in-time-restore).

- **Document recovery procedures**: Create and maintain documentation for database recovery procedures to minimize downtime during recovery operations. Include step-by-step instructions for different recovery scenarios.

## Service-specific security

Azure SQL Managed Instance offers unique security features that should be configured to enhance your overall security posture.

- **Use zone redundancy for high availability**: For business-critical workloads, configure zone redundancy to provide higher availability and resilience against zone failures within a region. See [High availability through zone redundancy](/azure/azure-sql/managed-instance/high-availability-sla-local-zone-redundancy?view=azuresql).

- **Implement Windows authentication for Microsoft Entra principals**: For applications that require Windows authentication, configure Windows authentication for Microsoft Entra principals to enable secure, integrated authentication without changing application code. See [What is Windows Authentication for Microsoft Entra principals on Azure SQL Managed Instance?](/azure/azure-sql/managed-instance/winauth-azuread-overview?view=azuresql).

- **Configure secure connections for data virtualization**: When using data virtualization features, ensure you properly configure authentication and access control for non-public storage accounts. Use managed identities with appropriate permissions. See [Access to nonpublic storage accounts](/azure/azure-sql/managed-instance/data-virtualization-overview?view=azuresql#access-to-nonpublic-storage-accounts).

- **Use proxy connection type for secure access**: Consider using the proxy connection type for connections requiring enhanced security, as it provides additional protection compared to redirect connections. See [Limitations](/azure/azure-sql/managed-instance/private-endpoint-overview?view=azuresql#limitations).

- **Place multiple instances in the same subnet**: When security requirements permit, place multiple managed instances in the same subnet to simplify networking infrastructure maintenance and reduce provisioning time. See [Security isolation](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview?view=azuresql#security-isolation).

## Learn more

- [Microsoft Cloud Security Benchmark – Azure SQL](/security/benchmark/azure/baselines/azure-sql-security-baseline)
- [Security overview for Azure SQL Database and Azure SQL Managed Instance](/azure/azure-sql/database/security-overview?view=azuresql)
- [Playbook for addressing common security requirements with Azure SQL Database and SQL Managed Instance](/azure/azure-sql/database/security-best-practice?view=azuresql)
  See also: [Azure Security Documentation](/azure/security/).
