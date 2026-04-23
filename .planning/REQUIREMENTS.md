# Requirements — MatrixRates Import/Export Docs

**Project:** MatrixRates Import/Export Docs
**Scope:** User guides + knowledge base for non-technical Shopify merchants
**v1 goal:** Merchant can import their first rate table and diagnose any failure without contacting support

---

## v1 Requirements

### Site Setup

- [x] **SITE-01**: Merchant sees a MatrixRates-branded site (not Mintlify starter kit) — `docs.json` updated with correct name, support link (support@dojoapps.co.uk), removed API reference tab, and placeholder brand color
- [x] **SITE-02**: All Mintlify starter kit content is removed — `essentials/`, `ai-tools/`, `api-reference/` directories and all boilerplate MDX pages deleted atomically
- [x] **SITE-03**: Site displays MatrixRates logo and favicon instead of Mintlify defaults
- [x] **SITE-04**: URL structure (all page paths) is defined in `docs.json` before any content pages are written, so cross-links never break

### Home Page

- [ ] **HOME-01**: Merchant arriving at the docs root sees a clear intro explaining what MatrixRates Import/Export does and who it is for
- [ ] **HOME-02**: Merchant can navigate to any major section (Import, Export, CSV Reference, Troubleshooting) from the home page via quick-start cards
- [ ] **HOME-03**: Home page shows a "Before you start" block listing prerequisites (app installed, Shopify admin access)

### Export Guide

- [x] **EXPORT-01**: Merchant can follow a numbered step-by-step guide to download their current rate table as a CSV from the app
- [x] **EXPORT-02**: Export guide ends with "What to do next" cards (edit and re-import, keep as backup, share with team)

### CSV Reference

- [x] **CSV-01**: Merchant can look up any column in a reference table showing column name, data type, required/optional status, accepted values, and an example value
- [x] **CSV-02**: Merchant can download a working sample CSV file directly from the docs page to use as a starting point
- [x] **CSV-03**: Page shows a complete filled-in example rate table inline so merchants can see what a real CSV looks like
- [x] **CSV-04**: Reference page documents edge case behaviour for blank values, zero prices, and boundary weight conditions

### Import Guide

- [x] **IMPORT-01**: Merchant can follow a numbered step-by-step walkthrough to upload a rate table CSV, with screenshots at each UI step
- [x] **IMPORT-02**: Import guide begins with a "Before you import" warning block linking to the CSV reference, so merchants check their file format first
- [x] **IMPORT-03**: Import guide includes post-import confirmation steps showing how to verify the import worked in Shopify shipping settings
- [x] **IMPORT-04**: Import guide ends with "What to do next" cards (test your rates, troubleshoot an error, export a backup)

### Troubleshooting

- [ ] **TROUBLE-01**: Merchant can find any import error by searching or scanning accordion entries, each showing the verbatim error message, a plain-English cause, and a step-by-step fix
- [ ] **TROUBLE-02**: Troubleshooting page has a "Common mistakes before importing" section covering wrong file format, missing required columns, and invalid values
- [ ] **TROUBLE-03**: Troubleshooting page ends with a "Still stuck?" block with a direct link to support@dojoapps.co.uk

---

## v2 Requirements (deferred)

- Full app settings documentation — out of scope for focused v1
- Billing and account management docs — different audience moment
- Developer/API documentation — wrong audience
- Advanced rate configuration guides — expand after core import/export is solid
- Versioned docs — only needed when the app has breaking changes
- Contextual AI chat (ChatGPT/Claude/Perplexity integration in docs.json) — hallucination risk until content is stable and authoritative

---

## Out of Scope

- API reference tab — wrong audience (merchants, not developers); Mintlify starter boilerplate to delete
- AI tools guide (Cursor, Claude Code, Windsurf pages) — Mintlify starter boilerplate, irrelevant to merchants
- Changelog or release notes — no versioning strategy defined yet
- Community forum or user-contributed content — out of scope for v1

---

## Traceability

| REQ-ID | Phase | Status |
|--------|-------|--------|
| SITE-01 | Phase 1 | Complete |
| SITE-02 | Phase 1 | Complete |
| SITE-03 | Phase 1 | Complete |
| SITE-04 | Phase 1 | Complete |
| EXPORT-01 | Phase 2 | Complete |
| EXPORT-02 | Phase 2 | Complete |
| CSV-01 | Phase 3 | Complete |
| CSV-02 | Phase 3 | Complete |
| CSV-03 | Phase 3 | Complete |
| CSV-04 | Phase 3 | Complete |
| IMPORT-01 | Phase 4 | Complete |
| IMPORT-02 | Phase 4 | Complete |
| IMPORT-03 | Phase 4 | Complete |
| IMPORT-04 | Phase 4 | Complete |
| TROUBLE-01 | Phase 5 | Pending |
| TROUBLE-02 | Phase 5 | Pending |
| TROUBLE-03 | Phase 5 | Pending |
| HOME-01 | Phase 6 | Pending |
| HOME-02 | Phase 6 | Pending |
| HOME-03 | Phase 6 | Pending |

---

*Generated: 2026-04-22 | Scope: fine granularity | Mode: YOLO*
