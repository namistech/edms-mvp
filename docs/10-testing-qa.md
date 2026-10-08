# 10 — Testing, QA & Documentation

## Test strategy

| Layer | Tooling | What it covers | Gate |
|---|---|---|---|
| Unit | Vitest / Jest | Permission resolver, retention calculator, workflow state machine, metadata validation | ≥ 85% coverage on core modules; CI blocks merge on failure |
| Integration | Jest + Testcontainers (real Postgres, MinIO, OpenSearch, Redis) | API endpoints end-to-end against real services | Every endpoint, every role |
| Authorization matrix | Generated tests | Every route × every role × allow/deny ACL cases — proves no endpoint leaks | 100% pass, no exceptions |
| End-to-end | Playwright | Upload with policy, search, version compare, approval flow, legal hold, disposition | Run on every PR against preview env |
| Search relevance | Golden query set | 50+ real queries with expected top results from your sample documents | Tracked per release |
| OCR accuracy | Sample scan set from your archive | Character accuracy on your actual paper quality | Benchmarked in discovery |
| Performance | k6 | 500 concurrent users, bulk upload of 10k files, search p95 | p95 search < 500 ms, API p95 < 300 ms |
| Security | ZAP DAST, Semgrep/CodeQL SAST, Trivy, dependency audit | OWASP Top 10 | No high/critical open at release |
| Penetration test | Independent third party | Full application + infra | All criticals/highs fixed before go-live |
| Accessibility | axe-core + manual screen-reader pass | WCAG 2.1 AA | No serious violations |
| UAT | Your team on staging | Scripted scenarios per role | Your written sign-off per milestone |

## Quality process

- Trunk-based with short-lived branches, PR review on every change, conventional commits.
- Preview environment per PR for you to click through.
- Every milestone ends with a **demo + UAT window** (5 working days) before payment release.
- Defects found in UAT on milestone scope are fixed at no cost.

## Documentation delivered

| Document | Audience |
|---|---|
| Architecture & ADRs (decision records) | Your IT / future developers |
| API reference (OpenAPI + Redoc) + integration guide + SDK examples | Integration developers |
| Deployment & runbook (provisioning, backups, restore, key rotation, incident response) | Ops |
| Security & controls mapping (ASVS / ISO 27001) | Security, auditors |
| Admin guide (users, roles, metadata, folder templates, policies, retention, holds) | Admins, Records Managers |
| User guide + 5–8 short screen-recorded tutorials | End users |
| Data migration report | Project sponsor |
| Source code with README, in **your** Git repository from day one | You — you own everything |
