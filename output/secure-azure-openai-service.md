---
title: Secure your Azure OpenAI Service deployment
description: Learn how to secure Azure OpenAI Service, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-ai-openai
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Azure OpenAI Service deployment

Azure OpenAI Service provides access to powerful large language models with enterprise-grade security and privacy controls. When deploying this service, it's important to follow security best practices to protect data, model access, and infrastructure from unauthorized use and potential threats.

This article provides guidance on how to best secure your Azure OpenAI Service deployment.

## Network security

Network security controls are fundamental to protecting Azure OpenAI resources from unauthorized access and establishing secure communication boundaries. Proper network configuration prevents data exfiltration and ensures that AI models are accessible only through approved channels.

- **Enable private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network. Private endpoints provide a secure, private connection that keeps your AI models isolated from the public internet. See [Configure Azure OpenAI networking](/azure/ai-foundry/openai/how-to/network).

- **Disable public network access**: Block all internet-based connections to prevent unauthorized access attempts and reduce your attack surface. This forces all communication through private endpoints within your controlled network environment. See [Network and access configuration for Azure OpenAI On Your Data](/azure/ai-foundry/openai/how-to/on-your-data-configuration#configure-azure-openai).

- **Implement network security perimeter**: Establish logical network boundaries around your platform services to control public network access through explicit rules. This approach prevents data exfiltration while maintaining necessary connectivity for applications. See [Configure network security perimeter for Azure OpenAI](/azure/ai-foundry/openai/how-to/network-security-perimeter).

- **Configure virtual network integration**: Deploy resources within a virtual network to create network-level isolation and enable secure communication between related Azure services. This provides an additional layer of network security beyond standard firewall rules. See [Configure Virtual Networks for Azure AI services](/azure/ai-services/cognitive-services-virtual-networks).

- **Implement proper DNS configuration**: Configure private DNS zones to ensure secure name resolution for private endpoints and prevent DNS hijacking attacks. This ensures that applications resolve service names to private IP addresses within your network. See [Network and access configuration for Azure OpenAI On Your Data](/azure/ai-foundry/openai/how-to/on-your-data-configuration#web-app).

## Identity and access management

Identity and access management controls ensure that only authorized users and applications can access Azure OpenAI resources. Proper authentication and authorization mechanisms prevent credential-based attacks and unauthorized model usage.

- **Use Microsoft Entra ID authentication**: Replace API keys with Microsoft Entra ID authentication to eliminate long-lived secrets and enable centralized identity management. This approach provides better audit trails and supports conditional access policies. See [Authenticate requests to Azure AI services](/azure/ai-services/authentication).

- **Enable managed identity authentication**: Configure system-assigned or user-assigned managed identities to eliminate the need for stored credentials in applications. Managed identities automatically rotate and are securely managed by Azure. See [Use managed identities with Azure OpenAI](/azure/ai-foundry/openai/how-to/managed-identity).

- **Implement role-based access control (RBAC)**: Assign users and applications the minimum required permissions using built-in roles like `Cognitive Services OpenAI User` or `Cognitive Services OpenAI Contributor`. This principle of least privilege reduces the impact of compromised accounts. See [Azure OpenAI Service RBAC roles](/azure/ai-services/openai/how-to/role-based-access-control).

- **Configure document-level access control**: Implement fine-grained access controls for data sources used with Azure OpenAI On Your Data to ensure users only access information appropriate for their roles. Use group-based permissions to manage access at scale. See [Network and access configuration for Azure OpenAI On Your Data](/azure/ai-foundry/openai/how-to/on-your-data-configuration#document-level-access-control).

- **Secure API key storage**: When API keys are necessary, store them securely in Azure Key Vault rather than embedding them in code or configuration files. This centralizes secret management and enables audit trails for key access. See [API keys with Azure Key Vault](/azure/key-vault/general/apps-api-keys-secrets).

## Data protection

Data protection mechanisms safeguard sensitive information processed by Azure OpenAI models, including training data, prompts, and generated responses. Strong data protection prevents unauthorized data access and maintains compliance with privacy regulations.

- **Enable customer-managed keys (CMK)**: Use your own encryption keys stored in Azure Key Vault to maintain full control over data encryption and meet regulatory compliance requirements. Customer-managed keys provide additional protection for sensitive training data and fine-tuned models. See [Azure OpenAI encryption of data at rest](/azure/ai-foundry/openai/encrypt-data-at-rest).

- **Configure content filtering**: Implement content filtering policies to prevent the generation of harmful, inappropriate, or sensitive content. Customize filtering thresholds based on your organization's risk tolerance and use case requirements. See [Azure OpenAI Service content filtering](/azure/ai-foundry/openai/concepts/content-filter).

- **Enable data loss prevention (DLP)**: Configure DLP capabilities to control which external URLs your Azure OpenAI resources can access, preventing accidental data exfiltration through model interactions. This creates an additional control layer for data protection. See [Configure data loss prevention for Azure AI services](/azure/ai-services/cognitive-services-data-loss-prevention).

- **Implement prompt injection protection**: Enable prompt shield features to detect and block potential prompt injection attacks that could manipulate model behavior or extract sensitive information from training data. See [Content filtering for prompt shields](/azure/ai-foundry/openai/concepts/content-filter-prompt-shields).

- **Configure groundedness detection**: For retrieval-augmented generation scenarios, enable groundedness detection to ensure model responses are based on provided source materials rather than hallucinated information. See [Content filtering for groundedness detection](/azure/ai-foundry/openai/concepts/content-filter-groundedness).

## Logging and monitoring

Comprehensive logging and monitoring provide visibility into Azure OpenAI usage patterns, security events, and potential threats. Proper monitoring enables rapid detection and response to security incidents.

- **Enable diagnostic settings**: Configure diagnostic settings to collect resource logs and metrics for security analysis and compliance reporting. Route logs to Azure Monitor Logs for advanced querying and correlation with other security events. See [Monitor Azure OpenAI](/azure/ai-foundry/openai/how-to/monitor-openai).

- **Configure Azure Monitor alerts**: Set up alerts for suspicious activities such as unusual usage patterns, failed authentication attempts, or content filtering violations. Automated alerts enable rapid response to potential security incidents. See [Monitor Azure OpenAI](/azure/ai-foundry/openai/how-to/monitor-openai#analyze-monitoring-data).

- **Enable Azure Activity Log monitoring**: Monitor subscription-level events to track resource creation, configuration changes, and administrative actions. Activity logs provide audit trails for compliance and security investigations. See [Monitor Azure OpenAI](/azure/ai-foundry/openai/how-to/monitor-openai#azure-activity-log).

- **Implement abuse monitoring**: Leverage Azure OpenAI's built-in abuse monitoring capabilities to detect and prevent misuse of the service. Configure appropriate thresholds and response actions based on your organization's policies. See [Abuse monitoring](/azure/ai-foundry/openai/concepts/abuse-monitoring).

- **Monitor content filtering events**: Track content filtering activities to understand usage patterns and identify potential policy violations or attack attempts. Use this data to refine filtering policies and improve security posture. See [Monitor Azure OpenAI](/azure/ai-foundry/openai/how-to/monitor-openai).

## Compliance and governance

Compliance and governance controls ensure that Azure OpenAI deployments meet regulatory requirements and organizational policies. These controls provide the foundation for responsible AI usage and risk management.

- **Implement Azure Policy**: Use Azure Policy to enforce consistent security configurations across Azure OpenAI resources, such as requiring private endpoints or specific encryption settings. Policy enforcement prevents configuration drift and ensures compliance. See [Azure Policy for Azure OpenAI](/azure/governance/policy/samples/built-in-policies#cognitive-services).

- **Enable Microsoft Purview integration**: Use Microsoft Purview to classify and govern data used with Azure OpenAI, ensuring appropriate handling of sensitive information throughout the AI lifecycle. See [Microsoft Purview security](purview/purview-security).

- **Configure responsible AI practices**: Implement responsible AI governance frameworks to ensure ethical use of AI models and compliance with organizational values and regulatory requirements. See [Responsible AI for Azure OpenAI](/azure/ai-foundry/responsible-ai/openai/).

- **Maintain compliance documentation**: Document your Azure OpenAI security configurations, policies, and procedures to support compliance audits and demonstrate adherence to regulatory requirements. See [Azure security baseline for Azure OpenAI](/security/benchmark/azure/baselines/azure-openai-security-baseline).

- **Implement data residency controls**: Configure regional deployments and data zone deployments to meet data residency requirements and ensure that data processing occurs within approved geographical boundaries. See [Azure OpenAI Service regions](/azure/ai-foundry/openai/concepts/models#model-summary-table-and-region-availability).

## Backup and recovery

Business continuity and disaster recovery planning ensure that Azure OpenAI services remain available during outages and that critical AI workloads can be restored quickly after incidents.

- **Implement multi-region deployments**: Deploy Azure OpenAI resources across multiple regions to provide redundancy and ensure service availability during regional outages. Use global deployments when possible for automatic failover capabilities. See [Business Continuity and Disaster Recovery considerations with Azure OpenAI](/azure/ai-foundry/openai/how-to/business-continuity-disaster-recovery).

- **Configure API Management for load balancing**: Deploy Azure API Management with circuit breaker patterns to distribute traffic across multiple Azure OpenAI endpoints and provide automatic failover during service disruptions. See [Business Continuity and Disaster Recovery considerations with Azure OpenAI](/azure/ai-foundry/openai/how-to/business-continuity-disaster-recovery#supporting-infrastructure).

- **Back up fine-tuned models**: Regularly export and back up custom fine-tuned models to prevent data loss and ensure models can be restored in secondary regions. Store backups in secure, geographically distributed locations. See [Fine-tuning Azure OpenAI models](/azure/ai-foundry/openai/how-to/fine-tuning).

- **Test disaster recovery procedures**: Regularly test failover procedures and validate that backup systems function correctly during simulated outages. Document recovery time objectives and update procedures based on test results. See [Business Continuity and Disaster Recovery considerations with Azure OpenAI](/azure/ai-foundry/openai/how-to/business-continuity-disaster-recovery).

- **Plan for data recovery**: Develop procedures for recovering training data, prompt templates, and other critical assets used with Azure OpenAI services. Ensure that recovery procedures account for dependencies between AI services and related infrastructure. See [Business Continuity and Disaster Recovery considerations with Azure OpenAI](/azure/ai-foundry/openai/how-to/business-continuity-disaster-recovery).

## Service-specific security

Azure OpenAI Service has unique security considerations related to AI model behavior, prompt engineering, and content generation that require specialized security controls beyond traditional infrastructure protection.

- **Configure model-specific content filters**: Customize content filtering policies for each deployed model based on its intended use case and risk profile. Different models may require different filtering thresholds based on their capabilities and deployment context. See [Configure content filters](/azure/ai-foundry/openai/how-to/content-filters).

- **Implement prompt engineering safeguards**: Design system messages and prompt templates that include security instructions to guide model behavior and prevent malicious use. Use clear boundaries and instructions to maintain model alignment with organizational policies. See [System message framework and template recommendations for Large Language Models (LLMs)](/azure/ai-foundry/openai/concepts/system-message).

- **Enable protected material detection**: Configure detection for copyrighted text and code to prevent intellectual property violations and ensure compliance with content licensing requirements. See [Content filtering for protected material](/azure/ai-foundry/openai/concepts/content-filter-protected-material).

- **Implement rate limiting and quota management**: Configure appropriate rate limits and quota allocations to prevent abuse and ensure fair usage across applications and users. Monitor quota consumption to detect unusual usage patterns. See [Azure OpenAI Service quotas and limits](/azure/ai-foundry/openai/quotas-limits).

- **Configure streaming security**: When using streaming responses, implement appropriate security controls for content filtering and monitoring. Consider using synchronous filtering for sensitive applications to ensure complete content review. See [Content streaming](/azure/ai-foundry/openai/concepts/content-streaming).

