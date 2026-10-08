# 03 — System Architecture

## High-level view

```mermaid
flowchart LR
  subgraph Clients
    W[Web app<br/>Next.js]
    I[CRM / ERP<br/>integrations]
    S[Scanners / legacy<br/>import]
  end

  subgraph Edge
    CDN[CDN + WAF]
    GW[API Gateway<br/>TLS 1.3 · rate limits]
  end

  subgraph Core["Core services (NestJS, stateless)"]
    API[Document API]
    AUTHZ[AuthZ / ACL engine]
    WF[Workflow engine]
    REC[Records engine<br/>retention · holds]
    AUD[Audit writer]
  end

  subgraph Pipeline["Async pipeline (BullMQ workers)"]
    AV[Virus scan<br/>ClamAV]
    EX[Text extraction<br/>Apache Tika]
    OCR[OCR<br/>OCRmyPDF + Tesseract]
    TH[Previews & thumbnails<br/>Gotenberg / LibreOffice]
    IDX[Indexer]
  end

  subgraph Data
    PG[(PostgreSQL<br/>metadata · ACL · audit)]
    OS[(OpenSearch<br/>full-text)]
    OBJ[(Object storage<br/>S3 · SSE-KMS)]
    WORM[(WORM bucket<br/>holds · audit anchors)]
    RD[(Redis<br/>queues · cache)]
  end

  IDP[SSO / IdP<br/>OIDC · SAML · MFA]

  W --> CDN --> GW
  I --> GW
  S --> GW
  GW --> API
  API --> AUTHZ
  API --> WF
  API --> REC
  API --> AUD
  API --> PG
  API -->|presigned upload/download| OBJ
  API --> RD
  RD --> AV --> EX --> OCR --> IDX --> OS
  RD --> TH --> OBJ
  REC --> WORM
  AUD --> PG
  AUD --> WORM
  W -.login.-> IDP
  GW -.verify JWT.-> IDP
```

## Why this shape

- **Modular monolith first.** One NestJS codebase with strict module boundaries (documents, metadata, search, workflow, records, audit, admin). Deploys as one API service plus workers. This is faster to build and operate than microservices, and any module can be split out later without rewrites.
- **Files never pass through the app server.** Clients upload/download directly to object storage with short-lived presigned URLs. The API only issues the URL after the ACL check and logs the event. This keeps the API fast and memory-flat regardless of file size.
- **Processing is asynchronous.** Uploads return immediately; OCR, extraction, previews and indexing run as queued jobs with retries and dead-letter handling. A 200-page scan doesn't block anyone.
- **PostgreSQL is the source of truth.** OpenSearch is a derived, rebuildable index. If the index is lost it is regenerated from Postgres + object storage.
- **Search is permission-trimmed at the engine.** Each indexed document carries its effective ACL principals; every query adds a filter for the caller's user + group + role IDs. No post-filtering, so no leaking of counts or snippets.

## Upload sequence

```mermaid
sequenceDiagram
  autonumber
  participant U as User (browser)
  participant A as API
  participant P as Policy engine
  participant S as Object storage
  participant Q as Job queue
  participant W as Workers
  U->>A: POST /uploads (filename, size, folderId, metadata)
  A->>P: validate against folder upload policy
  P-->>A: ok / list of violations
  A->>A: ACL check (folder:write)
  A-->>U: presigned multipart URLs + uploadId
  U->>S: PUT parts directly (resumable)
  U->>A: POST /uploads/{id}/complete
  A->>A: create Document + Version v1.0 (status: processing)
  A->>Q: enqueue scan → extract → ocr → preview → index
  A-->>U: 201 Created
  Q->>W: run pipeline
  W-->>A: status = ready (or quarantined)
  A-->>U: live status via SSE
```

## Deployment

| Environment | Purpose |
|---|---|
| `dev` | Per-developer, Docker Compose (Postgres, MinIO, OpenSearch, Redis, Keycloak) |
| `staging` | Mirrors prod at smaller scale; client UAT happens here |
| `prod` | Multi-AZ, auto-scaling app + workers, managed data services |

**Default hosting:** AWS (ECS Fargate or EKS, RDS PostgreSQL, S3, OpenSearch Service, ElastiCache, KMS). Azure equivalents (AKS, Azure Database for PostgreSQL, Blob Storage, Azure AI Search/OpenSearch) are a like-for-like swap if you're a Microsoft shop. On-premise deployment is possible on Kubernetes with MinIO — confirm in discovery.

Infrastructure is defined in **Terraform**; CI/CD via **GitHub Actions** with automated tests, security scans and blue/green deploys.
