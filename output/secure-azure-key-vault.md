---
title: Secure your Azure Key Vault deployment
description: Learn how to secure Azure Key Vault, with best practices for protecting your cryptographic keys, secrets, and certificates.
author: 
ms.author: 
ms.service: azure-key-vault
ms.topic: conceptual
ms.custom: horz-security
ms.date: 02/19/2026
ai-usage: ai-assisted
---

# Secure your Azure Key Vault deployment

Azure Key Vault provides centralized management for cryptographic keys, certificates, and secrets such as connection strings and passwords. When storing sensitive and business-critical data, you must take steps to maximize the security of your vaults and the data stored in them.

[!INCLUDE [Security horizontal Zero Trust statement](~/reusable-content/ce-skilling/azure/includes/security/zero-trust-security-horizontal.md)]

This article provides security recommendations to help protect your Azure Key Vault deployment.

## Service-specific security

Azure Key Vault has unique security considerations related to vault architecture and appropriate use of the service for storing cryptographic materials.

### Key vault architecture

- **Create separate key vaults per application, region, and environment**: Create separate vaults for development, preproduction, and production environments to reduce the impact of breaches. Key vaults define security boundaries for stored secrets; grouping secrets into the same vault increases the blast radius of a security event. See [Secure your Azure Key Vault](/azure/key-vault/general/secure-key-vault).

- **Use separate key vaults per tenant in multitenant solutions**: For multitenant SaaS solutions, use a separate key vault for each tenant to maintain data isolation. This approach is recommended for secure isolation of customer data and workloads. See [Multitenancy and Azure Key Vault](/azure/architecture/guide/multitenant/service/key-vault).

- **Choose the appropriate tier**: Use Standard tier for most applications, or Premium tier for HSM-protected keys with FIPS 140-3 Level 3 validation. For the highest security requirements, consider [Azure Managed HSM](/azure/key-vault/managed-hsm/overview) for dedicated, single-tenant HSM pools. See [About Azure Key Vault](/azure/key-vault/general/overview).

### Appropriate use of Key Vault

- **Do not use Key Vault as a general data store**: Key Vault is designed for cryptographic materials and secrets, not general configuration data or large payloads. For application configurations, use [Azure App Configuration](/azure/azure-app-configuration/overview). For large data, use [Azure Storage with encryption at rest](/azure/storage/common/storage-service-encryption).

- **Do not store certificates as secrets**: Store certificates using the Key Vault certificate object type, which provides automated lifecycle management, integration with certificate authorities, and automatic renewal capabilities. See [About Azure Key Vault certificates](/azure/key-vault/certificates/about-certificates).

- **Store secrets in appropriate formats**: For credentials with multiple components (like username/password), store them as a properly formatted connection string or a JSON object. Use tags for metadata like rotation schedules and ownership rather than storing metadata in the secret value. See [About Azure Key Vault secrets](/azure/key-vault/secrets/about-secrets).

## Data protection

Protecting data stored in Azure Key Vault requires enabling soft delete, purge protection, and implementing automated rotation of cryptographic materials.

- **Enable soft delete**: Soft delete allows recovery of deleted key vault objects within a 7 to 90-day retention period. Soft delete is enabled by default on new vaults and cannot be disabled once enabled. See [Azure Key Vault soft-delete overview](/azure/key-vault/general/soft-delete-overview).

- **Enable purge protection**: Purge protection prevents permanent deletion of key vault objects even after soft delete, ensuring that malicious insiders cannot bypass the retention period. When purge protection is enabled, it cannot be disabled or overridden by anyone, including Microsoft. See [Purge protection](/azure/key-vault/general/soft-delete-overview#purge-protection).

- **Implement autorotation for cryptographic assets**: Configure automatic rotation of keys, secrets, and certificates to minimize the risk of compromise and ensure compliance with security policies. See [Understanding autorotation in Azure Key Vault](/azure/key-vault/general/autorotation), [Configure key autorotation](/azure/key-vault/keys/how-to-configure-key-rotation), [Configure certificate autorotation](/azure/key-vault/certificates/tutorial-rotate-certificates), [Automate secret rotation for resources with one set of authentication credentials](/azure/key-vault/secrets/tutorial-rotation), and [Automate secret rotation for resources with two sets of authentication credentials](/azure/key-vault/secrets/tutorial-rotation-dual).

- **Set expiration dates on keys and secrets**: Cryptographic keys and secrets should have a defined expiration date and not be permanent. Keys that are valid forever provide a potential attacker more time to compromise the key. See [Key management in Azure Key Vault](/azure/key-vault/keys/about-keys-details).

- **Use HSM-protected keys for high-security scenarios**: For sensitive workloads requiring hardware-backed key protection, use HSM-protected keys (RSA-HSM, EC-HSM) available in the Premium tier, which are protected by FIPS 140-3 Level 3 validated hardware security modules. See [About Azure Key Vault keys](/azure/key-vault/keys/about-keys).

## Identity and access management

Azure Key Vault uses Microsoft Entra ID for authentication. Access is controlled through two interfaces: the control plane (for managing Key Vault itself) and the data plane (for working with keys, secrets, and certificates).

- **Use managed identities for application authentication**: Use Azure managed identities for all app and service connections to Key Vault to eliminate hard-coded credentials. Managed identities provide secure authentication while removing the need for explicit credentials. See [Azure Key Vault authentication](/azure/key-vault/general/authentication).

- **Use Azure role-based access control (RBAC)**: Use Azure RBAC to manage access to Key Vault data plane operations. RBAC provides centralized access management, supports Privileged Identity Management (PIM), and allows permissions at management group, subscription, resource group, or individual resource levels. See [Azure RBAC for Key Vault data plane operations](/azure/key-vault/general/rbac-guide).
    - **Do not use legacy access policies**: With access policies, users with Contributor permissions can grant themselves data plane access, which can result in unauthorized access. Access policies also lack support for Privileged Identity Management (PIM). Azure RBAC should be used for all production workloads. See [Azure role-based access control vs. access policies](/azure/key-vault/general/rbac-access-policy).

> [!IMPORTANT]
> The RBAC permission model allows vault-level role assignments for persistent access and eligible (JIT) assignments for privileged operations. Object-scope assignments only support read operations; administrative operations like network access control, monitoring, and object management require vault-level permissions. For secure isolation across application teams, use one Key Vault per application.

- **Assign just-in-time (JIT) privileged roles**: Use Azure Privileged Identity Management (PIM) to assign eligible JIT Azure RBAC roles for administrators and operators of Key Vault. See [Privileged Identity Management overview](/entra/id-governance/privileged-identity-management/pim-configure).
    - **Require approvals for privileged role activation**: Add an extra layer of security by ensuring that at least one approver is required to activate JIT roles. See [Configure Microsoft Entra role settings in Privileged Identity Management](/entra/id-governance/privileged-identity-management/pim-how-to-change-default-settings).
    - **Enforce multi-factor authentication for role activation**: Require MFA to activate JIT roles for operators and administrators. See [Microsoft Entra multifactor authentication](/entra/identity/authentication/concept-mfa-howitworks).

- **Apply the principle of least privilege**: Grant users and applications only the minimum permissions required for their role. Use granular built-in roles like Key Vault Secrets User, Key Vault Crypto User, or Key Vault Certificates Officer rather than broad administrative roles. See [Azure built-in roles for Key Vault data plane operations](/azure/key-vault/general/rbac-guide#azure-built-in-roles-for-key-vault-data-plane-operations).

- **Enable Microsoft Entra Conditional Access policies**: Apply access controls based on conditions such as user location, device compliance, or sign-in risk to add additional layers of protection for Key Vault access. See [Conditional Access overview](/entra/identity/conditional-access/overview).

- **Tightly control Contributor role access**: Users with Contributor permissions to the Key Vault control plane can grant themselves data plane access. Ensure only authorized persons have Contributor or Owner roles on your key vaults. See [Provide access to Key Vault with Azure RBAC](/azure/key-vault/general/rbac-guide#managing-administrative-access-to-key-vault).

## Network security

Network isolation reduces the attack surface by limiting which networks can access your key vault.

- **Disable public network access and use private endpoints**: Deploy Azure Private Link for private access to Key Vault. Private endpoints use a private IP address from your virtual network, eliminating exposure to the public internet. See [Integrate Key Vault with Azure Private Link](/azure/key-vault/general/private-link-service).
    - Some scenarios require trusted Microsoft services to bypass the firewall. In such cases, configure the key vault to allow trusted Microsoft services. See [Virtual network service endpoints for Azure Key Vault](/azure/key-vault/general/overview-vnet-service-endpoints#trusted-services).

- **Configure firewall rules to restrict access**: When public access is required, use Key Vault firewall rules to allow access only from specific IP addresses or virtual network subnets. Set the default action to deny and explicitly allow only authorized traffic. See [Configure network security for Azure Key Vault](/azure/key-vault/general/network-security).

- **Use virtual network service endpoints**: If private endpoints cannot be used, enable service endpoints for Key Vault on your virtual network subnets to route traffic through the Microsoft backbone network. See [Virtual network service endpoints for Azure Key Vault](/azure/key-vault/general/overview-vnet-service-endpoints).

- **Monitor private endpoint connection states**: Regularly review and audit private endpoint connections to ensure only approved connections exist. Remove disconnected or rejected connections promptly. See [Integrate Key Vault with Azure Private Link](/azure/key-vault/general/private-link-service).

- **Use Network Security Perimeter**: Define a logical network isolation boundary for PaaS resources deployed outside your organization's virtual network. Network Security Perimeter provides a unified network access control experience across supported Azure services. See [Configure network security for Azure Key Vault: Network Security Perimeter](/azure/key-vault/general/network-security#network-security-perimeter).
    - Note that `publicNetworkAccess: SecuredByPerimeter` overrides "Allow trusted Microsoft services to bypass the firewall", meaning some scenarios that require that trust will not work.

## TLS and HTTPS

Azure Key Vault supports TLS 1.2 and 1.3 protocol versions to ensure secure communication between clients and the service.

- **Enforce TLS version control**: The Key Vault front end (data plane) is a multitenant server where key vaults from different customers can share the same public IP address. To achieve isolation, each HTTP request is authenticated and authorized independently. The HTTPS protocol allows clients to participate in TLS negotiation, and clients can enforce the TLS version to ensure the entire connection uses the corresponding level of protection. See [Azure Key Vault logging](/azure/key-vault/general/logging) for sample Kusto queries to monitor TLS versions used by clients.

- **Use Azure Identity client libraries**: Use the official Azure Identity client libraries for authentication, which handle secure token acquisition and TLS configuration automatically. See [Authenticate to Key Vault in code](/azure/key-vault/general/developers-guide#authenticate-to-key-vault-in-code).

## Logging and monitoring

Comprehensive logging and monitoring enable detection of suspicious activities and compliance with audit requirements.

- **Enable diagnostic logging**: Key Vault logging saves information about operations performed on the vault, including authentication events, access attempts, and configuration changes. Send logs to Azure Storage, Azure Event Hubs, or Log Analytics for analysis. See [Azure Key Vault logging](/azure/key-vault/general/logging).

- **Enable Microsoft Defender for Key Vault**: Microsoft Defender for Key Vault monitors for suspicious and anomalous access patterns to your vaults, alerting on potential threats like unusual access from anonymous networks, access from suspicious IP addresses, or unusual application access. See [Introduction to Microsoft Defender for Key Vault](/azure/defender-for-cloud/defender-for-key-vault-introduction).

- **Configure alerts for security events**: Set up alerts to be notified when critical events are logged, such as access failures, configuration changes, or secret deletions. Use both static alerts for fixed thresholds and dynamic alerts for anomalous patterns. See [Configure Azure Key Vault alerts](/azure/key-vault/general/alert).

- **Monitor with Azure Event Grid**: Integrate Key Vault with Event Grid to receive notifications on changes to keys, certificates, or secrets, including near-expiry notifications for automated responses. See [Monitoring Key Vault with Azure Event Grid](/azure/key-vault/general/event-grid-overview).

- **Use Key Vault insights**: Use Azure Monitor for Key Vault to get a unified view of Key Vault requests, performance, failures, and latency across your vaults. See [Monitoring Key Vault with Key Vault insights](/azure/key-vault/key-vault-insights-overview).

## Compliance and governance

Regular compliance audits and governance policies ensure your Key Vault deployment adheres to security standards and organizational requirements.

- **Use Azure Policy to enforce secure configurations**: Configure Azure Policy to audit and enforce secure configurations for Key Vault, such as requiring soft delete, purge protection, private endpoints, or RBAC permission model. Set up alerts for deviations from policy. See [Azure Policy built-in definitions for Key Vault](/azure/key-vault/policy-reference).

- **Enable regulatory compliance controls**: Key Vault supports compliance with regulatory frameworks including SOC, ISO, PCI DSS, and FedRAMP. Review the security baseline for Key Vault to ensure your configuration meets your compliance requirements. See [Azure security baseline for Key Vault](/security/benchmark/azure/baselines/key-vault-security-baseline).

- **Use resource locks for critical vaults**: Apply resource locks to prevent accidental deletion or modification of production key vaults. Note that resource locks protect the vault resource itself, not the objects within it. See [Lock resources to prevent unexpected changes](/azure/azure-resource-manager/management/lock-resources).

## Backup and recovery

Azure Key Vault provides built-in redundancy and recovery features, supplemented by manual backup options for specific scenarios.

- **Rely on automatic redundancy**: Key Vault automatically replicates data across regions and handles failover during outages. For most scenarios, built-in redundancy provides sufficient protection without requiring manual backups. See [Azure Key Vault availability and redundancy](/azure/key-vault/general/disaster-recovery-guidance).

- **Back up critical objects that cannot be recreated**: For keys protecting irreplaceable data or secrets that cannot be regenerated from other sources, export and securely store backups. Backups are encrypted and can only be restored within the same Azure subscription and geography. See [Azure Key Vault backup and restore](/azure/key-vault/general/backup).

- **Test backup and recovery procedures**: Regularly test the restoration of Key Vault secrets, keys, and certificates to verify the effectiveness of backup processes and ensure you can recover from data loss scenarios. See [Azure Key Vault backup](/azure/key-vault/general/backup).

- **Document recovery procedures**: Maintain runbooks for key vault recovery scenarios, including recovery from soft delete, backup restoration, and failover procedures. Ensure operations teams understand recovery processes before incidents occur. See [Azure Key Vault recovery management with soft delete and purge protection](/azure/key-vault/general/key-vault-recovery).

## Related security articles

For security best practices specific to Key Vault sub-resources, see:

- [Secure your Azure Key Vault keys](/azure/key-vault/keys/secure-keys) - Key management, HSM protection, and key rotation
- [Secure your Azure Key Vault secrets](/azure/key-vault/secrets/secure-secrets) - Secret storage, formatting, and rotation
- [Secure your Azure Key Vault certificates](/azure/key-vault/certificates/secure-certificates) - Certificate management, CA integration, and renewal

## Next steps

- [Microsoft Cloud Security Benchmark v2 – Key Vault security baseline](/security/benchmark/azure/baselines/key-vault-security-baseline)
- [Well-Architected Framework – Azure Key Vault considerations](/azure/well-architected/service-guides/azure-key-vault)
- [Zero Trust guidance center](/security/zero-trust/zero-trust-overview)
- [Microsoft Cloud Security Benchmark v2 overview](/security/benchmark/azure/overview)
