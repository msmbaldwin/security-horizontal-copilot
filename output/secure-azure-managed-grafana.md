---
title: Secure your Azure Managed Grafana deployment
description: Learn how to secure Azure Managed Grafana, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: managed-grafana
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Azure Managed Grafana deployment

Azure Managed Grafana provides capabilities to visualize and monitor data from multiple sources with comprehensive dashboard and alerting functionality. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

[!INCLUDE [Security horizontal Zero Trust statement](~/reusable-content/ce-skilling/azure/includes/security/zero-trust-security-horizontal.md)]

This article provides security recommendations to help protect your Azure Managed Grafana deployment.

## Network security

Network security controls prevent unauthorized access to Azure Managed Grafana workspaces and establish secure communication boundaries for monitoring and visualization workloads.

- **Enable private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network using Azure Private Link. Private endpoints provide dedicated network interfaces that connect directly to Azure Managed Grafana while preventing data exfiltration risks. See [Set up private access](https://learn.microsoft.com/azure/managed-grafana/how-to-set-up-private-access).

- **Disable public network access**: Block all internet-based connections when using private endpoints by configuring the workspace to deny public network access. This forces all communication through private endpoints and reduces your attack surface. See [Set up private access](https://learn.microsoft.com/azure/managed-grafana/how-to-set-up-private-access).

- **Configure network security groups for private endpoints**: Apply Network Security Groups to control traffic flow to and from subnets hosting private endpoints by implementing restrictive rules that deny unauthorized access while allowing necessary communication patterns. Configure NSG rules on the subnet where the private endpoint is deployed to control access. See [Set up private access](https://learn.microsoft.com/azure/managed-grafana/how-to-set-up-private-access).

- **Use deterministic outbound IP addresses**: Enable deterministic outbound IP feature in Azure Managed Grafana Standard tier to obtain predictable IP addresses for outbound connections. This allows you to configure data source firewalls to allow specific IP addresses, reducing the need for broad network access rules. See [Use deterministic outbound IPs](https://learn.microsoft.com/azure/managed-grafana/how-to-deterministic-ip).

## Identity and access management

Identity and access management controls ensure that only authorized users and applications can access Azure Managed Grafana resources with appropriate permissions for dashboard management and data visualization.

- **Use Microsoft Entra ID authentication**: Replace local authentication mechanisms with Microsoft Entra ID to leverage centralized identity management, conditional access policies, and advanced security features. This provides better security through multi-factor authentication and eliminates long-lived secrets. See [Set up Azure Managed Grafana authentication and permissions](https://learn.microsoft.com/azure/managed-grafana/how-to-authentication-permissions).

- **Implement role-based access control (RBAC)**: Assign users and applications the minimum required permissions using built-in Grafana roles such as Grafana Viewer, Grafana Editor, or Grafana Admin rather than using administrative roles. Use Azure RBAC to enforce least-privilege access principles. See [Manage access and permissions for users and identities](https://learn.microsoft.com/azure/managed-grafana/how-to-manage-access-permissions-users-identities).

- **Enable managed identities for data source authentication**: Configure system-assigned or user-assigned managed identities to authenticate Azure Managed Grafana to data sources without storing credentials in code or configuration. Managed identities eliminate credential management overhead and provide automatic secret rotation. See [Set up Azure Managed Grafana authentication and permissions](https://learn.microsoft.com/azure/managed-grafana/how-to-authentication-permissions).

- **Assign Monitoring Reader role for data access**: Grant the managed identity associated with Azure Managed Grafana the Monitoring Reader role on target subscriptions to provide read-only access to monitoring data across all resources within the subscription. This follows least-privilege principles for data source access. See [Set up Azure Managed Grafana authentication and permissions](https://learn.microsoft.com/azure/managed-grafana/how-to-authentication-permissions).

## Data protection

Data protection controls ensure that configuration data, dashboard definitions, and user information stored in Azure Managed Grafana are properly encrypted and secured against unauthorized access.

- **Leverage default encryption at rest**: Azure Managed Grafana automatically encrypts all data at rest using server-side encryption with service-managed keys. Resource-provider metadata is stored in Azure Cosmos DB and Grafana user data is stored in Azure Database for PostgreSQL, both providing automatic encryption without additional configuration required. See [Encryption in Azure Managed Grafana](https://learn.microsoft.com/azure/managed-grafana/encryption).

- **Ensure data in transit encryption**: All communication to and from Azure Managed Grafana is automatically encrypted in transit using TLS. This includes connections from users to the Grafana interface and connections from Grafana to data sources. No additional configuration is required as this is enabled by default. See [Encryption in Azure Managed Grafana](https://learn.microsoft.com/azure/managed-grafana/encryption).

- **Secure data source connections**: Configure data sources to use managed identities rather than storing credentials directly in Grafana. The Azure Monitor data source is preconfigured with managed identity authentication, eliminating the need to manage service principals manually. For other data sources, use managed identity authentication where supported. See [How to manage data sources in Azure Managed Grafana](https://learn.microsoft.com/azure/managed-grafana/how-to-data-source-plugins-managed-identity).

- **Implement proper dashboard and folder permissions**: Use Grafana's built-in permission system to control access to dashboards and folders based on user roles and responsibilities. Ensure sensitive monitoring data is only accessible to authorized personnel. See [Manage access and permissions for users and identities](https://learn.microsoft.com/azure/managed-grafana/how-to-manage-access-permissions-users-identities).

## Logging and monitoring

Logging and monitoring capabilities provide visibility into Azure Managed Grafana usage patterns, authentication events, and system health for security analysis and compliance reporting.

- **Enable diagnostic settings for audit logging**: Configure diagnostic settings to stream Grafana login events and other logs to Log Analytics, Storage Account, or Event Hub for centralized monitoring and security analysis. This provides visibility into user access patterns and system events. See [Monitor Azure Managed Grafana using diagnostic settings](https://learn.microsoft.com/azure/managed-grafana/how-to-monitor-managed-grafana-workspace).

- **Configure log retention policies**: Establish appropriate retention periods for diagnostic logs based on compliance requirements and security analysis needs. Store logs in secure, tamper-evident storage locations with proper access controls. See [Monitor Azure Managed Grafana using diagnostic settings](https://learn.microsoft.com/azure/managed-grafana/how-to-monitor-managed-grafana-workspace).

- **Monitor workspace metrics**: Use Azure Monitor metrics to track performance indicators and detect anomalous behavior that might indicate security issues or resource exhaustion attacks. Monitor metrics such as total requests and response times through Azure Monitor's metric charts. See [Monitor Azure Managed Grafana using Azure Monitor's metric chart](https://learn.microsoft.com/azure/managed-grafana/how-to-monitor-managed-grafana-metrics).

- **Set up alerting for security events**: Create alerts based on diagnostic logs to detect suspicious activity such as failed authentication attempts or unusual access patterns. Use the Grafana Login Events log category to monitor authentication activities and configure alerts in Azure Monitor based on these logs. See [Monitor Azure Managed Grafana using diagnostic settings](https://learn.microsoft.com/azure/managed-grafana/how-to-monitor-managed-grafana-workspace).

## Compliance and governance

Compliance and governance controls ensure that Azure Managed Grafana deployments meet organizational policies and regulatory requirements for data protection and access management.

- **Implement Azure Policy controls**: Use Azure Policy to audit and enforce Azure Managed Grafana configurations across your organization. Built-in policies are available to disable public network access, enforce private link usage, disable service accounts, and ensure diagnostic logging is configured. See [Azure Policy built-in policy definitions for Managed Grafana](https://learn.microsoft.com/azure/governance/policy/samples/built-in-policies#managed-grafana).

- **Enforce private network access requirements**: Apply the "Azure Managed Grafana workspaces should disable public network access" policy to ensure all workspaces use private endpoints instead of public internet connections. This policy can audit existing configurations or deny creation of non-compliant workspaces. See [Azure Policy built-in policy definitions for Managed Grafana](https://learn.microsoft.com/azure/governance/policy/samples/built-in-policies#managed-grafana).

- **Configure resource tagging for governance**: Apply consistent tags to Azure Managed Grafana resources to support cost management, security classification, and compliance reporting. Use tags to identify resource owners, environments, and data sensitivity levels. See [Use tags to organize your Azure resources and management hierarchy](https://learn.microsoft.com/azure/azure-resource-manager/management/tag-resources).

- **Establish access review processes**: Implement regular access reviews for Azure Managed Grafana permissions to ensure users maintain only necessary access levels. Remove unused accounts and adjust permissions based on changing role requirements. See [What are Azure AD access reviews?](https://learn.microsoft.com/entra/id-governance/access-reviews-overview).

## Backup and recovery

Backup and recovery controls protect against data loss and ensure business continuity for Azure Managed Grafana monitoring capabilities during outages or disasters.

- **Enable zone redundancy for high availability**: Configure zone redundancy when creating Azure Managed Grafana workspaces in supported regions to distribute virtual machines across availability zones. This provides automatic failover capabilities during zone-level outages without user intervention. See [Azure Managed Grafana service reliability](https://learn.microsoft.com/azure/managed-grafana/high-availability).

- **Plan multi-region deployment strategy**: Deploy Azure Managed Grafana workspaces in multiple regions for disaster recovery purposes since Microsoft doesn't provide cross-region disaster recovery for this service. Maintain consistent dashboard configurations across regions for seamless failover capabilities during regional outages. See [Azure Managed Grafana service reliability](https://learn.microsoft.com/azure/managed-grafana/high-availability).

- **Export and version control dashboard configurations**: Regularly export dashboard definitions and data source configurations to version control systems or backup storage locations. This enables restoration of monitoring capabilities and maintains configuration history for compliance and recovery purposes. Use Grafana's native export functionality to backup dashboard JSON definitions. See [How to create a dashboard](https://learn.microsoft.com/azure/managed-grafana/how-to-create-dashboard).

## Service-specific security

Service-specific security controls address unique security considerations and features available in Azure Managed Grafana for API authentication and automated access management.

- **Disable API keys and service accounts when not needed**: Keep API key creation disabled by default to prevent automated access through service accounts unless specifically required for integration scenarios. Enable API keys only when necessary for legitimate automation use cases and implement proper token lifecycle management. See [How to use service accounts in Azure Managed Grafana](https://learn.microsoft.com/azure/managed-grafana/how-to-service-accounts).

- **Implement proper service account token management**: When service accounts are required, configure tokens with appropriate expiration times and minimal necessary permissions. Regularly rotate service account tokens and revoke unused tokens to reduce the risk of credential compromise. Store tokens securely and avoid embedding them in source code. See [How to use service accounts in Azure Managed Grafana](https://learn.microsoft.com/azure/managed-grafana/how-to-service-accounts).

- **Document recovery procedures**: Create detailed recovery procedures that include steps for recreating workspaces, restoring configurations, and reestablishing data source connections. Test recovery procedures regularly to ensure they remain current and effective. See [Azure Managed Grafana service reliability](https://learn.microsoft.com/azure/managed-grafana/high-availability).

## Service-specific security

Azure Managed Grafana provides unique security capabilities designed specifically for data visualization, dashboard management, and monitoring workloads that require specific attention to maintain secure operations.

- **Secure data source plugin configurations**: When configuring data source plugins, use managed identities wherever possible instead of storing connection credentials directly in Grafana. For data sources that require direct credentials, use Azure Key Vault to store and retrieve connection information securely. See [How to manage data sources in Azure Managed Grafana](https://learn.microsoft.com/azure/managed-grafana/how-to-data-source-plugins-managed-identity).

- **Control Grafana Enterprise plugin access**: If using Grafana Enterprise features, carefully review and approve the specific plugins enabled in your environment. Only install plugins that are required for your monitoring use cases and ensure they come from trusted sources. Monitor plugin usage for security issues. See [Enable Grafana Enterprise](https://learn.microsoft.com/azure/managed-grafana/how-to-grafana-enterprise).

- **Implement dashboard sharing security**: When sharing dashboards with external users or embedding them in applications, use appropriate authentication and ensure sensitive data is not exposed. Configure dashboard permissions to restrict editing capabilities to authorized users only. See [How to share a dashboard](https://learn.microsoft.com/azure/managed-grafana/how-to-share-dashboard).

- **Secure API and integration endpoints**: If integrating Azure Managed Grafana with external systems or using programmatic access, ensure API keys are properly secured and rotated regularly. Use service principals with minimal required permissions for automation scenarios. See [Manage access and permissions for users and identities](https://learn.microsoft.com/azure/managed-grafana/how-to-manage-access-permissions-users-identities).

