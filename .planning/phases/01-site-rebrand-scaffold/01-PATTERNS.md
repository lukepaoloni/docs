# Phase 1: Site Rebrand & Scaffold - Pattern Map

**Mapped:** 2026-04-22
**Files analyzed:** 6 (1 modified config, 1 modified MDX, 4 new MDX stubs)
**Analogs found:** 6 / 6

---

## File Classification

| New/Modified File | Role | Data Flow | Closest Analog | Match Quality |
|-------------------|------|-----------|----------------|---------------|
| `docs.json` | config | transform | `docs.json` (current) | exact — same file, surgical edits |
| `index.mdx` | page (home stub) | request-response | `index.mdx` (current) | exact — same file, content replaced |
| `export-guide.mdx` | page (stub) | request-response | `quickstart.mdx` | role-match — same two-field frontmatter pattern |
| `csv-reference.mdx` | page (stub) | request-response | `quickstart.mdx` | role-match |
| `import-guide.mdx` | page (stub) | request-response | `quickstart.mdx` | role-match |
| `troubleshooting.mdx` | page (stub) | request-response | `quickstart.mdx` | role-match |

---

## Pattern Assignments

### `docs.json` (config, transform)

**Analog:** `docs.json` (current file, lines 1–122)

This is the only file being edited. All changes are field-level. The full current structure is the analog — copy it and apply the diffs below.

**Top-level shell pattern** (lines 1–10):
```json
{
  "$schema": "https://mintlify.com/docs.json",
  "theme": "mint",
  "name": "MatrixRates Import/Export",
  "colors": {
    "primary": "#2dd4bf",
    "light": "#5eead4",
    "dark": "#0d9488"
  },
  "favicon": "/favicon.png",
```

Changes from current:
- `"name"`: `"Mint Starter Kit"` → `"MatrixRates Import/Export"`
- `"colors.primary"`: `"#16A34A"` → `"#2dd4bf"`
- `"colors.light"`: `"#07C983"` → `"#5eead4"` (Tailwind teal-300, lighter for dark mode)
- `"colors.dark"`: `"#15803D"` → `"#0d9488"` (Tailwind teal-600, darker for buttons)
- `"favicon"`: `"/favicon.svg"` → `"/favicon.png"`

**Navigation block — complete replacement** (lines 11–85, current):

Replace the entire `"navigation"` value with:
```json
"navigation": {
  "tabs": [
    {
      "tab": "Guides",
      "groups": [
        {
          "group": "MatrixRates Import/Export",
          "pages": [
            "index",
            "export-guide",
            "csv-reference",
            "import-guide",
            "troubleshooting"
          ]
        }
      ]
    }
  ]
},
```

Key rules enforced:
- No `.mdx` extension in page path strings (e.g., `"export-guide"` not `"export-guide.mdx"`)
- `"index"` maps to root URL `/`
- `navigation.global` block (lines 71–84 in current) is removed entirely — it contains Mintlify's own Documentation and Blog anchor links
- Second tab (`"API reference"`, lines 49–69) is removed entirely

**Logo block — unchanged** (lines 86–89, current):
```json
"logo": {
  "light": "/logo/light.svg",
  "dark": "/logo/dark.svg"
},
```

No path changes needed. User replaces the SVG files in-place per D-05.

**Navbar block — support link only, no primary button** (lines 90–102, current):

Replace the current navbar with:
```json
"navbar": {
  "links": [
    {
      "label": "Support",
      "href": "mailto:support@dojoapps.co.uk"
    }
  ]
},
```

Changes from current:
- `"href"`: `"mailto:hi@mintlify.com"` → `"mailto:support@dojoapps.co.uk"` (D-07)
- `"navbar.primary"` key removed entirely — do NOT set to null (D-08, Pitfall 5)

**Contextual block — developer options removed** (lines 103–114, current):
```json
"contextual": {
  "options": ["copy", "view", "chatgpt", "claude", "perplexity"]
},
```

Removes `"mcp"`, `"cursor"`, `"vscode"` from the current array (D-10).

**Footer block — removed entirely** (lines 115–121, current):

Delete the entire `"footer"` key and its value. Do not replace with `null` or empty object — omit the key entirely (D-09).

**Complete target state for `docs.json`:**
```json
{
  "$schema": "https://mintlify.com/docs.json",
  "theme": "mint",
  "name": "MatrixRates Import/Export",
  "colors": {
    "primary": "#2dd4bf",
    "light": "#5eead4",
    "dark": "#0d9488"
  },
  "favicon": "/favicon.png",
  "navigation": {
    "tabs": [
      {
        "tab": "Guides",
        "groups": [
          {
            "group": "MatrixRates Import/Export",
            "pages": [
              "index",
              "export-guide",
              "csv-reference",
              "import-guide",
              "troubleshooting"
            ]
          }
        ]
      }
    ]
  },
  "logo": {
    "light": "/logo/light.svg",
    "dark": "/logo/dark.svg"
  },
  "navbar": {
    "links": [
      {
        "label": "Support",
        "href": "mailto:support@dojoapps.co.uk"
      }
    ]
  },
  "contextual": {
    "options": ["copy", "view", "chatgpt", "claude", "perplexity"]
  }
}
```

---

### `index.mdx` (page, home stub)

**Analog:** `index.mdx` current (lines 1–4 for frontmatter; full body replaced)

**Frontmatter pattern** (lines 1–4, current `index.mdx`):
```mdx
---
title: "Introduction"
description: "Welcome to the new home for your documentation"
---
```

**Target state — replace entire file with:**
```mdx
---
title: "MatrixRates Import/Export"
description: "Documentation for the MatrixRates Import/Export Shopify app."
---

This guide is coming soon.
```

Changes: title and description updated to MatrixRates content; entire body replaced with a single placeholder line. Full home page content is Phase 6.

---

### `export-guide.mdx` (page, stub — new file)

**Analog:** `quickstart.mdx` lines 1–4 (frontmatter pattern only)

**Frontmatter pattern** (from `quickstart.mdx` lines 1–4):
```mdx
---
title: "Quickstart"
description: "Start building awesome documentation in minutes"
---
```

**Target state — new file:**
```mdx
---
title: "Export Guide"
description: "How to download your shipping rate table as a CSV file."
---

This guide is coming soon.
```

Rules:
- Two-field frontmatter only: `title` and `description` (no `icon` field — that is a starter kit pattern not used for content pages)
- Double-quoted strings in frontmatter
- One placeholder body line to prevent empty-body render

---

### `csv-reference.mdx` (page, stub — new file)

**Analog:** `quickstart.mdx` lines 1–4

**Target state — new file:**
```mdx
---
title: "CSV Reference"
description: "Column-by-column reference for the MatrixRates CSV format."
---

This guide is coming soon.
```

---

### `import-guide.mdx` (page, stub — new file)

**Analog:** `quickstart.mdx` lines 1–4

**Target state — new file:**
```mdx
---
title: "Import Guide"
description: "How to upload a rate table CSV to MatrixRates."
---

This guide is coming soon.
```

---

### `troubleshooting.mdx` (page, stub — new file)

**Analog:** `quickstart.mdx` lines 1–4

**Target state — new file:**
```mdx
---
title: "Troubleshooting"
description: "Fix common import errors in MatrixRates."
---

This guide is coming soon.
```

---

## Shared Patterns

### MDX Frontmatter (all MDX files)
**Source:** `quickstart.mdx` lines 1–4, `index.mdx` lines 1–4
**Apply to:** All 5 MDX files in this phase

The minimum valid frontmatter for a Mintlify page is two fields:
```mdx
---
title: "Page Title Here"
description: "One sentence description here."
---
```

Do NOT add `icon:` to content pages — that field appears in `essentials/settings.mdx` but is starter kit-specific (used in API reference and component reference pages, not in guide pages).

### Navigation Page Path Convention
**Source:** `docs.json` lines 18–21, 27–30, 35–40, 43–47
**Apply to:** All `navigation.tabs[].groups[].pages[]` entries in `docs.json`

Page paths are extension-free file path strings:
```json
"pages": [
  "index",
  "export-guide",
  "csv-reference",
  "import-guide",
  "troubleshooting"
]
```

Never include `.mdx` extension. `"index"` is the special path for the root URL `/`.

### Color Hex Format
**Source:** `docs.json` lines 5–9
**Apply to:** `docs.json` `colors` block

All color values must be 6-character hex strings matching `^#[a-fA-F0-9]{6}$`:
```json
"colors": {
  "primary": "#2dd4bf",
  "light": "#5eead4",
  "dark": "#0d9488"
}
```

---

## Deletion Targets

Files to delete in this phase (21 files total). These must be removed AFTER `docs.json` navigation is updated — never before.

| File/Directory | Reason |
|----------------|--------|
| `favicon.svg` | Replaced by user-supplied `favicon.png` |
| `quickstart.mdx` | Starter kit page |
| `development.mdx` | Starter kit page |
| `essentials/` (6 files) | Starter kit directory |
| `ai-tools/` (3 files) | Starter kit directory |
| `api-reference/` (6 files) | Starter kit directory |
| `images/` (3 files) | Starter kit hero images |

Execution order: (1) update `docs.json`, (2) create stubs, (3) delete starter files — all in one commit.

---

## No Analog Found

No files in this phase lack a codebase analog. All patterns are sourced from the existing `docs.json` and existing MDX files in the repo.

---

## Human-Action Dependencies (Not Executor Tasks)

These items block phase completion but cannot be executed by code:

| Action | Blocks |
|--------|--------|
| User places `favicon.png` in repo root | `docs.json` favicon path resolution; SITE-03 success criterion |
| User replaces `logo/light.svg` with MatrixRates logo | SITE-03 success criterion |
| User replaces `logo/dark.svg` with MatrixRates logo | SITE-03 success criterion |

The executor should update `docs.json` to reference `favicon.png` regardless — the path change is valid even before the file exists. The planner should include explicit human-action tasks for these three items.

---

## Metadata

**Analog search scope:** Repo root MDX files, `essentials/`, `docs.json`
**Files scanned:** `docs.json`, `index.mdx`, `quickstart.mdx`, `essentials/settings.mdx`
**Pattern extraction date:** 2026-04-22
