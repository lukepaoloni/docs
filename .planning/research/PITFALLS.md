# Domain Pitfalls

**Domain:** Shopify app documentation — non-technical merchant audience, import/export guides, CSV reference, Mintlify/MDX platform
**Researched:** 2026-04-22
**Confidence:** HIGH (platform mechanics from direct codebase inspection + Mintlify source files; content patterns from established technical writing practice)

---

## Critical Pitfalls

Mistakes that cause merchant failure or require significant rework.

---

### Pitfall 1: Writing for Developers Instead of Merchants

**What goes wrong:** Instructions use technical vocabulary — "parse the CSV", "validate the schema", "the payload must conform to" — that means nothing to a store owner managing shipping rates. The merchant reads the page, understands nothing, and contacts support instead.

**Why it happens:** The person writing docs (developer or AI) defaults to their own mental model of the domain. CSV and import/export workflows are inherently technical subjects. Without conscious effort, technical framing bleeds in.

**Consequences:**
- Merchant cannot self-serve — the core value proposition of the docs fails
- Support ticket volume stays high
- Merchants distrust the docs after one failed attempt and stop reading

**Prevention:**
- Write every step as a task the merchant performs in the Shopify admin: "Click the Import button", "Open the downloaded file in Excel or Google Sheets", "Save the file as CSV"
- Replace: "field delimiter", "UTF-8 encoding", "header row" with "the first row", "column names", "save as .csv"
- When you must use a technical term (e.g. "CSV"), define it once inline: "a CSV file — a plain spreadsheet you can open in Excel or Google Sheets"
- Read every step aloud. If it sounds like code documentation, rewrite it.

**Warning signs:**
- Any sentence containing "parse", "validate", "schema", "payload", "encode", "delimiter", "null", "boolean", "integer"
- Steps that describe what the system does rather than what the merchant does
- No screenshots on a multi-step process page

**Phase:** Site rebrand + all content phases (import guide, CSV reference, troubleshooting). Enforce as a writing rule from the first page written.

---

### Pitfall 2: Navigation Registered in docs.json Does Not Match File Path

**What goes wrong:** A new `.mdx` file is created (e.g. `guides/import.mdx`) but `docs.json` references the wrong path (`"import"` instead of `"guides/import"`), or vice versa — the path in `docs.json` is correct but the file is at a different location. The page either 404s or silently disappears from the nav.

**Why it happens:** Mintlify does not auto-discover pages. Every page must be manually registered in `docs.json` under `navigation.tabs[].groups[].pages[]`. The path must be root-relative and omit the `.mdx` extension. One typo in a folder name breaks the link without a loud error.

**Consequences:**
- Page exists on disk but cannot be found by merchants
- Internal links from other pages return 404
- Build deploys successfully (no blocking error) — the broken page is only discovered when browsing live

**Prevention:**
- After creating any `.mdx` file, update `docs.json` in the same commit
- Use `mint broken-links` CLI command before every merge to catch 404s
- Follow the established naming convention: kebab-case filenames, kebab-case directories, omit `.mdx` in `docs.json`
- Planned folder structure for this project: `guides/` and `reference/` — register these in `docs.json` before writing content

**Warning signs:**
- A new `.mdx` file committed without a simultaneous `docs.json` change
- Page exists in the sidebar but clicking it returns an error
- Search returns a page that the nav does not show

**Phase:** Site rebrand (when all starter kit content is removed and MatrixRates nav is set up). Lock the folder structure and `docs.json` skeleton before writing any content pages.

---

### Pitfall 3: CSV Column Specification Is Ambiguous at the Edge Cases

**What goes wrong:** The CSV reference documents the "happy path" column values but omits what is valid when a value is optional, blank, zero, or at a boundary. A merchant with an unusual rate table — zones with no weight upper bound, a rate of $0.00 for free shipping, or a carrier name with a comma in it — hits an undocumented case and their import fails with a cryptic error.

**Why it happens:** The author knows the CSV format intimately and documents what they expect merchants to send, not what merchants will actually try. Edge cases are invisible to the person who designed the format.

**Consequences:**
- Import failures for legitimate rate table configurations
- Merchants cannot self-diagnose because the reference does not cover their case
- Support escalations for questions the docs should answer

**Prevention:**
- For every column in the CSV reference, document: required vs optional, default when blank, minimum/maximum value, what happens on zero, what happens on empty string, character restrictions (commas, quotes, special characters)
- Include at least one "real" sample CSV file that merchants can download and edit — not a fragment, a complete working file
- Document the edge cases explicitly: "If this column is empty, the rate applies to all weights", "A value of 0 means free shipping for this zone"
- Cross-reference the troubleshooting guide from the CSV reference for every common invalid-value error

**Warning signs:**
- Column spec says "weight range" without defining what upper-bound blank means
- No sample file provided — only a table of column names
- No section addressing what happens with optional columns when omitted

**Phase:** CSV format reference page (core content phase). This is the highest-risk page in the project.

---

### Pitfall 4: Troubleshooting Guide Uses App Error Messages Verbatim Without Explaining Them

**What goes wrong:** The troubleshooting guide lists errors like "Row 14: Invalid rate_type value" or "Missing required header: zone_id" without translating what they mean in plain English and without showing the merchant what to look for in their spreadsheet. The merchant knows something is wrong but still does not know how to fix it.

**Why it happens:** The author copies error strings from the app and treats them as self-explanatory. They are not — they were written for developers, not merchants.

**Consequences:**
- Troubleshooting page does not reduce support volume (the primary reason it exists)
- Merchants feel the docs are unhelpful even though content is present
- The page creates false confidence in the docs without delivering value

**Prevention:**
- For every error entry: show the exact error message the app displays, then explain it in plain language: "This means one of your rows has a spelling mistake in the Rate Type column. Common values are 'flat' and 'per_kg' — check that column in your spreadsheet."
- Include a screenshot of the error as it appears in the app UI, not just the text
- Provide a concrete "How to fix it" action, not just a cause
- Map each error to the specific CSV column or step that caused it

**Warning signs:**
- Troubleshooting entries are just lists of error strings with one-line descriptions
- No screenshots of errors in context
- "Fix" instructions say "check your CSV" without specifying where to look

**Phase:** Troubleshooting guide page. Also affects the import guide (Step X of import should mention where errors appear).

---

### Pitfall 5: Starter Kit Content Left in Place During Partial Rebrand

**What goes wrong:** The MatrixRates rebrand replaces `index.mdx` and adds new guide pages, but old starter kit pages (`quickstart.mdx`, `development.mdx`, `essentials/`, `ai-tools/`, `api-reference/`) remain in the repo and are still registered in `docs.json`. Merchants visiting the site find Mintlify boilerplate about "OpenAPI specs" and "Font Awesome icons" in the navigation.

**Why it happens:** Pages are added incrementally. It is easy to add new content without removing old content. The `docs.json` still references all original pages from the starter.

**Consequences:**
- Destroys merchant trust immediately — the site looks unfinished or broken
- Confuses search results (Mintlify search indexes all registered pages)
- Undermines the brand if the live URL shows Mintlify's own documentation

**Prevention:**
- Treat the rebrand as a single atomic change: remove all starter kit entries from `docs.json` and delete or archive all non-MatrixRates content in the same PR
- Do not deploy a partial state where old tabs coexist with new tabs
- The `api-reference/` folder is particularly dangerous — it has a live OpenAPI playground pointing to a placeholder URL (`https://api.example.com`)

**Warning signs:**
- `docs.json` still contains `"api-reference/introduction"`, `"essentials/settings"`, or `"ai-tools/cursor"` after rebrand
- `name` field in `docs.json` still reads `"Mint Starter Kit"`
- Footer socials still link to `https://x.com/mintlify`

**Phase:** Site rebrand (first content phase). Must be completed entirely before any guide content is published.

---

## Moderate Pitfalls

---

### Pitfall 6: Screenshots Go Stale When the App UI Changes

**What goes wrong:** Screenshots taken at launch show UI elements (button labels, modal layouts, column names in the import screen) that the app subsequently changes. Merchants following the guide see a different screen than the one pictured and lose confidence in the instructions.

**Why it happens:** Screenshots feel final once written. There is no automated check that images still match the current app. Documentation maintenance is rarely prioritized alongside app development.

**Prevention:**
- Store screenshots with a consistent naming convention that includes context: `import-step-1-upload-button.png`, not `screenshot1.png`. This makes replacement obvious.
- Keep all screenshots in the `images/` directory with logical subdirectories: `images/import/`, `images/export/`, `images/errors/`
- When the app ships a UI change, add a doc update to the same release checklist — not as an afterthought
- Prefer screenshots that show small, focused interactions (one button, one modal) over full-page screenshots. Smaller scope = less fragile.

**Warning signs:**
- Screenshots showing full Shopify admin page layouts (high drift risk)
- No naming convention — files are `image1.png`, `screenshot-final.png`
- No process for flagging when app UI changes require doc updates

**Phase:** All content phases (import guide, export guide, troubleshooting). Establish screenshot naming convention in the rebrand phase before any images are added.

---

### Pitfall 7: Internal Links Break After Renaming or Moving Pages

**What goes wrong:** A guide page links to `/guides/csv-format` but that page is later renamed to `/reference/csv-columns`. The original link returns 404. Mintlify does not redirect automatically — you must set up redirects manually in `docs.json` or update every reference.

**Why it happens:** Page names feel provisional early and get "finalized" later. By then, cross-links exist throughout the content.

**Prevention:**
- Lock page file names and paths before writing any content that links to them. Treat paths as permanent once published.
- Run `mint broken-links` as part of every PR review
- When renaming is unavoidable, add a `redirects` entry in `docs.json`:
  ```json
  "redirects": [
    { "source": "/guides/csv-format", "destination": "/reference/csv-columns" }
  ]
  ```

**Warning signs:**
- File renamed without a `docs.json` redirects entry
- Cross-links written before target pages have stable names
- `mint broken-links` not part of the dev workflow

**Phase:** Site rebrand (plan the URL structure before content is written). Retroactive fixes are expensive.

---

### Pitfall 8: MDX Component Misuse Causes Silent Rendering Failures

**What goes wrong:** Mintlify's built-in components (`<Steps>`, `<Card>`, `<Warning>`, `<Accordion>`) fail silently or render incorrectly when improperly nested, missing required props, or when MDX syntax is written inside an arrow-function snippet. The page may appear blank, show raw JSX text, or lose formatting.

**Specific known issues from codebase:**
- `snippets/*.mdx` files must not use plain MDX syntax inside an arrow-function component body — "MDX does not compile inside the body of an arrow function" (documented in `essentials/reusable-snippets.mdx`)
- Using `api` as a top-level folder name is reserved by Next.js (Mintlify's underlying framework) — any folder named exactly `api` at the root will break routing
- Markdown-style relative links (`../text`) work but load slower and bypass Mintlify's optimizations — use root-relative paths (`/guides/import`)

**Prevention:**
- Use `<Frame>` around all images — do not embed images with bare markdown `![]()` syntax when you need consistent styling
- Always use root-relative paths for internal links: `/guides/import`, not `../guides/import`
- Never name a top-level directory `api` — use `api-reference` (already the project convention)
- Test component-heavy pages in `mint dev` before committing — silent failures are only visible in the browser

**Warning signs:**
- Page appears blank or shows raw component tags on `mint dev`
- Snippet file uses MDX syntax inside an arrow function body
- Internal links use relative (`../`) paths

**Phase:** All content phases. Establish `mint dev` as the required local check before any commit.

---

### Pitfall 9: Import Guide Missing the "What Could Go Wrong" Moment

**What goes wrong:** The import guide walks through the happy path (upload file, confirm, done) but does not tell merchants what happens if the import fails — where to find the error, what it looks like, and where to go next. When an import fails, the merchant is lost because the guide ends at "your rates are live."

**Why it happens:** Happy path thinking. The guide is written as a success walkthrough, and failure states are left to the troubleshooting guide. But merchants in a failure state are reading the import guide, not searching for a separate troubleshooting page.

**Prevention:**
- Add a "If something went wrong" section at the end of the import guide — a short paragraph that says "If you see an error message, [link to troubleshooting guide] has fixes for every common error."
- Within the import steps, at the "upload" step specifically, note: "If the upload fails immediately, check that your file is saved as .csv, not .xlsx or .xls."
- Cross-link the import guide and troubleshooting guide bidirectionally

**Warning signs:**
- Import guide has no mention of errors or failure states
- No link from import guide to troubleshooting guide

**Phase:** Import guide content phase. Write the cross-link at the same time as the import guide, not after.

---

## Minor Pitfalls

---

### Pitfall 10: Frontmatter title and description Left Generic

**What goes wrong:** Pages use generic frontmatter like `title: "Import Guide"` and `description: "How to import"`. Mintlify uses these for browser tab titles, search results, and the page header. Generic titles produce poor search results and confuse merchants who have multiple tabs open.

**Prevention:**
- `title` should be specific and action-oriented: `"Import Your Shipping Rate Table"` not `"Import Guide"`
- `description` should complete the sentence "This page helps you...": `"Upload a CSV file to set or replace your shipping rates in MatrixRates."`

**Warning signs:**
- `title` is one or two generic words
- `description` matches or nearly matches the `title`

**Phase:** All content phases. Easy to fix but worth setting right from the start.

---

### Pitfall 11: No Sample CSV File for Merchants to Download

**What goes wrong:** The CSV reference documents every column in a table, but merchants have to construct a valid CSV from scratch. Most merchants will copy the format incorrectly — wrong column order, wrong header names, stray spaces in headers — and get import errors they cannot diagnose.

**Prevention:**
- Provide at least one downloadable sample CSV file with realistic rate data (not placeholder values)
- Store it in a predictable location: `public/` or link directly in the CSV reference page
- The sample file should be small enough to understand (10-15 rows) but complete enough to cover the main cases: multiple zones, weight breakpoints, a free shipping threshold

**Warning signs:**
- CSV reference page has column table but no sample file or download link
- "Example" in the reference shows a 2-row fragment that merchants cannot use directly

**Phase:** CSV format reference phase. The sample file is as important as the column documentation.

---

### Pitfall 12: Export Guide Treated as an Afterthought

**What goes wrong:** Export is simple (click download, file arrives) but merchants use exports for two distinct purposes: backup before editing, and auditing current rates. Docs that only explain the mechanism miss the use case — merchants don't know they should export before importing, or that the export is the safest way to get a template.

**Prevention:**
- Frame the export guide around the merchant's goal, not the UI action: "Export Your Current Rates (Backup or Template)"
- Prominently recommend: "Export your current rates before importing new ones — this gives you a backup you can restore if something goes wrong."
- Show how the export file is the best starting template for a new import

**Warning signs:**
- Export guide is a single paragraph with one screenshot
- No mention of export as a backup step before importing

**Phase:** Export guide content phase.

---

## Phase-Specific Warnings

| Phase Topic | Likely Pitfall | Mitigation |
|-------------|---------------|------------|
| Site rebrand | Partial removal of starter kit leaves Mintlify boilerplate live | Remove all starter content and `docs.json` entries atomically in one PR |
| Site rebrand | `docs.json` path structure set early determines all future URLs | Plan URL structure before any content pages are written |
| Import guide | Happy-path-only guide leaves merchants stranded on failure | Add "if something went wrong" section with link to troubleshooting |
| Import guide | Technical language creeps into step-by-step instructions | Read every step as a merchant action, not a system description |
| CSV reference | Column specs ambiguous at edge cases (blank, zero, optional) | Document every column's behavior for blank/zero/omitted |
| CSV reference | No downloadable sample file | Provide a complete working sample CSV, not just a table |
| Troubleshooting | Error messages listed without plain-English translation | Every error entry: exact message + plain explanation + screenshot + fix |
| Export guide | Treated as trivial; misses backup and template use cases | Frame around merchant goal, not UI action |
| All pages | Screenshots taken once and never updated | Establish naming convention and app-update → doc-update process |
| All pages | Internal links break after path changes | Lock file paths before writing content; run `mint broken-links` on every PR |

---

## Sources

- Direct inspection of `/Users/lukepaoloni/Shopify/Apps/docs/` codebase (HIGH confidence)
- Mintlify platform mechanics: `essentials/navigation.mdx`, `essentials/reusable-snippets.mdx`, `essentials/images.mdx` (HIGH confidence — first-party Mintlify documentation)
- Mintlify CLI capabilities: `docs.json` schema, `development.mdx` (HIGH confidence)
- Codebase concerns: `.planning/codebase/CONCERNS.md` (HIGH confidence — already mapped)
- Content pitfall patterns: established technical writing practice for non-technical audiences (MEDIUM confidence — well-established domain knowledge)
