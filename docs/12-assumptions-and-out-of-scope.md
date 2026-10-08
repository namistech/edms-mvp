# 12 — Assumptions, Out of Scope & Open Questions

## Assumptions

1. Single organisation (one tenant) at launch; architecture is multi-tenant-ready.
2. Up to ~500 named users, ~5M documents and ~20 TB in the first 3 years. Larger volumes change infra sizing, not architecture.
3. You provide an identity provider (Azure AD / Google / Okta) or accept the bundled Keycloak.
4. Hosting on AWS by default (Azure or on-prem Kubernetes confirmed in discovery).
5. English UI at launch; OCR in English (other OCR languages can be added — Tesseract supports 100+).
6. Your team provides: a product owner with decision authority, sample documents (incl. representative scans) for testing, access to legacy systems for migration analysis, and UAT testers within the 5-day windows.
7. Retention periods and legal requirements are defined by your legal/compliance team; I implement and advise on mechanics, not legal interpretation.
8. One legacy source and one CRM/ERP connector are in the fixed price.

## Out of scope (unless added)

- Physical paper scanning service (we ingest the scans; hardware scanning is your process or a bureau)
- Native mobile apps (the web app is responsive for tablet/mobile browsers)
- Complex BPMN workflows (parallel branches, conditional routing, forms) — linear only, as per brief
- Advanced e-signature (qualified electronic signatures) — Phase 2 via DocuSign/Adobe Sign
- Real-time co-authoring of Office documents
- AI features (summarisation, classification, chat with documents) — easy to add on top of the extracted text later
- Formal certification audits (ISO 27001 / SOC 2) — the system is built to support them
- 24/7 production support (available as a retainer)

## Open questions for discovery

1. Which CRM/ERP systems need to integrate first, and what triggers the integration (document created in EDMS vs in ERP)?
2. What are the legacy sources (file shares, SharePoint, another DMS, database) and rough volumes?
3. Approximate daily scan volume and paper quality (affects OCR worker sizing)?
4. Regulatory frameworks you must meet (e.g. GDPR, HIPAA, SOX, local data residency)?
5. Cloud preference / existing cloud accounts, and any on-prem requirement?
6. How are departments structured — should folder templates mirror the org chart, the process, or the client/project?
7. Who will act as Records Manager and approve dispositions?
8. Do external parties (clients, auditors, vendors) need access? (Guest access / secure share links would be added.)
