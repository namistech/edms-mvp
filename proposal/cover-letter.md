# Upwork cover letter

> Paste into the proposal. Upwork shows only the first two lines in the preview, so keep them as they are.
> Before sending: open the prototype → Share → set it to "Anyone with the link". Replace [VIDEO LINK] / [REPO LINK] or delete those lines.

---

Hi! Instead of a cover letter, I built a working prototype of your EDMS. Click through it here: https://claude.ai/artifact/DpLvmCUjXBtir3PVtJaAis

2-minute walkthrough: [VIDEO LINK] · Full proposal docs (scope, architecture, security, timeline, pricing): [REPO LINK]

**What to try in the prototype (about 3 minutes):**
1. **Upload:** click Upload, then Browse files. The folder's policy blocks saving until the required metadata is filled in. One reference number is wrong and the TIFF file isn't allowed in Contracts, so fix those and Save turns on.
2. **Versions:** open "Master Services Agreement" → Versions → Compare v2.0 ↔ v3.0 to see the changed clauses and metadata. Restoring a version creates a new one, so history is never overwritten.
3. **Approvals:** on the same document, click Approve. The decision is tied to that exact version and recorded in the audit log.
4. **Search:** search "indemnity". One hit comes from a scanned paper document via OCR.
5. **Access control:** at the bottom left, switch the role from Admin to Viewer. Admin menus, the HR folder and restricted search results disappear completely.
6. **Records:** go to Records & holds to see retention rules, legal holds, two-person approved deletion and a legacy migration dry run.

**How I'd build it:** Next.js + NestJS (TypeScript), PostgreSQL, S3 with KMS encryption and Object Lock for legal holds, OpenSearch for full-text search, Tika + OCRmyPDF/Tesseract for scans, and SSO with MFA. Permissions are enforced in the API and inside the search engine, so users never see documents they can't access. The audit log is append-only and hash-chained, and there's an independent pen test before go-live. If .NET is your standard, I'm happy to use it instead.

**Plan:** a paid 2-week discovery and design phase, then 6 fixed-price milestones over about 16 weeks ($29,000 total; the breakdown is in the docs). Each milestone is demoed on staging and paid only when you accept it. You own the code from the first commit.

I'm a software architect and the founder of Netdrix. I handle architecture, UI/UX, backend, infrastructure and documentation myself, with my team covering QA and DevOps, so you'd have one point of contact who owns the whole build.

Would a 20-minute call to go through the prototype together work for you?

Aliyan Baig
