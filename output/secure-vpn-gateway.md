---
title: Secure your VPN Gateway deployment
description: Learn how to secure VPN Gateway, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: vpn-gateway
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your VPN Gateway deployment

Azure VPN Gateway provides capabilities to establish secure connections between your Azure Virtual Networks and your on-premises networks, or between different Azure Virtual Networks. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure VPN Gateway deployment.

## Network security

VPN Gateway network security focuses on establishing secure tunnels, isolating traffic, and protecting your connection endpoints from unauthorized access.

- **Configure custom IPsec/IKE policies**: Strengthen your VPN tunnel encryption by specifying custom cryptographic algorithms and key strengths instead of relying on default Azure policies. This enhances protection of your data in transit between networks. See [About VPN Gateway cryptographic requirements](/azure/vpn-gateway/vpn-gateway-about-compliance-crypto).

- **Deploy Network Security Groups**: Protect your VPN Gateway subnet by associating a Network Security Group (NSG) with appropriate security rules. This reduces the attack surface by controlling traffic flow to and from your gateway subnet. See [Network Security Groups overview](/azure/virtual-network/network-security-groups-overview).

- **Implement forced tunneling**: Configure your VPN Gateway to redirect all internet-bound traffic from your Azure VNets back to your on-premises infrastructure for inspection. This ensures all traffic follows your security policies before reaching the internet. See [Configure forced tunneling](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm).

- **Use Traffic Selectors**: Restrict which IP address ranges can send traffic through your VPN tunnel by configuring Traffic Selectors. This provides additional security by limiting access to only authorized network segments. See [About VPN Gateway connection settings](/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

## Identity and access management

Implementing strong identity controls for VPN Gateway helps prevent unauthorized access to your virtual networks and ensures only approved users and devices can establish connections.

- **Enable Microsoft Entra ID authentication for Point-to-Site VPNs**: Replace traditional certificate-based authentication with Microsoft Entra ID to centrally manage user access to your VPN. This provides enhanced security through unified identity management. See [Configure an Azure AD tenant for P2S VPN authentication](/azure/vpn-gateway/openvpn-azure-ad-tenant).

- **Enforce multi-factor authentication for VPN users**: Require MFA for all users connecting through Point-to-Site VPNs to significantly reduce the risk of unauthorized access even if credentials are compromised. See [Enable Azure AD MFA for VPN users](/azure/vpn-gateway/openvpn-azure-ad-mfa).

- **Implement Conditional Access policies**: Apply Conditional Access rules to VPN connections to restrict access based on user location, device compliance, risk level, and other factors. This provides contextual security controls beyond simple authentication. See [Conditional Access for VPN connectivity](/azure/vpn-gateway/openvpn-azure-ad-tenant#enable-conditional-access).

- **Use strong pre-shared keys for Site-to-Site connections**: Implement complex, unique pre-shared keys (PSK) for each site-to-site connection to prevent unauthorized tunnel establishment. Rotate these keys regularly to maintain security. See [How VPN tunnels are authenticated](/azure/vpn-gateway/vpn-gateway-vpn-faq#how-is-my-vpn-tunnel-authenticated).

## Data protection

VPN Gateway inherently provides data protection through encrypted tunnels, but additional measures can further enhance your security posture.

- **Use the strongest available encryption algorithms**: Select the strongest available encryption and hashing algorithms for your VPN tunnels to ensure maximum data protection during transit. Configure custom IPsec/IKE policies using AES-256 and SHA-256 or stronger algorithms. See [About cryptographic requirements and Azure VPN gateways](/azure/vpn-gateway/vpn-gateway-about-compliance-crypto).

- **Implement end-to-end encryption**: For sensitive workloads, implement application-level encryption in addition to the VPN tunnel encryption to provide defense-in-depth for your data. This protects data even if the VPN encryption is compromised. See [Azure encryption overview](/azure/security/fundamentals/encryption-overview).

- **Use higher-tier gateway SKUs for stronger security**: Deploy VPN Gateway using VpnGw2 or higher SKUs, which support stronger IPsec/IKE policies and provide better security capabilities than basic SKUs. This ensures your gateway can enforce the highest security standards. See [Gateway SKUs](/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku).

## Logging and monitoring

Comprehensive monitoring of VPN Gateway activities helps identify security incidents and ensures continuous operation of your secure connections.

- **Enable diagnostic logs**: Configure Azure Monitor to collect detailed VPN Gateway diagnostic logs, including tunnel status, IKE events, and routing information. This provides visibility into connection problems and potential security issues. See [Enable VPN Gateway diagnostic logs](/azure/vpn-gateway/troubleshoot-vpn-with-azure-diagnostics).

- **Create alerts for tunnel disconnections**: Set up monitoring alerts to notify your team when VPN tunnels disconnect or experience problems. This enables rapid response to potential security or connectivity issues. See [Monitor Azure Virtual WAN](/azure/virtual-wan/monitor-virtual-wan).

- **Use Network Watcher VPN troubleshoot**: Regularly run Network Watcher's VPN troubleshoot capability to identify configuration issues and ensure your tunnels are properly secured. This helps detect misconfigurations that could lead to security vulnerabilities. See [Diagnose on-premises VPN connectivity with Azure](/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity#troubleshoot-using-network-watcher-vpn-troubleshoot).

- **Monitor for unusual connection patterns**: Analyze connection logs to detect abnormal patterns such as connection attempts from unexpected locations or at unusual times. This helps identify potential compromise attempts. See [P2SDiagnosticLog](/azure/vpn-gateway/troubleshoot-vpn-with-azure-diagnostics#p2sdiagnosticlog).

## Compliance and governance

Establishing governance controls for your VPN Gateway deployments ensures consistent security and compliance with organizational and regulatory requirements.

- **Implement Azure Policy for VPN Gateway**: Apply Azure Policies to enforce security standards for your VPN Gateway deployments, such as requiring specific SKUs or authentication methods. This ensures consistent compliance with your security requirements. See [Azure Policy built-in definitions for Azure networking services](/azure/networking/policy-reference).

- **Regularly audit VPN configurations**: Perform regular security audits of your VPN Gateway configurations to verify compliance with your security standards and identify any deviations that need remediation. See [Azure security baseline for VPN Gateway](/security/benchmark/azure/baselines/vpn-gateway-security-baseline).

- **Document your VPN topology and security controls**: Maintain comprehensive documentation of your VPN topology, including all connections, authentication methods, and security controls. This facilitates security reviews and compliance assessments. See [VPN Gateway design](/azure/vpn-gateway/design).

## Backup and recovery

Ensuring your VPN Gateway configurations can be quickly restored is essential for maintaining secure connectivity during disruptions.

- **Document VPN Gateway configurations**: Maintain detailed documentation of all VPN Gateway configurations, including connection settings, IPsec policies, and network parameters. This enables rapid reconstruction if needed. See [About VPN Gateway configuration settings](/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

- **Implement Azure Service Health alerts**: Configure alerts for VPN Gateway service issues to receive early notifications of potential problems. This allows for proactive response to service disruptions. See [Azure Service Health](/azure/service-health/service-health-overview).

- **Configure active-active gateways**: Deploy VPN Gateways in active-active mode to provide automatic failover in case one gateway instance becomes unavailable. This enhances availability and ensures continuous secure connections. See [Highly available VPN Gateway connections](/azure/vpn-gateway/vpn-gateway-highlyavailable).

- **Plan for regional failover**: Design your VPN infrastructure with disaster recovery in mind, including the ability to establish connections to secondary regions if the primary region becomes unavailable. See [Cross-premises connectivity during Azure failures](/azure/vpn-gateway/vpn-gateway-highlyavailable-crossconnection).

## Service-specific security

VPN Gateway offers unique security features that should be configured to maximize protection for your specific connectivity scenarios.

- **Use BGP route filtering**: When using BGP with your VPN Gateway, implement prefix filters to control route advertisement and protect against unexpected routing changes. This prevents unauthorized network exposure through route manipulation. See [About BGP with Azure VPN Gateway](/azure/vpn-gateway/vpn-gateway-bgp-overview).

- **Implement ExpressRoute and VPN Gateway coexistence**: Configure ExpressRoute as your primary connection with VPN Gateway as a secure failover path. This provides a secure backup connection if your ExpressRoute circuit experiences issues. See [Configure ExpressRoute and Site-to-Site coexisting connections](/azure/vpn-gateway/vpn-gateway-coexist-resource-manager).

- **Configure split tunneling carefully**: If you implement split tunneling for Point-to-Site VPNs, carefully define which traffic should bypass the tunnel to maintain security while optimizing performance. Improper configuration can lead to security bypasses. See [About Point-to-Site VPN routing](/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing).

- **Keep VPN Gateway and on-premises devices updated**: Regularly update your VPN Gateway to the latest available version and ensure on-premises VPN devices have current firmware. This protects against known vulnerabilities in VPN implementations. See [VPN device configuration samples](/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

## Learn more

- [Microsoft Cloud Security Benchmark – VPN Gateway](/security/benchmark/azure/baselines/vpn-gateway-security-baseline)
- [About VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways)
- [Azure Network Security Overview](/azure/security/fundamentals/network-overview)
- [Azure encryption overview](/azure/security/fundamentals/encryption-overview)
