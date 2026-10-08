# Vaultline EDMS — Discovery MVP

> A working design package for your Enterprise Document Management System, built **before** the contract — so you can judge the thinking, not just the cover letter.

This repository is a discovery-phase deliverable produced in response to your Upwork brief *"Design and build a full Enterprise Document Management System (EDMS)"*. It answers every item you asked for in the proposal, and adds a clickable prototype of the main screens so you can see the product flow today.

**Working name:** *Vaultline* (placeholder — the system will carry your company's brand).

---

## What's in here

| Path | What it answers from your brief |
|---|---|
| [`prototype/index.html`](prototype/index.html) | **Wireframes & UI/UX** — clickable prototype of every main screen. **Live:** https://edms.apps.netdrix.com |
| [`docs/01-scope-of-work.md`](docs/01-scope-of-work.md) | Clear breakdown of the full scope, mapped 1:1 to your 8 feature areas |
| [`docs/02-ux-approach.md`](docs/02-ux-approach.md) | UI/UX design approach, screen inventory and user flows |
| [`docs/03-architecture.md`](docs/03-architecture.md) | System architecture, components, data flow |
| [`docs/04-tech-stack.md`](docs/04-tech-stack.md) | Recommended stack and *why* each choice |
| [`docs/05-data-model.md`](docs/05-data-model.md) | Core entities, versioning model, metadata schema engine |
| [`docs/06-security.md`](docs/06-security.md) | Authentication, RBAC + ACL model, encryption, audit integrity |
| [`docs/07-records-management.md`](docs/07-records-management.md) | Retention, legal hold, defensible deletion, legacy migration |
| [`docs/08-workflows.md`](docs/08-workflows.md) | Linear approval engine, statuses, notifications |
| [`docs/09-integrations-api.md`](docs/09-integrations-api.md) | API design, webhooks, CRM/ERP integration pattern |
| [`api/openapi.yaml`](api/openapi.yaml) | Starter OpenAPI 3.1 spec for the public API |
| [`docs/10-testing-qa.md`](docs/10-testing-qa.md) | Testing strategy, QA gates, documentation deliverables |
| [`docs/11-timeline-and-pricing.md`](docs/11-timeline-and-pricing.md) | Milestones, timeline, transparent fixed-price breakdown |
| [`docs/12-assumptions-and-out-of-scope.md`](docs/12-assumptions-and-out-of-scope.md) | Assumptions, exclusions, open questions for discovery |

---

## Viewing the prototype

```bash
# any of these work
open prototype/index.html          # macOS
xdg-open prototype/index.html      # Linux
start prototype/index.html         # Windows
```

Use the left sidebar to move between screens. Buttons that start a flow (Upload, Route for approval, Place legal hold, Compare versions) open the real interaction pattern with sample data. Nothing is sent anywhere — it is a self-contained HTML file.

### Screens included

1. **Dashboard** — my tasks, recent documents, storage & compliance at a glance
2. **Repository** — standardized folder tree, document grid, bulk actions
3. **Upload** — single/bulk drop zone with *policy-enforced required metadata*
4. **Document view** — preview, metadata panel, version history, compare, audit trail
5. **Search** — full-text (incl. OCR'd scans) with faceted filters and hit highlighting
6. **Approvals** — inbox with Pending / Approved / Rejected, step-by-step routing
7. **Records** — retention schedules, legal holds, disposition queue
8. **Audit log** — tamper-evident, filterable, exportable
9. **Admin** — roles & permissions matrix, metadata schemas, upload policies, API keys & webhooks

---

## The short version

- **Stack:** Next.js + TypeScript UI · NestJS (TypeScript) API · PostgreSQL · S3-compatible object storage with SSE-KMS · OpenSearch full-text · OCRmyPDF/Tesseract + Apache Tika extraction pipeline · Redis/BullMQ jobs · OIDC SSO (Keycloak or your Azure AD/Okta) with MFA.
- **Security posture:** TLS 1.3 everywhere, AES-256 at rest with per-tenant KMS keys, deny-by-default RBAC + folder/document ACLs enforced in the API *and* in search, append-only hash-chained audit log, WORM storage for records under hold.
- **Delivery:** paid 2-week discovery & design phase, then 6 build milestones. ~16 weeks to production. Fixed price per milestone — see [timeline & pricing](docs/11-timeline-and-pricing.md).

---

## About me

**Aliyan Baig** — Founder, Netdrix (software architecture & product engineering).
I design and build the whole thing myself with my team: architecture, wireframes, UI, backend, infrastructure and documentation. One point of contact, no hand-offs between agencies.

📧 aliyan@aliyanbaig.com
