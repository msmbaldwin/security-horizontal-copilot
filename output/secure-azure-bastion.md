---
title: Secure your Azure Bastion deployment
description: Learn how to secure Azure Bastion, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: bastion
ms.topic: conceptual
ms.custom: horz-security
ms.date: 08/12/2025
ai-usage: ai-assisted
---

# Secure your Azure Bastion deployment

Azure Bastion is a fully managed PaaS service that provides secure and seamless RDP/SSH connectivity to your virtual machines directly over TLS. When you connect via Azure Bastion, your virtual machines don't need a public IP address, agent, or special client software. This service helps eliminate exposure of RDP/SSH ports to the internet while still allowing secure remote administration.

This article provides guidance on how to best secure your Azure Bastion deployment.

## Network security

Azure Bastion acts as a secure gateway for accessing your VMs, making network security configuration critical to maintaining its protective barrier between your infrastructure and potential threats.

- **Deploy in a dedicated subnet**: Deploy Azure Bastion in its own dedicated subnet named 'AzureBastionSubnet' with a minimum size of /26 or larger to ensure proper isolation and functionality. See [Tutorial: Deploy Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal).

- **Configure Network Security Groups properly**: Create appropriate NSG rules for both the Bastion subnet and target VM subnets. For the Bastion subnet, allow inbound traffic on port 443 from the Internet, and ensure proper control plane and data plane communication. See [Working with NSG access and Azure Bastion](https://learn.microsoft.com/azure/bastion/bastion-nsg).

- **Restrict RDP/SSH access to only come through Bastion**: Configure NSGs on your target VM subnets to allow RDP/SSH traffic only from the Azure Bastion subnet, blocking direct internet access to these protocols. See [Working with NSG access and Azure Bastion](https://learn.microsoft.com/azure/bastion/bastion-nsg).

- **Use private-only deployment for premium security**: For enhanced security, consider using the private-only deployment option (available in Premium SKU) to restrict all Bastion traffic to private networks without public internet exposure. See [Private-only deployment](https://learn.microsoft.com/azure/bastion/private-only-deployment).

## Identity and access management

Proper identity management helps ensure that only authorized users can access your Azure Bastion and connected VMs.

- **Implement least privilege access**: Grant users only the permissions required to perform their tasks. Users connecting to VMs need Reader role on the target VM, its NIC, and the Bastion resource. See [Azure security baseline for Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline#privileged-access).

- **Utilize Azure role-based access control**: Apply RBAC to control who has permission to manage your Bastion resource while restricting VM access to only those users who require it. See [Azure security baseline for Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline#identity-management).

- **Enable Kerberos authentication**: Configure Kerberos authentication for enhanced security when connecting to Windows VMs through Azure Bastion, enabling single sign-on capabilities. See [Kerberos authentication portal](https://learn.microsoft.com/azure/bastion/kerberos-authentication-portal).

- **Store SSH keys securely in Azure Key Vault**: Use Azure Key Vault to store and manage SSH private keys for Linux VM connections, adding an additional layer of protection and centralized key management. See [Connect: Using a private key stored in Azure Key Vault](https://learn.microsoft.com/azure/bastion/bastion-connect-vm-ssh-linux#akv).

## Data protection

Data protection measures help secure the data flowing through your Bastion service and to your virtual machines.

- **Leverage built-in TLS encryption**: Azure Bastion automatically encrypts all data in transit using TLS 1.2, ensuring secure communications over port 443. No additional configuration is required for this protection. See [Azure security baseline for Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline#data-protection).

- **Disable copy/paste functionality when required**: For Standard and Premium SKUs, configure the copy/paste restriction to prevent sensitive data transfer between the local device and remote sessions. See [What is Azure Bastion?](https://learn.microsoft.com/azure/bastion/bastion-overview#skus).

- **Enable session recording for auditing**: For Premium SKU, enable session recording to capture and audit remote sessions for security monitoring and compliance purposes. See [Session recording](https://learn.microsoft.com/azure/bastion/session-recording).

- **Use shareable links with proper expiration**: When using shareable links feature (Standard and Premium SKUs), implement appropriate expiration times to limit the window of potential exposure. See [Shareable link](https://learn.microsoft.com/azure/bastion/shareable-link).

## Logging and monitoring

Comprehensive logging and monitoring helps you detect suspicious activities and maintain visibility into who is accessing your resources.

- **Enable resource logs**: Configure Azure Bastion diagnostic settings to capture detailed logs of remote sessions, including which users connected to which workloads, connection times, and other relevant information. See [Azure Bastion Resource Logs](https://learn.microsoft.com/azure/bastion/diagnostic-logs).

- **Configure log analytics integration**: Send Azure Bastion logs to a Log Analytics workspace for centralized analysis, querying, and long-term storage of security events. See [Monitor Azure Bastion](https://learn.microsoft.com/azure/bastion/diagnostic-logs).

- **Set up alerts for suspicious activities**: Create Azure Monitor alerts based on Bastion logs to notify administrators of unusual connection patterns, failed authentication attempts, or connections outside normal business hours. See [Use Azure Monitor alerts to notify you of issues](https://learn.microsoft.com/azure/bastion/diagnostic-logs#use-azure-monitor-alerts-to-notify-you-of-issues).

- **Monitor active sessions**: Regularly review and monitor active Bastion sessions, with the ability to forcibly disconnect any suspicious or unauthorized sessions. See [Connect to virtual machines through the Azure portal by using Azure Bastion](https://learn.microsoft.com/training/modules/connect-vm-with-azure-bastion/5-manage-remote-sessions).

## Compliance and governance

Implementing proper governance controls helps ensure your Bastion deployment adheres to organizational and regulatory requirements.

- **Implement Azure Policy for compliance**: Use Azure Policy to audit and enforce secure configurations for your Azure Bastion deployments, ensuring they meet organizational standards. See [Azure security baseline for Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline#asset-management).

- **Regularly review access permissions**: Periodically audit who has access to manage Bastion resources and connect to VMs through Bastion to maintain proper access control. See [Azure security baseline for Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline#privileged-access).

- **Document compliance with security baselines**: Use the Microsoft Cloud Security Benchmark to document compliance with security best practices for Azure Bastion. See [Azure security baseline for Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline).

- **Integrate with security information and event management**: Forward Bastion logs to your SIEM solution for comprehensive security monitoring and compliance reporting. See [Azure Monitor](https://learn.microsoft.com/azure/azure-monitor/logs/logs-data-export).

## Service-specific security

These security considerations are specific to Azure Bastion's unique functionality and features.

- **Choose the appropriate SKU tier**: Select the Bastion SKU (Basic, Standard, or Premium) that aligns with your security requirements, considering that higher tiers offer advanced security features like session recording and private-only deployment. See [What is Azure Bastion?](https://learn.microsoft.com/azure/bastion/bastion-overview#skus).

- **Deploy with availability zones for resilience**: In supported regions, deploy Azure Bastion with availability zone configuration to enhance reliability and maintain secure access even during zone failures. See [What is Azure Bastion?](https://learn.microsoft.com/azure/bastion/bastion-overview#availability-zones).

- **Implement proper host scaling**: Configure the appropriate number of Bastion host instances based on your concurrent session requirements to ensure reliable and secure access. See [What is Azure Bastion?](https://learn.microsoft.com/azure/bastion/bastion-overview#host-scaling).

- **Keep Bastion service updated**: Azure Bastion is automatically kept up to date by Microsoft, providing protection against zero-day exploits. Ensure you're using the latest features by reviewing Azure updates. See [What's new?](https://learn.microsoft.com/azure/bastion/bastion-overview#whats-new).

## Learn more

- [Microsoft Cloud Security Benchmark – Azure Bastion](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-bastion-security-baseline)
- [What is Azure Bastion?](https://learn.microsoft.com/azure/bastion/bastion-overview)
- [Working with NSG access and Azure Bastion](https://learn.microsoft.com/azure/bastion/bastion-nsg)
- [Introduction to Azure Bastion](https://learn.microsoft.com/training/modules/intro-to-azure-bastion/)
