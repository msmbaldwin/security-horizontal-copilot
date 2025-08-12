---
title: Secure your Azure App Service deployment
description: Learn how to secure Azure App Service, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: app-service
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure App Service deployment

Azure App Service provides a platform-as-a-service (PaaS) environment that enables you to build, deploy, and scale web apps, mobile backends, RESTful APIs, and more. When deploying this service, it's important to follow security best practices to protect your applications, data, and infrastructure.

This article provides guidance on how to best secure your Azure App Service deployment.

## Network security

Azure App Service hosts customer applications on shared infrastructure by default. Implementing proper network security controls is essential to isolate your applications and prevent unauthorized access.

- **Configure private endpoints**: Eliminate public internet exposure by routing traffic to your App Service through your virtual network using Azure Private Link, ensuring secure connectivity for clients in your private networks. See [Use private endpoints for Azure App Service](/azure/app-service/overview-private-endpoint).

- **Implement virtual network integration**: Secure your outbound traffic by enabling your app to access resources in or through an Azure virtual network while maintaining isolation from the public internet. See [Integrate your app with an Azure virtual network](/azure/app-service/overview-vnet-integration).

- **Configure IP restrictions**: Restrict access to your app by defining an allow list of IP addresses and subnets that can access your application, blocking all other traffic. See [Set up Azure App Service access restrictions](/azure/app-service/app-service-ip-restrictions).

- **Use Web Application Firewall**: Enhance protection against common web vulnerabilities and attacks by implementing Azure Front Door or Application Gateway with Web Application Firewall capabilities in front of your App Service. See [Azure Web Application Firewall on Azure Application Gateway](/azure/web-application-firewall/ag/ag-overview).

- **Deploy to App Service Environment**: For maximum network isolation, use an App Service Environment (ASE), which provides a dedicated hosting environment in your virtual network to ensure complete network isolation. See [Introduction to the App Service Environment](/azure/app-service/environment/intro).

## Identity and access management

Properly managing identities and access controls is essential for securing your Azure App Service deployments against unauthorized usage and potential credential theft.

- **Enable managed identities**: Authenticate to Azure services securely without storing credentials in your code or configuration by using managed identities, eliminating the need to manage service principals and connection strings. See [Use managed identities for App Service and Azure Functions](/azure/app-service/overview-managed-identity).

- **Configure authentication and authorization**: Implement App Service Authentication/Authorization to secure your application with Microsoft Entra ID or other identity providers, preventing unauthorized access without writing custom authentication code. See [Authentication and authorization in Azure App Service](/azure/app-service/overview-authentication-authorization).

- **Implement role-based access control**: Restrict access to your App Service resources by assigning the minimum necessary permissions to users and service principals following the principle of least privilege. See [Azure security features for AI services](/azure/ai-services/security-features).

- **Store secrets in Key Vault**: Protect sensitive configuration values like connection strings and API keys by storing them in Azure Key Vault and accessing them using managed identities, rather than storing them in application settings. See [Use Key Vault references for App Service and Azure Functions](/azure/app-service/app-service-key-vault-references).

- **Enable mutual TLS authentication**: Require client certificates for added security when your application needs to verify client identity, particularly for B2B scenarios or internal applications. See [Configure TLS mutual authentication for Azure App Service](/azure/app-service/app-service-web-configure-tls-mutual-auth).

## Data protection

Protecting data in transit and at rest is crucial for maintaining the confidentiality and integrity of your applications and their data.

- **Enforce HTTPS**: Redirect all HTTP traffic to HTTPS by enabling HTTPS-only mode, ensuring that all communication between clients and your app is encrypted. See [Enforce HTTPS](/azure/app-service/configure-ssl-bindings#enforce-https).

- **Configure TLS version**: Use modern TLS protocols by configuring the minimum TLS version to 1.2 or higher, and disable outdated, insecure protocols to prevent potential vulnerabilities. See [Configure TLS settings for your app in Azure App Service](/azure/app-service/configure-ssl-bindings#configure-tls-settings).

- **Manage TLS/SSL certificates**: Secure custom domains by using properly configured TLS/SSL certificates, either with the free App Service managed certificates or custom certificates, to establish trusted connections. See [Add a TLS/SSL certificate in Azure App Service](/azure/app-service/configure-ssl-certificate).

- **Enable at-rest encryption**: Ensure all application data stored in Azure Storage is automatically encrypted using Microsoft-managed keys or customer-managed keys when applicable. See [Azure Storage encryption for data at rest](/azure/storage/common/storage-service-encryption).

- **Protect application settings**: Encrypt sensitive application settings by using Key Vault references for confidential configuration values, ensuring they're not stored in plain text. See [Use Key Vault references for App Service and Azure Functions](/azure/app-service/app-service-key-vault-references).

## Logging and monitoring

Implementing comprehensive logging and monitoring is essential for detecting potential security threats and troubleshooting issues with your Azure App Service deployment.

- **Enable diagnostic logging**: Configure Azure App Service diagnostic logs to track application errors, web server logs, failed request traces, and detailed error messages to identify security issues and troubleshoot problems. See [Enable diagnostics logging for apps in Azure App Service](/azure/app-service/troubleshoot-diagnostic-logs).

- **Integrate with Azure Monitor**: Set up Azure Monitor to collect and analyze logs and metrics from your App Service, enabling comprehensive monitoring and alerting for security events and performance issues. See [Monitor apps in Azure App Service](/azure/app-service/web-sites-monitor).

- **Configure Application Insights**: Implement Application Insights to gain detailed insights into application performance, usage patterns, and potential security issues, with real-time monitoring and analytics capabilities. See [Monitor Azure App Service performance](/azure/azure-monitor/app/azure-web-apps).

- **Set up security alerts**: Create custom alerts to notify you of abnormal usage patterns, potential security breaches, or service disruptions affecting your App Service resources. See [Create, view, and manage metric alerts using Azure Monitor](/azure/azure-monitor/alerts/alerts-metric).

- **Enable health checks**: Configure health checks to monitor your application's operational status and automatically remediate issues when possible. See [Monitor App Service instances using Health check](/azure/app-service/monitor-instances-health-check).

## Compliance and governance

Establishing proper governance and ensuring compliance with relevant standards is crucial for the secure operation of Azure App Service applications.

- **Implement Azure Policy**: Enforce organization-wide security standards for your App Service deployments by creating and assigning Azure Policy definitions that audit and enforce compliance requirements. See [Azure Policy Regulatory Compliance controls for Azure App Service](/azure/app-service/security-controls-policy).

- **Review security recommendations**: Regularly assess your App Service security posture using Microsoft Defender for Cloud to identify and remediate security vulnerabilities and misconfigurations. See [Protect your Azure App Service web apps and APIs](/azure/defender-for-cloud/defender-for-app-service-introduction).

- **Conduct security assessments**: Perform regular security assessments and penetration testing of your App Service applications to identify potential vulnerabilities and security weaknesses. See [Microsoft cloud security benchmark](/security/benchmark/azure/introduction).

- **Maintain regulatory compliance**: Configure your App Service deployments in accordance with applicable regulatory requirements for your industry and region, particularly regarding data protection and privacy. See [Azure compliance documentation](/azure/compliance/).

- **Implement secure DevOps practices**: Establish secure CI/CD pipelines for deploying applications to App Service, including code scanning, dependency checks, and automated security testing. See [DevSecOps in Azure](/azure/architecture/solution-ideas/articles/devsecops-in-azure).

## Backup and recovery

Implementing robust backup and recovery mechanisms is essential for ensuring business continuity and data protection in your Azure App Service deployments.

- **Enable automated backups**: Configure scheduled backups for your App Service applications to ensure you can recover your applications and data in case of accidental deletion, corruption, or other failures. See [Back up and restore your app in Azure App Service](/azure/app-service/manage-backup).

- **Configure backup retention**: Set appropriate retention periods for your backups based on your business requirements and compliance needs, ensuring critical data is preserved for the required duration. See [Back up and restore your app in Azure App Service](/azure/app-service/manage-backup).

- **Implement multi-region deployments**: Deploy your critical applications across multiple regions to provide high availability and disaster recovery capabilities in case of regional outages. See [Tutorial: Create a highly available multi-region app in App Service](/azure/app-service/tutorial-multi-region-app).

- **Test backup restoration**: Regularly test your backup restoration process to ensure backups are valid and can be successfully restored when needed, verifying both application functionality and data integrity. See [Restore an app from a backup](/azure/app-service/manage-backup#restore-an-app-from-a-backup).

- **Document recovery procedures**: Create and maintain comprehensive documentation for recovery procedures, ensuring quick and effective response during service disruptions or disasters. See [Reliability in Azure App Service](/azure/reliability/reliability-app-service).

## Service-specific security

Azure App Service has unique security considerations that should be addressed to ensure the overall security of your web applications.

- **Keep the platform updated**: Ensure your App Service plan and application settings are configured to use the latest available platform versions and patches, reducing exposure to known vulnerabilities. See [Application and service updates in Azure App Service](/azure/app-service/overview-patch-os-runtime).

- **Secure FTP/FTPS deployments**: Disable FTP access or enforce FTPS-only mode when using FTP for deployments to prevent credentials and content from being transmitted in clear text. See [Enforce FTPS](/azure/app-service/deploy-ftp#enforce-ftps).

- **Implement application-level security**: Apply security best practices specific to your application framework or language, such as input validation, output encoding, and secure session management to prevent common vulnerabilities. See [OWASP Top Ten](https://owasp.org/www-project-top-ten/).

- **Protect application code and secrets**: Implement security controls in your application code to protect sensitive information, avoid hardcoding secrets, and implement proper error handling to prevent information disclosure. See [Security in Azure App Service](/azure/app-service/overview-security).

- **Use deployment slots for updates**: Implement deployment slots to validate application updates in an isolated environment before swapping to production, reducing the risk of introducing security issues. See [Set up staging environments in Azure App Service](/azure/app-service/deploy-staging-slots).

## Learn more

- [Microsoft Cloud Security Benchmark – App Service](/security/benchmark/azure/baselines/app-service-security-baseline)
- [Security in Azure App Service](/azure/app-service/overview-security)
- [Azure App Service network features](/azure/app-service/networking-features)
