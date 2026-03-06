---
title: Secure your Azure App Service deployment
description: Learn how to secure Azure App Service, with best practices for protecting your web applications, APIs, and infrastructure.
author: 
ms.author: 
ms.service: azure-app-service
ms.topic: conceptual
ms.custom: horz-security
ms.date: 02/19/2026
ai-usage: ai-assisted
---

# Secure your Azure App Service deployment

Azure App Service is a platform-as-a-service (PaaS) offering that enables you to build, deploy, and scale web apps, mobile app backends, RESTful APIs, and function apps. When deploying this service, it's important to follow security best practices to protect your applications, data, and infrastructure.

[!INCLUDE [Security horizontal Zero Trust statement](~/reusable-content/ce-skilling/azure/includes/security/zero-trust-security-horizontal.md)]

This article provides security recommendations to help protect your Azure App Service deployment.

## Network security

App Service supports multiple network security features to isolate your applications and control traffic flow.

- **Configure private endpoints**: Eliminate public internet exposure by routing traffic to your App Service through your virtual network using Azure Private Link. Private endpoints use an IP address from your Azure virtual network address space, and network traffic between clients on your private network and your app traverses over the virtual network and Private Link on the Microsoft backbone network. See [Use private endpoints for Azure App Service apps](/azure/app-service/networking/private-endpoint).
    - Private endpoints are available for Windows and Linux apps hosted on Basic, Standard, PremiumV2, PremiumV3, PremiumV4, IsolatedV2, and Functions Premium plans.

- **Enable virtual network integration**: Secure your outbound traffic by enabling your app to access resources in or through an Azure virtual network. Virtual network integration allows you to use network security groups (NSGs) to block outbound traffic, route tables (UDRs) to send outbound traffic where you want, and NAT gateway to get a dedicated outbound IP. See [Integrate your app with an Azure virtual network](/azure/app-service/overview-vnet-integration).

- **Configure IP access restrictions**: Restrict inbound access to your app by defining a priority-ordered allow/deny list of IP addresses and subnets. When there are one or more entries, an implicit deny all exists at the end of the list. See [Set up Azure App Service access restrictions](/azure/app-service/app-service-ip-restrictions).

- **Set up service endpoint restrictions**: Lock down inbound access to your app from specific subnets in your virtual networks using service endpoints, which work together with IP access restrictions to provide network-level filtering. See [Azure App Service access restrictions](/azure/app-service/overview-access-restrictions).

- **Use Web Application Firewall**: Enhance protection against common web vulnerabilities and attacks by implementing Azure Front Door or Application Gateway with Web Application Firewall (WAF) capabilities in front of your App Service. See [Azure Web Application Firewall on Azure Application Gateway](/azure/web-application-firewall/ag/ag-overview).

- **Use App Service Environment for complete network isolation**: Deploy your apps inside a dedicated App Service Environment in your own Azure Virtual Network instance for complete network isolation from shared infrastructure. App Service Environment provides dedicated public endpoints, internal load balancer (ILB) options for internal-only access, and the ability to use an ILB behind a web application firewall. See [Introduction to Azure App Service Environments](/azure/app-service/environment/intro).

## TLS and HTTPS

Azure App Service supports TLS 1.3, TLS 1.2 (default minimum), and TLS 1.1/1.0 for backward compatibility to ensure secure communication between clients and your applications.

- **Enforce HTTPS-only mode**: Redirect all HTTP traffic to HTTPS by enabling HTTPS-only mode, ensuring that all communication between clients and your app is encrypted. By default, App Service forces a redirect from HTTP requests to HTTPS. See [Configure general settings](/azure/app-service/configure-common#configure-general-settings).

- **Configure minimum TLS version**: Use modern TLS protocols by configuring the minimum TLS version to 1.2 or higher. TLS 1.2 provides strong encryption, broad compatibility, and meets compliance standards like PCI DSS. Configure the minimum TLS version for both your web app and SCM site. See [What is TLS/SSL in Azure App Service?](/azure/app-service/overview-tls).
    - TLS 1.3 is fully supported and introduces stronger security with simplified cipher suites, forward secrecy, and faster handshakes.

- **Configure minimum TLS cipher suite**: Select a cipher suite that blocks weaker encryption algorithms for your web app. The minimum TLS cipher suite includes a fixed list of cipher suites with optimal priority order. See [Minimum TLS cipher suite](/azure/app-service/overview-tls#minimum-tls-cipher-suite).
    - Minimum TLS cipher suite setting is supported on Basic SKUs or higher on multitenant App Service.

- **Manage TLS/SSL certificates**: Secure custom domains by using properly configured TLS/SSL certificates to establish trusted connections. App Service supports free App Service managed certificates, App Service certificates, third-party certificates, and certificates imported from Azure Key Vault. See [Add and manage TLS/SSL certificates in Azure App Service](/azure/app-service/configure-ssl-certificate).

## Identity and access management

Proper identity and access management helps protect your App Service resources and the applications running on them.

- **Authenticate through Microsoft Entra ID**: Configure built-in authentication using Microsoft Entra ID with OAuth 2.0 token-based authentication instead of implementing custom authentication code. Microsoft Entra ID provides enhanced security, centralized identity management, and support for conditional access policies. See [Authentication and authorization in Azure App Service](/azure/app-service/overview-authentication-authorization).

- **Use managed identities for Azure resource access**: Enable managed identities for your App Service to securely access other Azure services without managing credentials. Managed identities provide automatically managed identities in Microsoft Entra ID, eliminating the need to store and protect access keys or connection strings in your application code. See [Managed identities for Azure App Service](/azure/app-service/overview-managed-identity).

- **Apply least privilege access with RBAC**: Use Azure role-based access control (RBAC) to assign permissions to users, groups, and applications at the appropriate scope. Follow the principle of least privilege to limit access rights to only what's necessary for each role. See [What is Azure role-based access control (Azure RBAC)?](/azure/role-based-access-control/overview).

- **Disable basic authentication**: Disable basic username and password authentication for FTP and SCM endpoints in favor of Microsoft Entra ID-based authentication. Basic authentication credentials can be compromised, and Microsoft Entra OAuth 2.0 tokens have limited usable lifetimes and are specific to the applications they're issued for. See [Disable basic authentication in Azure App Service deployments](/azure/app-service/configure-basic-auth-disable).
    - **Require approvals for re-enabling basic authentication**: Use Azure Policy to audit apps that still use basic authentication and remediate noncompliant resources. See [Azure Policy built-in definitions for Azure App Service](/azure/app-service/policy-reference).

- **Restrict CORS to trusted domains**: Implement restrictive cross-origin resource sharing (CORS) policies in your web app to only accept requests from allowed domains, headers, and other criteria. Enforce CORS policies using built-in Azure Policy definitions. See [CORS on Azure App Service](/azure/app-service/app-service-web-tutorial-rest-api#enable-cors).

## Data protection

Protecting data in transit and at rest is crucial for maintaining the confidentiality and integrity of your applications and their data.

- **Store secrets in Azure Key Vault**: Protect sensitive configuration values like database credentials, API tokens, and private keys by storing them in Azure Key Vault and accessing them using managed identities. While app settings are encrypted at rest, Key Vault provides additional capabilities including access policies, audit history, and secret rotation. See [Use Key Vault references for App Service and Azure Functions](/azure/app-service/app-service-key-vault-references).

- **Enable end-to-end TLS encryption**: Enable end-to-end TLS encryption to encrypt front-end-to-worker communication within App Service. Without this feature, traffic from front ends to workers runs unencrypted inside the Azure infrastructure. This feature is available on Premium App Service plans. See [End-to-end TLS encryption in Azure App Service](/azure/app-service/overview-tls#end-to-end-tls-encryption).

- **Configure client certificate authentication**: Require client certificates for mutual TLS authentication when your applications need to verify client identities. This provides an additional layer of security beyond server-side certificates. See [Configure TLS mutual authentication for Azure App Service](/azure/app-service/app-service-web-configure-tls-mutual-auth).

> [!NOTE]
> Web site content in an App Service app, such as files, are stored in Azure Storage, which automatically encrypts the content at rest. Customer supplied secrets are encrypted at rest while stored in App Service configuration databases.

## Logging and monitoring

Implementing comprehensive logging and monitoring is essential for detecting potential security threats and troubleshooting issues.

- **Enable diagnostic logging**: Configure Azure App Service diagnostic logs to track application errors, web server logs, failed request traces, and detailed error messages to identify security issues and troubleshoot problems. See [Enable diagnostics logging for apps in Azure App Service](/azure/app-service/troubleshoot-diagnostic-logs).

- **Enable Microsoft Defender for App Service**: Use Microsoft Defender for App Service to identify attacks targeting applications running over App Service. Defender for App Service assesses the resources covered by your App Service plan, generates security recommendations, and detects threats to your App Service resources including the VM instance, management interface, requests and responses, and underlying sandboxes. See [Protect your web apps and APIs with Microsoft Defender for App Service](/azure/defender-for-cloud/defender-for-app-service-introduction).

- **Integrate with Azure Monitor**: Set up Azure Monitor to collect and analyze logs and metrics from your App Service, enabling comprehensive monitoring and alerting for security events and performance issues. See [Monitor apps in Azure App Service](/azure/app-service/web-sites-monitor).

- **Configure Application Insights**: Implement Application Insights to gain detailed insights into application performance, usage patterns, and potential security issues, with real-time monitoring and analytics capabilities. See [Monitor Azure App Service performance](/azure/azure-monitor/app/azure-web-apps).

- **Set up security alerts**: Create custom alerts to notify you of abnormal usage patterns, potential security breaches, or service disruptions affecting your App Service resources. See [Create, view, and manage metric alerts using Azure Monitor](/azure/azure-monitor/alerts/alerts-metric).

- **Enable health checks**: Configure health checks to monitor your application's operational status and automatically remediate unhealthy instances. See [Monitor App Service instances using Health check](/azure/app-service/monitor-instances-health-check).

## Compliance and governance

Implement governance controls to ensure your App Service deployments meet organizational and regulatory requirements.

- **Review the App Service security baseline**: Review the Microsoft Cloud Security Benchmark v2 security baseline for App Service to understand security controls and recommendations. The baseline provides prescriptive guidance for securing your App Service deployments. See [Azure security baseline for App Service](/security/benchmark/azure/baselines/app-service-security-baseline).

- **Use Azure Policy for compliance**: Deploy Azure Policy to enforce organizational standards and assess compliance at scale. Use built-in policy definitions to audit and enforce security configurations including HTTPS-only, minimum TLS version, and disabling basic authentication. See [Azure Policy built-in definitions for Azure App Service](/azure/app-service/policy-reference).

- **Enable resource locks**: Apply resource locks to prevent accidental deletion or modification of critical App Service resources. See [Lock your resources to protect your infrastructure](/azure/azure-resource-manager/management/lock-resources).

- **Implement deployment slots for safe deployments**: Use deployment slots to test changes in a staging environment before swapping to production. This enables validation of security configurations and application behavior before production deployment. See [Set up staging environments in Azure App Service](/azure/app-service/deploy-staging-slots).

## Backup and recovery

Implementing robust backup and recovery mechanisms ensures business continuity and data protection.

- **Enable automated backups**: Configure scheduled backups for your App Service applications to ensure you can recover your applications and data in case of accidental deletion, corruption, or other failures. Application backups include app configuration, file content, and connected database data. See [Back up and restore your app in Azure App Service](/azure/app-service/manage-backup).
    - Backup and restore is supported in Basic, Standard, Premium, and Isolated tiers. For Basic tier, only production slot backup is supported.

- **Configure backup retention**: Set appropriate retention periods for your backups based on your business requirements and compliance needs, ensuring critical data is preserved for the required duration. See [Back up and restore your app in Azure App Service](/azure/app-service/manage-backup).

- **Implement multi-region deployments**: Deploy your critical applications across multiple regions to provide high availability and disaster recovery capabilities in case of regional outages. See [Tutorial: Create a highly available multi-region app in App Service](/azure/app-service/tutorial-multi-region-app).

- **Test backup restoration**: Regularly test your backup restoration process to ensure backups are valid and can be successfully restored, verifying both application functionality and data integrity. See [Restore an app from a backup](/azure/app-service/manage-backup#restore-an-app-from-a-backup).

## Service-specific security

Azure App Service has unique security considerations based on its PaaS architecture and shared infrastructure model.

### App Service architecture

- **Understand App Service isolation model**: Each app in App Service is segregated from other Azure apps and resources in a secure sandbox. App Service actively secures and hardens its platform components, including Azure VMs, storage, network connections, and web frameworks. Regular updates of VMs and runtime software address newly discovered vulnerabilities. See [Azure App Service security](/azure/app-service/overview-security).

- **Disable remote debugging in production**: Turn off remote debugging for production environments so that inbound ports aren't opened. Remote debugging should only be enabled temporarily for troubleshooting purposes. See [Configure general settings](/azure/app-service/configure-common#configure-general-settings).

- **Use latest runtime and libraries**: Keep your application frameworks and dependencies updated to benefit from security patches and improvements. App Service supports the language runtime support policy for updating existing stacks and retiring end-of-support stacks. See [Language support policy for App Service](/azure/app-service/language-support-policy).

### Deployment security

- **Secure FTP/FTPS deployments**: Disable FTP access or enforce FTPS-only mode when using FTP for deployments to prevent credentials and content from being transmitted in clear text. New apps are set to accept only FTPS by default. See [Deploy your app to Azure App Service using FTP/S](/azure/app-service/deploy-ftp).

- **Configure SCM site access restrictions**: Apply separate access restrictions to the advanced tools site (SCM/Kudu) to control who can access deployment and management endpoints. You can configure unmatched rule behavior and inherit main site rules. See [Azure App Service access restrictions](/azure/app-service/overview-access-restrictions#restrict-access-to-the-advanced-tools-site).

- **Use deployment credentials securely**: App Service supports user-scope and app-scope deployment credentials. Protect these credentials and regenerate them if compromised. Consider using Microsoft Entra-based authentication methods instead. See [Configure deployment credentials for Azure App Service](/azure/app-service/deploy-configure-credentials).

### Appropriate use of App Service

- **Do not pin certificates to default wildcards**: Applications should never pin to the default wildcard (`*.azurewebsites.net`) TLS certificate or an App Service managed certificate. These certificates can be rotated anytime, and certificate-pinned applications will break. Provide a custom TLS certificate for applications that require certificate pinning. See [Best practices for Azure App Service](/azure/app-service/app-service-best-practices#certificate-pinning).

## Next steps

- [Microsoft Cloud Security Benchmark v2 – Azure App Service security baseline](/security/benchmark/azure/baselines/app-service-security-baseline)
- [Well-Architected Framework – App Service (Web Apps) guide](/azure/well-architected/service-guides/app-service-web-apps)
- [Security documentation for Azure App Service](/azure/app-service/overview-security)
- [Zero Trust guidance center](/security/zero-trust/zero-trust-overview)
- [Microsoft Cloud Security Benchmark v2 overview](/security/benchmark/azure/overview)
