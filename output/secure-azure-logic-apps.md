---
title: Secure your Azure Logic Apps deployment
description: Learn how to secure Azure Logic Apps, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: logic-apps
ms.topic: conceptual
ms.custom: horz-security
ms.date: 12/31/2024
ai-usage: ai-assisted
---

# Secure your Azure Logic Apps deployment

Azure Logic Apps provides capabilities to build automated workflows that integrate apps, data, services, and systems. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure Logic Apps deployment.

## Network security

Network security is critical for protecting Logic Apps workflows from unauthorized access and ensuring secure communication between components. Both Consumption and Standard Logic Apps support various network security configurations with different capabilities.

- **Deploy Logic Apps Standard in virtual networks**: Use Azure App Service integration to deploy Standard Logic Apps into virtual networks with subnet delegation. This provides network isolation and enables you to control traffic flow. See [Secure traffic between Standard logic apps and Azure virtual networks using private endpoints](https://learn.microsoft.com/azure/logic-apps/secure-single-tenant-workflow-virtual-network-private-endpoint).

- **Configure private endpoints for Standard Logic Apps**: Set up private endpoints to secure inbound traffic to your Standard Logic Apps workflows, ensuring communications remain within your virtual network boundaries. See [Set up inbound traffic through private endpoints](https://learn.microsoft.com/azure/logic-apps/secure-single-tenant-workflow-virtual-network-private-endpoint#set-up-inbound-traffic-through-private-endpoints).

- **Implement virtual network integration for outbound traffic**: Configure virtual network integration for Standard Logic Apps to control outbound connections and route traffic through your virtual network. See [Set up outbound traffic using virtual network integration](https://learn.microsoft.com/azure/logic-apps/secure-single-tenant-workflow-virtual-network-private-endpoint#set-up-outbound-traffic-using-virtual-network-integration).

- **Apply network security group rules**: Use Network Security Groups (NSGs) to control inbound and outbound network traffic to Logic Apps deployed in virtual networks, restricting access to specific ports and protocols. See [Azure security baseline for Logic Apps - Network Security Group Support](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#network-security).

- **Configure IP address restrictions**: Set up IP address restrictions to limit access to your Logic Apps workflows from specific IP ranges, providing an additional layer of network-based access control. See [Firewall configuration: IP addresses and service tags](https://learn.microsoft.com/azure/logic-apps/logic-apps-limits-and-config#firewall-configuration-ip-addresses-and-service-tags).

- **Disable public network access**: For Standard Logic Apps, disable public network access by using App Service Environment v3 (ASE v3) or enabling private endpoints to ensure workflows are only accessible through private connections. See [Disable Public Network Access](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#network-security).

## Identity and access management

Identity and access management controls are essential for ensuring only authorized users and systems can access Logic Apps workflows and their resources. Logic Apps supports multiple authentication methods with Microsoft Entra ID integration being the recommended approach.

- **Enable managed identities for authentication**: Use system-assigned or user-assigned managed identities to authenticate Logic Apps workflows with Azure services without storing credentials in code. This provides superior security by eliminating credential management overhead. See [Authenticate access and connections to Azure resources with managed identities in Azure Logic Apps](https://learn.microsoft.com/azure/logic-apps/authenticate-with-managed-identity).

- **Configure OAuth 2.0 with Microsoft Entra ID for request triggers**: Set up OAuth 2.0 with Microsoft Entra ID for Consumption Logic Apps that use request-based triggers, ensuring only authenticated callers can invoke your workflows. See [Enable OAuth with Microsoft Entra ID for your Consumption logic app resource](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#enable-azure-ad-oauth-for-your-consumption-logic-app-resource).

- **Implement role-based access control (RBAC)**: Apply appropriate RBAC roles to control who can create, modify, run, and monitor Logic Apps workflows, following the principle of least privilege. See [Azure security baseline for Logic Apps - Azure RBAC for Data Plane](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#privileged-access).

- **Use authorization policies for fine-grained access control**: Define authorization policies that specify required claims in access tokens for request-based triggers, ensuring only properly authenticated calls can trigger workflow execution. See [Enable OAuth with Microsoft Entra ID for your Consumption logic app resource](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#enable-azure-ad-oauth-for-your-consumption-logic-app-resource).

- **Disable shared access signature (SAS) authentication when using OAuth**: For enhanced security, disable SAS authentication when using OAuth 2.0 with Microsoft Entra ID to prevent authentication bypass. See [Disable shared access signature (SAS) authentication](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#disable-sas).

## Data protection

Data protection ensures sensitive information processed by Logic Apps workflows is encrypted, handled securely, and protected from unauthorized access throughout the data lifecycle.

- **Encrypt data in transit with TLS 1.2**: Ensure all HTTP communications use TLS 1.2 or higher encryption for data in transit. Logic Apps enforces TLS 1.2 minimum for inbound calls to request-based triggers. See [Access for inbound calls to request-based triggers](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-inbound-requests).

- **Secure sensitive data in run history**: Enable secure inputs and outputs settings on actions and triggers to obfuscate sensitive data in workflow run history, preventing exposure of credentials and sensitive information. See [Secure data in run history by using obfuscation](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate).

- **Store secrets in Azure Key Vault**: Use Azure Key Vault to store sensitive information like connection strings, API keys, and passwords, then reference them in Logic Apps workflows through app settings rather than embedding them directly. See [Service Credential and Secrets Support Integration and Storage in Azure Key Vault](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#identity-management).

- **Implement customer-managed keys for Logic Apps ISE**: For Integration Service Environment (ISE) deployments, use customer-managed keys for encryption at rest to maintain control over encryption keys. See [Logic Apps Integration Service Environment should be encrypted with customer-managed keys](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1fafeaf6-7927-4059-a50a-8eb2a7a6f2b5).

- **Use secured parameters in workflow definitions**: Define sensitive parameters using `securestring` or `secureobject` types in workflow definitions to prevent exposure during editing and in deployment templates. See [Secure parameters in workflow definitions](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-parameters-workflow).

## Logging and monitoring

Comprehensive logging and monitoring enable detection of security events, performance issues, and compliance violations while providing audit trails for Logic Apps workflows.

- **Enable diagnostic logging and resource logs**: Configure diagnostic settings to capture workflow execution logs, performance metrics, and security events. Send logs to Log Analytics, Storage Accounts, or Event Hubs for analysis. See [Monitor and collect diagnostic data for workflows in Azure Logic Apps](https://learn.microsoft.com/azure/logic-apps/monitor-workflows-collect-diagnostic-data).

- **Configure Azure Monitor integration**: Set up Azure Monitor to collect and analyze Logic Apps metrics, logs, and alerts for proactive monitoring of workflow health and performance. See [Azure Resource Logs](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#logging-and-threat-detection).

- **Enable Microsoft Defender for App Service**: For Standard Logic Apps, enable Microsoft Defender for App Service to provide advanced threat detection and security monitoring capabilities. See [Microsoft Defender for Service](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#logging-and-threat-detection).

- **Implement alerting for security events**: Create alerts for suspicious activities, authentication failures, and unusual workflow execution patterns to enable rapid response to potential security incidents. See [Overview of Defender for App Service](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-app-service-introduction).

- **Track workflow run history securely**: Monitor workflow execution while ensuring sensitive data remains protected through proper configuration of secure inputs and outputs. See [Secure data in run history by using obfuscation](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate).

## Compliance and governance

Implementing proper compliance and governance controls ensures Logic Apps deployments meet regulatory requirements and organizational policies while maintaining security standards.

- **Apply Azure Policy for compliance monitoring**: Use built-in Azure Policy definitions to ensure Logic Apps comply with security standards such as enabling diagnostic logging and encryption requirements. See [Azure Policy Regulatory Compliance controls for Azure Logic Apps](https://learn.microsoft.com/azure/logic-apps/security-controls-policy).

- **Enable Customer Lockbox for support scenarios**: Configure Customer Lockbox to control and audit Microsoft support access to your Logic Apps data during support incidents, providing additional oversight over data access. See [Customer Lockbox](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#privileged-access).

- **Implement resource tagging for governance**: Apply consistent resource tags to Logic Apps and related resources to support governance, cost tracking, and security monitoring across your environment. See [Azure Resource Manager API spec](https://learn.microsoft.com/azure/logic-apps/azure-resource-manager-api-spec).

- **Follow Microsoft Cloud Security Benchmark**: Align Logic Apps security configurations with the Microsoft Cloud Security Benchmark recommendations for comprehensive security coverage. See [Azure security baseline for Logic Apps](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline).

- **Document security configurations**: Maintain documentation of security settings, authentication configurations, and network access controls to support compliance audits and change management processes.

## Backup and recovery

Backup and recovery capabilities ensure business continuity and data protection for Logic Apps workflows and their associated configurations and data.

- **Export and version control workflow definitions**: Regularly export Logic Apps workflow definitions and store them in version control systems to enable recovery and rollback capabilities. See [Azure Resource Manager templates for Logic Apps](https://learn.microsoft.com/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview).

- **Implement Infrastructure as Code (IaC) for deployment**: Use Azure Resource Manager (ARM) templates, Bicep, or Terraform to define Logic Apps infrastructure as code, enabling consistent and repeatable deployments. See [Automate deployment for Azure Logic Apps by using Azure Resource Manager templates](https://learn.microsoft.com/azure/logic-apps/logic-apps-azure-resource-manager-templates-overview).

- **Back up connected service configurations**: Ensure configurations for connected services like storage accounts, Key Vault, and databases are included in backup and recovery procedures. See [Logic Apps Standard App Settings](https://learn.microsoft.com/azure/app-service/app-service-key-vault-references).

- **Test disaster recovery procedures**: Regularly test the restoration of Logic Apps workflows and their dependencies to validate recovery procedures and identify potential issues before they impact operations.

- **Document recovery procedures**: Maintain comprehensive documentation of backup locations, recovery steps, and contact information to support rapid response during service disruptions.

## Service-specific security

Logic Apps has unique security considerations related to workflow execution, connector authentication, and integration patterns that require specific attention to maintain secure operations.

- **Secure connector connections with managed identities**: Configure managed connectors to use managed identities when connecting to Azure services, eliminating the need to store connection credentials. See [Managed Identities](https://learn.microsoft.com/security/benchmark/azure/baselines/logic-apps-security-baseline#identity-management).

- **Validate webhook signatures and authentication**: For workflows that receive webhook calls, implement signature validation and authentication checks to ensure requests come from trusted sources. See [HTTP Webhook trigger](https://learn.microsoft.com/azure/connectors/connectors-native-webhook).

- **Control workflow execution through conditions**: Use conditional logic and approval workflows to add human validation steps for sensitive operations, preventing automated execution of high-risk actions. See [Control workflow execution](https://learn.microsoft.com/azure/logic-apps/logic-apps-control-flow-conditional-statement).

- **Implement retry policies and error handling**: Configure appropriate retry policies and error handling to prevent sensitive data exposure through error messages and ensure graceful handling of failures. See [Handle errors and exceptions in Azure Logic Apps](https://learn.microsoft.com/azure/logic-apps/logic-apps-exception-handling).

- **Secure API connections with certificates**: Use client certificate authentication for API connections that support it to provide strong authentication without password-based credentials. See [Client certificate authentication](https://learn.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#authentication-types-supported-triggers-actions).

- **Monitor and secure integration account artifacts**: For B2B scenarios using Integration Accounts, secure trading partner configurations, certificates, and maps with appropriate access controls and monitoring. See [Secure B2B messages by using certificates](https://learn.microsoft.com/azure/logic-apps/logic-apps-enterprise-integration-certificates).

