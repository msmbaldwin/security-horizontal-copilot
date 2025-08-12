---
title: Secure your Azure Load Balancer deployment
description: Learn how to secure Azure Load Balancer, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: load-balancer
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure Load Balancer deployment

Azure Load Balancer is a cloud service that distributes incoming network traffic across backend resources such as virtual machines (VMs) or instances in a virtual machine scale set. When deploying this service, it's important to follow security best practices to protect your infrastructure from potential threats.

This article provides guidance on how to best secure your Azure Load Balancer deployment.

## Network security

Azure Load Balancer operates at layer 4 of the OSI model and serves as the entry point for traffic to your backend resources. Implementing proper network security controls helps protect your resources from unauthorized access and potential attacks.

- **Use Standard Load Balancer SKU**: Standard Load Balancer provides enhanced security features including secure-by-default protection, where all endpoints are closed until you explicitly allow traffic with Network Security Groups. See [Azure Load Balancer SKUs](https://learn.microsoft.com/azure/load-balancer/skus).

- **Configure Network Security Groups**: Implement NSGs on subnets containing your backend resources to control which traffic can reach your workloads. Without NSGs, traffic cannot flow through the load balancer to your backend resources. See [Network security groups overview](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview).

- **Implement internal load balancers for non-public workloads**: Use internal load balancers with private IP addresses to distribute traffic within your virtual network, avoiding direct internet exposure for internal applications. See [Internal Load Balancer](https://learn.microsoft.com/azure/load-balancer/components#frontend-ip-configurations).

- **Enable Source Network Address Translation (SNAT)**: When using public load balancers, implement SNAT to mask the IP addresses of backend resources, providing protection from direct internet exposure. See [Outbound connections in Azure Load Balancer](https://learn.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

- **Deploy DDoS Protection**: Protect public load balancer endpoints from distributed denial-of-service attacks by implementing Azure DDoS Protection. See [Protect your public load balancer with Azure DDoS Protection](https://learn.microsoft.com/azure/load-balancer/tutorial-protect-load-balancer-ddos).

## Identity and access management

Properly managing access to your Azure Load Balancer configuration is essential to prevent unauthorized changes that could compromise your security posture.

- **Implement role-based access control**: Assign the least privileged roles needed for managing your load balancer resources to minimize unauthorized configuration changes. See [Azure built-in roles](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles).

- **Use Azure Private Link for private connectivity**: For cross-virtual network connectivity, implement Azure Private Link to provide secure access without requiring public IP addresses and restrict access from non-peered networks. See [What is Azure Private Link?](https://learn.microsoft.com/azure/private-link/private-link-overview).

- **Authorize private links via RBAC**: Restrict access to private links to only the identities that need it, using role-based access control permissions. See [Azure Private Link role-based access control](https://learn.microsoft.com/azure/private-link/rbac-permissions).

## Data protection

Azure Load Balancer processes traffic in real-time without storing customer data. Implementing proper data protection controls ensures traffic flowing through your load balancer remains secure.

- **Integrate with Azure Application Gateway for HTTPS traffic**: Since Load Balancer operates at Layer 4 and doesn't support SSL/TLS termination, use Application Gateway alongside Load Balancer for HTTPS traffic requiring encryption. See [What is Azure Application Gateway?](https://learn.microsoft.com/azure/application-gateway/overview).

- **Implement end-to-end encryption**: For applications requiring encryption, ensure traffic is encrypted before reaching the load balancer and remains encrypted until it reaches the backend services. See [Network security best practices](https://learn.microsoft.com/azure/security/fundamentals/network-best-practices).

## Logging and monitoring

Effective monitoring of your Azure Load Balancer helps detect potential security issues and ensures your deployment maintains proper security posture.

- **Enable resource logging**: Capture Load Balancer resource logs to monitor traffic patterns and detect potential security anomalies or issues. See [Standard Load Balancer diagnostics with metrics, alerts, and resource logs](https://learn.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics).

- **Configure multidimensional metrics**: Set up multidimensional metrics to gain insights into the performance and health of your load balancer, with appropriate alerting thresholds to detect unusual activity. See [Multidimensional metrics](https://learn.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#multi-dimensional-metrics).

- **Monitor health events**: Track Load Balancer health events to detect changes in backend resource health that could indicate security issues or compromise. See [Load Balancer health event logs](https://learn.microsoft.com/azure/load-balancer/load-balancer-health-event-logs).

- **Implement Azure Monitor Insights**: Use the built-in Azure Monitor Insights dashboard for Load Balancer to visualize key metrics and quickly identify potential security issues. See [Insights for Azure Load Balancer](https://learn.microsoft.com/azure/load-balancer/load-balancer-insights).

## Compliance and governance

Ensure your Azure Load Balancer deployments comply with organizational policies and industry regulations by implementing proper governance controls.

- **Apply Azure Policy definitions**: Use Azure Policy to enforce organizational standards and compliance requirements for your Load Balancer deployments. See [Azure Policy built-in policy definitions for Azure Load Balancer](https://learn.microsoft.com/azure/governance/policy/samples/built-in-policies).

- **Implement infrastructure as code**: Deploy and configure Load Balancer using infrastructure as code to ensure consistent, secure deployments that comply with your organization's security standards. See [Azure Resource Manager templates for Load Balancer](https://learn.microsoft.com/azure/templates/microsoft.network/loadbalancers).

- **Review security baselines**: Regularly review and apply the Azure security baseline for Load Balancer to ensure your deployment follows current security best practices. See [Azure security baseline for Azure Load Balancer](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-load-balancer-security-baseline).

## Service-specific security

Azure Load Balancer has specific security considerations that should be addressed to ensure your deployment is properly protected.

- **Secure backend servers with additional layers**: For designs using Load Balancer as the entry point, implement traffic inspection at the endpoint level. Since Load Balancer doesn't have built-in security features like WAF, add extra security measures for applications requiring advanced protection. See [Integrating Azure Firewall with Azure Load Balancer](https://learn.microsoft.com/azure/firewall/integrate-lb).

- **Configure proper health probes**: Implement appropriate health probes to ensure traffic is only sent to healthy backend instances, preventing potential security issues from unhealthy or compromised resources. See [Load Balancer health probes](https://learn.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview).

- **Use Admin State for maintenance**: During maintenance operations, use Admin State to safely take backend instances out of rotation without disrupting existing connections, ensuring secure maintenance workflows. See [Admin State for Azure Load Balancer](https://learn.microsoft.com/azure/load-balancer/admin-state-overview).

- **Consider using Gateway Load Balancer**: For advanced security scenarios requiring traffic inspection, integrate with Gateway Load Balancer to enable third-party security appliances for traffic inspection. See [What is Azure Gateway Load Balancer?](https://learn.microsoft.com/azure/load-balancer/gateway-overview).

## Learn more

- [Microsoft Cloud Security Benchmark – Azure Load Balancer](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-load-balancer-security-baseline)
- [Well-Architected Framework – Azure Load Balancer guide](https://learn.microsoft.com/azure/well-architected/service-guides/azure-load-balancer)
- [Azure Load Balancer documentation](https://learn.microsoft.com/azure/load-balancer/)
  See also: [Azure Security Documentation](https://learn.microsoft.com/azure/security/).
