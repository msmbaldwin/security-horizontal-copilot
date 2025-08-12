---
title: Secure your Fabric Data Warehouse deployment
description: Learn how to secure Fabric Data Warehouse, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: fabric-data-warehouse
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Fabric Data Warehouse deployment

Fabric Data Warehouse provides a comprehensive platform for data and analytics, featuring advanced query processing and full transactional T-SQL capabilities for easy data management and analysis. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Fabric Data Warehouse deployment.

## Network security

Fabric Data Warehouse requires secure network configurations to prevent unauthorized access and protect data in transit. Proper network security controls limit the exposure of your warehouse to potential threats.

- **Configure private links**: Eliminate public internet exposure by routing traffic through your virtual network, ensuring your Fabric Data Warehouse is only accessible through private endpoints. See [Private links for secure access to Fabric](/fabric/security/security-private-links-overview).

- **Enable Microsoft Entra Conditional Access**: Restrict access to your data warehouse based on user identity, location, device state, and risk level to prevent unauthorized connections. See [Conditional access in Fabric](/fabric/security/security-conditional-access).

- **Use TLS 1.2 or higher for all connections**: Ensure all client applications connecting to your Fabric Data Warehouse negotiate TLS 1.2 or higher to maintain secure communications and protect data in transit. See [Enforcing TLS version usage](/power-bi/admin/service-admin-power-bi-security#enforcing-tls-version-usage).

## Identity and access management

Implementing robust identity and access management controls is essential for protecting your Fabric Data Warehouse from unauthorized access while ensuring legitimate users can access required resources.

- **Implement Microsoft Entra ID authentication**: Eliminate the security risks of SQL authentication by exclusively using Microsoft Entra ID for authentication, enabling advanced security features like multi-factor authentication and conditional access. See [Microsoft Entra authentication as an alternative to SQL authentication](/fabric/data-warehouse/entra-id-authentication).

- **Configure workspace roles appropriately**: Assign users to the least-privileged workspace role needed (Admin, Member, Contributor, or Viewer) to limit access to warehouse resources according to job requirements. See [Security for data warehousing in Microsoft Fabric](/fabric/data-warehouse/security).

- **Implement SQL granular permissions**: Use T-SQL GRANT, DENY, and REVOKE statements to apply fine-grained access control to specific database objects instead of granting broad workspace roles. See [SQL granular permissions in Microsoft Fabric](/fabric/data-warehouse/sql-granular-permissions).

- **Use service principals for automation**: Configure service principals instead of user accounts for automated processes and applications to avoid dependency on individual user credentials and improve security. See [Service principal in Fabric Data Warehouse](/fabric/data-warehouse/service-principals).

## Data protection

Protecting sensitive data stored in your Fabric Data Warehouse is critical to maintain confidentiality and comply with regulatory requirements.

- **Implement row-level security**: Create security predicates that filter query results based on user attributes, ensuring users can only view data rows appropriate for their role or department. See [Row-level security in Fabric data warehousing](/fabric/data-warehouse/row-level-security).

- **Configure column-level security**: Restrict access to sensitive columns containing personally identifiable information (PII) or other confidential data by using column permissions, preventing unauthorized users from viewing sensitive data. See [Column-level security in Fabric data warehousing](/fabric/data-warehouse/column-level-security).

- **Apply dynamic data masking**: Implement data masking to obscure sensitive information in query results while preserving the underlying data, reducing exposure of sensitive data to unauthorized users. See [Dynamic data masking in Fabric data warehousing](/fabric/data-warehouse/dynamic-data-masking).

- **Use sensitivity labels**: Apply Microsoft Purview Information Protection sensitivity labels to classify and protect sensitive content in your warehouse, ensuring protection follows the data even when exported to supported formats. See [Apply sensitivity labels to Fabric items](/fabric/fundamentals/apply-sensitivity-labels).

- **Enable customer-managed keys**: Implement an additional layer of encryption using your own Azure Key Vault keys to encrypt Microsoft's encryption keys for enhanced control over your data security. See [Workspace customer managed keys](/fabric/security/workspace-customer-managed-keys).

## Logging and monitoring

Comprehensive logging and monitoring are essential for detecting security incidents, troubleshooting issues, and meeting compliance requirements.

- **Enable SQL audit logs**: Configure SQL audit logs to record database events including authentication attempts, schema changes, and data access operations, providing an audit trail for security analysis and compliance. See [SQL audit logs in Fabric Data Warehouse](/fabric/data-warehouse/sql-audit-logs).

- **Configure appropriate audit action groups**: Select the specific audit action groups that align with your security and compliance requirements to capture relevant events while minimizing storage costs. See [Database-level audit action groups and actions](/fabric/data-warehouse/sql-audit-logs#database-level-audit-action-groups-and-actions).

- **Monitor user activities**: Track user actions in your Fabric Data Warehouse through Microsoft Purview and PowerShell to identify unauthorized access or suspicious behavior that might indicate security issues. See [Track user activities in Microsoft Fabric](/fabric/admin/track-user-activities).

- **Set up workspace monitoring**: Implement workspace monitoring to collect and analyze logs and metrics from Fabric items in your workspace, enabling proactive identification of security issues. See [Workspace monitoring overview](/fabric/fundamentals/workspace-monitoring-overview).

## Compliance and governance

Establish strong governance controls to maintain compliance with organizational policies and regulatory requirements.

- **Implement Microsoft Purview integration**: Use Microsoft Purview to govern, protect, and manage your data from a single location, enabling comprehensive data governance across your Fabric Data Warehouse deployment. See [Microsoft Purview and Fabric](/fabric/governance/microsoft-purview-fabric).

- **Apply data loss prevention policies**: Configure Microsoft Purview Data Loss Prevention policies to detect and protect sensitive data, automatically triggering risk remediation when sensitive data is uploaded. See [Learn about data loss prevention](/purview/dlp-learn-about-dlp).

- **Configure multi-geo support**: For organizations with global presence, configure multi-geo support to ensure data remains stored at rest in specific regions to comply with data sovereignty regulations. See [Configure Multi-Geo support for Fabric](/fabric/admin/service-admin-premium-multi-geo).

## Backup and recovery

Implementing proper backup and disaster recovery measures ensures business continuity and data availability in case of system failures or data corruption.

- **Understand data resiliency capabilities**: Fabric provides built-in data resiliency to ensure your data is available even during disasters, automatically protecting your warehouse data. See [Reliability in Microsoft Fabric](/azure/reliability/reliability-fabric).

- **Create warehouse snapshots**: Generate point-in-time copies of your warehouse to preserve specific versions, enabling rollback to previous states if needed. See [Warehouse snapshot](/fabric/data-warehouse/warehouse-snapshot).

## Service-specific security

Fabric Data Warehouse offers unique security controls specific to its architecture and capabilities that should be configured to maximize security.

- **Implement object-level security**: Control access to specific database objects such as tables, views, and procedures based on user privileges, ensuring users can only interact with authorized objects. See [SQL granular permissions in Microsoft Fabric](/fabric/data-warehouse/sql-granular-permissions).

- **Secure warehouse sharing**: When sharing warehouses, customize permission levels granted to recipients to provide appropriate access for downstream consumption through SQL, Spark, or Power BI. See [Share your data and manage permissions](/fabric/data-warehouse/share-warehouse-manage-permissions).

- **Use source control for warehouses**: Implement source control for your warehouse objects to manage development and deployment of versioned warehouse objects, ensuring secure and controlled changes to your environment. See [Source control with Warehouse](/fabric/data-warehouse/source-control).

## Learn more

- [Security for data warehousing in Microsoft Fabric](/fabric/data-warehouse/security)
- [Secure a Microsoft Fabric data warehouse (Training)](/training/modules/secure-data-warehouse-in-microsoft-fabric/)
- [Security in Microsoft Fabric](/fabric/security/security-overview)
- [Microsoft Fabric security fundamentals](/fabric/security/security-fundamentals)
