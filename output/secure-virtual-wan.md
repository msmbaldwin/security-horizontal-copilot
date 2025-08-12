---
title: Secure your Virtual WAN deployment
description: Learn how to secure Virtual WAN, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: virtual-wan
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Virtual WAN deployment

Azure Virtual WAN is a networking service that brings many networking, security, and routing functionalities together to provide a single operational interface. It enables a global transit network architecture by connecting branches, users, virtual networks, and cloud services through a hub-and-spoke model. When deploying this service, it's important to follow security best practices to protect your network infrastructure, control traffic flow, and prevent unauthorized access.

This article provides guidance on how to best secure your Azure Virtual WAN deployment.

## Network security

Azure Virtual WAN serves as a global transit network backbone for your organization, making network security fundamental to protect traffic flows between branches, users, and cloud workloads. Implementing proper security controls helps isolate resources and prevent unauthorized lateral movement across your network.

- **Convert your Virtual WAN hubs to secured hubs**: Deploy Azure Firewall or supported Network Virtual Appliances (NVAs) directly in your Virtual WAN hub to inspect and filter all transit traffic. This provides a security perimeter for traffic between virtual networks, branches, and the internet without requiring additional hops. See [Install Azure Firewall in a Virtual WAN hub](https://learn.microsoft.com/azure/virtual-wan/howto-firewall).

- **Implement Virtual Hub Routing Intent**: Configure routing intent to direct all traffic through the firewall for inspection before it reaches its destination. This ensures all cross-hub and cross-region traffic is properly secured and filtered. See [Configure routing intent and policies in Azure Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/how-to-routing-policies).

- **Enable DDoS Protection on spoke VNets**: While Azure Virtual WAN secure hubs don't directly support Azure DDoS Protection, deploy DDoS Protection Standard on your spoke virtual networks to protect internet-facing endpoints. See [Azure DDoS Protection Standard overview](https://learn.microsoft.com/azure/ddos-protection/ddos-protection-overview).

- **Configure secure routing policies**: Protect your network from routing misconfigurations by implementing route maps and policies on your on-premises devices. Block overly generic routes (like 0.0.0.0/0), excessively specific routes, and non-Azure prefixes to prevent routing loops and unwanted traffic flows. See [ExpressRoute routing samples](https://learn.microsoft.com/azure/expressroute/expressroute-config-samples-routing#route-maps).

- **Segment traffic using Virtual Hub route tables**: Use virtual hub route tables to control communication paths between different types of connections (VNet, VPN, ExpressRoute) and establish network boundaries between different workload segments. See [About virtual hub routing](https://learn.microsoft.com/azure/virtual-wan/about-virtual-hub-routing).

## Identity and access management

Properly configured identity and access controls help protect Virtual WAN management and connectivity while ensuring only authorized users and devices can connect to your network.

- **Implement Microsoft Entra authentication for P2S VPN**: Use Microsoft Entra ID (formerly Azure AD) authentication for point-to-site VPN connections to enable strong user identity verification with multifactor authentication and conditional access policies. See [Configure P2S OpenVPN with Microsoft Entra authentication](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-point-to-site-azure-ad).

- **Configure user IP address pools for granular access control**: Create different IP address pools for different user groups to enable more specific firewall rules based on user roles and access requirements. This allows you to restrict user access to only required resources. See [Configure user groups and IP address pools for P2S User VPNs](https://learn.microsoft.com/azure/virtual-wan/user-groups-create).

- **Apply role-based access control (RBAC)**: Assign the minimum necessary permissions to users managing Virtual WAN resources using custom Azure roles. Use built-in roles like Network Contributor for day-to-day management and restrict Virtual WAN Administrator role to a limited group. See [Virtual WAN roles and permissions](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-faq#what-are-the-virtual-wan-roles-and-permissions).

- **Secure pre-shared keys for site-to-site VPN**: Store VPN pre-shared keys in Azure Key Vault to centrally manage and audit these critical secrets. Regularly rotate these keys following your organization's security policies. See [Azure Key Vault overview](https://learn.microsoft.com/azure/key-vault/general/overview).

- **Enable multifactor authentication for P2S VPN connections**: Require multifactor authentication for all point-to-site VPN connections to prevent unauthorized access in case of credential compromise. See [Configure MFA authentication for P2S VPN](https://learn.microsoft.com/azure/virtual-wan/openvpn-azure-ad-mfa).

## Data protection

Protecting data as it traverses your Virtual WAN network is essential for maintaining confidentiality and integrity of communications between your various endpoints.

- **Enable encryption for ExpressRoute traffic**: Implement IPsec over ExpressRoute to encrypt private traffic between on-premises networks and Azure virtual networks, providing an additional layer of protection for data in transit. See [ExpressRoute encryption: IPsec over ExpressRoute for Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/vpn-over-expressroute).

- **Configure TLS inspection in Azure Firewall Premium**: Deploy Azure Firewall Premium in your secure hub and enable TLS inspection for critical applications to detect and prevent threats that hide in encrypted traffic. See [Azure Firewall Premium features](https://learn.microsoft.com/azure/firewall/premium-features).

- **Implement Private Link for PaaS services**: Use Azure Private Link to access Azure PaaS services through private endpoints in your spoke virtual networks, eliminating exposure to the public internet. See [What is Azure Private Link](https://learn.microsoft.com/azure/private-link/private-link-overview).

- **Use strong encryption algorithms for VPN connections**: Configure site-to-site and point-to-site VPN connections with strong encryption algorithms and perfect forward secrecy to ensure data confidentiality. See [About cryptography requirements](https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-about-compliance-crypto).

- **Enable Threat Intelligence on Azure Firewall**: Configure Azure Firewall with Threat Intelligence in "Alert and Deny" mode to automatically block network traffic to and from known malicious IP addresses and domains. See [Azure Firewall Threat Intelligence](https://learn.microsoft.com/azure/firewall/threat-intel).

## Logging and monitoring

Comprehensive logging and monitoring are essential for detecting security threats, troubleshooting network issues, and ensuring compliance across your Virtual WAN deployment.

- **Enable Azure Monitor for Virtual WAN**: Configure Azure Monitor Insights for Virtual WAN to gain visibility into the end-to-end topology, connection status, and performance metrics of your global network. This helps identify potential security issues early. See [Azure Monitor Insights for Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/azure-monitor-insights).

- **Configure diagnostic settings for Virtual WAN resources**: Enable resource logs for Virtual WAN hubs, gateways, and connections to capture detailed operational data. Send these logs to Log Analytics for centralized analysis and long-term storage. See [Resource logs for Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/monitor-virtual-wan-reference#diagnostic).

- **Use Azure Firewall workbooks for security insights**: Leverage Azure Firewall workbooks to gain visibility into firewall activities, including denied traffic, IDPS events, and threat intelligence alerts. Regularly review these workbooks to optimize your security rules. See [Azure Firewall workbook](https://learn.microsoft.com/azure/firewall/firewall-workbook).

- **Create alerts for BGP route changes**: Set up alerts to monitor the number and frequency of BGP route changes to detect potential routing issues or malicious route advertisements. This helps maintain the integrity of your network topology. See [Monitor Azure Virtual WAN](https://learn.microsoft.com/azure/virtual-wan/monitor-virtual-wan#monitoring-azure-virtual-wan---best-practices).

- **Integrate with Microsoft Defender for Cloud**: Enable Microsoft Defender for Cloud to get advanced threat protection across your Virtual WAN deployment. Review security recommendations and implement suggested remediations to enhance your security posture. See [Introduction to Microsoft Defender for Cloud](https://learn.microsoft.com/azure/defender-for-cloud/defender-for-cloud-introduction).

## Compliance and governance

Establishing proper governance controls helps ensure your Virtual WAN deployment maintains compliance with organizational policies and regulatory requirements.

- **Implement Azure Policy for Virtual WAN**: Use Azure Policy to enforce and audit security configurations across your Virtual WAN resources. Create custom policies or use built-in policy definitions to maintain compliance with your organization's standards. See [Azure Policy Regulatory Compliance controls for Azure networking services](https://learn.microsoft.com/azure/networking/security-controls-policy).

- **Document network security architecture**: Maintain comprehensive documentation of your Virtual WAN topology, security controls, and routing configurations to support compliance requirements and facilitate security reviews. See [Microsoft cloud security benchmark for Virtual WAN](https://learn.microsoft.com/security/benchmark/azure/baselines/virtual-wan-security-baseline).

- **Conduct regular security assessments**: Periodically review your Virtual WAN security configuration against best practices and compliance requirements. Address any identified gaps or vulnerabilities promptly. See [Azure security baseline for Virtual WAN](https://learn.microsoft.com/security/benchmark/azure/baselines/virtual-wan-security-baseline).

- **Use tags for resource governance**: Apply consistent tags to Virtual WAN resources for better organization, cost management, and compliance tracking. Include tags for business criticality, data classification, and compliance requirements. See [Azure resource tagging decision guide](https://learn.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging).

- **Implement Zero Trust principles**: Apply Zero Trust principles to your Virtual WAN deployment by verifying explicitly, using least privileged access, and assuming breach in your security design. See [Apply Zero Trust principles to an Azure Virtual WAN deployment](https://learn.microsoft.com/security/zero-trust/azure-virtual-wan).

## Backup and recovery

Implementing robust backup and recovery strategies for Virtual WAN ensures business continuity during service disruptions or security incidents.

- **Document manual configuration backup**: Maintain documentation of your Virtual WAN configuration including hub settings, connection details, and routing policies to facilitate quick recovery in case of configuration issues. See [Azure Virtual WAN FAQ](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-faq).

- **Implement multi-region redundancy**: Deploy Virtual WAN hubs in multiple Azure regions with appropriate connections to on-premises locations to provide geographic redundancy and increase your overall disaster recovery capabilities. See [Virtual WAN network topology](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/virtual-wan-network-topology).

- **Configure active-active VPN gateways**: Establish redundant VPN connections from on-premises locations to ensure continuity if a single connection fails. Use BGP routing to facilitate automatic failover between connections. See [About BGP with Azure VPN Gateway](https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview).

- **Enable ExpressRoute redundancy**: For critical workloads, implement redundant ExpressRoute circuits connecting to different Virtual WAN hubs to maintain connectivity during circuit failures or maintenance events. See [ExpressRoute design considerations](https://learn.microsoft.com/azure/expressroute/expressroute-optimize-routing).

- **Create a disaster recovery plan**: Develop and regularly test a disaster recovery plan that includes procedures for recovering Virtual WAN connectivity in case of major service disruptions. Include scenarios for hub failures, connection issues, and regional outages. See [Business continuity and disaster recovery for Azure applications](https://learn.microsoft.com/azure/architecture/framework/resiliency/backup-and-recovery).

## Service-specific security

These security recommendations address features and capabilities specific to Azure Virtual WAN that require additional security considerations.

- **Secure branch connectivity with SD-WAN partners**: When using partner SD-WAN solutions with Virtual WAN, follow vendor-specific security recommendations and ensure the integration is configured according to security best practices. See [Virtual WAN partners and locations](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-locations-partners).

- **Implement security for spoke VNets**: Apply micro-segmentation within spoke VNets using Network Security Groups (NSGs) and Application Security Groups (ASGs) to regulate traffic flows within virtual networks connected to Virtual WAN. See [Network Security Groups](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview).

- **Configure secure hub routing intent**: Use routing intent to ensure all inter-spoke traffic is inspected by Azure Firewall. This prevents lateral movement between workloads in different virtual networks in case of compromise. See [Virtual hub routing intent and routing policies](https://learn.microsoft.com/azure/virtual-wan/how-to-routing-policies).

- **Secure internet-bound traffic**: Configure your secure Virtual WAN hub as the default route for internet-bound traffic from connected spokes to ensure all outbound traffic is inspected by Azure Firewall or your chosen network virtual appliance. See [Secure your internet access with Azure Firewall in a Virtual WAN hub](https://learn.microsoft.com/azure/virtual-wan/howto-firewall-internet-traffic).

- **Deploy Application Gateway in spoke VNets**: For inbound HTTP/HTTPS traffic, deploy Azure Application Gateway with Web Application Firewall (WAF) in spoke VNets to provide application-layer protection for web applications. See [Web Application Firewall on Azure Application Gateway](https://learn.microsoft.com/azure/web-application-firewall/ag/ag-overview).

## Learn more

- [Microsoft Cloud Security Benchmark – Virtual WAN](https://learn.microsoft.com/security/benchmark/azure/baselines/virtual-wan-security-baseline)
- [Virtual WAN network topology](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/virtual-wan-network-topology)
- [Apply Zero Trust principles to an Azure Virtual WAN deployment](https://learn.microsoft.com/security/zero-trust/azure-virtual-wan)
- [What is Azure Virtual WAN?](https://learn.microsoft.com/azure/virtual-wan/virtual-wan-about)
- [Azure Security Documentation](https://learn.microsoft.com/azure/security/)
