---
title: Secure your Azure Application Gateway deployment
description: Learn how to secure Azure Application Gateway, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: application-gateway
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure Application Gateway deployment

Azure Application Gateway is a web traffic load balancer that operates at the application layer (OSI layer 7), enabling you to manage traffic to your web applications. When deploying this service, it's important to follow security best practices to protect your applications, data, and infrastructure from various threats and vulnerabilities.

This article provides guidance on how to best secure your Azure Application Gateway deployment.

## Network security

Implementing proper network security controls for your Azure Application Gateway helps minimize the attack surface and protect against unauthorized access and network-based attacks.

- **Deploy into a dedicated subnet**: Place your Application Gateway in a dedicated subnet within your virtual network to provide network isolation and enable granular traffic control. This separation helps contain potential security incidents and enables targeted security policies. See [Application Gateway infrastructure configuration](https://learn.microsoft.com/azure/application-gateway/configuration-infrastructure).

- **Apply Network Security Groups**: Configure Network Security Groups (NSGs) to restrict traffic by port, protocol, source IP address, or destination IP address. Create NSG rules to limit access to only required ports and prevent management ports from being accessed from untrusted networks. See [Network security groups](https://learn.microsoft.com/azure/application-gateway/configuration-infrastructure#network-security-groups).

- **Configure private link**: Deploy private endpoints for your Application Gateway to establish private access points that eliminate exposure to the public internet. This reduces your attack surface by keeping traffic within your virtual network. See [Configure Azure Application Gateway Private Link](https://learn.microsoft.com/azure/application-gateway/private-link-configure).

- **Enable DDoS protection**: Deploy Azure DDoS Network Protection on the virtual network hosting your Application Gateway to protect against large-scale DDoS attacks. This provides enhanced DDoS mitigation capabilities including adaptive tuning and attack notifications. See [Protect your application gateway with Azure DDoS Network Protection](https://learn.microsoft.com/azure/application-gateway/tutorial-protect-application-gateway-ddos).

- **Implement private-only deployment**: Configure a private-only Application Gateway deployment to eliminate risk of data exfiltration and control privacy of communication within the virtual network. This creates a zero-trust network model with no public internet exposure. See [Private-only Application Gateway deployment](https://learn.microsoft.com/azure/application-gateway/application-gateway-private-deployment).

## Web application protection

Web Application Firewall (WAF) provides essential protection against common web vulnerabilities and attacks targeting your applications, helping to defend against OWASP Top 10 threats.

- **Deploy Web Application Firewall**: Enable WAF on your Application Gateway to protect against common web vulnerabilities including SQL injection, cross-site scripting, and other web attacks. Start in Detection mode to understand traffic patterns, then switch to Prevention mode to actively block threats. See [What is Azure Web Application Firewall on Azure Application Gateway?](https://learn.microsoft.com/azure/web-application-firewall/ag/ag-overview).

- **Configure custom WAF rules**: Create custom rules to address specific threats targeting your applications, including rate limiting, IP blocking, and geo-filtering. Custom rules provide targeted protection beyond managed rule sets. See [Create and use custom rules](https://learn.microsoft.com/azure/web-application-firewall/ag/create-custom-waf-rules).

- **Enable bot protection**: Use the bot protection managed rule set to identify and block malicious bots while allowing legitimate traffic from search engines and monitoring tools. This helps prevent automated attacks and scraping attempts. See [Configure bot protection](https://learn.microsoft.com/azure/web-application-firewall/ag/bot-protection).

- **Implement rate limiting**: Configure rate limiting rules to prevent abuse and DDoS attacks by controlling the number of requests allowed from individual IP addresses within specified time windows. This mitigates brute force attacks and other high-volume request patterns. See [Rate limiting overview](https://learn.microsoft.com/azure/web-application-firewall/ag/rate-limiting-overview).

- **Use the latest Core Rule Set (CRS)**: Deploy the most recent OWASP Core Rule Set version available in WAF to ensure you're protected against the latest known vulnerabilities and attack patterns. Newer rule sets include improved detection capabilities and fewer false positives. See [Web Application Firewall CRS rule groups and rules](https://learn.microsoft.com/azure/web-application-firewall/ag/application-gateway-crs-rulegroups-rules).

## Identity and access management

Proper authentication and authorization controls ensure only authorized users and systems can access your Application Gateway and its configuration.

- **Implement role-based access control**: Apply Azure RBAC to limit who can modify Application Gateway configurations. Assign the minimum necessary permissions to users and service principals following the principle of least privilege. See [Azure built-in roles](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles).

- **Configure mutual authentication**: Implement mutual TLS authentication to verify client certificates, providing an extra layer of security for sensitive applications. This ensures both the client and server authenticate each other. See [Configure mutual authentication with Application Gateway](https://learn.microsoft.com/azure/application-gateway/mutual-authentication-portal).

- **Use managed identities for Key Vault access**: Configure managed identities when integrating Application Gateway with Azure Key Vault for certificate storage. This eliminates the need to manage credentials manually and provides a more secure authentication method. See [TLS termination with Key Vault certificates](https://learn.microsoft.com/azure/application-gateway/key-vault-certs).

- **Implement conditional access policies**: Use Microsoft Entra ID conditional access policies to control access to Application Gateway management based on factors such as user location, device compliance, and risk detection. See [Conditional Access: Cloud apps, actions, and authentication context](https://learn.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-cloud-apps).

## Data protection

Data protection for Application Gateway focuses on securing data in transit through encryption and proper certificate management.

- **Enable TLS encryption**: Configure TLS termination to encrypt data in transit between clients and your Application Gateway. Use TLS v1.2 or later and disable legacy versions like SSL 3.0 and TLS v1.0/v1.1 to protect against known vulnerabilities. See [Overview of TLS termination and end to end TLS with Application Gateway](https://learn.microsoft.com/azure/application-gateway/ssl-overview).

- **Store certificates in Azure Key Vault**: Use Azure Key Vault to securely store and manage your TLS certificates instead of embedding them in configuration files. This enables automatic certificate rotation and centralized management of secrets. See [TLS termination with Key Vault certificates](https://learn.microsoft.com/azure/application-gateway/key-vault-certs).

- **Implement HTTP to HTTPS redirection**: Configure automatic redirection from HTTP to HTTPS to ensure all traffic is encrypted. This prevents sensitive data from being transmitted in plaintext and protects against eavesdropping. See [Create an application gateway with HTTP to HTTPS redirection](https://learn.microsoft.com/azure/application-gateway/redirect-http-to-https-portal).

- **Configure end-to-end TLS**: Enable TLS encryption between Application Gateway and backend servers for maximum data protection throughout the entire communication path. This ensures data remains encrypted from client to backend service. See [Overview of TLS termination and end to end TLS with Application Gateway](https://learn.microsoft.com/azure/application-gateway/ssl-overview).

- **Implement secure certificate management**: Set up automatic rotation of certificates in Azure Key Vault based on a defined schedule or when approaching expiration. Ensure certificate generation follows security standards with sufficient key sizes and appropriate validity periods. See [Configure an Application Gateway with TLS termination](https://learn.microsoft.com/azure/application-gateway/create-ssl-portal#configuration-tab).

## Logging and monitoring

Comprehensive logging and monitoring provide visibility into Application Gateway operations and help detect potential security threats.

- **Enable diagnostic logging**: Configure Azure resource logs to capture detailed information about Application Gateway operations, including access patterns, performance metrics, and security events. Send these logs to a Log Analytics workspace for analysis. See [Backend health and diagnostic logs for Application Gateway](https://learn.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

- **Configure WAF logging**: Enable detailed logging for WAF to capture information about detected threats, blocked requests, and rule matches. This provides visibility into attack patterns and helps tune your security configurations. See [Web Application Firewall logs](https://learn.microsoft.com/azure/web-application-firewall/ag/web-application-firewall-logs).

- **Set up monitoring and alerting**: Create alerts based on Application Gateway metrics and logs to detect unusual traffic patterns, failed authentication attempts, or performance anomalies that might indicate security issues. Use Azure Monitor to establish baseline performance and identify deviations. See [Application Gateway metrics](https://learn.microsoft.com/azure/application-gateway/application-gateway-metrics).

- **Implement centralized log management**: Integrate Application Gateway logs with your security information and event management (SIEM) system to correlate events across your infrastructure and enable automated threat detection and response. See [Send logs to Azure Monitor](https://learn.microsoft.com/azure/azure-monitor/essentials/diagnostic-settings).

- **Configure custom health probes**: Set up custom health probes to monitor backend server health more effectively than default probes. Custom probes can detect application-level issues and ensure traffic only reaches healthy servers. See [Application Gateway health probes overview](https://learn.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## Compliance and governance

Implementing compliance and governance controls ensures your Application Gateway configurations adhere to organizational policies and security standards.

- **Implement Azure Policy governance**: Use Azure Policy to audit and enforce configurations across your Application Gateway deployments. Create policies that prevent insecure configurations and ensure compliance with security standards. See [Azure Policy built-in definitions for Azure networking services](https://learn.microsoft.com/azure/networking/policy-reference).

- **Enable Microsoft Defender for Cloud**: Use Microsoft Defender for Cloud to continuously monitor your Application Gateway configurations and receive alerts when deviations from security baselines are detected. Set up automated remediation where possible to maintain consistent security posture. See [Introduction to Microsoft Defender for Cloud](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-cloud-introduction).

- **Apply resource locks**: Implement Azure resource locks to prevent accidental deletion or modification of critical Application Gateway resources. This helps maintain the availability and integrity of your security configurations. See [Lock your resources to protect your infrastructure](https://learn.microsoft.com/azure/azure-resource-manager/management/lock-resources).

- **Implement tags for governance**: Apply consistent tagging to Application Gateway resources to facilitate governance, cost management, and operational management. Tags help identify resource ownership, business purpose, and security requirements. See [Use tags to organize your Azure resources](https://learn.microsoft.com/azure/azure-resource-manager/management/tag-resources).

## Service-specific security

Application Gateway offers additional security features that address specific security considerations unique to this service.

- **Configure URL path-based routing**: Implement URL path-based routing to direct different types of requests to specific backend pools optimized for handling those requests. This improves security by ensuring sensitive endpoints are processed by appropriately secured backend servers. See [URL path-based routing overview](https://learn.microsoft.com/azure/application-gateway/url-route-overview).

- **Enable multi-site hosting securely**: When hosting multiple sites behind a single Application Gateway, configure separate listeners with dedicated TLS certificates for each site. This ensures proper encryption and isolation between different applications. See [Multiple site hosting](https://learn.microsoft.com/azure/application-gateway/multiple-site-overview).

- **Implement connection draining**: Configure connection draining to ensure graceful removal of backend instances during updates or maintenance. This prevents connection disruptions that could lead to security issues or application errors. See [Connection draining](https://learn.microsoft.com/azure/application-gateway/features#connection-draining).

- **Scale appropriately for traffic volume**: Configure autoscaling for Application Gateway v2 to ensure sufficient capacity during traffic spikes or DDoS attacks. Inadequate scaling can lead to service disruption during high-volume events. See [Application Gateway high traffic support](https://learn.microsoft.com/azure/application-gateway/high-traffic-support).

- **Implement header rewriting**: Configure header rewriting rules to remove sensitive information from HTTP headers before forwarding to backend servers. This reduces information disclosure and helps maintain security between different application tiers. See [Rewrite HTTP headers with Application Gateway](https://learn.microsoft.com/azure/application-gateway/rewrite-http-headers-url).

## Learn more

- [Microsoft Cloud Security Benchmark – Application Gateway](https://learn.microsoft.com/security/benchmark/azure/baselines/application-gateway-security-baseline)
- [Well-Architected Framework – Azure Application Gateway](https://learn.microsoft.com/azure/well-architected/service-guides/azure-application-gateway)
- [Security documentation for Application Gateway](https://learn.microsoft.com/azure/application-gateway/secure-application-gateway)
