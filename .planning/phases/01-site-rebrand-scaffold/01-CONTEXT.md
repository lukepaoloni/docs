# Phase 1: Site Rebrand & Scaffold - Context

**Gathered:** 2026-04-22
**Status:** Ready for planning

<domain>
## Phase Boundary

Strip all Mintlify starter kit content and configuration, apply MatrixRates branding, and lock the complete URL navigation structure in `docs.json`. No content pages are written in this phase — only the structural scaffold and visual identity. Every future page's URL path is registered before any content phase begins.

</domain>

<decisions>
## Implementation Decisions

### Navigation Structure
- **D-01:** Single "Guides" tab — no multi-tab structure. All sections live under one tab.
- **D-02:** Five pages total. Each section is exactly one MDX file (no sub-pages):
  - Home: `index.mdx` at `/` (root)
  - Export Guide: `export-guide.mdx` at `/export-guide`
  - CSV Reference: `csv-reference.mdx` at `/csv-reference`
  - Import Guide: `import-guide.mdx` at `/import-guide`
  - Troubleshooting: `troubleshooting.mdx` at `/troubleshooting`
- **D-03:** URL slugs are descriptive and kebab-case. These paths are LOCKED — all future phases must use these exact paths for cross-links.

### Brand Assets
- **D-04:** Primary brand color: `#2dd4bf` (teal). Use this for both `colors.primary` in docs.json. Derive `light` and `dark` variants as Mintlify requires (typically lightened/darkened from primary).
- **D-05:** Logo files: user will replace `logo/light.svg` and `logo/dark.svg` in-place. The docs.json logo paths (`/logo/light.svg`, `/logo/dark.svg`) stay unchanged — no path updates needed.
- **D-06:** Favicon: user will provide a PNG file. Update `docs.json` favicon path from `/favicon.svg` to `/favicon.png` (or the actual filename the user provides). The old `favicon.svg` should be removed.

### Navbar & Footer
- **D-07:** Support link in navbar: `support@dojoapps.co.uk` — replace the existing `hi@mintlify.com` mailto link. Keep the label "Support".
- **D-08:** No primary button in the navbar — remove the "Dashboard" button entirely.
- **D-09:** Footer: stripped entirely. Remove all footer social links (x, github, linkedin) and the `footer.socials` block from docs.json.

### Contextual AI Options
- **D-10:** Keep only `copy`, `view`, `chatgpt`, `claude`, `perplexity` in `contextual.options`. Remove `mcp`, `cursor`, `vscode` — these are developer tools irrelevant to a non-technical merchant audience.

### Content Cleanup
- **D-11:** Delete all starter kit directories and files: `essentials/`, `ai-tools/`, `api-reference/`, `quickstart.mdx`, `development.mdx`, `images/` (Mintlify hero images).
- **D-12:** `index.mdx` becomes the home page placeholder — replace with minimal placeholder content (e.g., "Coming soon" or a simple heading) that won't 404. Full home page content is Phase 6.
- **D-13:** Create placeholder MDX stubs for all four content pages so no registered URL 404s: `export-guide.mdx`, `csv-reference.mdx`, `import-guide.mdx`, `troubleshooting.mdx`.
- **D-14:** Site name in docs.json: `MatrixRates Import/Export` (replace "Mint Starter Kit").

### Claude's Discretion
- Exact Mintlify `colors.light` and `colors.dark` values derived from #2dd4bf — Claude may choose appropriate lightened/darkened hex variants that look good in the Mintlify theme.
- Placeholder content wording in stub MDX files — minimal, non-embarrassing text that can go live without blocking the rebrand.
- Whether to update the `theme` value (currently "mint") to a different Mintlify theme if a better fit for the teal palette exists.

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Site Configuration
- `docs.json` — Current Mintlify config at repo root. All changes to branding, navigation, colors, navbar, footer, and contextual options are made here.

### Requirements
- `.planning/REQUIREMENTS.md` — SITE-01 through SITE-04 define exactly what this phase must deliver.
- `.planning/ROADMAP.md` Phase 1 — Success criteria (5 criteria) that must all be true for phase completion.

No external ADRs or specs — requirements fully captured in decisions above.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `logo/light.svg`, `logo/dark.svg` — Already referenced at correct paths in docs.json. User replaces files in-place; no config path changes needed for logo.
- `docs.json` — The single source of truth for all site config. Surgeon-level edits only; don't rewrite the whole file.

### Established Patterns
- Mintlify `navigation.tabs` array structure in docs.json — the planner must produce a valid tabs array matching Mintlify schema. One tab object with `tab` + `groups` array.
- Mintlify `contextual.options` is a string array — valid values are: `"copy"`, `"view"`, `"chatgpt"`, `"claude"`, `"perplexity"`, `"mcp"`, `"cursor"`, `"vscode"`.

### Integration Points
- `docs.json` `navigation.tabs[0].groups` — all future pages (Phases 2–6) register their paths here in Phase 1. Every phase that creates a new MDX file cross-links using paths established here.
- `docs.json` `navbar.links` — support email goes here.
- Favicon path in `docs.json` must match the actual file name provided by user.

</code_context>

<specifics>
## Specific Ideas

- Brand color confirmed by user: `#2dd4bf` (teal/turquoise)
- Favicon will be a PNG, not SVG — the docs.json path must be updated to match
- User confirmed they have brand assets ready — no placeholder approach needed
- Logo files are swapped in-place by the user — planner does not need to generate or modify SVG content

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 01-site-rebrand-scaffold*
*Context gathered: 2026-04-22*
