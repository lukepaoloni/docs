# Phase 1: Site Rebrand & Scaffold - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-04-22
**Phase:** 01-site-rebrand-scaffold
**Areas discussed:** Navigation structure & URLs, Brand asset strategy, Navbar & footer config, Contextual AI chatbot

---

## Navigation structure & URLs

### Tab structure

| Option | Description | Selected |
|--------|-------------|----------|
| Single tab (Recommended) | One 'Guides' tab with sections grouped inside | ✓ |
| Two tabs | Split into 'Guides' and 'Reference' | |
| No tabs, just groups | Sidebar navigation without top tabs | |

**User's choice:** Single tab

---

### URL slugs

| Option | Description | Selected |
|--------|-------------|----------|
| Descriptive slugs (Recommended) | /export-guide, /import-guide, /csv-reference, /troubleshooting | ✓ |
| Short slugs | /export, /import, /csv, /troubleshoot | |
| Section-prefixed slugs | /guides/export, /guides/import, /reference/csv | |

**User's choice:** Descriptive slugs

---

### Page depth

| Option | Description | Selected |
|--------|-------------|----------|
| Each section = one page (Recommended) | One MDX file per section | ✓ |
| Import guide has sub-pages | /import-guide/prepare, /upload, /verify | |
| Troubleshooting has sub-pages | /troubleshooting/csv-errors, /upload-errors | |

**User's choice:** One page per section

---

## Brand asset strategy

### Asset availability

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — I'll provide them | User has logo SVGs, favicon, and hex color ready | ✓ |
| Use a placeholder, swap later | Proceed with stubs | |
| Logo/favicon not ready, but I have the color | Partial assets | |

**User's choice:** Assets available

---

### Brand color

**User's choice:** `#2dd4bf` (entered via Other field, teal/turquoise)

---

### Logo files

| Option | Description | Selected |
|--------|-------------|----------|
| Replace logo/light.svg and logo/dark.svg directly | In-place file replacement | ✓ |
| New logo files at different paths | Different filenames/locations | |
| Text-only name, no logo image | Skip logo SVG | |

**User's choice:** Replace in-place

---

### Favicon

| Option | Description | Selected |
|--------|-------------|----------|
| Replace favicon.svg in the root | Keep .svg format | |
| Use a PNG favicon instead | .png file, update docs.json path | ✓ |
| Leave the placeholder for now | Keep Mintlify leaf temporarily | |

**User's choice:** PNG favicon — docs.json favicon path must be updated

---

## Navbar & footer config

### Navbar primary button

| Option | Description | Selected |
|--------|-------------|----------|
| Remove it entirely (Recommended) | No CTA button | ✓ |
| Install app button | Link to Shopify app store | |
| Keep a Dashboard link | Link to MatrixRates app or Shopify admin | |

**User's choice:** Remove entirely

---

### Footer

| Option | Description | Selected |
|--------|-------------|----------|
| Strip footer entirely (Recommended) | No footer links | ✓ |
| Support email only | support@dojoapps.co.uk in footer | |
| Social links for DojoApps | Add brand social links | |

**User's choice:** Strip footer entirely

---

## Contextual AI chatbot

| Option | Description | Selected |
|--------|-------------|----------|
| Strip all — keep only copy (Recommended) | Remove all AI integrations | |
| Keep Claude, ChatGPT, Perplexity only | Strip developer tools (MCP, Cursor, VSCode) | ✓ |
| Keep all current options | Leave full list as-is | |

**User's choice:** Keep copy, view, claude, chatgpt, perplexity — remove mcp, cursor, vscode

---

## Claude's Discretion

- Exact `colors.light` and `colors.dark` hex variants derived from #2dd4bf
- Placeholder content wording in stub MDX files
- Whether to change the Mintlify theme value from "mint"

## Deferred Ideas

None.
