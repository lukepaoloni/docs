# Feature Landscape: MatrixRates Import/Export Documentation Site

**Domain:** Merchant-facing Shopify app knowledge base (import/export, non-technical audience)
**Researched:** 2026-04-22
**Confidence:** HIGH for table stakes (drawn from Shopify's own help center patterns, Klaviyo, Gorgias, Bold Commerce, and DataFeedWatch doc sites as reference class); MEDIUM for differentiators (pattern-matched from high-performing Shopify app support outcomes)

---

## Table Stakes

Features that non-technical Shopify merchants expect to find. Missing any of these and users bounce to support chat or leave a 1-star review saying "no documentation."

| Feature | Why Expected | Complexity | Dependencies |
|---------|--------------|------------|--------------|
| Branded home/landing page | Merchants need to confirm they're in the right place. "Mint Starter Kit" as the title destroys trust immediately. | Low — update `docs.json` name/colors, replace `index.mdx` | None |
| Step-by-step import guide | #1 pain point. Every CSV import app — DataFeedWatch, Matrixify, Transporter — leads with this. Non-technical users cannot infer steps from a UI. | Medium — requires numbered Steps component, screenshots at each step, exact field names matching the app UI | Home page (links to it), CSV format reference (links from it) |
| CSV format reference page | #3 pain point. Merchants have an existing rate table and don't know what column headers to use or what values are valid. Without this, every CSV upload is a guess. | Medium — column table with type, required/optional, accepted values, and a worked example | Import guide (cross-referenced) |
| Troubleshooting guide | #2 pain point. The most-searched page type on Shopify app help sites after the main how-to. Must cover the 5-7 most common error messages verbatim, because merchants copy/paste errors into search. | High — requires knowledge of real error messages from the app, cause + fix for each | Import guide (linked from it), CSV format reference (linked from it) |
| Export guide | Merchants need to back up rates before editing. Also the prerequisite workflow for "download → edit → re-import." Without this, the import guide assumes knowledge users don't have. | Low — simpler than import; fewer failure modes | Import guide (export is step 0 of the edit-and-reimport workflow) |
| Working navigation | Every page must be reachable from the sidebar. No dead links, no orphaned pages, no broken paths in `docs.json`. | Low — declarative config in `docs.json` | All content pages must exist before registering them |
| Plain-language tone throughout | Shopify's own merchant help center uses eighth-grade reading level. "Weight breakpoints" needs to become "weight ranges." Technical jargon in merchant docs is a documented churn driver. | Zero additional complexity — tone is a writing discipline, not a feature | None |
| Screenshots on every procedural step | Non-technical merchants cannot navigate UI they have never seen without visual reference. Shopify's own docs never describe UI navigation without a screenshot. | Medium — requires capturing app screens, embedding in MDX with `/images/` | None (but a dependency for the import + export guides) |

---

## Differentiators

Features that separate a good doc site from a forgettable one. Not expected, but they measurably reduce support volume and improve merchant confidence. Build these after table stakes are solid.

| Feature | Value Proposition | Complexity | Dependencies |
|---------|-------------------|------------|--------------|
| Downloadable sample CSV | Eliminates the #1 "I don't know how to format my file" failure mode. Merchants download a real, valid file, populate it with their numbers, and upload. Used by every high-performing CSV app doc (Matrixify, DataFeedWatch, Transporter). | Low — create a static `.csv` file in `/public/` or link from a hosted URL; one sentence on the page | CSV format reference page |
| Annotated CSV example inline on the format reference | Show the actual CSV with callout annotations for each column (e.g., "This column is the weight in grams"). More scannable than a data table for non-technical readers. | Low — Mintlify code block with comments or a screenshot of a spreadsheet | CSV format reference page |
| "Before you start" prerequisites block | A short checklist at the top of the import guide (e.g., "You need: your shipping zones already set up, rates in a spreadsheet, admin access"). Prevents merchants from hitting step 6 and realising they lack something from step 0. | Very low — Mintlify `<Note>` or `<Info>` component at the top of the import page | Import guide |
| Error message verbatim search hits | The troubleshooting page titles/headings should contain the exact error strings merchants see in the app (e.g., "Invalid header: zone_name"). This makes Mintlify's built-in search and Google indexing surface the right page when merchants paste an error. | Zero additional complexity — this is a heading/content discipline | Troubleshooting guide |
| "What to do next" CTA at the end of each guide | After completing the import guide, merchants don't know what to do next. A short "Next steps" card linking to Troubleshooting (if something looks wrong) or Export (to verify your upload) keeps users in docs rather than abandoning. | Very low — Mintlify `<Card>` component at the bottom of each guide | All three main guides |
| FAQ accordion on troubleshooting page | Consolidates short "why does X happen?" questions that don't deserve their own page. Accordions keep the page scannable. The existing FAQ on the merchant's website is a ready source of content. | Low — Mintlify `<Accordion>` component; requires sourcing FAQ content from existing site | Troubleshooting guide |
| Light/dark logo variants for MatrixRates branding | Mintlify renders different logos in light vs dark mode. Having a branded logo in both variants signals that this is a polished, maintained product — not a dumped template. | Low — design work for the SVGs; trivial `docs.json` config | None (independent of content) |
| Support link in navbar | Every Shopify app doc site has a direct "Get help" or "Contact support" link in the top nav. Merchants who exhaust the docs need a one-click escape hatch to email or chat support. Prevents frustration and abandonment. | Very low — one `navbar.links` entry in `docs.json` pointing to the support email or Shopify app support URL | None |

---

## Anti-Features

Features to deliberately not build for v1. Each one adds authoring complexity, navigation noise, or cognitive load for a merchant who just wants to import a CSV.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| API reference section | The audience is non-technical store owners. An API reference tab confuses them, signals the wrong product, and the current `api-reference/` content is placeholder data anyway. Zero merchants will look for this. | Delete the tab from `docs.json` navigation entirely. API docs can be a separate site if ever needed. |
| AI tool integration guides (Cursor, Claude Code, Windsurf) | These are boilerplate from the Mintlify starter kit aimed at developers. No merchant importing shipping rates cares about AI coding assistants. | Delete `ai-tools/` directory and remove from navigation. |
| Versioned documentation | Version selectors add UI complexity. MatrixRates Import/Export is a single-version SaaS app — merchants are always on the latest version. | Keep a single canonical doc set. Add a changelog notice if the import format ever changes. |
| "Development" or "local setup" guide | There is no Mintlify-specific developer guide that belongs in the merchant-facing site. The current `development.mdx` is for Mintlify contributors, not merchants. | Delete from navigation. Keep internally if needed for the content authoring workflow. |
| Separate billing / account management section | Shopify handles billing natively. Merchants who have payment questions go to the Shopify App Store, not the app's help site. This is explicitly out of scope in PROJECT.md. | Link out to the Shopify App Store listing if a billing question surfaces. Do not author billing docs. |
| Community forum or user-generated content | Adds moderation overhead and risks outdated community answers conflicting with official docs. Wrong for a small-app v1. | Direct merchants to the support email for community-type questions. |
| Advanced rate configuration deep-dives | Rate logic (conditional rates, multi-zone matrices) is out of scope per PROJECT.md and adds complexity before the core import flow is documented. | Note these as "coming soon" in a single line if needed; do not build until core docs are complete and validated. |
| Inline contextual AI chat (ChatGPT / Claude / Perplexity options) | These are enabled in `docs.json` today but add cognitive clutter for non-technical merchants and can hallucinate answers about a specific app. | Disable the `contextual` AI options in `docs.json` for v1. Re-evaluate once content is stable and tested with real merchants. The `copy` option is fine to keep. |

---

## Feature Dependencies

```
Home page
  └── links to → Import guide
  └── links to → Export guide
  └── links to → CSV format reference
  └── links to → Troubleshooting guide

Export guide
  └── is step 0 of → Import guide (edit-and-reimport workflow)

Import guide
  └── cross-links → CSV format reference (for column definitions mid-flow)
  └── cross-links → Troubleshooting guide (for error recovery)
  └── uses → Screenshots (images/)
  └── optionally links → Sample CSV download

CSV format reference
  └── cross-links → Import guide (for how to use the format in practice)
  └── contains → Annotated example (differentiator)
  └── optionally links → Sample CSV download

Troubleshooting guide
  └── cross-links → Import guide (for retrying after fixing an error)
  └── cross-links → CSV format reference (for format-related errors)
  └── contains → FAQ accordion (differentiator)

Support link in navbar (no content dependencies — `docs.json` config only)
Sample CSV download (no page dependencies — static file asset only)
```

---

## MVP Recommendation

Build in this order. Each phase unblocks the next:

**Phase 1 — Rebrand and scaffold (no merchant value yet, but required before publishing)**
1. Update `docs.json`: name, colors, logo paths, navbar support link, remove API/AI-tools tabs, disable contextual AI options
2. Replace `index.mdx` home page with MatrixRates-branded landing page linking to the three main guides
3. Delete starter kit content (api-reference/, ai-tools/, essentials/, development.mdx, quickstart.mdx)

**Phase 2 — Core import flow (highest merchant value, covers pain point #1)**
4. Export guide — short, low-risk, prerequisite for import edit-and-reimport workflow
5. CSV format reference — required to make the import guide self-contained
6. Import guide — the primary deliverable; depends on export guide and CSV reference being linkable

**Phase 3 — Error recovery (covers pain points #2 and #3, reduces support volume)**
7. Troubleshooting guide — covers the 5-7 most common import errors with verbatim error strings

**Defer to post-v1:**
- Sample CSV download — high value but low urgency once the format reference has an inline annotated example
- FAQ accordion expansion — add as real merchant questions accumulate from support

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Table stakes | HIGH | Pattern is consistent across Shopify's own help center, Matrixify, DataFeedWatch, Bold Commerce, Transporter docs — all use step-by-step guides + CSV reference + troubleshooting as the core structure for import/export features |
| Differentiators | MEDIUM | Based on observed patterns in high-performing app doc sites; impact on support volume is a documented pattern but not validated for MatrixRates specifically |
| Anti-features | HIGH | Starter kit content (API reference, AI tools, development guide) is obviously wrong for the merchant audience; the rest follows directly from PROJECT.md scope constraints |
| Phase ordering | HIGH | Export-before-import ordering follows the logical merchant workflow and is consistent with how Matrixify and DataFeedWatch structure their CSV guides |

---

## Gaps to Address

- **Real error messages unknown:** The troubleshooting guide requires the exact error strings that MatrixRates surfaces to merchants. These need to come from the app itself or from existing support tickets. The guide structure is defined here; the error-specific content is a content authoring dependency, not a documentation architecture question.
- **CSV format not fully specified here:** The columns, required fields, and accepted values for the MatrixRates CSV format are not documented in this research. The existing FAQ on the merchant's website (noted in PROJECT.md as potentially outdated) is the nearest available source and should be reviewed before authoring the CSV format reference page.
- **Screenshots require app access:** The import and export guides depend on current screenshots of the MatrixRates Import/Export app UI. These cannot be sourced from documentation research — they require access to the live app.
