---
title: Secure your Azure AI Foundry deployment
description: Learn how to secure Azure AI Foundry, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-ai-foundry
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Azure AI Foundry deployment

Azure AI Foundry provides capabilities to build, customize, evaluate, and operate AI agents and applications with production-grade infrastructure. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure AI Foundry deployment.

## Network security

Network security controls prevent unauthorized access to Azure AI Foundry resources and establish secure communication boundaries for AI workloads.

- **Enable private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network using Azure Private Link. Private endpoints provide dedicated network interfaces that connect directly to Azure AI Foundry while maintaining full functionality and performance. See [Configure private link for Azure AI Foundry](/azure/ai-foundry/how-to/configure-private-link).

- **Configure managed virtual networks**: Use managed virtual network isolation to secure your Azure AI Foundry hub and projects with automated network provisioning. This creates an isolated environment where inbound access is only allowed through private endpoints while outbound access can be configured as needed. See [Configure managed virtual network for Azure AI Foundry](/azure/ai-foundry/how-to/configure-managed-network).

- **Deploy network security perimeters**: Implement network security perimeter (preview) to restrict data-plane access and group Azure AI Foundry with other protected resources within a logical security boundary. This reduces data exfiltration risk by containing traffic within defined access rules. See [Add Azure AI Foundry to a network security perimeter](/azure/ai-foundry/how-to/add-foundry-to-network-security-perimeter).

- **Disable public network access**: Configure Azure AI Foundry resources to disable public network access when using private endpoints, ensuring all connectivity occurs through your controlled virtual network environment. See [Configure private link for Azure AI Foundry](/azure/ai-foundry/how-to/configure-private-link).

- **Secure dependent resources**: Create private endpoints for all supporting services including Azure Storage, Azure Key Vault, Azure Container Registry, and Azure Cosmos DB to maintain consistent security posture across your AI workload components. See [Network isolation for connections](/azure/ai-foundry/how-to/connections-add#network-isolation).

## Identity and access management

Proper identity and access controls ensure only authorized users and services can access Azure AI Foundry resources and perform appropriate operations.

- **Implement role-based access control**: Use Azure AI Foundry built-in roles (Azure AI User, Azure AI Project Manager, Azure AI Account Owner) to provide least-privilege access based on user responsibilities. Start with the Reader role and elevate permissions only when necessary. See [Role-based access control for Azure AI Foundry](/azure/ai-foundry/concepts/rbac-azure-ai-foundry).

- **Enable managed identities**: Configure system-assigned or user-assigned managed identities for Azure AI Foundry resources to provide secure, passwordless authentication to other Azure services. This eliminates the need to store credentials in code or configuration files. See [Azure AI Foundry architecture](/azure/ai-foundry/concepts/architecture#security-driven-separation-of-concerns).

- **Configure Microsoft Entra ID integration**: Use Microsoft Entra ID for centralized identity management and enable conditional access policies to control access based on user, location, and device conditions. This provides enterprise-grade authentication capabilities. See [Role-based access control for Azure AI Foundry](/azure/ai-foundry/concepts/rbac-azure-ai-foundry).

- **Enable keyless authentication**: Configure Microsoft Entra ID authentication for model deployments instead of using API keys to provide more secure and auditable access. This approach eliminates key management overhead and improves security posture. See [Configure keyless authentication](/azure/ai-foundry/foundry-models/how-to/configure-entra-id).

- **Rotate API access keys**: Regularly rotate API keys used for service authentication to limit exposure from compromised credentials. Implement automated key rotation where possible to maintain security without operational overhead. See [Rotate API access keys](/azure/ai-services/rotate-keys).

## Data protection

Data protection controls safeguard sensitive information processed by AI workloads through encryption, key management, and access controls.

- **Configure customer-managed keys**: Enable customer-managed key (CMK) encryption using Azure Key Vault to maintain control over encryption keys for data at rest. This provides additional protection beyond Microsoft-managed encryption and helps meet strict compliance requirements. See [Customer-managed keys for encryption with Azure AI Foundry](/azure/ai-foundry/concepts/encryption-keys-portal).

- **Enable Key Vault integration**: Connect your own Azure Key Vault to Azure AI Foundry to manage connection secrets and encryption keys. This provides centralized key management and supports compliance requirements for key custody. See [Customer-managed keys for encryption with Azure AI Foundry](/azure/ai-foundry/concepts/encryption-keys-portal).

- **Disable shared key access**: Disable shared key access to Azure Storage accounts used by Azure AI Foundry to enforce Microsoft Entra ID authentication for all storage operations. This eliminates the risk of compromised storage account keys. See [Disable shared key access to storage](/azure/ai-foundry/how-to/disable-local-auth).

- **Implement data classification**: Use Microsoft Purview to classify and protect sensitive data used in AI applications. This ensures appropriate handling of confidential information and supports compliance with data protection regulations. See [Use Microsoft Purview capabilities for AI apps](purview/developer/secure-ai-with-purview).

- **Configure Azure Storage encryption**: Ensure all Azure Storage accounts used by Azure AI Foundry have encryption at rest enabled with either Microsoft-managed or customer-managed keys. Data is encrypted using FIPS 140-2 compliant AES-256 encryption by default. See [Azure AI Foundry architecture](/azure/ai-foundry/concepts/architecture#data-storage).

## Logging and monitoring

Comprehensive logging and monitoring capabilities provide visibility into Azure AI Foundry operations and enable threat detection.

- **Configure Application Insights monitoring**: Set up Application Insights integration to capture telemetry, traces, and performance metrics from Azure AI Foundry Agent Service. This provides detailed observability for debugging and performance optimization. See [Monitor Azure AI Foundry Agent Service](/azure/ai-foundry/agents/how-to/metrics).

- **Enable Azure Monitor metrics**: Use Azure Monitor platform metrics to track agent operations, token usage, tool calls, and system performance. Configure alerts based on these metrics to proactively identify issues. See [Monitor Azure AI Foundry Agent Service](/azure/ai-foundry/agents/how-to/metrics).

- **Configure diagnostic settings**: Enable diagnostic settings to send Azure AI Foundry logs to Azure Monitor Log Analytics, Azure Storage, or Azure Event Hubs for centralized log management and analysis. This supports security monitoring and compliance requirements. See [Enable NSP logging](/azure/azure-monitor/fundamentals/network-security-perimeter).

- **Implement security monitoring**: Enable Microsoft Defender for Cloud to provide security monitoring and threat detection for AI workloads. Activate AI-specific threat protection to monitor security risks and misconfigurations. See [Trustworthy AI for Azure AI Foundry](/azure/ai-foundry/responsible-use-of-ai-overview).

- **Enable audit logging**: Configure Azure activity logs and diagnostic logs to track administrative operations, resource changes, and access patterns. This provides an audit trail for security investigations and compliance reporting. See [Azure AI Foundry architecture](/azure/ai-foundry/concepts/architecture#security-driven-separation-of-concerns).

## Compliance and governance

Governance controls ensure Azure AI Foundry deployments meet organizational policies and regulatory requirements while maintaining operational consistency.

- **Apply Azure Policy controls**: Implement built-in Azure Policy definitions for Azure AI Foundry to enforce security configurations, compliance requirements, and operational standards across your AI resources. Use policies to restrict model access and deployment options. See [Audit and manage Azure AI Foundry resources](/azure/ai-foundry/how-to/azure-policy).

- **Enable conditional access policies**: Configure Microsoft Entra conditional access policies to control access to Azure AI Foundry portal and APIs based on user identity, location, device compliance, and risk level. This provides additional security layers beyond basic authentication. See [Conditional access policies](/azure/ai-foundry/how-to/azure-policy#conditional-access-policies).

- **Configure cost management**: Implement cost monitoring and budgets using Azure Cost Management to track spending across Azure AI Foundry resources and projects. Set up alerts to prevent unexpected charges and optimize resource utilization. See [Plan and manage costs for Azure AI Foundry](/azure/ai-foundry/how-to/costs-plan-manage).

- **Implement data loss prevention**: Use Microsoft Purview Data Loss Prevention (DLP) policies to detect and prevent sharing of sensitive information through AI applications. This protects against data leaks and ensures compliance with privacy regulations. See [Protect against data leaks with Purview](purview/developer/secure-ai-with-purview#protect-against-data-leaks-and-insider-risks).

- **Enable content filtering**: Configure Azure AI Content Safety integration to detect and filter harmful content in AI inputs and outputs. This includes protection against copyrighted material, inappropriate content, and security threats. See [Content filtering in Azure AI Foundry](/azure/ai-foundry/concepts/content-filtering).

## Backup and recovery

Business continuity planning ensures Azure AI Foundry services remain available during outages and enables rapid recovery from disruptions.

- **Deploy across multiple regions**: Implement multi-region deployments for production Azure AI Foundry workloads to provide redundancy and ensure high availability during regional outages. Deploy both resources and supporting infrastructure across geographically distributed regions. See [Customer-enabled disaster recovery](/azure/ai-foundry/how-to/disaster-recovery).

- **Configure Cosmos DB backup**: For Azure AI Foundry Agent Service, provision and manage your own Cosmos DB account with appropriate backup and recovery policies configured. This ensures agent state can be preserved and recovered during regional outages. See [Business continuity and disaster recovery for agents](/azure/ai-foundry/agents/overview#business-continuity-and-disaster-recovery-bcdr-for-agents).

- **Enable storage redundancy**: Configure geo-redundant storage (GRS) or read-access geo-redundant storage (RA-GRS) for Azure Storage accounts used by Azure AI Foundry to protect data against regional disasters and enable recovery capabilities. See [Design for high availability](/azure/ai-foundry/how-to/disaster-recovery#design-for-high-availability).

- **Implement automated failover**: Set up Azure Traffic Manager or Azure Front Door to provide automated failover between regional deployments. This ensures continuous service availability during primary region outages without manual intervention. See [Business continuity and disaster recovery with Azure OpenAI](/azure/ai-foundry/openai/how-to/business-continuity-disaster-recovery).

- **Test disaster recovery procedures**: Regularly test disaster recovery plans including data restoration processes and service failover procedures to validate effectiveness and identify gaps. Document test results and update procedures based on lessons learned. See [Manage AI business continuity](/azure/cloud-adoption-framework/scenarios/ai/manage#manage-ai-business-continuity).

## Service-specific security

Azure AI Foundry provides unique security capabilities designed specifically for AI workloads and agent-based applications.

- **Configure Agent Service security**: Use Standard Setup with private networking for Azure AI Foundry Agent Service to ensure no public egress, container network injection, and private resource access while maintaining authentication and security controls. See [Create network-secured environment with user-managed identity](/azure/ai-foundry/agents/how-to/virtual-networks).

- **Enable responsible AI controls**: Implement content safety filters, evaluation frameworks, and monitoring controls to ensure AI outputs meet quality and safety standards. Use built-in evaluators to assess model performance and detect potential risks. See [Trustworthy AI for Azure AI Foundry](/azure/ai-foundry/responsible-use-of-ai-overview).

- **Secure model deployments**: Use private endpoints for model deployments and configure network access controls to prevent unauthorized access to AI models. Implement authentication and authorization controls for model inference endpoints. See [Configure private link for Azure AI Foundry](/azure/ai-foundry/how-to/configure-private-link).

- **Configure connection security**: Secure connections to external data sources and services using managed identities, private endpoints, and encrypted channels. Validate that all connected resources follow the same security standards as your Azure AI Foundry deployment. See [Add connections to Azure AI Foundry](/azure/ai-foundry/how-to/connections-add).

- **Implement project isolation**: Use Azure AI Foundry project containers to provide secure boundaries for access control, files, agents, and evaluations. This ensures proper separation of concerns between different use cases and teams. See [Azure AI Foundry architecture](/azure/ai-foundry/concepts/architecture#security-driven-separation-of-concerns).

