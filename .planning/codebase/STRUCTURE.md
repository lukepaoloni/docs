# Directory Structure

*Mapped: 2026-04-22 | Focus: arch*

## Overview

Mintlify documentation starter kit. All content is flat MDX files; no build system or source code.

---

## Directory Layout

```
/Users/lukepaoloni/Shopify/Apps/docs/
├── docs.json                  # Site config: theme, colors, navigation, logo, navbar, footer
├── favicon.svg                # Browser tab icon
├── index.mdx                  # Home / Introduction page (route: /)
├── quickstart.mdx             # Quickstart guide (route: /quickstart)
├── development.mdx            # Local development guide (route: /development)
├── LICENSE                    # MIT license
├── README.md                  # Repo readme
│
├── essentials/                # Core authoring guides
│   ├── code.mdx               # Code block usage
│   ├── images.mdx             # Image embedding
│   ├── markdown.mdx           # MDX/Markdown syntax reference
│   ├── navigation.mdx         # Navigation configuration guide
│   ├── reusable-snippets.mdx  # Snippet system guide
│   └── settings.mdx           # docs.json settings reference
│
├── ai-tools/                  # AI editor integration guides
│   ├── claude-code.mdx        # Claude Code rules + workflow
│   ├── cursor.mdx             # Cursor rules file + Mintlify component reference
│   └── windsurf.mdx           # Windsurf rules file
│
├── api-reference/             # API documentation
│   ├── introduction.mdx       # API overview page
│   ├── openapi.json           # OpenAPI 3.x spec (placeholder URL)
│   └── endpoint/
│       ├── create.mdx         # POST endpoint example
│       ├── delete.mdx         # DELETE endpoint example
│       ├── get.mdx            # GET endpoint example
│       └── webhook.mdx        # Webhook endpoint example
│
├── images/                    # Static images referenced in pages
│   ├── checks-passed.png
│   ├── hero-dark.png
│   └── hero-light.png
│
├── logo/                      # Site logo (light/dark variants)
│   ├── dark.svg
│   └── light.svg
│
└── snippets/                  # Reusable MDX content fragments
    └── snippet-intro.mdx      # DRY intro copy used across pages
```

---

## Key File Locations

| Purpose | Path |
|---------|------|
| Site configuration | `docs.json` |
| Home page | `index.mdx` |
| Navigation structure | `docs.json` → `navigation.tabs` |
| Logo assets | `logo/dark.svg`, `logo/light.svg` |
| Favicon | `favicon.svg` |
| OpenAPI spec | `api-reference/openapi.json` |
| Reusable snippets | `snippets/*.mdx` |
| AI tool rules | `ai-tools/*.mdx` (documentation only — actual rule files created per-repo) |

---

## Naming Conventions

- **Files:** kebab-case (e.g., `reusable-snippets.mdx`, `claude-code.mdx`)
- **Directories:** kebab-case, lowercase (e.g., `ai-tools/`, `api-reference/`)
- **Extensions:** `.mdx` for all content pages; `.json` for config; `.svg`/`.png` for assets
- **Route paths in `docs.json`:** omit file extension (e.g., `"essentials/navigation"` → `essentials/navigation.mdx`)

---

## Adding New Content

| What | Where | Notes |
|------|-------|-------|
| New guide page | `essentials/` or top-level | Add path to `docs.json` navigation |
| New API endpoint | `api-reference/endpoint/` | Also update `openapi.json` |
| New tab section | New directory | Register in `docs.json → navigation.tabs` |
| Reusable copy | `snippets/` | Import in pages with `<Snippet file="..." />` |
| Images | `images/` | Reference as `/images/filename.ext` |

---

*Last updated: 2026-04-22*
