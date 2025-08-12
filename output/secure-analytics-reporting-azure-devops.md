---
title: Secure your Analytics & Reporting - Azure DevOps deployment
description: Learn how to secure Analytics & Reporting in Azure DevOps, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-devops
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Analytics & Reporting - Azure DevOps deployment

Analytics & Reporting in Azure DevOps provides capabilities to gain insights from your data and make data-driven decisions through dashboards, widgets, and custom reports. When deploying this service, it's important to follow security best practices to protect sensitive data, configurations, and user access.

This article provides guidance on how to best secure your Azure DevOps Analytics & Reporting deployment.

## Identity and access management

Managing who has access to your Analytics data is critical since Analytics does not support security filtering by area path, potentially exposing sensitive information to unauthorized users.

- **Implement role-based access control**: Control who can access Analytics data by assigning appropriate permissions. By default, the View Analytics permission is granted to Contributors and Readers, but you should audit these permissions regularly. See [Set permissions to access Analytics and Analytics views](/azure/devops/report/powerbi/analytics-security?view=azure-devops).

- **Restrict Analytics access for sensitive projects**: For projects containing sensitive data, limit the View Analytics permission to only those users who need access to the entire project's data. Analytics does not support security filtering by area path, so users with Analytics access can view all project data. See [Access denied response](/azure/devops/report/powerbi/analytics-security?view=azure-devops#access-denied-response).

- **Configure shared Analytics view permissions**: Manage permissions for shared Analytics views to control who can create, edit, delete, or view specific views containing sensitive information. This provides granular control over which users can access specific data insights. See [Manage permissions for a shared view](/azure/devops/report/powerbi/analytics-security?view=azure-devops#manage-permissions-for-a-shared-view).

- **Require multi-factor authentication**: Reduce the risk of credential compromise by enforcing multi-factor authentication for all users with Analytics access, especially for those who can create and edit shared views. See [Secure access to your organization](/azure/devops/organizations/security/data-protection?view=azure-devops#secure-access-to-your-organization).

- **Connect to Microsoft Entra ID**: Manage user access centrally and enforce stronger authentication policies by connecting your Azure DevOps organization to Microsoft Entra ID. This allows centralized identity management, password policies, and user lifecycle management. See [Adopt Microsoft Entra ID](/azure/devops/organizations/security/data-protection?view=azure-devops#adopt-microsoft-entra-id).

## Data protection

Analytics contains valuable data about your projects, work items, and organizational processes that needs to be properly protected.

- **Classify your Analytics data**: Identify sensitive data in your Analytics views and reports to determine appropriate security controls. Data classification should be based on sensitivity and risk horizon, with appropriate restrictions for highly sensitive information. See [Classify your data](/azure/devops/organizations/security/data-protection?view=azure-devops#classify-your-data).

- **Implement conditional access policies**: Control access to Analytics data based on conditions like user location, device compliance, or risk level using Microsoft Entra ID conditional access policies. This helps protect sensitive Analytics data from unauthorized access. See [Secure access to your organization](/azure/devops/organizations/security/data-protection?view=azure-devops#secure-access-to-your-organization).

- **Limit data sharing in reports**: When creating Power BI reports from Analytics views, ensure that sharing permissions are appropriately restricted to prevent unintended data exposure. Consider using Power BI workspace security to control report distribution. See [Manage permissions for a shared view](/azure/devops/report/powerbi/analytics-security?view=azure-devops#manage-permissions-for-a-shared-view).

- **Secure API access tokens**: When creating applications or scripts that query Analytics data through the OData API, use secure methods to store and manage access tokens. Rotate personal access tokens regularly and limit their scope to only the required access. See [Limit use of alternate authentication credentials](/azure/devops/organizations/security/data-protection?view=azure-devops#limit-use-of-alternate-authentication-credentials).

## Network security

Controlling network access to your Analytics data helps prevent unauthorized access from untrusted networks.

- **Configure IP address restrictions**: Limit access to Analytics data by implementing IP address-based restrictions through Microsoft Entra conditional access policies. This ensures Analytics data can only be accessed from trusted networks. See [Secure access to your organization](/azure/devops/organizations/security/data-protection?view=azure-devops#secure-access-to-your-organization).

- **Enforce HTTPS for all connections**: Ensure all clients connecting to Analytics APIs use HTTPS to encrypt data in transit, preventing interception of sensitive information. Azure DevOps enforces this by default, but verify client applications maintain this requirement. See [Data protection overview](/azure/devops/organizations/security/data-protection?view=azure-devops#service-security).

## Logging and monitoring

Tracking how Analytics data is accessed and used helps detect potential security incidents and compliance issues.

- **Enable Azure DevOps auditing**: Set up auditing streams to capture and monitor user activity related to Analytics views and data access. This provides visibility into who is accessing your data and helps detect suspicious patterns. See [Create an audit stream](/azure/devops/organizations/audit/azure-devops-auditing).

- **Monitor Analytics view usage**: Regularly review access patterns to shared Analytics views to ensure they are being used appropriately and by authorized users only. Watch for unusual access patterns or unexpected query volumes. See [Analytics permissions](/azure/devops/report/powerbi/reporting-roadmap?view=azure-devops#analytics-permissions).

- **Review dashboard access permissions**: Periodically audit the permissions of dashboards containing Analytics widgets to ensure that only authorized users have access to view sensitive data visualizations. See [Dashboard permissions](/azure/devops/report/dashboards/dashboard-permissions?view=azure-devops).

## Compliance and governance

Maintaining compliance with security standards and implementing governance controls helps ensure Analytics data is handled securely.

- **Implement data retention policies**: Establish policies for how long Analytics data should be retained in reports and exported datasets to comply with organizational data governance requirements. See [Data protection overview](/azure/devops/organizations/security/data-protection?view=azure-devops#data-privacy).

- **Document Analytics security controls**: Maintain documentation of all security controls implemented for Analytics data, including access permissions, data classification, and sharing restrictions for compliance reporting. See [Data protection overview](/azure/devops/organizations/security/data-protection?view=azure-devops#building-confidence).

- **Conduct regular security reviews**: Periodically review Analytics security configurations and permissions to identify potential vulnerabilities or access control issues. This should include reviewing shared Analytics views and their permissions. See [Set permissions to access Analytics and Analytics views](/azure/devops/report/powerbi/analytics-security?view=azure-devops).

## Service-specific security

Analytics & Reporting in Azure DevOps has unique security considerations that should be addressed.

- **Use project-level filters in queries**: When querying Analytics data, always provide project-level filters instead of using global queries to prevent "access denied" errors and ensure users only access data they have permissions for. See [Access denied response](/azure/devops/report/powerbi/analytics-security?view=azure-devops#access-denied-response).

- **Limit Analytics access for Stakeholders**: Users with Stakeholder access don't have permission to view or edit Analytics views by default. Maintain this restriction to prevent potential data exposure to users with limited access. See [Set permissions to access Analytics and Analytics views](/azure/devops/report/powerbi/analytics-security?view=azure-devops).

- **Follow OData query best practices**: When creating custom OData queries against Analytics, follow query guidelines to ensure optimal performance and security. Improperly constructed queries can expose excessive data or cause performance issues. See [OData Analytics query guidelines](/azure/devops/report/extend-analytics/odata-query-guidelines?view=azure-devops).

- **Control feedback collection**: For organizations with strict privacy requirements, use the feedback toggle feature to control whether users can provide feedback that might inadvertently contain sensitive data. See [Managing privacy policies for admins to control user feedback collection](/azure/devops/organizations/security/data-protection?view=azure-devops#data-privacy).

## Learn more

- [What is Analytics?](/azure/devops/report/powerbi/what-is-analytics?view=azure-devops)
- [Set permissions to access Analytics and Analytics views](/azure/devops/report/powerbi/analytics-security?view=azure-devops)
- [Data protection overview](/azure/devops/organizations/security/data-protection?view=azure-devops)
- [Azure DevOps security best practices](/azure/devops/organizations/security/security-best-practices)
