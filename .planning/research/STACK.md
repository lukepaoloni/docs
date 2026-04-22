# Technology Stack

**Project:** MatrixRates Import/Export Docs
**Researched:** 2026-04-22
**Confidence:** HIGH — derived from the live codebase (docs.json, existing MDX files) and Mintlify starter kit source, which reflect the current platform surface directly.

---

## Platform Foundation

| Layer | Technology | Why |
|-------|-----------|-----|
| Documentation platform | Mintlify (hosted, SaaS) | Already wired up with GitHub auto-deploy. Handles rendering, search, hosting, and dark/light theming with zero infrastructure. |
| Content format | MDX (Markdown + JSX) | Mintlify's native format. Lets you embed interactive components inside prose without leaving markdown. |
| Site config | `docs.json` | Single source of truth for nav, colors, navbar, footer, contextual AI — no separate config files. |
| Local preview | `mint` CLI (`npm i -g mint`) | `mint dev` at localhost:3000 with hot reload. `mint broken-links` for link validation. |
| Deployment | GitHub push via Mintlify GitHub App | Auto-deploys on every push to default branch. Already installed in this repo. |
| Node.js | v19+ | Minimum required by the `mint` CLI. No local `package.json` needed — CLI is global. |
| Icons | Font Awesome (via Mintlify) | Used in docs.json `anchors`, `Card` components, and MDX frontmatter `icon` field. |

---

## Mintlify Components — Recommended for This Project

These are confirmed available in the current Mintlify version (verified from the installed starter kit MDX files).

### Tier 1: Use on every applicable page

**`<Steps>` / `<Step>`**
The primary workhorse for merchant-facing guides. Numbered, visually distinct steps prevent merchants from losing their place during a multi-action task like uploading a CSV.

```mdx
<Steps>
  <Step title="Open the MatrixRates app">
    In your Shopify Admin, go to Apps > MatrixRates Import/Export.
  </Step>
  <Step title="Click Import">
    Select **Import Rates** from the main navigation.
  </Step>
</Steps>
```

Use for: Import guide, Export guide, any task with 3+ sequential actions.

---

**`<Warning>`**
High-visibility callout for data-loss or irreversible actions. Use sparingly so merchants don't tune it out.

```mdx
<Warning>
  Importing a new rate table **replaces your existing rates**. Export your current
  rates first if you need a backup.
</Warning>
```

Use for: Import overwrites, CSV format errors that cause silent failures, anything where a mistake has real consequences.

---

**`<Note>`**
Low-urgency informational callout. For clarifications that don't block the task but help understanding.

```mdx
<Note>
  The CSV must use UTF-8 encoding. Files exported from Excel are UTF-8 by default.
</Note>
```

Use for: Format constraints, "FYI" context, things merchants often ask about but don't need to act on.

---

**`<Tip>`**
Positive/helpful shortcut callout. Lighter than Note — for quick wins or best practices.

```mdx
<Tip>
  Download our [CSV template](/csv-reference/template) to start with the correct column headers.
</Tip>
```

Use for: Shortcuts, downloadable templates, things that save time.

---

**`<Frame>`**
Wraps images with consistent styling (rounded corners, contained width). **Required for every screenshot.** Without Frame, raw `<img>` tags render edge-to-edge with inconsistent sizing.

```mdx
<Frame>
  <img src="/images/import-button.png" alt="The Import Rates button in the MatrixRates dashboard" />
</Frame>
```

Use for: Every screenshot. Non-negotiable for a merchant audience that navigates by visual recognition.

---

**`<Card>` / `<CardGroup>`**
Landing-page and section-index navigation. Lets merchants orient themselves with visual tiles instead of a plain list.

```mdx
<CardGroup cols={2}>
  <Card title="Import Rates" icon="file-import" href="/guides/import">
    Upload a CSV to set or update your shipping rate table.
  </Card>
  <Card title="Export Rates" icon="file-export" href="/guides/export">
    Download your current rate table as a CSV.
  </Card>
</CardGroup>
```

Use for: Home page (`index.mdx`), section landing pages. Do not use inside step-by-step guides — it breaks narrative flow.

---

**`<Accordion>` / `<AccordionGroup>`**
Progressive disclosure for edge cases and troubleshooting. Merchants who hit a specific error can expand only that item; happy-path merchants skip the noise.

```mdx
<AccordionGroup>
  <Accordion title="Error: 'Invalid header row'">
    Your CSV's first row does not match the required column names. Check the
    [CSV format reference](/csv-reference) for the exact expected headers.
  </Accordion>
  <Accordion title="Error: 'Missing required field: zone'">
    Every row must include a zone value. Blank zone cells are not accepted.
  </Accordion>
</AccordionGroup>
```

Use for: Troubleshooting guide (each error is one Accordion). Do not use as a substitute for Steps.

---

### Tier 2: Use where relevant

**`<Info>`**
Prerequisites callout at the top of a page. Lighter weight than Warning — communicates "before you start" context.

```mdx
<Info>
  **Before you begin:** You'll need a CSV file with your shipping rates ready to upload.
</Info>
```

Use for: Top of Import guide, Export guide.

---

**`<Tabs>` / `<Tab>`**
Side-by-side content variants without separate pages. Appropriate when the same task has meaningfully different paths (e.g., Excel vs Google Sheets CSV export).

```mdx
<Tabs>
  <Tab title="Export from Excel">
    1. File > Save As
    2. Choose **CSV UTF-8** format.
  </Tab>
  <Tab title="Export from Google Sheets">
    1. File > Download
    2. Choose **Comma-separated values (.csv)**.
  </Tab>
</Tabs>
```

Use for: CSV preparation variants. Do not use for navigation that belongs in separate pages.

---

**Code blocks (fenced markdown)**
For CSV content, column names, and example values — not for showing code. Merchants benefit from the monospace formatting for exact strings they must match.

```mdx
The first row of your CSV must contain these exact headers:

```csv
zone,min_weight,max_weight,min_price,max_price,rate
```
```

Use for: CSV column headers, exact field values, example CSV rows. Adds a copy button automatically — useful for merchants copying template data.

---

**`<ResponseField>` / `<Expandable>`**
Structured field documentation with nested expansion. Ideal for the CSV column reference where each column has type, required/optional, accepted values, and examples.

```mdx
<ResponseField name="zone" type="string" required>
  The shipping zone name. Must match exactly how zones are defined in your
  MatrixRates settings.

  Example: `US_Domestic`
</ResponseField>
```

Use for: CSV format reference page. Provides structured, scannable field documentation that plain prose does not.

---

**Snippets (`/snippets/*.mdx`)**
Reusable MDX fragments imported into multiple pages. Use for content that must stay in sync across guides.

```mdx
// snippets/csv-download-tip.mdx
<Tip>
  Not sure about the format? Download the [CSV template](/csv-reference/template)
  first — it includes all required columns pre-filled.
</Tip>
```

Then import:
```mdx
import CsvDownloadTip from '/snippets/csv-download-tip.mdx';

<CsvDownloadTip />
```

Use for: The CSV template tip (appears in both Import guide and CSV reference), the "contact support" footer callout, any warning that repeats across 2+ pages.

---

### Tier 3: Do not use for this project

| Component | Reason to Avoid |
|-----------|----------------|
| `<CodeGroup>` | For showing code alternatives side-by-side. Audience is non-technical; no code to compare. |
| `<ResponseField>` for API params | Only use in the CSV reference context (field docs), not for API-style parameter docs — we're not documenting an API. |
| API Reference tab / OpenAPI playground | Remove entirely. The sample `api-reference/` folder and its `openapi.json` are Mintlify boilerplate. Out of scope per PROJECT.md. |
| `<Latex>` | No mathematical content in merchant shipping docs. |
| `contextual` AI options (MCP, Cursor, VSCode) | Currently in docs.json. Remove — irrelevant to non-technical merchants and creates noise. Keep `copy`, `chatgpt`, `claude` only if desired. |
| Versions dropdown | No versioning needed for a single-app docs site. |
| `backgroundImage` | Decorative. Adds visual complexity without aiding comprehension for a functional how-to site. |

---

## docs.json Configuration — Recommended Changes

The current `docs.json` is the Mintlify starter kit. These changes are required before content work begins.

| Field | Current Value | Should Be | Reason |
|-------|--------------|-----------|--------|
| `name` | `"Mint Starter Kit"` | `"MatrixRates Help"` | Shown as the site title and in browser tabs |
| `colors.primary` | `#16A34A` (Mintlify green) | MatrixRates brand color | Visual identity |
| `favicon` | Mintlify favicon | MatrixRates favicon | Brand |
| `logo.light` / `logo.dark` | Mintlify logos | MatrixRates logo SVGs | Brand |
| `navbar.links` | Support → mintlify.com | Support → merchant's support email | Merchants need to contact the app developer, not Mintlify |
| `navbar.primary` button | "Dashboard" → dashboard.mintlify.com | App install CTA or remove | Confusing for merchants to see a "Dashboard" button pointing to Mintlify |
| `footer.socials` | Mintlify social accounts | MatrixRates/developer accounts or remove | Defaults to Mintlify's social media |
| `navigation.tabs` | Guides + API Reference | Guides only (or Guides + CSV Reference) | No API docs needed; API tab is boilerplate |
| `navigation.global.anchors` | Mintlify Documentation + Blog | MatrixRates app store page + support link | Anchors should link to the app, not Mintlify |
| `contextual.options` | All 8 options including MCP/Cursor/VSCode | Reduce to `copy`, `view`, `chatgpt`, `claude` | Developer tools are irrelevant for store owners |

---

## Navigation Structure — Recommended

```json
{
  "navigation": {
    "tabs": [
      {
        "tab": "Guides",
        "groups": [
          {
            "group": "Getting Started",
            "pages": ["index"]
          },
          {
            "group": "Import & Export",
            "pages": [
              "guides/import",
              "guides/export"
            ]
          },
          {
            "group": "Troubleshooting",
            "pages": ["guides/troubleshooting"]
          }
        ]
      },
      {
        "tab": "CSV Reference",
        "groups": [
          {
            "group": "Format",
            "pages": [
              "csv-reference/overview",
              "csv-reference/columns",
              "csv-reference/examples"
            ]
          }
        ]
      }
    ],
    "global": {
      "anchors": [
        {
          "anchor": "Get the App",
          "href": "https://apps.shopify.com/matrixrates-import-export",
          "icon": "bag-shopping"
        },
        {
          "anchor": "Support",
          "href": "mailto:support@[developer-domain]",
          "icon": "envelope"
        }
      ]
    }
  }
}
```

**Why this structure:** Guides tab covers task-oriented workflows (import, export, troubleshooting). CSV Reference tab separates the reference material from how-to guides — merchants scan reference while doing; they read guides start-to-finish. The anchors surface the two highest-value actions for a merchant who can't find their answer: get the app or contact support.

---

## File Organization

```
/
├── docs.json                  # Site config
├── index.mdx                  # Home page — CardGroup of guides
├── guides/
│   ├── import.mdx             # Step-by-step import walkthrough
│   ├── export.mdx             # Step-by-step export walkthrough
│   └── troubleshooting.mdx    # Accordion per error type
├── csv-reference/
│   ├── overview.mdx           # What the CSV format is, when to use it
│   ├── columns.mdx            # ResponseField per column
│   └── examples.mdx           # Full example CSVs with annotations
├── snippets/
│   ├── csv-tip.mdx            # Reusable "download the template" tip
│   └── support-callout.mdx    # Reusable support contact CTA
├── images/
│   ├── import/                # Screenshots for import guide
│   ├── export/                # Screenshots for export guide
│   └── logo/                  # MatrixRates brand assets
└── logo/
    ├── dark.svg
    └── light.svg
```

**Why images in subfolders:** Import guide alone will have 5-8 screenshots. Flat `/images/` becomes unmanageable. Subfolder per guide makes it obvious which screenshot belongs where.

---

## Screenshot Standards

Since the audience is non-technical and navigates by visual recognition, screenshot quality is load-bearing for this project.

| Standard | Requirement | Reason |
|----------|------------|--------|
| Format | PNG | Lossless — sharp UI text; smaller than uncompressed formats for web |
| Max file size | 5 MB (Mintlify platform limit) | Above this, must host externally (Cloudinary/S3) |
| Wrapping | Always inside `<Frame>` | Consistent rounded corners and max-width constraints |
| Alt text | Always descriptive | Accessibility; also helps when images fail to load |
| Naming | `action-context.png` e.g. `import-upload-button.png` | Scannable without opening the file |
| Shopify Admin context | Crop to show just the relevant UI section, not the full browser window | Full-window screenshots overwhelm; merchants need to see what to click |

---

## Content Patterns — Merchant-Facing

These are structural patterns, not just components. They define how content is written, not just rendered.

**Pattern: Action-Result framing**
Every step describes the action AND the expected result.
- Bad: "Click Import."
- Good: "Click **Import Rates**. A file picker dialog will open."

Why: Non-technical users need confirmation they did the right thing. Without the expected result, they're unsure if they made a mistake.

**Pattern: Error → Cause → Fix**
Every troubleshooting entry follows: what the error message says → why it happens → exactly what to do.
- Error: "Invalid header row"
- Cause: The first row of your CSV has different column names than expected.
- Fix: Rename the headers to match exactly: `zone`, `min_weight`, `max_weight`...

Why: Merchants come to troubleshooting with a specific error. They need the fix immediately, not an explanation of how CSVs work.

**Pattern: Screenshot before instruction**
For tasks that require finding a UI element, put the screenshot first, then the instruction.
Why: Merchants scan visually. Seeing the button before reading "click Import" confirms they're looking at the right screen.

**Pattern: One task per page**
Import guide = import only. Export guide = export only. Do not combine into a single "Using MatrixRates" page.
Why: Merchants arrive with intent ("I need to import"). A combined page forces them to locate their section. Search also returns the page, not the section.

**Pattern: Warning before irreversible steps**
The `<Warning>` component must appear immediately before any step that overwrites, deletes, or is hard to undo.
Why: Merchants skim steps. If the warning is at the top of the page, they've already forgotten it by step 4.

---

## What NOT to Build

| Anti-Pattern | Why to Avoid |
|-------------|-------------|
| FAQ page as a catch-all | FAQs are unsearchable walls of text. Move each Q&A into the relevant guide (as a Tip or Note) or Troubleshooting accordion. |
| "Overview of all features" page | The audience is task-driven. They want "how do I import" not "here are all the things MatrixRates can do." |
| Developer-style API reference for the CSV format | ResponseField components work, but framing the CSV as "API params" creates the wrong mental model for merchants. Frame as "what goes in each column." |
| Inline HTML for styling | Mintlify components handle all visual layout needs. Raw `<div>` or `<style>` tags create maintenance debt and may not survive Mintlify version updates. |
| Collapsible nav with many nested groups | Non-technical users get disoriented. Keep nav flat: 2 tabs, max 3 groups per tab, max 5 pages per group. |

---

## Sources

| Source | Confidence | Notes |
|--------|-----------|-------|
| Existing codebase MDX files (starter kit) | HIGH | Directly shows which components render in the current deployed Mintlify version |
| `docs.json` in repo | HIGH | Authoritative for current config schema — tabs, navigation, contextual AI options |
| `development.mdx` | HIGH | CLI commands and Node.js version requirement confirmed from Mintlify's own starter content |
| `essentials/reusable-snippets.mdx` | HIGH | Snippets system, import syntax, variable/component patterns confirmed from live docs |
| Mintlify component knowledge (training data, Aug 2025 cutoff) | MEDIUM | Core components (Steps, Warning, Note, Tip, Frame, Accordion, Card, Tabs, ResponseField) stable and unchanged. `contextual` AI options config structure confirmed from docs.json. Web access was denied for live docs verification. |

**Note on web access:** WebFetch and WebSearch permissions were denied during this research session. All component and configuration claims are sourced from the live codebase files (which were written by Mintlify itself as their starter kit) rather than external documentation. Confidence remains HIGH for components confirmed in those files. The one area that would benefit from live verification is whether any new Mintlify components were released between August 2025 and April 2026 — there may be additions not covered here.
