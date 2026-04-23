# Roadmap — MatrixRates Import/Export Docs

**Project:** MatrixRates Import/Export Docs
**Milestone:** 1 — Complete merchant documentation for import/export workflows
**Granularity:** Fine
**Requirements:** 20 v1 requirements mapped across 6 phases
**Coverage:** 20/20 ✓

---

## Phases

- [x] **Phase 1: Site Rebrand & Scaffold** — Strip Mintlify starter kit, apply MatrixRates branding, and lock URL structure atomically
- [x] **Phase 2: Export Guide** — Write the single-page export walkthrough so merchants can download their current rate table ✓
- [ ] **Phase 3: CSV Reference** — Write the complete CSV column reference with a downloadable sample and inline worked example ✓ Planned
- [ ] **Phase 4: Import Guide** — Write the multi-step import walkthrough with screenshots, pre-flight warnings, and post-import verification
- [ ] **Phase 5: Troubleshooting Guide** — Write the error reference with verbatim error strings, plain-English explanations, and fix steps
- [ ] **Phase 6: Home Page** — Write the docs root page with intro copy, navigation cards, and prerequisites block

---

## Phase Details

### Phase 1: Site Rebrand & Scaffold
**Goal**: The live Mintlify site reflects MatrixRates identity and has a locked navigation skeleton — no starter kit content remains
**Depends on**: Nothing (first phase)
**Requirements**: SITE-01, SITE-02, SITE-03, SITE-04
**Success Criteria** (what must be TRUE):
  1. Visiting the live docs URL shows the MatrixRates name and brand color — no Mintlify green, no "Mint Starter Kit" text visible anywhere
  2. The MatrixRates logo and favicon appear in the browser tab and navbar — not the Mintlify leaf icon
  3. All starter kit directories (`essentials/`, `ai-tools/`, `api-reference/`) and their MDX files are deleted; no boilerplate pages are reachable via any URL
  4. `docs.json` declares the complete URL path structure for all planned pages before any content pages exist — every future cross-link target is registered
  5. The support email (support@dojoapps.co.uk) is present in the navbar and no API Reference tab appears in the site navigation
**Plans**: 3 plans (01-01, 01-02, 01-03)
**UI hint**: yes

### Phase 2: Export Guide
**Goal**: Merchants can follow a numbered walkthrough to download their current rate table as a CSV, with a clear "what to do next" prompt
**Depends on**: Phase 1
**Requirements**: EXPORT-01, EXPORT-02
**Success Criteria** (what must be TRUE):
  1. A merchant can open the export page and follow each numbered step to completion without needing to refer to any external resource
  2. Every step in the export sequence has an expected result written in plain language ("A file called rates.csv will download to your computer")
  3. The page ends with a "What to do next" card group with at least three actionable paths (edit and re-import, keep as backup, share with team)
**Plans**: 1 plan (02-01)
**UI hint**: yes

### Phase 3: CSV Reference
**Goal**: Merchants can look up any column in their rate table CSV, download a working sample file, and understand edge case behaviour for blank/zero/boundary values
**Depends on**: Phase 1
**Requirements**: CSV-01, CSV-02, CSV-03, CSV-04
**Success Criteria** (what must be TRUE):
  1. A merchant can find any column by name in a reference table that shows column name, data type, required/optional status, accepted values, and an example value
  2. A working sample CSV file is downloadable directly from the page and opens correctly in spreadsheet software (correct columns, valid values)
  3. A complete filled-in example rate table is visible inline on the page — a merchant can see what a real, valid CSV looks like without downloading anything
  4. The page documents what happens when a cell is blank, when a price is zero, and when a weight value is at the minimum or maximum boundary — each case has an explicit outcome stated
**Plans**: TBD
**UI hint**: yes

### Phase 4: Import Guide
**Goal**: Merchants can follow a numbered, screenshot-guided walkthrough to upload a rate table CSV, verify the import succeeded, and know exactly what to do next
**Depends on**: Phase 2, Phase 3
**Requirements**: IMPORT-01, IMPORT-02, IMPORT-03, IMPORT-04
**Success Criteria** (what must be TRUE):
  1. A merchant can open the import guide and complete the upload from start to finish using only the numbered steps — no guessing, no dead links
  2. The guide opens with a visible "Before you import" warning block that links directly to the CSV Reference page, so merchants check their file format before uploading
  3. Each UI step has a screenshot showing the exact state of the MatrixRates app interface the merchant should see at that point
  4. The guide includes post-import verification steps explaining how to confirm the import worked in Shopify shipping settings
  5. The guide ends with a "What to do next" card group covering at least: test your rates, troubleshoot an error, export a backup
**Plans**: TBD
**UI hint**: yes

### Phase 5: Troubleshooting Guide
**Goal**: A merchant who encounters any import error can find it by scanning or searching, read a plain-English explanation, and follow concrete fix steps without contacting support
**Depends on**: Phase 4
**Requirements**: TROUBLE-01, TROUBLE-02, TROUBLE-03
**Success Criteria** (what must be TRUE):
  1. Every error entry uses the verbatim error string as its heading — a merchant can paste the exact error text from the app and find the matching entry
  2. Every error entry contains three things: the plain-English cause, a screenshot of the error in context, and a specific numbered fix sequence that resolves it
  3. A "Common mistakes before importing" section covers at least three pre-import failure modes: wrong file format, missing required columns, and invalid values — each with a fix
  4. The page ends with a "Still stuck?" block containing a direct, clickable mailto link to support@dojoapps.co.uk
**Plans**: TBD
**UI hint**: yes

### Phase 6: Home Page
**Goal**: Merchants arriving at the docs root immediately understand what MatrixRates Import/Export does, who it is for, and where to start — with no broken navigation cards
**Depends on**: Phase 2, Phase 3, Phase 4, Phase 5
**Requirements**: HOME-01, HOME-02, HOME-03
**Success Criteria** (what must be TRUE):
  1. A merchant landing on the docs root reads a clear one-paragraph intro explaining what MatrixRates Import/Export does and explicitly naming the audience (Shopify store owners)
  2. Navigation cards for all four major sections (Import, Export, CSV Reference, Troubleshooting) are visible on the home page and each card links to a live, non-404 page
  3. A "Before you start" block on the home page lists the two prerequisites: the app is installed, and the merchant has Shopify admin access
**Plans**: TBD
**UI hint**: yes

---

## Progress Table

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Site Rebrand & Scaffold | 3/3 | ✓ Complete | 3/3 |
| 2. Export Guide | 1/1 | ✓ Complete | 1/1 |
| 3. CSV Reference | 1/1 | ✓ Planned | 1/1 |
| 4. Import Guide | 0/? | Not started | 0/? |
| 5. Troubleshooting Guide | 0/? | Not started | 0/? |
| 6. Home Page | 0/? | Not started | 0/? |

---

## Coverage Map

| Requirement | Phase | Status |
|-------------|-------|--------|
| SITE-01 | Phase 1 | Pending |
| SITE-02 | Phase 1 | Pending |
| SITE-03 | Phase 1 | Pending |
| SITE-04 | Phase 1 | Pending |
| EXPORT-01 | Phase 2 | ✓ Complete |
| EXPORT-02 | Phase 2 | ✓ Complete |
| CSV-01 | Phase 3 | Pending |
| CSV-02 | Phase 3 | Pending |
| CSV-03 | Phase 3 | Pending |
| CSV-04 | Phase 3 | Pending |
| IMPORT-01 | Phase 4 | Pending |
| IMPORT-02 | Phase 4 | Pending |
| IMPORT-03 | Phase 4 | Pending |
| IMPORT-04 | Phase 4 | Pending |
| TROUBLE-01 | Phase 5 | Pending |
| TROUBLE-02 | Phase 5 | Pending |
| TROUBLE-03 | Phase 5 | Pending |
| HOME-01 | Phase 6 | Pending |
| HOME-02 | Phase 6 | Pending |
| HOME-03 | Phase 6 | Pending |

**Total mapped: 20/20**

---

*Generated: 2026-04-22 | Milestone 1 | Granularity: fine*
