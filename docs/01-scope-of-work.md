# 01 — Scope of Work

Each section below maps directly to the numbered feature areas in your brief. **MVP** = in the first production release. **Phase 2** = designed for now, built after go-live (priced separately, optional).

---

## 1. Document Repository & Organization

| Capability | Detail | Release |
|---|---|---|
| Central cloud repository | Single logical store backed by S3-compatible object storage; documents never live on app servers | MVP |
| Standardized folder structures | Admin-defined **folder templates** (e.g. `Department / Year / Document Type`) that can be stamped out per department or client; templates lock structure so users can't drift | MVP |
| File types | PDF, PDF/A, TIFF (multi-page), JPG/PNG, DOCX/DOC, XLSX/XLS, PPTX, TXT, CSV, EML/MSG; any other type stored as binary with metadata | MVP |
| Upload | Drag-and-drop single + bulk (folder drop, 1,000+ files), resumable chunked uploads for large files (up to 5 GB/file) | MVP |
| Policy-based upload rules | Per-folder **upload policies**: required metadata fields, allowed file types, max size, naming convention regex, mandatory document type. A document cannot be committed until the policy passes | MVP |
| Scanner / email capture | Watched SFTP/email inbox drop folder for MFP scanners | Phase 2 |
| Duplicate detection | SHA-256 content hash; warn on duplicate upload | MVP |

## 2. Metadata & Indexing

| Capability | Detail | Release |
|---|---|---|
| Configurable fields | Admin UI to define fields: text, number, date, single/multi-select, user, reference number (with auto-sequence), boolean | MVP |
| Document types | Each type (Invoice, Contract, HR Record…) carries its own field set and default retention rule | MVP |
| Edit metadata | Inline editing with validation; every change is versioned and audited | MVP |
| Bulk metadata edit | Apply values to many documents at once | MVP |
| Filter & search by fields | Every field is indexed and usable as a search facet | MVP |
| Auto-extraction | Suggest metadata from OCR text (dates, reference numbers) | Phase 2 |

## 3. Search

| Capability | Detail | Release |
|---|---|---|
| Full-text search | Content of electronic files (via Apache Tika) **and** scanned images (via OCR) indexed in OpenSearch | MVP |
| Filters | Date range, any metadata field, folder (incl. subfolders), document type, owner, status, file type | MVP |
| Hit highlighting | Matched terms highlighted in result snippets and in the viewer | MVP |
| Permission-aware | Results trimmed by ACL at query time — users never see a document's existence if they lack access | MVP |
| Saved searches | Save and share frequently used queries | MVP |
| Performance target | p95 < 500 ms on 5M documents (sized on discovery) | MVP |

## 4. Version Control & Audit Trail

| Capability | Detail | Release |
|---|---|---|
| Version history | Every upload of new content creates an immutable version (major/minor numbering) | MVP |
| Check-out / check-in | Optional lock while editing to prevent conflicting updates | MVP |
| View / restore | Open any prior version; restore creates a new version (history never rewritten) | MVP |
| Compare | Side-by-side visual compare; text diff for DOCX/PDF text layers; metadata diff | MVP |
| Audit log | Every action: upload, view, download, print, edit metadata, version, move, share, approve, reject, delete, hold, permission change, login — with user, timestamp (UTC), IP, user agent | MVP |
| Tamper evidence | Hash-chained, append-only audit table; daily anchor exported to WORM storage | MVP |

## 5. Approval Workflows

| Capability | Detail | Release |
|---|---|---|
| Linear workflows | Admin-defined templates: ordered steps, each step = a user, a role, or "any of a group" | MVP |
| Ad-hoc routing | Send a document for review to chosen people without a template | MVP |
| Statuses | Draft → Pending Review → Approved / Rejected / Changes Requested / Cancelled | MVP |
| Sign-off | Approve/reject with mandatory comment on reject; approval stamps a version (later edits don't inherit approval) | MVP |
| Notifications | In-app + email; reminders and escalation after configurable SLA | MVP |
| Teams / Slack notifications | Webhook-based | Phase 2 |
| E-signature integration | DocuSign / Adobe Sign | Phase 2 |

## 6. Security & Access Control

| Capability | Detail | Release |
|---|---|---|
| Roles | Admin, Records Manager, Manager, User, Viewer, Auditor (read-only audit access) — custom roles supported | MVP |
| Granular ACL | Folder-level permissions with inheritance; document-level overrides; explicit deny | MVP |
| Encryption | TLS 1.3 in transit; AES-256 SSE-KMS at rest; encrypted DB and backups | MVP |
| Authentication | OIDC/SAML SSO (Azure AD, Google, Okta) + local accounts with MFA (TOTP/WebAuthn) | MVP |
| Session security | Short-lived tokens, idle timeout, device/session revocation | MVP |
| Watermarking | Dynamic watermark on view/download for sensitive classes | Phase 2 |

## 7. Records Management & Archival

| Capability | Detail | Release |
|---|---|---|
| Retention policies | Rules by document type / folder / metadata; trigger = created date, event date, or custom field (e.g. "7 years after contract end") | MVP |
| Legal hold | Place hold on documents, folders or a saved search; overrides retention and blocks deletion | MVP |
| Defensible deletion | Disposition queue → Records Manager review → certified destruction with certificate & audit record | MVP |
| Archival tier | Auto-move aged content to cold storage (S3 Glacier IR) transparently | MVP |
| Legacy migration | Migration toolkit: CSV/XML manifest + file import, metadata mapping, validation report, dry-run | MVP (tooling) / per-source effort scoped in discovery |

## 8. Integrations

| Capability | Detail | Release |
|---|---|---|
| REST API | Full CRUD for documents, versions, metadata, folders, search, workflows; OpenAPI 3.1 spec + Swagger UI | MVP |
| Auth for integrations | OAuth2 client credentials + scoped API keys | MVP |
| Webhooks | Signed events (document.created, version.added, workflow.approved, …) | MVP |
| SDK | TypeScript + Python client generated from the spec | MVP |
| CRM/ERP connectors | Specific systems (e.g. Salesforce, Dynamics, SAP, Odoo) — one connector included, others per connector | MVP: 1 connector |
| Office integration | Open/save from Word/Excel via WebDAV or Office add-in | Phase 2 |

---

## Non-functional requirements

- **Availability:** 99.9% target, multi-AZ database, stateless app tier
- **Backups:** daily full + point-in-time recovery (35 days), quarterly restore drill
- **Scale baseline:** 500 concurrent users, 5M documents, 20 TB — horizontally scalable beyond
- **Accessibility:** WCAG 2.1 AA
- **Browser support:** latest 2 versions of Chrome, Edge, Safari, Firefox
- **Observability:** structured logs, metrics, traces, uptime alerts
