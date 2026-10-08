# 04 — Recommended Technology Stack

| Layer | Choice | Why this, for an EDMS |
|---|---|---|
| **Frontend** | Next.js (React) + TypeScript, Tailwind, Radix UI primitives, TanStack Query | Mature enterprise ecosystem; accessible primitives for WCAG AA; server components keep initial loads fast for large folder trees |
| **Document viewer** | PDF.js + server-rendered page images for TIFF/Office | Views every format in-browser without the user downloading the file (download is a separately audited action) |
| **Backend API** | NestJS (Node.js, TypeScript) | Strong module boundaries, dependency injection and guards make RBAC/ACL enforcement consistent on every endpoint; one language front-to-back reduces cost and bugs |
| **Database** | PostgreSQL 16 | ACID for versions and audit; JSONB for configurable metadata with GIN indexes; row-level security as a second defence layer; proven at hundreds of millions of rows |
| **ORM / migrations** | Prisma (or Drizzle) with versioned SQL migrations | Type-safe queries, reviewable migrations |
| **Object storage** | Amazon S3 (or MinIO / Azure Blob) | Effectively unlimited, 11-nines durability, SSE-KMS encryption, Object Lock for WORM legal hold, lifecycle rules for archival tiers |
| **Full-text search** | OpenSearch | Open-source (no Elastic licence risk), BM25 relevance, highlighting, facets, document-level security filters; scales horizontally |
| **Text extraction** | Apache Tika | Extracts text + embedded metadata from 1,000+ formats (Office, PDF, email) |
| **OCR** | OCRmyPDF + Tesseract 5 | Produces searchable PDF/A from scans, multi-language, runs inside your environment (no data leaves). Optional AWS Textract/Azure DI for handwriting/forms |
| **Previews** | Gotenberg (LibreOffice headless) | Converts Office files to PDF for consistent preview and visual compare |
| **Background jobs** | Redis + BullMQ | Reliable retries, priorities, rate limits, dashboards; simple to operate |
| **Virus scanning** | ClamAV | Every upload scanned before it becomes available |
| **Auth** | OIDC/SAML via Keycloak (or your Azure AD / Okta directly) | SSO, MFA, SCIM user provisioning; no passwords stored by the app when SSO is used |
| **Secrets & keys** | AWS KMS + Secrets Manager (or Azure Key Vault) | Envelope encryption, key rotation, no secrets in code |
| **Email** | Amazon SES / SMTP relay | Workflow notifications |
| **Infra as code** | Terraform | Reproducible environments, reviewable changes |
| **CI/CD** | GitHub Actions | Tests, SAST (CodeQL/Semgrep), dependency audit, container scan (Trivy), deploy |
| **Observability** | OpenTelemetry → Grafana (Loki/Tempo/Prometheus) or CloudWatch | Logs, traces, metrics, alerting |
| **Testing** | Vitest/Jest, Playwright, k6, OWASP ZAP | Unit, E2E, load and security testing |

## Alternatives considered

| Instead of | Considered | Why not as default |
|---|---|---|
| Building custom | Alfresco / Nuxeo / M-Files customisation | Licence cost, heavy customisation tax, harder to own your roadmap; your brief asks for a system built for you |
| NestJS | .NET 8 / Java Spring Boot | Both excellent. If your IT team is .NET-standard, I'll switch — architecture is unchanged |
| OpenSearch | PostgreSQL full-text only | Fine to 500k docs; falls short on relevance, highlighting and scale at enterprise volume |
| Microservices from day 1 | — | Slower to deliver, more ops cost, no benefit at this scale; modular monolith gives a clean split path |

## Hosting cost estimate (indicative, AWS, staging + prod)

| Scale | Monthly infra |
|---|---|
| Up to 100 users, 1 TB | ~$450–700 |
| Up to 500 users, 10 TB | ~$1,200–2,000 |

Exact figures produced during discovery once document volumes and retention periods are known.
