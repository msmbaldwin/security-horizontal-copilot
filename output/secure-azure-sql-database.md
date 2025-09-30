---
title: Secure your Azure SQL Database deployment
description: Learn how to secure Azure SQL Database, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-sql
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Azure SQL Database deployment

Azure SQL Database provides capabilities to deliver a relational database service for cloud and enterprise applications with built-in intelligence. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure SQL Database deployment.

## Network security

Network security controls prevent unauthorized network access to your database, reducing exposure to attacks and ensuring only trusted sources can reach your databases through proper network isolation and access controls.

- **Configure IP firewall rules**: Create server-level and database-level firewall rules to restrict network access to specific IP addresses and ranges. Configure the most restrictive rules possible to minimize attack surface while allowing legitimate access. See [Azure SQL Database and Azure Synapse Analytics firewall rules](/azure/azure-sql/database/firewall-configure).

- **Implement virtual network service endpoints**: Use virtual network service endpoints to enable secure communication between Azure services within your virtual network. This helps isolate SQL Database traffic from the internet and provides additional network-level security controls. See [Virtual network service endpoints and rules for servers in Azure SQL Database](/azure/azure-sql/database/vnet-service-endpoint-rule-overview).

- **Deploy Azure Private Link for private connectivity**: Enable private endpoints to access your SQL Database through a private IP address within your virtual network. This eliminates public internet exposure and provides network isolation for database connections. See [Private Link for Azure SQL Database and Azure Synapse Analytics](/azure/azure-sql/database/private-endpoint-overview).

- **Disable public network access when appropriate**: Turn off public network access if your application architecture allows connections only through private endpoints or virtual network service endpoints. This provides the highest level of network isolation. See [Connectivity settings for Azure SQL Database](/azure/azure-sql/database/connectivity-settings).

- **Enforce minimum TLS version**: Configure the minimum TLS version to 1.2 or higher to ensure all connections use strong encryption protocols. Disable older TLS versions that contain known security vulnerabilities. See [Connectivity settings for Azure SQL Database](/azure/azure-sql/database/connectivity-settings).

## Identity and access management

Identity and access management controls ensure only authenticated and authorized users can access your database while maintaining the principle of least privilege through proper authentication and authorization mechanisms.

- **Use Microsoft Entra authentication**: Enable Microsoft Entra ID (formerly Azure Active Directory) authentication for centralized identity management and enhanced security features. This provides single sign-on capabilities, conditional access policies, and multi-factor authentication support. See [Use Microsoft Entra authentication for authentication with SQL Database](/azure/azure-sql/database/authentication-aad-overview).

- **Configure Microsoft Entra-only authentication**: Enable Microsoft Entra-only authentication to disable SQL authentication entirely when organizational policy requires centralized identity management. This enforces the use of Microsoft Entra ID for all database connections. See [Microsoft Entra-only authentication with Azure SQL](/azure/azure-sql/database/authentication-azure-ad-only-authentication).

- **Implement conditional access policies**: Apply conditional access policies to control database access based on user identity, location, device compliance, and risk level. This provides additional security layers beyond basic authentication. See [Conditional Access with Azure SQL Database](/azure/azure-sql/database/conditional-access-configure).

- **Create contained database users for Microsoft Entra principals**: Use contained database users mapped to Microsoft Entra groups instead of server-level logins to simplify permission management and improve security isolation between databases. See [Configure and manage Microsoft Entra authentication with Azure SQL](/azure/azure-sql/database/authentication-aad-configure).

- **Apply principle of least privilege**: Grant users only the minimum permissions required for their roles. Use database roles to group permissions and assign users to appropriate roles rather than granting individual permissions directly. See [Manage logins and users](/azure/azure-sql/database/logins-create-manage).

## Data protection

Data protection safeguards your information through encryption, access controls, and data classification to prevent unauthorized disclosure, tampering, or loss of sensitive information.

- **Enable Transparent Data Encryption with customer-managed keys**: Use customer-managed keys in Azure Key Vault for TDE to maintain control over encryption keys and meet compliance requirements. This provides additional security and control compared to service-managed keys. See [Transparent data encryption with customer-managed keys](/azure/azure-sql/database/transparent-data-encryption-byok-overview).

- **Implement Always Encrypted for highly sensitive data**: Use Always Encrypted to protect sensitive data such as credit card numbers, social security numbers, or personal health information. This ensures data remains encrypted even from database administrators and privileged users. See [Always Encrypted with Azure SQL Database](/azure/azure-sql/database/always-encrypted-certificate-store-configure).

- **Configure Dynamic Data Masking**: Apply dynamic data masking to obfuscate sensitive data for non-privileged users while preserving data functionality for applications. This prevents accidental exposure of sensitive information during development, testing, or support activities. See [Get started with SQL Database Dynamic Data Masking](/azure/azure-sql/database/dynamic-data-masking-overview).

- **Implement Row-Level Security**: Use Row-Level Security to control access to specific rows based on user characteristics such as group membership or execution context. This provides fine-grained access control without requiring application changes. See [Row-Level Security](https://learn.microsoft.com/sql/relational-databases/security/row-level-security).

- **Use Data Discovery and Classification**: Enable automatic discovery and classification of sensitive data in your databases. Apply sensitivity labels to columns containing personally identifiable information, financial data, or other regulated data types. See [Data discovery and classification for Azure SQL Database](/azure/azure-sql/database/data-discovery-and-classification-overview).

- **Enable ledger for tamper-evident records**: Use ledger functionality to create an immutable, cryptographically protected record of database transactions. This provides tamper-evidence for sensitive data and supports compliance requirements. See [Configure a ledger database](/azure/azure-sql/database/ledger-create-a-single-database-with-ledger-enabled).

## Logging and monitoring

Comprehensive logging and monitoring provide visibility into database operations, security events, and performance metrics to enable threat detection, compliance reporting, and operational oversight.

- **Enable SQL Database auditing**: Configure auditing to track database events and write them to Azure Storage, Log Analytics workspace, or Event Hubs. Capture login events, data access, schema changes, and administrative operations for security analysis and compliance reporting. See [Get started with SQL Database auditing](/azure/azure-sql/database/auditing-overview).

- **Configure diagnostic settings**: Enable diagnostic settings to collect detailed resource logs and metrics for security monitoring and performance analysis. Send diagnostic data to Azure Monitor Logs for advanced querying and correlation with other security events. See [Monitoring Azure SQL Database with Azure Monitor](/azure/azure-sql/database/monitoring-sql-database-azure-monitor).

- **Set up Azure Monitor alerts**: Create alerts for suspicious activities such as failed authentication attempts, unusual query patterns, configuration changes, or resource consumption anomalies. Use action groups to enable automated response to potential security incidents. See [Create alerts for Azure SQL Database](/azure/azure-sql/database/alerts-create).

- **Monitor Azure Activity Log**: Track administrative operations, configuration changes, and resource management actions through Azure Activity Log. Configure alerts for critical changes such as firewall rule modifications, authentication setting updates, or access policy changes. See [Monitoring Azure SQL Database with Azure Monitor](/azure/azure-sql/database/monitoring-sql-database-azure-monitor).

## Compliance and governance

Compliance and governance controls ensure database deployments meet regulatory requirements and organizational policies through proper configuration management and audit capabilities.

- **Enable Microsoft Defender for SQL**: Configure Microsoft Defender for SQL to provide advanced threat protection, vulnerability assessment, and security recommendations. This offers comprehensive security monitoring and automated threat detection for your database environment. See [Microsoft Defender for SQL](/azure/azure-sql/database/azure-defender-for-sql).

- **Configure vulnerability assessment**: Use SQL vulnerability assessment to regularly scan your databases for security misconfigurations, excessive permissions, and compliance violations. Remediate identified vulnerabilities promptly to maintain a strong security posture. See [SQL vulnerability assessment](/azure/defender-for-cloud/sql-azure-vulnerability-assessment-overview).

- **Apply Azure Policy definitions**: Use Azure Policy to enforce security configurations and monitor compliance across SQL Database resources. Apply built-in policies for encryption requirements, network access restrictions, and auditing configurations. See [Azure Policy Regulatory Compliance controls for Azure SQL Database](/azure/azure-sql/database/security-controls-policy).

- **Implement resource tagging for governance**: Apply consistent resource tags to SQL Database servers and databases for cost management, security monitoring, compliance tracking, and governance. Use tags to identify data classification, owner information, and regulatory requirements. See [Use tags to organize your Azure resources](/azure/azure-resource-manager/management/tag-resources).

- **Configure Customer Lockbox**: Enable Customer Lockbox for scenarios where Microsoft support needs to access your data. This provides an approval workflow for Microsoft data access requests and ensures you maintain control over when and how your data is accessed. See [Customer Lockbox for Microsoft Azure](/azure/security/fundamentals/customer-lockbox-overview).

## Backup and recovery

Reliable backup and recovery processes protect your data from loss due to failures, disasters, or attacks while ensuring you can meet your recovery time and point objectives.

- **Verify automated backup configuration**: Ensure automated backups are properly configured with appropriate retention periods for your business requirements. Azure SQL Database provides automated backups by default, but verify the retention period meets your recovery objectives. See [Automated backups in Azure SQL Database](/azure/azure-sql/database/automated-backups-overview).

- **Configure long-term retention policies**: Implement long-term backup retention for compliance requirements that exceed the default retention period. Store backups for up to 10 years to meet regulatory and business continuity requirements. See [Long-term backup retention](/azure/azure-sql/database/long-term-retention-overview).

- **Use geo-redundant backup storage**: Configure geo-redundant storage for backups to protect against regional disasters. This ensures your data can be recovered even if your primary region becomes unavailable through geo-restore capabilities. See [Backup storage redundancy](/azure/azure-sql/database/automated-backups-overview).

- **Test backup and restore procedures**: Regularly test point-in-time restore operations to verify backup integrity and validate recovery procedures. Ensure restored databases are fully functional and meet your recovery time objectives. See [Recover using automated database backups](/azure/azure-sql/database/recovery-using-backups).

- **Implement active geo-replication or failover groups**: Configure active geo-replication or failover groups for high availability and disaster recovery across multiple regions. This provides automatic failover capabilities and continuous data synchronization for mission-critical applications. See [Active geo-replication](/azure/azure-sql/database/active-geo-replication-overview).

## Service-specific security

Service-specific security controls address unique security considerations and advanced features that are specific to Azure SQL Database's architecture and capabilities.

- **Configure connection policies**: Set connection policy to redirect for optimal performance and security when connecting from Azure. Use proxy mode only when necessary for specific firewall or network requirements. See [Azure SQL connectivity architecture](/azure/azure-sql/database/connectivity-architecture).

- **Enable Advanced Threat Protection**: Configure Advanced Threat Protection to detect SQL injection attacks, access from unusual locations, brute force attacks, and anomalous access patterns. Set up email notifications and customize threat detection policies for your environment. See [Configure Advanced Threat Protection](/azure/azure-sql/database/threat-detection-configure).

- **Implement column-level security**: Grant permissions at the column level to restrict access to sensitive data. Only provide SELECT, UPDATE, or REFERENCES permissions to users who specifically need access to sensitive columns containing personal or financial data. See [Column-level security](https://learn.microsoft.com/sql/relational-databases/security/column-level-security).

- **Use SQL Database firewall at database level**: Implement database-level firewall rules for granular access control per database. This provides additional security isolation when hosting multiple databases with different access requirements on the same logical server. See [Database-level firewall rules](/azure/azure-sql/database/firewall-configure).

- **Configure server roles and permissions**: Use server-level roles such as loginmanager and dbmanager judiciously, granting these elevated permissions only when necessary for specific administrative tasks. Regularly review and audit server-level permissions. See [Server-level roles](/azure/azure-sql/database/security-server-roles).

