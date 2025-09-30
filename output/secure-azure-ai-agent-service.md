---
title: Secure your Azure AI Agent Service deployment
description: Learn how to secure Azure AI Agent Service, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-ai-foundry
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Azure AI Agent Service deployment

Azure AI Agent Service provides capabilities to build, deploy, and manage AI agents with conversational interfaces and tool integrations. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure AI Agent Service deployment.

## Network security

Network security controls are critical for Azure AI Agent Service deployments as they protect agent communications, tool interactions, and data flows from unauthorized access and ensure traffic remains within trusted boundaries.

- **Deploy with private networking using Standard Setup**: Implement complete network isolation by using the Standard Setup with Private Networking option to keep all agent traffic within your virtual network and prevent public internet exposure. See [Create a new network-secured environment with user-managed identity](/azure/ai-foundry/agents/how-to/virtual-networks).

- **Configure private endpoints for all dependent resources**: Establish private endpoints for Azure Storage, Azure AI Search, Cosmos DB, and Azure AI Foundry resources to ensure all data plane operations occur over private connections. See [How to configure a private link for Azure AI Foundry](/azure/ai-foundry/how-to/configure-private-link).

- **Enable network security groups with restrictive rules**: Apply Network Security Groups (NSGs) to subnets hosting agent infrastructure to control traffic by port, protocol, and source IP address with deny-by-default policies. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#network-security).

- **Implement proper subnet sizing and delegation**: Configure dedicated agent subnets with minimum /24 (256 addresses) size and delegate to `Microsoft.App/environments` to support container injection and platform network requirements. See [Create a new network-secured environment with user-managed identity](/azure/ai-foundry/agents/how-to/virtual-networks).

## Identity and access management

Identity and access controls protect Azure AI Agent Service from unauthorized access and ensure proper authentication and authorization for users, services, and agent operations across all deployment models.

- **Use Microsoft Entra ID for authentication**: Replace API key authentication with Microsoft Entra ID to leverage centralized identity management, conditional access policies, and advanced security features. See [Security for Azure AI services](/azure/ai-services/security-features).

- **Assign Azure AI User RBAC role to developers**: Grant users the built-in Azure AI User role at the project scope to enable agent creation and management while maintaining least privilege access principles. See [Built-in enterprise readiness with standard agent setup](/azure/ai-foundry/agents/concepts/standard-agent-setup).

- **Configure managed identities for service authentication**: Enable system-assigned or user-assigned managed identities for Azure AI Foundry projects to authenticate to dependent services without storing credentials. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#identity-management).

- **Implement role-based access control for dependent resources**: Assign specific RBAC roles including Storage Blob Data Contributor, Search Index Data Contributor, and Cosmos DB Built-in Data Contributor to project managed identities on respective resources. See [Built-in enterprise readiness with standard agent setup](/azure/ai-foundry/agents/concepts/standard-agent-setup).

## Data protection

Data protection safeguards agent state, conversation history, files, and vector stores through encryption, key management, and secure storage practices across all Azure AI Agent Service components.

- **Deploy Standard Setup for data sovereignty**: Use Standard Setup configurations to store all agent data (threads, messages, files, vector stores) in your own Azure resources rather than Microsoft-managed multitenant storage. See [Built-in enterprise readiness with standard agent setup](/azure/ai-foundry/agents/concepts/standard-agent-setup).

- **Enable customer-managed keys for encryption**: Configure Azure Key Vault with customer-managed keys (CMK) for encrypting data at rest in Azure Storage, Cosmos DB, and Azure AI Search when regulatory compliance requires additional control. See [Azure security baseline for Azure AI services](/security/benchmark/azure/baselines/cognitive-services-security-baseline#data-protection).

- **Ensure encryption in transit**: Verify all communications use TLS 1.2 or higher encryption for data in transit between agent components, tools, and external services. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#data-protection).

- **Implement project-level data isolation**: Configure separate blob containers and Cosmos DB containers per project to enforce strict data boundaries between different agent deployments and workloads. See [Built-in enterprise readiness with standard agent setup](/azure/ai-foundry/agents/concepts/standard-agent-setup).

## Logging and monitoring

Comprehensive logging and monitoring provide visibility into agent operations, security events, and performance metrics to enable threat detection and operational oversight.

- **Enable Azure Monitor metrics collection**: Configure platform metrics collection for agents, threads, messages, runs, tokens, and tool calls to monitor service health and detect anomalies in agent behavior. See [Monitor Azure AI Foundry Agent Service](/azure/ai-foundry/agents/how-to/metrics).

- **Implement Application Insights integration**: Connect Application Insights to capture detailed traces, performance data, and custom telemetry for comprehensive observability across agent lifecycles. See [Monitor Azure AI Foundry Agent Service](/azure/ai-foundry/agents/how-to/metrics).

- **Configure Azure resource logs**: Enable diagnostic logging to capture detailed service operations, authentication events, and configuration changes for security investigation and compliance audit trails. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#logging-and-threat-detection).

- **Set up alerting for security events**: Create Azure Monitor alerts for suspicious activities, failed authentication attempts, unusual resource usage patterns, and service availability issues. See [Monitor Azure AI Foundry Agent Service](/azure/ai-foundry/agents/how-to/metrics).

## Compliance and governance

Compliance and governance controls ensure Azure AI Agent Service deployments meet regulatory requirements and organizational policies through proper configuration management and audit capabilities.

- **Deploy Microsoft Defender for Cloud protection**: Enable Microsoft Defender for relevant Azure services to monitor security posture, detect threats, and receive security recommendations for agent infrastructure. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#logging-and-threat-detection).

- **Implement Azure Policy for configuration enforcement**: Use Azure Policy definitions to audit and enforce security configurations across Azure AI Foundry resources and ensure compliance with organizational standards. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#asset-management).

- **Configure vulnerability assessment scanning**: Enable Microsoft Defender vulnerability assessment capabilities for compute resources and container images used in agent deployments to identify and remediate security issues. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#posture-and-vulnerability-management).

- **Maintain audit trails for compliance**: Ensure Azure Activity Log and resource logs capture all administrative actions, configuration changes, and access attempts for regulatory compliance and security investigations. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#logging-and-threat-detection).

## Backup and recovery

Backup and recovery strategies protect agent data and ensure business continuity through proper data replication, disaster recovery planning, and service resilience configurations.

- **Configure Cosmos DB backup and recovery**: Implement automated backup policies and point-in-time restore capabilities for Cosmos DB accounts storing agent threads, messages, and metadata to ensure data recovery capabilities. See [What is Azure AI Foundry Agent Service?](/azure/ai-foundry/agents/overview#business-continuity-and-disaster-recovery-bcdr-for-agents).

- **Enable Azure Storage redundancy**: Configure geo-redundant storage (GRS) or geo-zone-redundant storage (GZRS) for Azure Storage accounts containing agent files and vector data to protect against regional failures. See [Azure security baseline for Azure AI Foundry](/security/benchmark/azure/baselines/azure-ai-foundry-security-baseline#backup-and-recovery).

- **Implement multi-region deployment strategy**: Deploy agent infrastructure across multiple Azure regions with proper failover mechanisms to maintain service availability during regional outages. See [What is Azure AI Foundry Agent Service?](/azure/ai-foundry/agents/overview#business-continuity-and-disaster-recovery-bcdr-for-agents).

- **Plan for agent state preservation**: Ensure critical agent configurations, fine-tuned models, and training data are replicated to secondary regions to enable rapid recovery and minimize disruption. See [Customer-enabled disaster recovery](/azure/ai-foundry/how-to/disaster-recovery).

## Service-specific security

Azure AI Agent Service includes unique security controls for agent tools, safety checks, and specialized deployment configurations that address AI-specific risks and operational requirements.

- **Enable safety checks for computer use tools**: Implement built-in safety mechanisms that detect and prevent malicious instructions in computer use scenarios, requiring explicit acknowledgment before executing potentially harmful actions. See [Azure AI Foundry Agent Service Computer Use Tool](/azure/ai-foundry/agents/how-to/tools/computer-use).

- **Implement tool-specific authentication**: Configure secure authentication methods for each agent tool including Azure AI Search RBAC, Logic Apps managed identity, and OpenAPI specification authentication to prevent unauthorized tool access. See [How to use the Azure AI Search tool](/azure/ai-foundry/agents/how-to/tools/azure-ai-search).

- **Configure content filtering and responsible AI**: Apply content filters and responsible AI policies to prevent generation of harmful content and ensure agent responses meet organizational safety and ethical guidelines. See [What is Azure AI Foundry Agent Service?](/azure/ai-foundry/agents/overview#how-do-agents-in-ai-foundry-work).

- **Restrict public egress in private network deployments**: Ensure private network configurations prevent any outbound internet connectivity from agent infrastructure while maintaining access to required Azure services through private endpoints. See [Create a new network-secured environment with user-managed identity](/azure/ai-foundry/agents/how-to/virtual-networks).

