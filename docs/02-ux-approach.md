# 02 — UI/UX Design Approach

> The clickable prototype is in [`prototype/index.html`](../prototype/index.html). This document explains the decisions behind it.

## Principles

1. **Familiar first.** Users already know file explorers. The repository looks and behaves like one (tree left, items right, breadcrumbs, right-click actions) so training time is near zero.
2. **Compliance is invisible until it matters.** Retention, holds and audit run silently. Users only see them as a calm badge ("Retained until 2033", "On legal hold") — never as friction on everyday work.
3. **Policy at the door, not after.** Required metadata is asked *during* upload with smart defaults, so the repository never fills with untagged junk.
4. **One document, one page.** Preview, metadata, versions, approvals and audit for a document all live on one screen in tabs — no hunting.
5. **Calm, enterprise-grade visual language.** Neutral grayscale with a single blue accent for actions; status colours reserved strictly for meaning (green approved, amber pending, red rejected/hold).

## Information architecture

```
Dashboard
Repository ── Folder ── Document
                          ├─ Preview
                          ├─ Details (metadata)
                          ├─ Versions (view · restore · compare)
                          ├─ Workflow
                          └─ Activity (audit)
Search (global, ⌘K from anywhere)
Approvals (My tasks · Sent by me · All)
Records (Retention · Legal holds · Disposition)   ← Records Manager
Audit log                                         ← Admin / Auditor
Admin (Users & roles · Permissions · Metadata · Folder templates · Upload policies · API & webhooks)
```

## Key flows

### Upload with policy enforcement
1. User drops files on a folder (or uses Upload).
2. System reads the folder's **upload policy** → shows required fields with type-aware inputs.
3. Fields pre-fill from folder context (department, year) and filename patterns.
4. Files that fail the policy (wrong type, missing field) are flagged inline; the rest can be committed.
5. Background: hash → virus scan → text extraction / OCR → index. Status chip updates live.

### Route for approval
1. From a document: *Route for approval* → pick a workflow template or add reviewers ad-hoc.
2. Each reviewer gets in-app + email task.
3. Approve / Reject (comment required) / Request changes.
4. Approval is bound to the exact version approved.

### Find anything
1. ⌘K or the search bar → full-text query.
2. Facets on the left (type, department, date, status, folder).
3. Results show highlighted snippets including text OCR'd from scans.
4. Save the search; optionally place a legal hold on the result set (Records Manager).

### Defensible deletion
1. Retention engine nightly flags eligible items → **Disposition queue**.
2. Records Manager reviews, approves batch.
3. System checks no active hold, purges content and versions, writes destruction certificate + audit event.

## Design deliverables in the discovery phase

- Validated sitemap & role-based navigation
- Hi-fi designs in Figma for all MVP screens (desktop + tablet), design tokens, component library
- Interactive Figma prototype for stakeholder walkthrough
- Usability walkthrough with 3–5 of your actual users
