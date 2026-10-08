# 06 — Security

Security is designed in, not bolted on. Target alignment: **OWASP ASVS Level 2**, with controls mapped to **ISO 27001 Annex A** and **SOC 2** so your auditors have a head start.

## Authentication

- **SSO** via OIDC or SAML 2.0 (Azure AD / Entra ID, Google Workspace, Okta, or bundled Keycloak).
- **MFA** enforced for all users (TOTP or WebAuthn/passkeys); required for Admin and Records Manager roles.
- **SCIM** provisioning so leavers are de-provisioned automatically from your directory.
- Short-lived access tokens (10 min) + rotating refresh tokens; idle timeout (configurable, default 30 min); admin can revoke any session.
- Local accounts (if used): Argon2id hashing, breached-password check, lockout with backoff.

## Authorization

Two layers, both deny-by-default:

1. **Role-based** — what a user can do in the system (Admin, Records Manager, Manager, User, Viewer, Auditor, custom).
2. **Resource ACLs** — what they can do *to which folder/document*, inherited down the folder tree with document-level overrides and explicit deny.

Enforcement points:
- A single NestJS guard computes effective permissions for every request; there is no endpoint that bypasses it (enforced by a lint rule and a test that enumerates all routes).
- PostgreSQL **row-level security** on `tenant_id` as a second wall against cross-tenant leakage.
- **Search** filtered by principal IDs inside the OpenSearch query.
- **Presigned URLs** issued only after the check, valid for 60 seconds, single object.

## Encryption

| Where | How |
|---|---|
| In transit | TLS 1.3 (HSTS, modern ciphers) at the edge; TLS between services and to managed data stores |
| At rest — files | S3 SSE-KMS, AES-256, per-tenant customer-managed key; optional bring-your-own-key |
| At rest — database | RDS/Azure encryption (AES-256), encrypted snapshots and backups |
| At rest — search index | OpenSearch encryption at rest + node-to-node TLS |
| Secrets | KMS / Secrets Manager, rotated; never in code or env files in repo |

## Audit integrity

- Append-only `audit_event` table; the application DB role has `INSERT` only.
- Each event is hash-chained to the previous one. A verification job walks the chain nightly and alerts on any break.
- The day's final hash is written to a **WORM (S3 Object Lock, compliance mode)** bucket — even an administrator cannot alter history without detection.
- Audit export (CSV/JSON) for auditors, filterable by user, document, action, date.

## Application security

- Input validation with schema validation on every DTO; parameterised queries only.
- File handling: ClamAV scan, MIME sniffing (not trusting extensions), quarantine bucket, preview rendering in sandboxed containers with no network.
- CSP, X-Frame-Options, SameSite cookies, CSRF protection.
- Rate limiting and brute-force protection at the gateway.
- Dependency scanning (Dependabot/Snyk), SAST (CodeQL/Semgrep), container scanning (Trivy) in CI — builds fail on high severity.
- Secrets scanning on every commit.

## Operational security

- Least-privilege IAM per service; no shared credentials.
- Private subnets for all data stores; bastion-less access via SSM.
- WAF with OWASP managed rules.
- Centralised logs with 1-year retention; security alerts (impossible travel, mass download, permission escalation).
- Backup encryption + quarterly restore test.
- **Independent penetration test** before go-live (third-party, report shared with you), all criticals/highs fixed before launch.

## Privacy

- Data residency: deploy in the region you choose.
- GDPR support: subject access search, export, and erasure workflow that respects legal holds.
