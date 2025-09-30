---
title: Secure your Azure Cache for Redis deployment
description: Learn how to secure Azure Cache for Redis, with best practices for protecting your deployment.
author: 
ms.author: 
ms.service: azure-cache-for-redis
ms.topic: conceptual
ms.custom: horz-security
ms.date: 09/29/2025
ai-usage: ai-assisted
---

# Secure your Azure Cache for Redis deployment

Azure Cache for Redis provides capabilities to deliver high-performance, scalable in-memory data caching based on the open-source Redis software. When deploying this service, it's important to follow security best practices to protect data, configurations, and infrastructure.

This article provides guidance on how to best secure your Azure Cache for Redis deployment.

## Network security

Network security controls prevent unauthorized access to Redis cache instances and establish secure communication boundaries for in-memory data access patterns.

- **Enable private endpoints**: Eliminate public internet exposure by routing traffic through your virtual network using Azure Private Link. Private endpoints provide secure, dedicated network interfaces that connect directly to Redis while preventing data exfiltration risks. See [Azure Cache for Redis with Azure Private Link](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-private-link).

- **Disable public network access**: Block all internet-based connections when using private endpoints by setting the `publicNetworkAccess` flag to `Disabled`. This forces all communication through private endpoints and reduces your attack surface. See [Azure Cache for Redis with Azure Private Link](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-private-link).

- **Deploy into virtual networks**: For Premium tier caches, inject the cache directly into a virtual network subnet to provide network-level isolation and enable advanced network security configurations with complete control over traffic flow. See [Configure Virtual Network support for a Premium Azure Cache for Redis instance](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet).

- **Configure network security groups**: Apply Network Security Groups to control traffic flow to and from subnets hosting Redis cache instances by implementing restrictive rules that deny unauthorized access while allowing necessary communication patterns. See [Configure Virtual Network support for a Premium Azure Cache for Redis instance](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet).

- **Implement firewall rules**: Configure IP-based firewall rules to restrict cache access to specific IP address ranges when virtual network injection or private endpoints aren't feasible. This provides an additional layer of network access control. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

## Identity and access management

Identity and access management controls ensure that only authorized users and applications can access Redis cache resources with appropriate authentication mechanisms and access permissions.

- **Use Microsoft Entra ID authentication**: Replace access keys with Microsoft Entra ID authentication to leverage centralized identity management, conditional access policies, and eliminate long-lived secrets. This provides better audit trails and supports multi-factor authentication. See [Use Microsoft Entra ID for cache authentication](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-azure-active-directory-for-authentication).

- **Disable access key authentication**: Turn off access key authentication to enforce Microsoft Entra ID authentication for all cache connections. This eliminates the risk of compromised access keys and provides centralized identity management with advanced security features. See [Use Microsoft Entra ID for cache authentication](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-azure-active-directory-for-authentication).

- **Configure role-based access control**: Use data access configuration with custom access policies to implement fine-grained permissions based on Redis ACL functionality. This allows precise control over which operations users can perform on specific data patterns. See [Configure role-based access control in Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure-role-based-access-control).

- **Rotate access keys regularly**: When access key authentication is required, implement regular key rotation using both primary and secondary keys to enable seamless rotation without service interruption while limiting exposure from compromised credentials. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

- **Avoid disabling authentication**: Never set the `AuthNotRequired` property to true as this allows unauthenticated access to cache data and is highly discouraged from a security perspective. Always maintain authentication requirements for cache access. See [Azure security baseline for Azure Cache for Redis](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-cache-for-redis-security-baseline).

## Data protection

Data protection mechanisms safeguard cached data through encryption, secure connections, and persistent storage security practices.

- **Enforce TLS encryption for all connections**: Disable non-TLS ports by ensuring "Allow access only via SSL" is enabled to protect data in transit. Use TLS 1.2 or higher for all client connections to prevent eavesdropping and man-in-the-middle attacks. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

- **Configure customer-managed keys for disk encryption**: Enable customer-managed key (CMK) encryption using Azure Key Vault for Enterprise and Enterprise Flash tiers to maintain control over encryption keys for persistence disks and temporary files. This provides additional protection beyond Microsoft-managed encryption. See [Configure disk encryption for Azure Cache for Redis instances using customer managed keys](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-encryption).

- **Secure data persistence storage**: When using data persistence features in Premium or Enterprise tiers, ensure the target Azure Storage account uses encryption at rest and appropriate access controls to protect persisted Redis data files (RDB/AOF). See [How to configure persistence for a Premium Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence).

- **Implement import/export security**: When importing or exporting Redis data, use storage accounts with private endpoints and disable public access to prevent data exposure during transfer operations. Use managed identity authentication instead of storage account keys. See [Import and Export data in Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data).

## Logging and monitoring

Comprehensive logging and monitoring provide visibility into Redis cache operations, connection patterns, and security events to enable threat detection and operational oversight.

- **Enable diagnostic settings**: Configure diagnostic settings to collect connection logs and metrics for security monitoring and compliance auditing. Send diagnostic data to Azure Monitor Logs, storage accounts, or Event Hubs for centralized analysis and retention. See [Monitor Azure Cache for Redis using diagnostic settings](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-monitor-diagnostic-settings).

- **Monitor connection events**: Track client connections, authentication events, and disconnection events to detect anomalous access patterns that might indicate security threats or unauthorized access attempts. Use connection logs for security auditing purposes. See [Monitor Azure Cache for Redis using diagnostic settings](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-monitor-diagnostic-settings).

- **Set up Azure Monitor alerts**: Configure alerts for critical metrics including high CPU usage, memory pressure, connection failures, and authentication events. Automated alerts enable rapid response to potential security incidents and performance issues. See [Monitor and alert with Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-monitor).

- **Enable Microsoft Entra authentication audit logging**: When using Microsoft Entra ID authentication, enable MSEntra authentication audit logs to track authentication events and user access patterns for security investigations and compliance reporting. See [Monitor Azure Cache for Redis using diagnostic settings](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-monitor-diagnostic-settings).

- **Monitor Azure Activity Log**: Track administrative operations, configuration changes, and resource management actions through Azure Activity Log. Configure alerts for critical changes such as authentication method modifications or network access rule updates. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

## Compliance and governance

Compliance and governance controls ensure Redis cache deployments meet regulatory requirements and organizational policies through proper configuration management and audit capabilities.

- **Apply Azure Policy definitions**: Use built-in Azure Policy definitions to enforce security configurations such as private link usage, SSL-only connections, and authentication requirements. Policies help maintain consistent security posture across environments. See [Azure Policy Regulatory Compliance controls for Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/security-controls-policy).

- **Implement resource tagging**: Apply consistent resource tags to Redis cache instances for cost management, security monitoring, compliance tracking, and governance. Use tags to identify data classification, owner information, and regulatory requirements. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

- **Configure conditional access policies**: Implement Microsoft Entra conditional access policies to control access to Redis cache resources based on user identity, location, device compliance, and risk level when using Microsoft Entra ID authentication. See [Use Microsoft Entra ID for cache authentication](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-azure-active-directory-for-authentication).

- **Maintain compliance documentation**: Document Redis cache security configurations, access policies, and procedures to support compliance audits and demonstrate adherence to regulatory requirements including data protection regulations. See [Azure security baseline for Azure Cache for Redis](https://learn.microsoft.com/security/benchmark/azure/baselines/azure-cache-for-redis-security-baseline).

- **Enable audit trails**: Ensure comprehensive audit logging captures all cache administration actions, authentication attempts, and configuration changes for regulatory compliance and security investigations using diagnostic settings. See [Monitor Azure Cache for Redis using diagnostic settings](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-monitor-diagnostic-settings).

## Backup and recovery

Backup and recovery strategies protect cached data and ensure business continuity through proper data persistence, geo-replication, and disaster recovery planning.

- **Configure data persistence**: Enable data persistence in Premium and Enterprise tiers to automatically save Redis data to Azure Storage or managed disks. This protects against data loss during cache reboots, maintenance events, or unexpected failures. See [How to configure persistence for a Premium Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence).

- **Implement geo-replication**: Set up passive geo-replication between Redis cache instances in different Azure regions to provide disaster recovery capabilities and protect against regional outages. This creates read-only replicas that can be promoted during failover scenarios. See [How to set up Geo-replication for Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-geo-replication).

- **Enable zone redundancy**: Configure zone redundancy to distribute cache nodes across multiple availability zones within a region. This provides automatic failover capabilities and improves availability during datacenter-level outages without manual intervention. See [How to enable zone redundancy for Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-zone-redundancy).

- **Schedule maintenance updates**: Use scheduled updates to control when Redis server patches and updates are applied during defined maintenance windows. This minimizes disruption to applications and allows planning around business-critical operations. See [Administration of Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-administration).

- **Implement client retry logic**: Configure client applications with proper retry mechanisms and connection resilience patterns to handle temporary disruptions during failover events, maintenance operations, or transient network issues. See [Connection resilience best practices for Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-best-practices-connection).

## Service-specific security

Redis cache provides unique security capabilities designed specifically for in-memory data storage, cache operations, and Redis-specific functionality.

- **Configure memory management policies**: Set appropriate maxmemory policies and memory reservations to prevent cache failures under memory pressure. Configure `maxmemory-reserved` and `maxfragmentationmemory-reserved` settings to maintain cache stability and prevent security issues related to resource exhaustion. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

- **Implement clustering security**: For clustered Redis deployments, ensure proper shard distribution and configure cluster authentication consistently across all nodes. Use clustering policies that align with your security requirements and data distribution needs. See [How to configure clustering for a Premium Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-clustering).

- **Secure Redis console access**: Disable or restrict access to the Redis console in production environments to prevent unauthorized direct access to cache data. When private endpoints are used, the portal-based Redis console is automatically disabled for additional security. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

- **Configure keyspace notifications**: When enabling keyspace notifications for application monitoring, ensure notifications don't expose sensitive data patterns or cache keys that could provide insights into application behavior or data structures. See [How to configure Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-configure).

- **Implement connection limits**: Monitor and configure appropriate connection limits to prevent resource exhaustion attacks and ensure fair resource allocation among client applications. Use connection pooling and singleton connection multiplexer patterns to optimize connection usage. See [Connection resilience best practices for Azure Cache for Redis](https://learn.microsoft.com/azure/azure-cache-for-redis/cache-best-practices-connection).

