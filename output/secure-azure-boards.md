---
title: Secure your Azure Boards deployment
description: Learn how to secure Azure Boards, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-devops
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure Boards deployment

Azure Boards provides capabilities to track work items, manage backlogs, and visualize progress with customizable dashboards. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure Boards deployment.

## Identity and access management

Azure Boards requires proper identity and access controls to ensure only authorized users can access and modify work items. Implementing strong authentication and appropriate permission boundaries helps prevent unauthorized access to sensitive project information.

- **Implement Microsoft Entra ID integration**: Connect your Azure DevOps organization with Microsoft Entra ID to enforce consistent identity policies, enabling Single Sign-On and conditional access policies. See [Connect your organization to Microsoft Entra ID](/azure/devops/organizations/accounts/connect-organization-to-azure-ad?view=azure-devops).

- **Configure proper access levels**: Assign appropriate access levels (Basic, Stakeholder) based on user needs to limit access to Azure Boards features. Stakeholder access provides limited functionality at no cost for users who need minimal access. See [Default permissions and access levels for Azure Boards](/azure/devops/boards/get-started/permissions-access-boards?view=azure-devops).

- **Implement team-level permissions**: Restrict access to boards and backlogs by properly configuring team memberships, limiting visibility to only those who need it. See [Add team administrators](/azure/devops/organizations/settings/add-team-administrator?view=azure-devops).

- **Set area path permissions**: Use area path permissions to control who can view or modify work items in specific project areas. This enables you to restrict sensitive work items based on project structure. See [Set permissions and access for work tracking](/azure/devops/organizations/security/set-permissions-access-work-tracking?view=azure-devops).

- **Restrict query permissions**: Control who can create, view, and manage work item queries by setting appropriate permissions on shared queries and query folders. See [Set permissions on queries and query folders](/azure/devops/boards/queries/set-query-permissions?view=azure-devops).

- **Enable multi-factor authentication**: Require multi-factor authentication through Microsoft Entra ID for all user accounts that have access to Azure Boards, particularly for project administrators. See [Plan your Microsoft Entra deployment](/entra/identity/fundamentals/deployment-plan-identity).

## Network security

Securing network access to Azure Boards helps prevent unauthorized access to your work item tracking data and project information.

- **Configure IP address restrictions**: Limit access to your Azure DevOps organization by defining IP address ranges allowed to connect to your organization. This helps prevent access from unauthorized networks. See [Configure and manage Azure DevOps organization access with Microsoft Entra ID](/azure/devops/organizations/accounts/manage-azure-active-directory-groups?view=azure-devops).

- **Enable private connections with Azure Private Link**: For enhanced security, use Azure Private Link to establish a private endpoint in your virtual network to connect to Azure DevOps, ensuring traffic stays on the Microsoft backbone network. See [Secure your critical Azure service resources to only your virtual networks](/azure/security/fundamentals/network-best-practices#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

- **Use VPN or ExpressRoute for on-premises connectivity**: When accessing Azure Boards from on-premises networks, establish secure connections using Azure VPN Gateway or ExpressRoute to ensure all traffic is encrypted and secure. See [About ExpressRoute for Azure](/azure/expressroute/expressroute-introduction).

- **Restrict public network access**: Consider restricting public network access to your Azure DevOps organization when using private endpoints to minimize the attack surface. See [Azure Private Link and private endpoints](/azure/virtual-network/vnet-integration-for-azure-services#private-link-and-private-endpoints).

## Data protection

Azure Boards contains valuable project data and potentially sensitive information that requires protection through proper encryption and data handling practices.

- **Enable project-scoped users**: Limit user visibility to specific projects they need access to, rather than the entire organization, reducing exposure of sensitive data. See [Manage your organization, Limit user visibility for projects and more](/azure/devops/user-guide/manage-organization-collection?view=azure-devops#project-scoped-user-group).

- **Enforce TLS 1.2 or higher**: Ensure that all connections to Azure Boards use TLS 1.2 or higher encryption to protect data in transit. Microsoft enforces this by default but verify client configurations. See [Encryption in Azure](/purview/office-365-azure-encryption).

- **Apply work item rules for sensitive fields**: Configure work item field rules to control how data is entered, validated, and displayed, helping protect sensitive information. See [Apply rules to workflow states](/azure/devops/organizations/settings/work/apply-rules-to-workflow-states?view=azure-devops).

- **Implement custom field level security**: For highly sensitive information, consider implementing field-level security to restrict visibility of specific work item fields to authorized users only. See [Rules applied to a work item type that restrict select operations](/azure/devops/organizations/security/troubleshoot-permissions?view=azure-devops#rules-applied-to-a-work-item-type-that-restrict-select-operations).

- **Configure service connection security**: If using service connections to integrate Azure Boards with other services, ensure proper security measures are in place including restricting access to service connections. See [Grant or restrict permissions to select tasks](/azure/devops/organizations/security/restrict-access?view=azure-devops).

## Logging and monitoring

Effective logging and monitoring are essential for maintaining security awareness and responding to potential security issues in your Azure Boards deployment.

- **Enable Azure DevOps auditing**: Turn on auditing for your Azure DevOps organization to track important security events and user activities including work item changes, permission modifications, and access attempts. See [Audit logging and monitoring overview](/compliance/assurance/assurance-audit-logging).

- **Configure diagnostic settings**: Set up diagnostic settings to capture Azure DevOps logs in Azure Monitor for analysis and alerting on suspicious activities. See [How to collect platform logs and metrics with Azure Monitor](/azure/azure-monitor/essentials/diagnostic-settings).

- **Set up alerts for critical events**: Configure alerts for critical security events such as permission changes, access control modifications, or unusual access patterns to respond promptly to potential security issues. See [Create, view, and manage metric alerts](/azure/azure-monitor/alerts/alerts-metric).

- **Implement log retention policies**: Configure appropriate retention periods for your Azure DevOps audit logs to ensure they're available for security investigations and compliance requirements. See [Change the data retention period in Log Analytics](/azure/azure-monitor/logs/manage-cost-storage#change-the-data-retention-period).

- **Review work item change history**: Regularly review work item change history to detect unauthorized modifications or suspicious activities that might indicate security issues. See [View and add work item attachments, links, and history](/azure/devops/boards/backlogs/add-link?view=azure-devops).

## Compliance and governance

Ensuring compliance with organizational policies and industry regulations helps maintain a secure and properly governed Azure Boards environment.

- **Establish work item security policies**: Create and document security policies for work items, including classification of sensitive information, access control requirements, and proper handling procedures. See [Default permissions and access for Azure Boards](/azure/devops/boards/get-started/permissions-access-boards?view=azure-devops).

- **Implement work item templates**: Use work item templates to ensure consistent security-related information is captured and properly classified in work items. See [Use work item templates to quickly fill in forms](/azure/devops/boards/backlogs/work-item-template?view=azure-devops).

- **Configure process templates for security**: Customize process templates to include security requirements, ensuring that all projects follow standardized security practices. See [Customize Azure Boards to support SAFe®](/azure/devops/boards/plans/safe-customize?view=azure-devops).

- **Enforce access reviews**: Regularly review and validate user access to Azure Boards projects and features, removing access for users who no longer need it. See [Add users and assign licenses](/azure/devops/organizations/accounts/add-organization-users?view=azure-devops).

- **Maintain documented security controls**: Document all security controls implemented for Azure Boards to demonstrate compliance with organizational policies and regulatory requirements. See [Make your Azure DevOps secure](/azure/devops/organizations/security/security-overview?view=azure-devops#secure-your-services).

## Service-specific security

Azure Boards has unique security considerations related to its specific functionality for work item tracking and project management.

- **Secure integration with other tools**: When integrating Azure Boards with third-party tools or services, ensure secure authentication methods are used and limit permissions to only what's necessary. See [Connect Azure Boards with GitHub](/azure/devops/boards/github/index).

- **Configure appropriate notification settings**: Review and configure notification settings to prevent sensitive information from being sent to unauthorized users through email or other notification channels. See [Manage personal notification settings](/azure/devops/notifications/manage-your-personal-notifications?view=azure-devops).

- **Implement work item tagging strategy**: Develop a consistent tagging strategy for work items that includes security-related tags to help identify and manage items containing sensitive information. See [Add work item tags to categorize and filter lists and boards](/azure/devops/boards/queries/add-tags-to-work-items?view=azure-devops).

- **Configure delivery plans permissions**: If using delivery plans, set appropriate permissions to control who can access potentially sensitive roadmap information. See [Set permissions for Boards objects](/azure/devops/organizations/security/set-object-level-permissions?view=azure-devops#set-permissions-for-boards-objects).

- **Secure team dashboards**: Configure appropriate permissions for dashboards that might display sensitive project information or metrics. See [Set dashboard permissions](/azure/devops/report/dashboards/dashboard-permissions?view=azure-devops).

## Learn more

- [Make your Azure DevOps secure](/azure/devops/organizations/security/security-overview?view=azure-devops)
- [Default permissions and access for Azure Boards](/azure/devops/boards/get-started/permissions-access-boards?view=azure-devops)
- [Set permissions and access for work tracking](/azure/devops/organizations/security/set-permissions-access-work-tracking?view=azure-devops)
- [Azure Security Documentation](/azure/security/)
