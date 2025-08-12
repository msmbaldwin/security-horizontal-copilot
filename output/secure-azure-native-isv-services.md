---
title: Secure your Azure Native ISV Services deployment
description: Learn how to secure Azure Native ISV Services, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: partner-solutions
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure Native ISV Services deployment

Azure Native ISV Services provide capabilities to extend Azure platform functionality through validated and certified third-party solutions. When deploying these services, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure Native ISV Services deployment.

## Network security

Proper network security for Azure Native ISV Services ensures that only authorized users and services can access your ISV solution, minimizing potential attack vectors. Implementing network security controls helps protect against unauthorized access and network-based threats.

- **Configure private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network, providing a private and secure connection to your ISV service. See [Azure Private Link for ISV services](/azure/private-link/private-link-overview).

- **Implement Azure DDoS Protection**: Protect your ISV services from distributed denial-of-service attacks by enabling DDoS Protection Standard, which provides enhanced mitigation capabilities. See [Azure DDoS Protection Standard overview](/azure/ddos-protection/ddos-protection-overview).

- **Configure Network Security Groups (NSGs)**: Restrict network traffic to your ISV service resources by implementing NSGs with appropriate inbound and outbound rules. See [Network Security Groups overview](/azure/virtual-network/network-security-groups-overview).

- **Enable Azure Firewall**: Implement centralized network traffic filtering to protect your ISV service resources from potential threats by using Azure Firewall with appropriate rules. See [Azure Firewall documentation](/azure/firewall/overview).

## Identity and access management

Effective identity and access management for Azure Native ISV Services ensures that only authorized users can access your resources, following the principle of least privilege. Implementing strong authentication and authorization controls helps prevent unauthorized access to your ISV solutions.

- **Use Microsoft Entra ID for authentication**: Integrate your ISV services with Microsoft Entra ID to centralize identity management and enable single sign-on, multi-factor authentication, and conditional access policies. See [Microsoft Entra integration with Azure services](/azure/active-directory/fundamentals/active-directory-how-to-integrate).

- **Implement role-based access control**: Restrict access to your ISV service resources by assigning appropriate roles to users based on the principle of least privilege. See [Azure RBAC documentation](/azure/role-based-access-control/overview).

- **Enable Privileged Identity Management**: Implement just-in-time access and approval workflows for privileged roles to reduce the risk of excessive access. See [Azure Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure).

- **Regularly review access permissions**: Periodically audit and review user access rights to ensure that only appropriate users have access to your ISV service resources. See [Access reviews in Microsoft Entra ID](/azure/active-directory/governance/access-reviews-overview).

## Data protection

Protecting data within Azure Native ISV Services is crucial to maintain confidentiality, integrity, and availability. Implementing appropriate data protection controls helps safeguard sensitive information from unauthorized access and potential breaches.

- **Enable encryption in transit**: Ensure all data transmitted to and from your ISV service is encrypted using TLS 1.2 or higher to protect data confidentiality. See [Data encryption in transit](/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit).

- **Configure encryption at rest**: Protect stored data by enabling encryption at rest for all storage accounts and databases used by your ISV service. See [Azure data encryption at rest](/azure/security/fundamentals/encryption-atrest).

- **Implement customer-managed keys**: Use your own encryption keys for enhanced control over data encryption for supported ISV services that integrate with Azure Key Vault. See [Customer-managed keys with Azure Key Vault](/azure/security/fundamentals/encryption-models#client-side-encryption-model).

- **Classify sensitive data**: Identify and classify sensitive data processed by your ISV service to apply appropriate protection controls based on data sensitivity. See [Microsoft Purview Information Protection](/azure/purview/overview).

## Logging and monitoring

Comprehensive logging and monitoring for Azure Native ISV Services provide visibility into system activities, potential security incidents, and compliance status. Implementing robust monitoring helps detect and respond to security threats in a timely manner.

- **Enable diagnostic logging**: Configure diagnostic settings to collect detailed logs from your ISV service for security analysis and troubleshooting. See [Azure Monitor diagnostic settings](/azure/azure-monitor/essentials/diagnostic-settings).

- **Implement Azure Monitor**: Set up monitoring for your ISV service to collect telemetry data, detect anomalies, and receive alerts on potential security issues. See [Azure Monitor overview](/azure/azure-monitor/overview).

- **Configure Microsoft Defender for Cloud**: Enable Microsoft Defender for Cloud to get security recommendations, threat protection, and compliance monitoring for your ISV service resources. See [Microsoft Defender for Cloud](/azure/defender-for-cloud/defender-for-cloud-introduction).

- **Set up Security Information and Event Management (SIEM)**: Integrate with Azure Sentinel or another SIEM solution to centralize security event monitoring and incident response for your ISV service. See [Azure Sentinel documentation](/azure/sentinel/overview).

## Compliance and governance

Establishing strong compliance and governance practices for Azure Native ISV Services ensures alignment with regulatory requirements and organizational policies. Implementing appropriate controls helps maintain compliance and manage risks effectively.

- **Implement Azure Policy**: Define and enforce compliance rules for your ISV service resources using Azure Policy to ensure they meet organizational and regulatory requirements. See [Azure Policy overview](/azure/governance/policy/overview).

- **Use Microsoft Defender for Cloud regulatory compliance**: Monitor your ISV service compliance status against industry standards and regulations using the compliance dashboard. See [Regulatory compliance dashboard](/azure/defender-for-cloud/regulatory-compliance-dashboard).

- **Perform regular security assessments**: Conduct periodic security assessments of your ISV service to identify vulnerabilities and ensure compliance with security standards. See [Microsoft Defender for Cloud secure score](/azure/defender-for-cloud/secure-score-security-controls).

- **Implement Azure Blueprints**: Use Azure Blueprints to define a repeatable set of governance tools and standard Azure resources for your ISV service deployments. See [Azure Blueprints overview](/azure/governance/blueprints/overview).

## Backup and recovery

Implementing robust backup and recovery mechanisms for Azure Native ISV Services ensures business continuity and minimizes data loss in case of failures or disasters. Establishing appropriate backup strategies helps maintain service availability and data integrity.

- **Configure automated backups**: Set up regular automated backups for your ISV service data to prevent data loss and enable point-in-time recovery. See [Azure Backup overview](/azure/backup/backup-overview).

- **Implement disaster recovery**: Create a disaster recovery plan for your ISV service to ensure business continuity in case of regional outages or major failures. See [Azure Site Recovery overview](/azure/site-recovery/site-recovery-overview).

- **Test backup and recovery procedures**: Regularly test your backup and recovery procedures to ensure they work as expected and can be relied upon during actual incidents. See [Test disaster recovery](/azure/site-recovery/site-recovery-test-failover-to-azure).

- **Monitor backup status**: Implement monitoring for backup jobs to ensure they complete successfully and take immediate action on any failures. See [Monitor Azure Backup](/azure/backup/backup-azure-monitoring-built-in).

## Service-specific security

Each Azure Native ISV Service has unique security considerations based on its functionality and architecture. Implementing service-specific security controls helps address the specific security requirements of your ISV solution.

- **Review ISV security documentation**: Understand the security features and best practices specific to your chosen ISV service by reviewing their security documentation. See [Azure Partner Solutions overview](/azure/partner-solutions/overview).

- **Validate ISV service compliance**: Ensure the ISV service meets your compliance requirements by reviewing their compliance certifications and documentation. See [Microsoft Azure compliance offerings](/azure/compliance/offerings/).

- **Configure service-specific security controls**: Implement security controls specific to your ISV service, such as application-level authentication, encryption, and data protection features. See [Azure security technical capabilities](/azure/security/fundamentals/technical-capabilities).

- **Stay updated with security patches**: Regularly apply security updates and patches provided by the ISV to protect against known vulnerabilities. See [Azure update management](/azure/automation/update-management/overview).

## Learn more

- [Microsoft Cloud Security Benchmark](/security/benchmark/azure/introduction)
- [Well-Architected Framework – Security](/azure/well-architected/security/security-principles)
- [Azure Partner Solutions documentation](/azure/partner-solutions/)
  See also: [Azure Security Documentation](/azure/security/).
