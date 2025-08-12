---
title: Secure your Azure DNS deployment
description: Learn how to secure Azure DNS, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: dns
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure DNS deployment

Azure DNS provides a reliable and secure way to host your DNS domains and manage DNS records in Azure. As a critical networking service that translates domain names to IP addresses and routes traffic across the internet, securing your DNS deployment is essential to prevent DNS attacks, unauthorized changes, and service disruptions that could affect your entire infrastructure.

This article provides guidance on how to best secure your Azure DNS deployment.

## Network security

Network security for Azure DNS focuses on protecting DNS infrastructure from external threats and ensuring that DNS resolution services remain available and secure. Proper network controls help prevent DNS attacks and maintain service integrity.

- **Use private DNS zones for internal resources**: Deploy Azure Private DNS zones for internal name resolution within virtual networks to prevent exposure of internal DNS records to public DNS servers. Private DNS zones keep your internal infrastructure hidden from external reconnaissance. See [Azure Private DNS overview](/azure/dns/private-dns-overview).

- **Configure DNS security policies**: Implement DNS security policies to filter and log DNS queries at the virtual network level, protecting against DNS-based attacks by allowing you to block, alert, or allow DNS queries based on domain lists. This helps prevent malicious DNS activity within your networks. See [DNS security policy](/azure/dns/dns-security-policy).

- **Implement alias records for automatic updates**: Use DNS alias records to automatically update IP address references when underlying resources change, preventing security risks from stale DNS entries that might redirect users to compromised or incorrect resources. Alias records also help prevent dangling DNS entries that could lead to subdomain takeover vulnerabilities. See [Azure DNS alias records overview](/azure/dns/dns-alias).

- **Prevent dangling DNS entries**: Ensure you have processes to prevent dangling DNS entries when decommissioning resources. Dangling DNS entries can lead to subdomain takeover vulnerabilities where attackers can claim abandoned subdomains. See [Prevent dangling DNS entries and avoid subdomain takeover](/azure/security/fundamentals/subdomain-takeover#prevent-dangling-dns-entries).

## Identity and access management

Identity and access management for Azure DNS ensures that only authorized users can modify DNS zones and records while following the principle of least privilege. Proper access controls prevent unauthorized DNS changes that could redirect traffic or compromise your services.

- **Implement role-based access control**: Use Azure role-based access control (RBAC) to manage access to DNS resources through built-in role assignments. Assign roles to users, groups, service principals, and managed identities based on their specific responsibilities to enforce least-privilege principles. See [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets#azure-role-based-access-control).

- **Use granular DNS permissions**: Assign specific DNS roles like DNS Zone Contributor or Private DNS Zone Contributor rather than broad administrative roles. Create custom roles for fine-grained control, such as allowing users to manage only specific record types like CNAME records. See [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets#custom-roles).

- **Apply zone-level and record-level access controls**: Configure RBAC permissions at different levels - subscription, resource group, individual DNS zone, or individual record set - to ensure users can only access the DNS resources they need for their responsibilities. See [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets#zone-level-azure-rbac).

- **Implement resource locks for critical zones**: Apply Azure Resource Manager locks to DNS zones and record sets to prevent accidental deletion or modification. Use CanNotDelete locks to prevent zone deletion and ReadOnly locks to prevent all changes. See [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets#resource-locks).

## Data protection

Data protection for Azure DNS focuses on protecting DNS data integrity and preventing unauthorized access to DNS configuration information and query data.

- **Use defense-in-depth for zone protection**: Implement both custom roles and resource locks simultaneously as a comprehensive defense-in-depth approach to protect critical DNS zones from accidental or malicious changes. See [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets#protecting-against-zone-deletion).

- **Implement two-step deletion process**: For critical zones, use custom roles that don't include zone delete permissions. Without delete permissions, administrators must first grant delete permissions and then perform the deletion as a two-step process to prevent accidental zone deletion. See [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets#custom-roles).

- **Protect DNS query integrity**: Implement DNS security policies to filter and monitor DNS queries, helping to detect potential DNS tunneling attempts, queries to known malicious domains, or other indicators of compromise that could indicate data exfiltration through DNS channels. See [DNS security policy](/azure/dns/dns-security-policy).

## Logging and monitoring

Comprehensive logging and monitoring for Azure DNS provides essential visibility into DNS query patterns and potential security events. Logging and monitoring enables effective threat detection, security investigation, and helps meet compliance requirements.

- **Enable Microsoft Defender for DNS**: Use Microsoft Defender for DNS to monitor DNS queries and detect suspicious activities without requiring agents on your resources. Microsoft Defender for DNS provides real-time threat detection for DNS-based attacks including data exfiltration, malware communications, and DNS attacks. See [Overview of Microsoft Defender for DNS](/azure/defender-for-cloud/defender-for-dns-introduction).

- **Configure Azure resource logs**: Enable resource logs for Azure DNS service to capture detailed DNS query information and send them to Log Analytics workspaces, storage accounts, or event hubs for analysis and long-term retention. This ensures you have the necessary data for security investigations and compliance reporting. See [Azure DNS Metrics and Alerts](/azure/dns/dns-alerts-metrics).

- **Set up monitoring and alerting**: Configure Azure Monitor alerts to notify administrators when unusual DNS query patterns, configuration changes, or potential security threats are detected. Create specific alerts to quickly identify and respond to security incidents. See [Monitor Azure DNS](/azure/dns/dns-alerts-metrics).

- **Monitor DNS query analytics**: Analyze DNS query logs to identify unusual traffic patterns, potential DNS tunneling attempts, or queries to known malicious domains. Regular analysis helps detect potential security incidents early. See [DNS security policy](/azure/dns/dns-security-policy).

## Compliance and governance

Asset management for Azure DNS involves implementing governance controls, monitoring configurations, and ensuring compliance with organizational security policies. Proper governance helps maintain security posture and operational visibility.

- **Implement Azure Policy governance**: Use Azure Policy to monitor and enforce configurations of your Azure DNS resources. Configure policies to audit and enforce secure configuration across DNS zones and records. See [Azure Policy built-in definitions for Azure networking services](/azure/networking/policy-reference).

- **Use Microsoft Defender for Cloud**: Configure Azure Policy through Microsoft Defender for Cloud to audit and enforce configurations of your DNS resources. Create alerts when configuration deviations are detected to ensure continuous compliance with security policies. See [Microsoft cloud security benchmark: Network security](/security/benchmark/azure/mcsb-network-security).

- **Apply configuration enforcement**: Use Azure Policy deny and deploy-if-not-exists effects to enforce secure configurations across Azure DNS resources and prevent noncompliant deployments. This ensures all DNS resources maintain a consistent security posture. See [Azure Policy Regulatory Compliance controls for Azure networking services](/azure/networking/security-controls-policy).

- **Implement resource tagging**: Apply consistent resource tags to DNS resources for organization, cost tracking, and security compliance purposes. Proper tagging simplifies resource management and security assessments. See [Use tags to organize your Azure resources and management hierarchy](/azure/azure-resource-manager/management/tag-resources).

## Service-specific security

DNS-specific security measures help protect against common DNS-related threats and ensure the reliability and integrity of your DNS infrastructure.

- **Implement custom domain verification**: When creating DNS entries for Azure App Service, create an asuid.{subdomain} TXT record with the Domain Verification ID. This prevents other Azure subscriptions from validating and taking over the custom domain. See [Prevent dangling DNS entries and avoid subdomain takeover](/azure/security/fundamentals/subdomain-takeover#use-azure-app-services-custom-domain-verification).

- **Secure against DNS-based attacks**: Configure DNS security policies with rules to protect against common DNS-based attacks, including DNS tunneling, DNS amplification, and DNS poisoning. Create block rules for known malicious domains to prevent communication with these targets. See [DNS security policy](/azure/dns/dns-security-policy).

- **Separate public and private DNS resolution**: Maintain clear separation between your public-facing DNS and private internal DNS to limit exposure of internal resources. Use Azure Private DNS for internal name resolution to keep internal infrastructure information from being exposed publicly. See [Azure Private DNS overview](/azure/dns/private-dns-overview).

- **Monitor for accidental changes**: Create alerts to detect unexpected DNS zone or record changes and respond quickly to security incidents or errors. Timely detection of unexpected changes helps prevent or mitigate potential security issues. See [Azure DNS Metrics and Alerts](/azure/dns/dns-alerts-metrics).

## Learn more

- [Microsoft Cloud Security Benchmark – Azure DNS](/security/benchmark/azure/baselines/azure-dns-security-baseline)
- [Security Control: Network Security – DNS](/security/benchmark/azure/security-controls-v3-network-security#ns-10-ensure-domain-name-system-dns-security)
- [How to protect DNS zones and records](/azure/dns/dns-protect-zones-recordsets)
- [DNS security policy](/azure/dns/dns-security-policy)
- [Azure Security Documentation](/azure/security/)
