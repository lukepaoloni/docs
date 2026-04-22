# Project Research Summary

**Project:** MatrixRates Import/Export Docs
**Domain:** Merchant-facing Shopify app documentation (non-technical audience, import/export + CSV reference)
**Researched:** 2026-04-22
**Confidence:** HIGH

## Executive Summary

This is a focused, task-oriented knowledge base for non-technical Shopify store owners who need to import and export shipping rates using the MatrixRates app. Experts in this space (Matrixify, DataFeedWatch, Transporter, Klaviyo, Gorgias, Bold Commerce) converge on the same structure: a step-by-step import guide, a CSV format reference, and a troubleshooting guide with verbatim error strings. The platform is Mintlify, already deployed via GitHub auto-deploy. The codebase is a Mintlify starter kit that must be fully replaced — none of the existing content is suitable for a merchant audience.

The recommended approach is to build in three ordered phases. Phase 1 strips the starter kit boilerplate and configures the site's identity (branding, navigation skeleton, docs.json). Phase 2 writes the core task guides (export, CSV reference, import) in dependency order. Phase 3 adds the troubleshooting guide. This order is non-negotiable: the CSV column reference is a dependency of the import guide, and the import guide is a dependency of the troubleshooting guide. Inverting this order produces orphaned pages and broken cross-links before the site has any content.

The single largest risk is writing for the wrong audience. The domain is inherently technical (CSV, rate matrices, field validation), but the readers are store owners who manage shipping in a spreadsheet. Every step must be framed as a merchant action ("Click Import Rates") with an expected result ("A file picker will open"), not a system description. A secondary risk is the starter kit content: leaving any Mintlify boilerplate visible while adding MatrixRates pages destroys merchant trust immediately. Both risks are fully preventable with discipline, not complexity.

---

## Key Findings

### Recommended Stack

Mintlify (hosted SaaS) is the platform — already wired up with GitHub auto-deploy. Content is written in MDX (Markdown + embedded JSX components). Site configuration lives entirely in `docs.json`. Local preview uses `mint dev` (requires Node 19+). No infrastructure or build pipeline work is needed; the only technical work is configuring `docs.json` and writing `.mdx` files.

Six Mintlify components are load-bearing for this project and should be used on every applicable page:

**Core components:**
- `<Steps>` / `<Step>` — numbered, visually distinct workflow steps for every multi-action task
- `<Warning>` — high-visibility callout before any irreversible action (import overwrites existing rates)
- `<Note>` / `<Tip>` / `<Info>` — low-urgency callouts for format constraints, prerequisites, shortcuts
- `<Frame>` — required wrapper for every screenshot (consistency, rounded corners, width constraints)
- `<Card>` / `<CardGroup>` — visual navigation tiles for the home page and section landing pages
- `<Accordion>` / `<AccordionGroup>` — progressive disclosure for troubleshooting errors

Secondary components (`<Tabs>`, `<ResponseField>`, code blocks, snippets) are appropriate for the CSV reference and variant workflows. Components to avoid entirely: API reference playground, versioning dropdown, `<CodeGroup>`, inline HTML styling.

**docs.json requires these specific changes before any content is published:** replace `name`, `colors.primary`, `logo`, `favicon`, `navbar.links`, `navbar.primary`, `footer.socials`, and strip the API Reference and AI Tools tabs. See STACK.md for the full change table.

### Expected Features

**Must have (table stakes) — merchant bounces to support if missing:**
- Branded home/landing page — "Mint Starter Kit" visible destroys trust immediately
- Step-by-step import guide — the #1 pain point; non-technical users cannot infer steps from the UI
- CSV format reference page — merchants have existing rate tables and do not know the column spec
- Troubleshooting guide — the most-searched page type on Shopify app help sites after the main how-to
- Export guide — required as step 0 of the edit-and-reimport workflow and as a backup mechanism
- Working navigation — no dead links, no orphaned pages
- Plain-language tone — eighth-grade reading level throughout; no technical jargon
- Screenshots on every procedural step — non-technical merchants navigate by visual recognition

**Should have (differentiators) — measurably reduce support volume:**
- Downloadable sample CSV — eliminates the #1 format error mode
- Annotated inline CSV example — more scannable than a column table for non-technical readers
- "Before you start" prerequisites block at the top of the import guide
- Verbatim error strings as headings in troubleshooting — enables search and Google indexing to surface the right page
- "What to do next" CTA at the bottom of each guide
- Support link in the navbar — one-click escape hatch when docs fail

**Defer to post-v1:**
- Sample CSV download (high value but not blocking if inline annotated example is present)
- FAQ accordion expansion (add as real merchant questions accumulate)
- Advanced rate configuration docs (out of scope per PROJECT.md)

**Delete entirely (anti-features — starter kit boilerplate, wrong audience):**
- `api-reference/` directory and tab
- `ai-tools/` directory (Cursor, Claude Code, Windsurf guides)
- `development.mdx`, `quickstart.mdx`
- `essentials/` (all 6 files)
- Versioning dropdown, inline contextual AI chat, developer-oriented `contextual` options in docs.json

### Architecture Approach

The site uses a two-tab structure in `docs.json`: "Guides" (task-oriented, read top-to-bottom) and "CSV Reference" (reference material, scanned repeatedly). This separation matches the two distinct modes merchants arrive in: "help me do the thing" vs. "what does this column mean." The Guides tab has four groups: Getting Started, Importing Rates (4 pages), Exporting Rates (1 page), and Troubleshooting (3 pages). The CSV Reference tab has one group with 4 pages (overview, columns, zones, examples).

**Major components:**
1. `docs.json` — navigation declaration; must be updated atomically with every new page (path mismatch causes silent 404s)
2. `import/` (4 pages: overview, prepare-your-csv, upload-your-file, after-import) — the primary deliverable
3. `csv/` (4 pages: overview, columns, zones, examples) — reference dependency of the import guide
4. `troubleshooting/` (3 pages) — depends on import guide and CSV columns for cross-links
5. `export/` (1 page) — single page; simpler than import, no multi-page flow needed
6. `snippets/` (4 files) — shared fragments: CSV template callout, support CTA, rate table definition, required columns list

**Content dependency critical path:** `snippets/` and `csv/columns.mdx` have no dependencies and must be written first. The import guide flow depends on `csv/columns.mdx`. The troubleshooting guide depends on both the import guide and `csv/columns.mdx`. `index.mdx` is written last (links to all sections).

### Critical Pitfalls

1. **Writing for developers instead of merchants** — technical vocabulary ("parse", "validate", "schema", "payload", "delimiter") is invisible to store owners and makes docs unusable. Prevention: frame every step as a merchant action in the Shopify admin, define any unavoidable technical term inline, read every step aloud. Enforce from the first page written.

2. **Partial starter kit removal** — deploying new MatrixRates pages while `docs.json` still references `api-reference/introduction`, `essentials/settings`, or `ai-tools/cursor` is immediately visible to merchants and destroys trust. Prevention: treat the rebrand as one atomic PR — remove all starter kit entries from `docs.json` and delete all non-MatrixRates files in a single commit.

3. **docs.json path mismatch** — a `.mdx` file at `guides/import.mdx` registered as `"import"` in `docs.json` silently 404s. The build succeeds; the error only appears in the browser. Prevention: register every path in `docs.json` in the same commit as the file creation; run `mint broken-links` before every merge.

4. **CSV column spec ambiguous at edge cases** — documenting the "happy path" without specifying what blank, zero, optional, or boundary values mean causes import failures for legitimate configurations that the docs do not cover. Prevention: for every column, document required vs optional, default when blank, min/max value, what zero means, character restrictions.

5. **Troubleshooting guide lists error strings without translation** — copying error messages without plain-English explanation and concrete fix steps means the page exists but does not reduce support volume. Prevention: every error entry must include the exact error string, a plain-English explanation, a screenshot of the error in context, and a specific "how to fix it" action.

---

## Implications for Roadmap

### Phase 1: Rebrand and Scaffold

**Rationale:** Nothing can be published until Mintlify boilerplate is gone and the site has MatrixRates identity. This phase is a prerequisite for all content phases — publishing new content alongside the starter kit boilerplate is worse than publishing nothing.

**Delivers:** A live, correctly branded Mintlify site with a navigation skeleton registered in `docs.json`, all starter kit content deleted, and `index.mdx` replaced with a MatrixRates home page. No guide content yet — placeholder pages are acceptable.

**Addresses:** Table stakes "branded home page" and "working navigation"; deletion of all anti-feature content (API reference, AI tools, development guide, essentials).

**Avoids:** Pitfall 5 (partial rebrand leaves boilerplate visible); Pitfall 7 (URL structure locked before any content pages are written).

**Blocker:** Brand color, logo SVGs (light + dark), favicon, and support email/URL must be known before this phase can be completed. See Open Questions below.

### Phase 2: Core Task Guides

**Rationale:** The import guide is the primary deliverable and the #1 merchant pain point, but it depends on the CSV column reference being linkable. The export guide is short, low-risk, and the prerequisite "step 0" of the edit-and-reimport workflow. Build order within this phase: export first (no dependencies), then CSV columns (no dependencies), then the import multi-page flow.

**Delivers:** A working, complete import workflow (4 pages) with cross-links to the CSV reference; a single-page export guide framed around backup and template use cases; a CSV reference with annotated examples and a downloadable sample file.

**Addresses:** All table stakes features except troubleshooting; differentiators (downloadable sample CSV, annotated example, prerequisites block, "what to do next" CTAs).

**Avoids:** Pitfall 1 (developer language); Pitfall 3 (ambiguous CSV edge cases); Pitfall 9 (import guide missing failure states — write the cross-link to troubleshooting at the same time as the import guide); Pitfall 12 (export treated as afterthought).

**Blocker:** Actual screenshots of the MatrixRates app UI and the exact CSV column specification (names, types, required/optional, accepted values) must be available before this phase can be written.

### Phase 3: Troubleshooting Guide

**Rationale:** The troubleshooting guide is the most-searched page type on Shopify app help sites, but it depends on the import guide and CSV reference existing to link back to. It also requires real error message strings from the app — content that cannot be invented. Build last so cross-links resolve.

**Delivers:** A troubleshooting guide covering the 5-7 most common import errors with verbatim error strings as headings, plain-English translations, screenshots of errors in context, and specific fix steps linking back to the CSV reference and import guide.

**Addresses:** Table stakes "troubleshooting guide"; differentiator "verbatim error string search hits".

**Avoids:** Pitfall 4 (error strings without translation); Pitfall 1 (technical language).

**Blocker:** Exact error message strings that MatrixRates surfaces to merchants must be sourced from the app or existing support tickets before this phase can be written.

### Phase Ordering Rationale

- Phase 1 before Phase 2: navigation paths must be locked before content is written to prevent URL changes that break cross-links (Pitfall 7).
- Export before import (within Phase 2): export is the prerequisite "step 0" workflow; writing it first means the import guide can reference it as a prior step.
- CSV columns before import guide (within Phase 2): `import/prepare-your-csv.mdx` links directly to `csv/columns.mdx`; the column reference must exist first.
- Import guide before troubleshooting: the troubleshooting guide cross-links back to specific import steps and CSV column definitions.
- `index.mdx` last: the home page's CardGroup links to all four sections; writing it last ensures no cards point to non-existent pages.

### Research Flags

Phases needing deeper research or content sourcing during planning:
- **Phase 2 (CSV Reference):** The exact CSV column specification is not documented anywhere in this research. Must be sourced from the app developer before content can be written. This is the highest-risk content gap.
- **Phase 3 (Troubleshooting):** Real error message strings from the app are unknown. Placeholder troubleshooting entries cannot substitute for verbatim strings because merchants search by copying the exact error text.

Phases with standard patterns (no additional research needed):
- **Phase 1 (Rebrand):** Pure configuration and deletion. Mintlify docs.json schema is fully understood from the live codebase.
- **Phase 2 (Import/Export guides — structure):** Component usage and guide structure are well-established. The only dependency is content inputs (screenshots, column spec), not research.

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | Derived from the live codebase and Mintlify starter kit files; all components confirmed from first-party source files |
| Features | HIGH (table stakes), MEDIUM (differentiators) | Table stakes pattern consistent across 6+ comparable Shopify app doc sites; differentiator impact on support volume is documented pattern but not validated for MatrixRates specifically |
| Architecture | HIGH | Based on direct repo inspection and established Mintlify navigation conventions |
| Pitfalls | HIGH (platform mechanics), MEDIUM (content patterns) | Platform pitfalls sourced from first-party Mintlify docs in the codebase; content pitfalls from established technical writing practice |

**Overall confidence:** HIGH on approach, structure, and tooling. MEDIUM on content specifics (column spec, error messages, screenshots) because those require app access, not research.

### Gaps to Address

These are content inputs that must come from the app or app developer before writing can begin. They are not research gaps — the research approach is clear. They are production blockers.

- **Brand color (`colors.primary` in docs.json):** Unknown. Required for Phase 1. Without it the site uses Mintlify green.
- **Support email or URL:** Unknown. Required for Phase 1 navbar and `snippets/support-cta.mdx`.
- **App store URL:** Unknown. Required for Phase 1 global anchors. Likely `https://apps.shopify.com/matrixrates-import-export` but unverified.
- **Logo SVGs (light + dark variants):** Not present in the repo. Required for Phase 1.
- **Favicon:** Not present in the repo. Required for Phase 1.
- **Exact CSV column specification:** Column names, types, required vs optional, accepted values, and edge case behavior (blank, zero, boundary) are undocumented in this research. Critical input for Phase 2. The existing merchant FAQ site is a starting point but noted as potentially outdated in PROJECT.md.
- **Actual app screenshots:** The import and export guides require current screenshots of the MatrixRates UI. Cannot be sourced without live app access.
- **Actual error message strings:** The troubleshooting guide requires verbatim error strings the app surfaces to merchants. Must come from the app or support ticket history. Required for Phase 3.

---

## Sources

### Primary (HIGH confidence)
- `/Users/lukepaoloni/Shopify/Apps/docs/docs.json` — live navigation schema, confirmed Mintlify config structure
- `/Users/lukepaoloni/Shopify/Apps/docs/` MDX files (Mintlify starter kit) — confirmed component availability and MDX frontmatter patterns
- `/Users/lukepaoloni/Shopify/Apps/docs/.planning/PROJECT.md` — scope, audience, constraints
- `/Users/lukepaoloni/Shopify/Apps/docs/.planning/codebase/ARCHITECTURE.md` — layer analysis

### Secondary (MEDIUM confidence)
- Comparable Shopify app doc sites (Matrixify, DataFeedWatch, Transporter, Klaviyo, Gorgias, Bold Commerce) — feature pattern validation; confirmed table stakes structure
- Established technical writing practice for non-technical audiences — content pitfall patterns

### Tertiary (LOW confidence — needs validation)
- Existing merchant FAQ on the MatrixRates website — potential source for CSV column spec; noted as potentially outdated in PROJECT.md

---
*Research completed: 2026-04-22*
*Ready for roadmap: yes — pending resolution of open questions (brand assets, CSV column spec, error strings, screenshots)*
