---
title: Secure your Speech service deployment
description: Learn how to secure Speech service, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-ai-speech
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Speech service deployment

Azure Speech service provides capabilities to convert speech to text, text to speech, and perform real-time speech translation with high accuracy. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure Speech service deployment.

## Network security

Network security controls prevent unauthorized access to Speech service resources and establish secure communication boundaries for speech processing workloads.

- **Enable private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network using Azure Private Link. Private endpoints provide dedicated network interfaces that connect directly to Speech service while maintaining full functionality and performance. See [Use Speech service through a private endpoint](/azure/ai-services/speech-service/speech-services-private-link).

- **Configure Virtual Network service endpoints**: Use Virtual Network service endpoints to secure Speech service resources to only your virtual networks when private endpoints aren't feasible. Service endpoints help secure critical Azure service resources by providing optimized routing over the Azure backbone network. See [Use Speech service through a Virtual Network service endpoint](/azure/ai-services/speech-service/speech-service-vnet-service-endpoint).

- **Restrict network access with firewall rules**: Configure network access rules to allow traffic only from specific IP address ranges or virtual networks. Use the "Selected Networks and Private Endpoints" option to implement network-level access controls that prevent unauthorized access. See [Configure Azure AI services virtual networks](/azure/ai-services/cognitive-services-virtual-networks).

- **Use custom domain names**: Create custom domain names for Speech resources to enable advanced networking features like private endpoints and Virtual Network service endpoints. Custom domains are required for private endpoint scenarios and provide enhanced security configurations. See [Use Speech service through a private endpoint](/azure/ai-services/speech-service/speech-services-private-link#create-a-custom-domain-name).

## Identity and access management

Identity and access management controls ensure that only authorized users and applications can access Speech service resources with appropriate permissions.

- **Use Microsoft Entra authentication**: Replace subscription keys with Microsoft Entra ID authentication to provide better security, auditing, and fine-grained access control. Microsoft Entra authentication supports multi-factor authentication and conditional access policies. See [Microsoft Entra authentication with the Speech SDK](/azure/ai-services/speech-service/how-to-configure-azure-ad-auth).

- **Implement role-based access control**: Assign users and applications the minimum required roles such as "Cognitive Services Speech Contributor" or "Cognitive Services Speech User" rather than using administrative roles. Use Azure RBAC to enforce least-privilege access principles. See [Microsoft Entra authentication with the Speech SDK](/azure/ai-services/speech-service/how-to-configure-azure-ad-auth#assign-roles).

- **Enable managed identities**: Use system-assigned or user-assigned managed identities for applications and services to access Speech service without storing credentials in code. Managed identities eliminate the need to manage API keys and provide automatic credential rotation. See [Azure security baseline for Azure AI services](/security/benchmark/azure/baselines/cognitive-services-security-baseline#identity-management).

- **Rotate API access keys regularly**: If using subscription key authentication, implement regular key rotation to limit exposure from compromised credentials. Use both primary and secondary keys to enable seamless rotation without service interruption. See [Authenticate requests to Azure AI services](/azure/ai-services/authentication#regenerate-keys).

## Data protection

Data protection controls safeguard sensitive speech data through encryption, key management, and secure storage practices.

- **Enable customer-managed keys for encryption**: Configure customer-managed key (CMK) encryption using Azure Key Vault to maintain control over encryption keys for data at rest. This provides additional protection beyond Microsoft-managed encryption and helps meet strict compliance requirements. See [Speech service encryption of data at rest](/azure/ai-services/speech-service/speech-encryption-of-data-at-rest).

- **Implement bring your own storage (BYOS)**: Use BYOS to store Speech service data in your own Azure Storage account with full control over encryption, access policies, and data governance. BYOS enables customer-managed key encryption and provides similar data controls to Customer Lockbox. See [Speech service encryption of data at rest](/azure/ai-services/speech-service/speech-encryption-of-data-at-rest#bring-your-own-storage-byos).

- **Ensure encryption in transit**: Verify all communications use TLS 1.2 or higher encryption for data in transit between applications and Speech service endpoints. All Speech service endpoints enforce TLS 1.2 by default to protect audio data and API communications. See [Security for Azure AI services](/azure/ai-services/security-features).

- **Configure data loss prevention**: Implement data loss prevention controls to restrict the types of URLs and external resources that Speech service can access. This prevents potential data exfiltration through malicious URI submissions in API calls. See [Azure AI services data loss prevention](/azure/ai-services/cognitive-services-data-loss-prevention).

## Logging and monitoring

Comprehensive logging and monitoring provide visibility into Speech service operations, security events, and performance metrics to enable threat detection and operational oversight.

- **Enable diagnostic logging**: Configure diagnostic settings to collect Speech service resource logs and metrics for security monitoring and compliance auditing. Send diagnostic data to Azure Monitor Logs, storage accounts, or Event Hubs for analysis and retention. See [Enable diagnostic logging for Azure AI services](/azure/ai-services/diagnostic-logging).

- **Monitor API usage patterns**: Track Speech service API calls, response times, and error rates to detect anomalous usage patterns that might indicate security threats or service abuse. Use Azure Monitor metrics and custom queries to identify suspicious activity. See [Monitor Azure resources with Azure Monitor](/azure/azure-monitor/essentials/monitor-azure-resource).

- **Set up activity log monitoring**: Monitor Azure Activity Log for Speech service resource changes, configuration modifications, and administrative actions. Configure alerts for critical changes such as network access rule modifications or key regeneration events. See [Azure Activity Log](/azure/azure-monitor/essentials/activity-log).

- **Configure audit logging**: Enable audit logging for custom speech and custom voice operations to track data uploads, model training activities, and deployment changes. Audit logs help maintain compliance and provide forensic capabilities for security investigations. See [Enable diagnostic logging for Azure AI services](/azure/ai-services/diagnostic-logging).

## Compliance and governance

Compliance and governance controls ensure Speech service deployments meet regulatory requirements and organizational policies through proper configuration management and oversight.

- **Apply Azure Policy definitions**: Use Azure Policy to enforce security configurations and monitor compliance across Speech service resources. Apply built-in policies for network access restrictions, encryption requirements, and authentication standards. See [Azure security baseline for Azure AI services](/security/benchmark/azure/baselines/cognitive-services-security-baseline).

- **Implement resource tagging**: Apply consistent resource tags to Speech service resources for cost management, security monitoring, and compliance tracking. Use tags to identify data classification, owner information, and regulatory requirements. See [Use tags to organize your Azure resources](/azure/azure-resource-manager/management/tag-resources).

- **Configure access reviews**: Implement periodic access reviews for Speech service permissions and role assignments to ensure users maintain only necessary access rights. Remove unused accounts and regularly validate business justification for access. See [Azure AD access reviews](/azure/active-directory/governance/access-reviews-overview).

- **Establish data governance policies**: Define and implement data governance policies for speech data processing, retention, and deletion in compliance with privacy regulations like GDPR. Document data flows and processing purposes for Speech service deployments. See [Azure security baseline for Azure AI services](/security/benchmark/azure/baselines/cognitive-services-security-baseline#data-protection).

## Backup and recovery

Backup and recovery controls protect Speech service custom assets and ensure business continuity through proper data protection and disaster recovery planning.

- **Implement cross-region backup for custom models**: Back up custom speech models, custom voice fonts, and speaker recognition profiles to multiple regions to protect against regional outages. Custom assets are region-specific and require manual backup procedures for disaster recovery. See [Back up and recover speech customer resources](/azure/ai-services/speech-service/resiliency-and-recovery-plan).

- **Document recovery procedures**: Create and maintain documented procedures for recreating custom speech models and voice fonts in alternate regions during disaster scenarios. Test recovery procedures regularly to ensure they meet recovery time objectives. See [Back up and recover speech customer resources](/azure/ai-services/speech-service/resiliency-and-recovery-plan).

- **Export training data and configurations**: Regularly export custom speech training data, test datasets, and model configurations to secure storage locations outside the primary region. Maintain version control and documentation for all custom assets to enable rapid recovery. See [Back up and recover speech customer resources](/azure/ai-services/speech-service/resiliency-and-recovery-plan).

- **Plan for service continuity**: Design applications with regional failover capabilities and alternative Speech service endpoints to maintain operations during regional outages. Implement circuit breaker patterns and graceful degradation for speech-dependent features. See [Back up and recover speech customer resources](/azure/ai-services/speech-service/resiliency-and-recovery-plan).

## Service-specific security

Speech service provides unique security capabilities designed specifically for speech processing workloads and audio data protection.

- **Disable endpoint logging**: Turn off endpoint logging for production Speech service resources to prevent storage of customer audio data and transcriptions when not required for business operations. Endpoint logging is disabled by default but should be verified. See [Speech service encryption of data at rest](/azure/ai-services/speech-service/speech-encryption-of-data-at-rest).

- **Configure Speech Studio access controls**: Implement appropriate network access controls for Speech Studio when building custom speech models or custom voice fonts. Use firewall rules to restrict Speech Studio access to authorized IP addresses or virtual networks. See [Use Speech service through a private endpoint](/azure/ai-services/speech-service/speech-services-private-link#use-of-speech-studio).

- **Secure batch transcription workflows**: Configure batch transcription jobs with proper authentication and network isolation when processing large audio files. Use managed identities and secure storage accounts with private endpoints for batch processing scenarios. See [Batch transcription audio data and security](/azure/ai-services/speech-service/batch-transcription-audio-data).

- **Implement voice data protection**: Apply appropriate data protection measures for custom voice training data including consent management, data minimization, and secure deletion procedures. Implement voice biometric data protection controls when using speaker recognition features. See [Speech service encryption of data at rest](/azure/ai-services/speech-service/speech-encryption-of-data-at-rest#bring-your-own-storage-byos).

