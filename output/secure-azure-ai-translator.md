---
title: Secure your Azure AI Translator deployment
description: Learn how to secure Azure AI Translator, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-ai-translator
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
---

# Secure your Azure AI Translator deployment

Azure AI Translator is a cloud-based neural machine translation service that translates text and documents between languages in near real time. When deploying this service, it's important to follow security best practices to protect your data, API keys, and resources from unauthorized access or exposure.

This article provides guidance on how to best secure your Azure AI Translator deployment.

## Network security

Proper network security for Azure AI Translator helps protect your translation resources from unauthorized access and potential exploitation while ensuring legitimate traffic can still reach the service.

- **Configure firewall settings**: Restrict access to your Translator resource by configuring network isolation through the Azure portal. This prevents unauthorized requests and reduces your attack surface. See [Use Azure AI Translator behind firewalls](https://learn.microsoft.com/en-us/azure/ai-services/translator/how-to/use-firewalls).

- **Enable Virtual Network (VNet) service endpoints**: Secure your Custom Translator resources by routing traffic through your VNet instead of over the public internet. This provides enhanced security by eliminating exposure to public networks. See [Enable Azure AI Custom Translator through Azure Virtual Network](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/how-to/enable-vnet-service-endpoint).

- **Use private endpoints for sensitive deployments**: Implement Azure Private Link to establish private connectivity to Azure AI Translator, ensuring your data doesn't traverse the public internet. This provides an additional layer of network isolation for highly secure environments. See [Azure AI services Virtual Network](https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-virtual-networks).

## Identity and access management

Proper identity and access controls for Azure AI Translator protect your API keys and prevent unauthorized usage of your translation resources.

- **Implement Azure role-based access control (RBAC)**: Assign the minimum necessary permissions to users who need to manage your Translator resources, following the principle of least privilege. This reduces the risk of unauthorized access or accidental misconfigurations. See [Azure RBAC for Azure AI services](https://learn.microsoft.com/en-us/azure/ai-services/authentication?tabs=portal#azure-role-based-access-control-azure-rbac).

- **Store API keys securely**: Never embed API keys directly in your application code or source control. Instead, use Azure Key Vault to securely store and access your Translator service keys, preventing unauthorized access and key exposure. See [Azure Key Vault Overview](https://learn.microsoft.com/en-us/azure/key-vault/general/overview).

- **Rotate API keys regularly**: Periodically regenerate your Translator service keys to minimize the impact of potential key compromise. Implement a key rotation process that updates applications with new keys without service disruption. See [Regenerate your API keys](https://learn.microsoft.com/en-us/azure/ai-services/authentication?tabs=portal#regenerate-your-api-keys).

- **Use managed identities for Azure resources**: When integrating Translator with other Azure services like Azure Functions or App Service, utilize managed identities instead of connection strings or keys to eliminate the need to store credentials. See [What are managed identities for Azure resources?](https://learn.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview).

## Data protection

Azure AI Translator processes sensitive text data that may contain personal or proprietary information. Implementing proper data protection measures ensures this information remains secure throughout the translation process.

- **Enable customer-managed keys (CMK)**: For enhanced control over encryption, configure customer-managed keys with Azure Key Vault for your regional Translator resources. This gives you complete ownership of the encryption keys protecting your data. See [Azure AI Translator encryption of data at rest](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/concepts/encrypt-data-at-rest#customer-managed-keys-with-azure-key-vault).

- **Use regional resources for data sovereignty**: When data residency is important, deploy Translator resources in specific regions rather than using the global option. This helps ensure your data is processed within geographic boundaries that comply with your regulatory requirements. See [Create a Translator resource](https://learn.microsoft.com/en-us/azure/ai-services/translator/how-to/create-translator-resource#complete-your-project-and-instance-details).

- **Implement TLS/SSL for all communications**: Always use HTTPS when calling the Translator APIs to ensure data in transit is encrypted. Never transmit API keys or sensitive text over unencrypted connections. See [Use Azure AI Translator APIs](https://learn.microsoft.com/en-us/azure/ai-services/translator/text-translation/how-to/use-rest-api).

- **Understand the no-trace policy**: Be aware that Translator doesn't persist customer data submitted for translation. Text translation processes data at REST without storage, while document translation temporarily stores data during processing but deletes it after completion. See [Data, privacy, and security for Azure AI Translator](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/translator/data-privacy-security).

## Logging and monitoring

Effective logging and monitoring help detect and respond to potential security incidents involving your Azure AI Translator resources.

- **Enable diagnostic logging**: Configure diagnostic settings for your Translator resources to capture usage metrics, API calls, and system events. This provides visibility into access patterns and potential security anomalies. See [Azure AI services logging](https://learn.microsoft.com/en-us/azure/ai-services/diagnostic-logging).

- **Set up alerts for unusual activity**: Configure alerts in Azure Monitor to notify you of abnormal usage patterns, such as sudden spikes in API calls or access attempts from unusual locations. This helps identify potential unauthorized access or service abuse. See [Create, view, and manage metric alerts using Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-metric).

- **Monitor service health**: Regularly check the Azure Service Health dashboard for any security advisories or service issues related to Azure AI Translator. Stay informed about security patches and necessary actions. See [Azure Service Health](https://learn.microsoft.com/en-us/azure/service-health/service-health-overview).

- **Implement request rate monitoring**: Track API usage patterns to identify abnormal access patterns that might indicate misuse of your Translator resources. Set up monitoring to detect when requests approach or exceed service limits. See [Service and request limits](https://learn.microsoft.com/en-us/azure/ai-services/translator/service-limits).

## Compliance and governance

Adhering to regulatory requirements and implementing proper governance ensures your Azure AI Translator deployment meets security and compliance standards.

- **Review compliance offerings**: Understand which compliance certifications Azure AI services adhere to, such as SOC, ISO, and GDPR, to ensure your Translator deployment meets your regulatory requirements. See [Azure compliance documentation](https://learn.microsoft.com/en-us/azure/compliance/).

- **Implement data handling policies**: Define and enforce policies for how translation data is handled within your organization, including who can submit content for translation and how translated content is stored and shared. See [Azure AI Translator Transparency Note](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/translator/transparency-note).

- **Conduct regular security assessments**: Periodically review your Translator service configuration for potential security vulnerabilities, such as overly permissive network settings or improperly secured API keys. See [Azure security baseline for Azure AI services](https://learn.microsoft.com/en-us/security/benchmark/azure/baselines/cognitive-services-security-baseline).

- **Document security controls**: Maintain documentation of all security controls implemented for your Translator deployment to facilitate compliance audits and security reviews. See [Microsoft cloud security benchmark](https://learn.microsoft.com/en-us/security/benchmark/azure/overview).

## Service-specific security

Azure AI Translator has unique security considerations related to its translation functionality and integration patterns.

- **Implement a human oversight process**: For sensitive translations, establish a human review workflow to validate translations before they're used in production scenarios. This helps prevent potential issues from inaccurate or inappropriate translations. See [Azure AI Translator Transparency Note](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/translator/transparency-note#evaluating-and-integrating-azure-ai-translator-for-your-use).

- **Secure custom translation models**: When using Custom Translator, implement appropriate controls to protect training data and custom models, particularly when they contain proprietary terminology or sensitive information. See [What is Custom Translator?](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/overview#securely-translate-anytime-anywhere-on-all-your-apps-and-services).

- **Validate translation quality**: Establish quality validation procedures for translations to ensure they maintain the security context of sensitive information and don't introduce vulnerabilities through mistranslation of security-related content. See [Azure AI Translator Transparency Note](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/translator/transparency-note#evaluating-and-integrating-azure-ai-translator-for-your-use).

- **Use containers for isolated environments**: For high-security scenarios requiring air-gapped environments, consider deploying Translator as a container. This provides translation capabilities in isolated environments without direct internet connectivity. See [Install and run Translator containers](https://learn.microsoft.com/en-us/azure/ai-services/translator/containers/translator-how-to-install-container).

## Learn more

- [Azure security baseline for Azure AI services](https://learn.microsoft.com/en-us/security/benchmark/azure/baselines/cognitive-services-security-baseline)
- [Azure AI services security documentation](https://learn.microsoft.com/en-us/azure/ai-services/security-features)
- [Data, privacy, and security for Azure AI Translator](https://learn.microsoft.com/en-us/azure/ai-foundry/responsible-ai/translator/data-privacy-security)
  See also: [Azure Security Documentation](https://learn.microsoft.com/azure/security/).
