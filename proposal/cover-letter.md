# Upwork cover letter

> Paste into the proposal. Replace `[REPO LINK]` and `[VIDEO LINK]`. Keep the first two lines — Upwork shows only those in the preview.

---

Hi — instead of a cover letter, I built you a discovery MVP for this EDMS. 2-min video: [VIDEO LINK] · Repo with clickable prototype + full proposal docs: [REPO LINK]

What's in it, mapped to what you asked for:

- **Wireframes / UI flow:** a clickable prototype of every main screen — dashboard, repository with folder templates, bulk upload with **policy-enforced required metadata**, document view with **version history, visual compare and restore**, full-text search including **OCR'd scans**, linear approval workflows, **retention / legal hold / defensible deletion**, hash-chained audit log, roles & permissions, and API/webhooks. You can switch roles (Admin → Viewer) and watch menus, folders and search results change.
- **Scope breakdown:** your 8 feature areas, each split into concrete capabilities, MVP vs Phase 2.
- **Stack and why:** Next.js + NestJS (TypeScript), PostgreSQL, S3 with KMS encryption and Object Lock, OpenSearch, Tika + OCRmyPDF/Tesseract, SSO + MFA. The docs explain each choice and the alternatives (.NET is fine too if that's your standard).
- **Security:** deny-by-default RBAC plus folder/document ACLs, enforced in the API **and inside the search engine**, so users never learn that a restricted document exists. AES-256 at rest, TLS 1.3, append-only hash-chained audit with a WORM anchor, and an independent pen test before go-live.
- **Timeline and price:** paid 2-week discovery & design phase, then 6 fixed-price milestones over ~16 weeks. Every milestone is demoed on staging and released only when you accept it. You own all code from the first commit.
- **Assumptions and out of scope:** written down, plus 8 questions I'd settle in discovery.

About me: I'm a software architect and founder of Netdrix. I design and build end to end — architecture, UI/UX, backend, infrastructure and documentation — with my team covering QA and DevOps. You'd work with one person who owns the whole thing.

Happy to start with the discovery phase. A 20-minute call to go through the prototype would be the quickest next step.

Aliyan Baig
