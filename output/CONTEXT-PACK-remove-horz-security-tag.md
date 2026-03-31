# Context Pack: Remove `horz-security` Tag from Mistagged Files

## Task
Remove the `ms.custom: horz-security` metadata tag from granular security articles that don't follow the "Secure your \<Service>" rubric.

## Repository
**microsoftdocs/azure-docs-pr** (private repo)

## What to Remove
The `horz-security` value from `ms.custom` in YAML frontmatter. May be:
- `ms.custom: horz-security` (sole value - remove entire line)
- `ms.custom: horz-security, other-tag` (comma-separated - remove only `horz-security`)

---

## Files to Modify

### PostgreSQL Flexible Server (15 files)
Path: `articles/postgresql/flexible-server/`

| Filename |
|----------|
| security-access-control.md |
| security-audit.md |
| security-audit-entra.md |
| security-azure-policy.md |
| security-configure-data-encryption.md |
| security-connect-scram.md |
| security-data-encryption.md |
| security-defender-for-cloud.md |
| security-managed-database-users.md |
| security-managed-identity-overview.md |
| security-entra-concepts.md |
| security-entra-authentication.md |
| security-reset-admin-password.md |
| security-confidential-computing.md |
| security-compliance.md |

### MySQL Flexible Server (14 files)
Path: `articles/mysql/flexible-server/`

| Filename |
|----------|
| security-tls-root-certificate-rotation.md |
| security-how-to-connect.md |
| security-customer-managed-key.md |
| security-tls-how-to-connect.md |
| security-tls-root-certificate-rotation-faq.md |
| security-how-to-create-users.md |
| security-entra-authentication.md |
| security-how-to-data-encryption-cli.md |
| security-how-to-data-encryption-portal.md |
| security-how-to-entra.md |
| security-configure-managed-identities-system-assigned.md |
| security-configure-managed-identities-user-assigned.md |
| security-tls.md |
| security-entra-configure.md |

---

## Total: 29 files

## DO NOT MODIFY
These files in the same folders **correctly** have the tag:
- `mysql/flexible-server/security-overview.md` → "Secure your Azure Database for MySQL Server" ✅

---

## Workflow
1. Clone/update azure-docs-pr
2. Create branch (e.g., `postgresql-mysql-remove-horz-tag`)
3. Remove `horz-security` from ms.custom in each file
4. Update ms.date on each modified file
5. Create draft PR to upstream (microsoftdocs)
