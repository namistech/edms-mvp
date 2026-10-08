# 07 — Records Management & Archival

## Retention policies

A **retention rule** has:
- **Scope** — document type(s), folder(s), and/or metadata condition (e.g. `department = Finance AND type = Invoice`)
- **Trigger** — created date, last modified, or a date metadata field (contract end, employee exit date)
- **Period** — e.g. 7 years
- **Action at expiry** — *Review* (Records Manager decides), *Archive* (move to cold tier), or *Dispose*

Most specific rule wins; conflicts resolved by **longest retention** (the safe default). Each document shows its computed `Retain until` date and the rule that set it. Changes to rules are versioned and audited, and recomputation is previewed ("this change affects 12,480 documents") before it is applied.

## Legal hold

- Apply a hold to: individual documents, entire folders (including future additions), or the result set of a saved search.
- Each hold records matter name/reference, reason, custodian, who placed it and when.
- While held: no delete, no disposal, no version purge, metadata edits still allowed but audited. Retention timers keep running but cannot fire.
- Held content is additionally protected using **S3 Object Lock (legal hold flag)** so the storage layer itself refuses deletion.
- Releasing a hold requires the Records Manager role and a reason; release is audited.

## Defensible deletion

Defensible = you can prove *what* was deleted, *why*, *under which policy*, *who approved it*, and that nothing on hold was touched.

1. Nightly job finds documents past retention with action *Dispose* → **Disposition queue**.
2. Records Manager reviews (list, filters, spot-check), excludes items if needed, approves the batch (optionally two-person approval).
3. Executor re-checks holds at execution time, deletes all versions and renditions from storage and index, keeps a tombstone record (id, title, type, rule, dates — no content).
4. Generates a signed **Certificate of Destruction** (PDF) stored in the WORM bucket; audit events written per item.

## Archival

- Lifecycle tiering: documents not accessed for N months move to S3 Glacier Instant Retrieval (or Azure Cool/Archive) — transparent to users, cost drops by ~68%+.
- Archived documents remain searchable (index stays hot).

## Legacy migration toolkit

| Step | What happens |
|---|---|
| Inventory | Analyse source (file shares, SharePoint, another DMS export, database + blob) — counts, types, sizes, metadata available |
| Mapping | Map source folders → folder templates, source fields → metadata fields, set default type/retention |
| Dry run | Import into staging; validation report (missing fields, unsupported types, duplicates by hash) |
| Migration | Parallel, resumable import via the same pipeline (scan, OCR, index); original timestamps & authors preserved in metadata |
| Reconciliation | Count + checksum report source vs target, signed off by you |
| Cutover | Delta sync, source set read-only |

Migration effort per source is estimated in discovery once we see the source system and data quality.
