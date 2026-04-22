# Architecture Patterns

**Project:** MatrixRates Import/Export Docs
**Domain:** Focused Shopify app user guide (import/export + CSV reference + troubleshooting)
**Researched:** 2026-04-22
**Confidence:** HIGH — based on direct repo inspection, existing docs.json structure, and established Mintlify conventions

---

## Recommended Architecture

The site has three structural layers that must work together: the **navigation declaration** in `docs.json`, the **file system layout** of `.mdx` content, and the **reusable snippets** in `snippets/`. These are not independent — they form a tightly-coupled contract. A page that exists on disk but is not registered in `docs.json` is unreachable. A path registered in `docs.json` with no corresponding `.mdx` file breaks the build.

### Proposed Information Architecture

```
MatrixRates Docs
│
├── Tab: "Guides"                          (docs.json: navigation.tabs[0])
│   │
│   ├── Group: "Getting Started"
│   │   └── index.mdx                     (welcome page + navigation hub)
│   │
│   ├── Group: "Importing Rates"
│   │   ├── import/overview.mdx           (what importing does, when to use it)
│   │   ├── import/prepare-your-csv.mdx   (before you upload: format requirements)
│   │   ├── import/upload-your-file.mdx   (step-by-step upload walkthrough)
│   │   └── import/after-import.mdx       (verify, activate, what happens next)
│   │
│   ├── Group: "Exporting Rates"
│   │   └── export/overview.mdx           (download current table, use cases)
│   │
│   └── Group: "Troubleshooting"
│       ├── troubleshooting/overview.mdx  (how to read error messages)
│       ├── troubleshooting/csv-errors.mdx (column/format errors with fixes)
│       └── troubleshooting/upload-errors.mdx (connectivity, size, app errors)
│
└── Tab: "CSV Reference"                  (docs.json: navigation.tabs[1])
    │
    └── Group: "CSV Format"
        ├── csv/overview.mdx              (what the CSV is, download template link)
        ├── csv/columns.mdx              (every column: name, type, required, values)
        ├── csv/zones.mdx                (zone column rules, accepted values)
        └── csv/examples.mdx            (2-3 complete worked examples)
```

Two tabs are the right call for this content volume. A single tab with six groups would be overwhelming for non-technical merchants. The "CSV Reference" tab separates reference material (scan to look something up) from guide material (read top-to-bottom to accomplish a task) — this is the standard docs.rs / Stripe pattern and it works at this scale.

---

## Component Boundaries

Each content group is a self-contained unit with a clear responsibility. Components should not bleed across these boundaries.

| Component | Responsibility | Entry Point | Communicates With |
|-----------|----------------|-------------|-------------------|
| Welcome / Home (`index.mdx`) | Orient the merchant. Tell them what the app does and send them to the right guide. | `/` (root) | Links to import/overview, export/overview, troubleshooting/overview |
| Import Guide (`import/`) | Task-oriented walkthrough. Merchant follows steps to complete a successful import. | `import/overview.mdx` | Links to CSV Reference for format details; links to Troubleshooting for errors |
| Export Guide (`export/`) | Task-oriented walkthrough. Merchant completes a download. | `export/overview.mdx` | Links to import/ if they want to re-import after editing |
| CSV Reference (`csv/`) | Reference material. Merchant scans to look up a specific column or rule. | `csv/overview.mdx` | Linked from import/prepare-your-csv.mdx; linked from troubleshooting/csv-errors.mdx |
| Troubleshooting (`troubleshooting/`) | Problem-resolution. Merchant arrives with a specific error or symptom. | `troubleshooting/overview.mdx` | Links back to import/ for corrective actions; links to csv/ for format corrections |
| Snippets (`snippets/`) | Shared content fragments. Avoid duplicating warnings, definitions, or repeated UI steps. | Imported via MDX `import` | Used by import/, csv/, troubleshooting/ |

---

## Content Dependencies

These dependencies determine build order. Writing content out of order produces either orphaned pages (no links in) or broken links (links out to pages that don't exist yet).

```
snippets/ (no dependencies — write first)
    └── required by: import/, csv/, troubleshooting/

csv/columns.mdx (no dependencies — pure reference)
    └── required by: import/prepare-your-csv.mdx
                     troubleshooting/csv-errors.mdx

import/overview.mdx (no dependencies)
    └── required by: index.mdx

import/prepare-your-csv.mdx
    └── requires: csv/columns.mdx (links to column definitions)

import/upload-your-file.mdx
    └── requires: import/prepare-your-csv.mdx (step flow)

import/after-import.mdx
    └── requires: import/upload-your-file.mdx (step flow)

export/overview.mdx (no dependencies)

troubleshooting/csv-errors.mdx
    └── requires: csv/columns.mdx (links to specific column definitions)

troubleshooting/upload-errors.mdx
    └── requires: import/upload-your-file.mdx (links to correct upload steps)

index.mdx (written last)
    └── requires: all sections exist (cards link to each section entry point)
```

The critical path is: snippets → CSV columns reference → import guide flow → troubleshooting → index.

---

## Suggested Build Order

Build in this order to ensure every page you write can link correctly to pages that already exist.

**Phase 1 — Foundation (no dependencies)**
1. `docs.json` navigation structure (register all paths before writing any content)
2. `snippets/` — any shared fragments (CSV format note, error message callout pattern, "contact support" CTA)
3. `csv/columns.mdx` — the column reference is the single most-referenced page in the site

**Phase 2 — Core Task Guides (depend on Phase 1)**
4. `import/overview.mdx`
5. `import/prepare-your-csv.mdx` (links to csv/columns.mdx)
6. `import/upload-your-file.mdx`
7. `import/after-import.mdx`
8. `export/overview.mdx`

**Phase 3 — Supporting Reference (depends on Phase 1 + 2)**
9. `csv/overview.mdx`
10. `csv/zones.mdx`
11. `csv/examples.mdx`

**Phase 4 — Troubleshooting (depends on Phase 1 + 2)**
12. `troubleshooting/overview.mdx`
13. `troubleshooting/csv-errors.mdx` (links to csv/columns.mdx)
14. `troubleshooting/upload-errors.mdx` (links to import/upload-your-file.mdx)

**Phase 5 — Home and Branding (depends on everything)**
15. `index.mdx` (cards link to all sections)
16. `docs.json` branding update (name, colors, logo, navbar links)

---

## How Snippets Should Be Organized

Snippets prevent content drift. Define a snippet once, import everywhere. These are the candidates for the MatrixRates site:

| Snippet File | Content | Used By |
|---|---|---|
| `snippets/csv-template-callout.mdx` | "Download the CSV template" callout with link | import/prepare-your-csv.mdx, csv/overview.mdx |
| `snippets/support-cta.mdx` | "Still stuck? Contact support at [email]" block | troubleshooting/csv-errors.mdx, troubleshooting/upload-errors.mdx |
| `snippets/rate-table-definition.mdx` | One-paragraph definition of what a rate table is | import/overview.mdx, export/overview.mdx, index.mdx |
| `snippets/csv-required-columns.mdx` | Inline list of required columns (referenced in both import guide and CSV reference) | import/prepare-your-csv.mdx, csv/columns.mdx |

Naming convention: `snippets/[noun]-[purpose].mdx`. All lowercase, hyphen-separated. Snippets that are short enough to import inline should use named exports; snippets that are full-paragraph blocks should use default export.

---

## File System Layout (Target State)

This is the layout after all starter kit content is replaced:

```
/
├── docs.json                           (site config: nav, branding, anchors)
├── index.mdx                           (welcome / hub page)
├── favicon.svg                         (replace with MatrixRates favicon)
│
├── logo/
│   ├── light.svg                       (replace with MatrixRates logo)
│   └── dark.svg                        (replace with MatrixRates logo)
│
├── images/
│   └── [screenshots added per phase]   (import UI, upload dialog, error screens)
│
├── import/
│   ├── overview.mdx
│   ├── prepare-your-csv.mdx
│   ├── upload-your-file.mdx
│   └── after-import.mdx
│
├── export/
│   └── overview.mdx
│
├── csv/
│   ├── overview.mdx
│   ├── columns.mdx
│   ├── zones.mdx
│   └── examples.mdx
│
├── troubleshooting/
│   ├── overview.mdx
│   ├── csv-errors.mdx
│   └── upload-errors.mdx
│
└── snippets/
    ├── csv-template-callout.mdx
    ├── support-cta.mdx
    ├── rate-table-definition.mdx
    └── csv-required-columns.mdx
```

Files to delete entirely (starter kit content with no reuse value):
- `essentials/` (all 6 files)
- `ai-tools/` (all 3 files)
- `api-reference/` (entire directory — no API docs in scope)
- `development.mdx`
- `quickstart.mdx`
- `images/hero-dark.png`, `images/hero-light.png`, `images/checks-passed.png`

---

## docs.json Navigation Schema (Target State)

The `docs.json` navigation block should be replaced in full. The registered paths must exactly match the file system layout above.

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
            "group": "Importing Rates",
            "pages": [
              "import/overview",
              "import/prepare-your-csv",
              "import/upload-your-file",
              "import/after-import"
            ]
          },
          {
            "group": "Exporting Rates",
            "pages": ["export/overview"]
          },
          {
            "group": "Troubleshooting",
            "pages": [
              "troubleshooting/overview",
              "troubleshooting/csv-errors",
              "troubleshooting/upload-errors"
            ]
          }
        ]
      },
      {
        "tab": "CSV Reference",
        "groups": [
          {
            "group": "CSV Format",
            "pages": [
              "csv/overview",
              "csv/columns",
              "csv/zones",
              "csv/examples"
            ]
          }
        ]
      }
    ],
    "global": {
      "anchors": [
        {
          "anchor": "Shopify App Store",
          "href": "https://apps.shopify.com/matrixrates",
          "icon": "bag-shopping"
        },
        {
          "anchor": "Support",
          "href": "mailto:[support-email]",
          "icon": "envelope"
        }
      ]
    }
  }
}
```

Note: The `contextual` block (AI assistant integrations) in the existing `docs.json` can be kept as-is — Mintlify handles it and it is beneficial for non-technical merchants.

---

## Key Architecture Decisions

**Single-section export guide (one page, not a multi-page flow)**
Export is a simpler task than import — it has no format requirements or error surface. A single `export/overview.mdx` is sufficient. Adding an export/ multi-page group to match the import/ structure would create false symmetry and pad the nav without value.

**Two tabs, not one**
With 12-14 pages, a single tab would require a long sidebar scroll. Separating "Guides" (task-oriented, read once) from "CSV Reference" (reference, scanned repeatedly) matches the two different mental modes merchants arrive in: "help me do the thing" vs "what does this column mean."

**No version tabs or versioned paths**
MatrixRates is a Shopify app — it auto-updates. There is no concept of a docs version that a merchant pins to. Version tabs would add complexity with zero merchant benefit.

**Troubleshooting is a tab-group, not a separate tab**
With only 2-3 troubleshooting pages, a dedicated tab is visual overhead. It lives as a group inside the Guides tab. If the troubleshooting content grows to 6+ pages, it should be promoted to its own tab.

**Images live flat in `images/`**
Mintlify resolves images from the repo root. There is no benefit to sub-directories at this content volume. Naming convention: `images/[section]-[description].png` (e.g., `images/import-upload-dialog.png`).

---

## Scalability Considerations

| Concern | At current scope (14 pages) | If scope grows |
|---|---|---|
| Navigation depth | Two tabs, flat groups — fine | Add groups within tabs before adding tabs |
| Snippet sprawl | 4 snippets — manageable by inspection | Consider a snippets index comment at the top of each snippet |
| Image management | Flat `images/` — fine | Sub-directories by section when count exceeds ~20 images |
| Troubleshooting volume | 3 pages | Promote to its own tab when error count warrants 6+ pages |
| Export guide | 1 page | Expand to multi-page flow only if export has multi-step workflows (bulk export, scheduled export, etc.) |

---

## Sources

- Direct inspection of `/Users/lukepaoloni/Shopify/Apps/docs/docs.json` (existing navigation schema, confirmed Mintlify conventions)
- Direct inspection of existing `.mdx` files (confirmed MDX frontmatter pattern, Mintlify component usage)
- `/Users/lukepaoloni/Shopify/Apps/docs/.planning/PROJECT.md` (scope, audience, constraints)
- `/Users/lukepaoloni/Shopify/Apps/docs/.planning/codebase/ARCHITECTURE.md` (layer analysis, data flow, abstractions)
- Mintlify navigation model inferred from live `docs.json` schema (`$schema: https://mintlify.com/docs.json`)
