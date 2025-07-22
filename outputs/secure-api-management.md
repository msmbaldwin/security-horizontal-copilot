---
title: Secure your API management deployment
description: Learn how to secure API management, with best practices for protecting your deployment.
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: conceptual
ms.custom: horz-security
ms.date: 07/22/2025
ai-usage: ai-assisted
---

# Secure your API management deployment

Azure API Management enables organizations to publish, secure, transform, maintain, and monitor APIs at scale. Securing your API Management deployment is essential to protect sensitive data, control access, and ensure compliance with organizational and regulatory requirements.

This article provides guidance on how to best secure your Azure API Management deployment.

## Network security

Network security controls access to your APIs and management endpoints, helping prevent unauthorized access and data exposure.

- **Deploy API Management in a virtual network**: Isolate your API Management instance and backend services using Azure Virtual Network integration. See [Virtual network concepts](/azure/api-management/virtual-network-concepts).

- **Configure network security groups (NSGs)**: Restrict traffic to API Management subnets by applying NSGs. See [Configure NSG rules for API Management](/azure/api-management/api-management-using-with-vnet#configure-nsg-rules).

- **Enable private endpoints**: Prevent public access by routing traffic through private endpoints. See [Private endpoint for API Management](/azure/api-management/private-endpoint).

- **Disable public network access**: Use service-level IP ACLs or the public network access toggle to block public connectivity. See [Disable public network access](/azure/api-management/private-endpoint#optionally-disable-public-network-access).

- **Integrate with Application Gateway and Web Application Firewall (WAF)**: Protect APIs with a reverse proxy and L7 firewall. See [Integrate API Management with Application Gateway](/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway).

## Identity management

Identity management ensures only authorized users and applications can access APIs and management features.

- **Use Microsoft Entra ID for centralized identity management**: Authenticate users and control access to APIs and the developer portal. See [Protect backend APIs with Microsoft Entra ID](/azure/api-management/api-management-howto-protect-backend-with-aad).

- **Require multi-factor authentication (MFA)**: Enforce MFA for all administrative and developer portal access to reduce account compromise risk.

- **Disable local authentication methods**: Avoid using local accounts and passwords; prefer Microsoft Entra ID for authentication. See [Authentication policies](/azure/api-management/api-management-authentication-policies#Basic).

- **Leverage managed identities for secure resource access**: Use managed identities to access Azure resources securely without credentials. See [Managed identities overview](/azure/active-directory/managed-identities-azure-resources/overview).

- **Integrate with Azure Key Vault for secrets and certificates**: Store and manage secrets and certificates securely. See [Key Vault integration](/azure/api-management/api-management-howto-properties).

## Privileged access

Limiting privileged access reduces the risk of accidental or malicious changes to your API Management environment.

- **Apply least privilege with RBAC**: Assign only necessary permissions using Azure role-based access control. See [RBAC in API Management](/azure/api-management/api-management-role-based-access-control).

- **Use just-in-time access via PIM**: Temporarily elevate privileges with Microsoft Entra Privileged Identity Management. See [Privileged Identity Management](/entra/id-governance/privileged-identity-management/pim-configure).

- **Disable or restrict local admin accounts**: Use Azure AD accounts for administration and restrict local admin accounts to emergency use only. See [Manage user accounts](/azure/api-management/api-management-howto-create-or-invite-developers).

## Logging and threat detection

Monitoring and threat detection help you identify and respond to suspicious activity and security incidents.

- **Enable Microsoft Defender for APIs**: Monitor, detect, and respond to threats targeting APIs managed in API Management. See [Defender for APIs](/azure/api-management/protect-with-defender-for-apis).

- **Enable resource logs for auditing and troubleshooting**: Collect GatewayLogs and WebSocketConnectionLogs for visibility into API activity. See [Resource logs in API Management](/azure/api-management/api-management-howto-use-azure-monitor#resource-logs).

- **Aggregate logs in Microsoft Sentinel**: Centralize and analyze security data to detect anomalies. See [Microsoft Sentinel](/azure/sentinel/).

## Backup and recovery

Backup and recovery protect against data loss and outages, ensuring business continuity.

- **Automate regular backups**: Use API Management's native backup capability to protect configurations and data. See [Backup and restore in API Management](/azure/api-management/api-management-howto-disaster-recovery-backup-restore#calling-the-backup-and-restore-operations).

- **Store backups in customer-owned Azure Storage accounts**: Ensure backups are securely stored and accessible for recovery.

- **Test recovery procedures**: Periodically validate recovery processes to ensure readiness in case of failure.

## Additional recommendations

- **Rotate subscription keys and secrets regularly**: Reduce risk of unauthorized access by rotating keys and secrets. See [Subscriptions in API Management](/azure/api-management/api-management-subscriptions).

- **Validate JWT tokens and claims for API calls**: Ensure only authorized clients can access APIs. See [Validate JWT policy](/azure/api-management/validate-jwt-policy).

- **Monitor compliance with Azure Policy**: Use built-in Azure Policy definitions to enforce secure configurations. See [Azure Policy for API Management](/azure/api-management/policy-reference).
