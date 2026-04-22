# Phase 1: Site Rebrand & Scaffold — Research

**Researched:** 2026-04-22
**Domain:** Mintlify docs.json configuration, MDX scaffolding, brand color application
**Confidence:** HIGH

---

## Summary

Phase 1 is a pure configuration and scaffolding phase: no content is written, only the Mintlify `docs.json` is edited and starter kit files are deleted. The primary work surface is a single JSON file plus five minimal MDX stubs. All user decisions are locked in `01-CONTEXT.md`; this research confirms the exact schema syntax and catches the operational gotchas.

The current `docs.json` is valid and well-structured. Every required change maps to a named field in the Mintlify schema with no ambiguity. The starter kit file inventory has been verified by filesystem scan. The five MDX stubs require only a two-line frontmatter block — no body content is needed to avoid a 404. The biggest operational risk is deleting files that are still referenced in `docs.json` before the navigation block is updated; the two edits must be atomic (nav update + file delete in one commit).

**Primary recommendation:** Edit `docs.json` first (all fields in one pass), then delete starter kit files, then create the four placeholder MDX stubs. Verify with `mint dev` locally before pushing to main.

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Navigation Structure**
- D-01: Single "Guides" tab — no multi-tab structure. All sections live under one tab.
- D-02: Five pages total. Each section is exactly one MDX file (no sub-pages):
  - Home: `index.mdx` at `/` (root)
  - Export Guide: `export-guide.mdx` at `/export-guide`
  - CSV Reference: `csv-reference.mdx` at `/csv-reference`
  - Import Guide: `import-guide.mdx` at `/import-guide`
  - Troubleshooting: `troubleshooting.mdx` at `/troubleshooting`
- D-03: URL slugs are descriptive and kebab-case. These paths are LOCKED — all future phases must use these exact paths for cross-links.

**Brand Assets**
- D-04: Primary brand color: `#2dd4bf` (teal). Use for `colors.primary` in docs.json. Derive `light` and `dark` variants as Mintlify requires.
- D-05: Logo files: user replaces `logo/light.svg` and `logo/dark.svg` in-place. The docs.json logo paths stay unchanged — no path updates needed.
- D-06: Favicon: user provides a PNG file. Update docs.json favicon path from `/favicon.svg` to `/favicon.png`. Remove old `favicon.svg`.

**Navbar & Footer**
- D-07: Support link in navbar: `support@dojoapps.co.uk` — replace `hi@mintlify.com` mailto link. Keep label "Support".
- D-08: No primary button in the navbar — remove the "Dashboard" button entirely.
- D-09: Footer: stripped entirely. Remove all `footer` block from docs.json.

**Contextual AI Options**
- D-10: Keep only `copy`, `view`, `chatgpt`, `claude`, `perplexity` in `contextual.options`. Remove `mcp`, `cursor`, `vscode`.

**Content Cleanup**
- D-11: Delete all starter kit directories and files: `essentials/`, `ai-tools/`, `api-reference/`, `quickstart.mdx`, `development.mdx`, `images/`.
- D-12: `index.mdx` becomes home page placeholder — replace with minimal placeholder content.
- D-13: Create placeholder MDX stubs: `export-guide.mdx`, `csv-reference.mdx`, `import-guide.mdx`, `troubleshooting.mdx`.
- D-14: Site name in docs.json: `MatrixRates Import/Export`.

### Claude's Discretion

- Exact `colors.light` and `colors.dark` values derived from `#2dd4bf` — choose appropriate lightened/darkened hex variants.
- Placeholder content wording in stub MDX files — minimal, non-embarrassing text that can go live.
- Whether to update the `theme` value (currently "mint") to a different Mintlify theme if a better fit for the teal palette exists.

### Deferred Ideas (OUT OF SCOPE)

None — discussion stayed within phase scope.

</user_constraints>

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| SITE-01 | Merchant sees MatrixRates-branded site — `docs.json` updated with correct name, support link, removed API reference tab, placeholder brand color | Confirmed: all fields map directly to verified docs.json schema fields |
| SITE-02 | All Mintlify starter kit content removed — `essentials/`, `ai-tools/`, `api-reference/` directories and boilerplate MDX pages deleted atomically | Confirmed: full file inventory verified by filesystem scan |
| SITE-03 | Site displays MatrixRates logo and favicon instead of Mintlify defaults | Confirmed: logo paths stay unchanged per D-05; favicon path change to PNG per D-06 |
| SITE-04 | URL structure (all page paths) defined in `docs.json` before any content pages are written | Confirmed: navigation paths without extension resolve to kebab-case URLs; index maps to `/` |

</phase_requirements>

---

## Architectural Responsibility Map

| Capability | Primary Tier | Secondary Tier | Rationale |
|------------|-------------|----------------|-----------|
| Site branding (colors, name, logo) | Static config (`docs.json`) | — | All visual identity lives in `docs.json`; no runtime layer |
| Navigation structure / URL registration | Static config (`docs.json`) | MDX file paths | Navigation entries in `docs.json` determine URLs; MDX files must exist at those paths |
| Favicon display | Static asset + `docs.json` | — | `docs.json` `favicon` field points to file in repo root |
| Navbar links | Static config (`docs.json`) | — | `navbar.links` array in `docs.json` |
| Footer | Static config (`docs.json`) | — | `footer` block removed entirely from `docs.json` |
| Placeholder page content | MDX files | — | Each registered path needs a corresponding `.mdx` file to avoid 404 |

---

## Standard Stack

### Core

| Library / Tool | Version | Purpose | Why Standard |
|----------------|---------|---------|--------------|
| Mintlify platform | Current (cloud) | Documentation hosting, rendering | Locked project choice |
| MDX | 2.x (Mintlify-managed) | Content format for pages | Mintlify's native content format |
| docs.json | `$schema: mintlify.com/docs.json` | Single source of truth for all config | Mintlify's config file as of Feb 2025 |

**No npm packages to install for this phase.** This phase is purely file edits and deletions.

### Dev tooling (local preview only)

| Tool | Version | Purpose | When to Use |
|------|---------|---------|-------------|
| `mint` CLI | Latest | Local preview (`mint dev`) | Verifying changes at `localhost:3000` before push |
| Node.js | v19+ | Required by `mint` CLI | Only if running local preview |

**Note:** `mint` CLI is NOT required to execute the phase — the GitHub auto-deploy path works without it. Local preview is a validation option, not a build requirement. [VERIFIED: CLAUDE.md]

---

## Architecture Patterns

### System Architecture Diagram

```
User browser
    |
    v
Mintlify Cloud (GitHub App auto-deploy)
    |
    +--- docs.json ──── site config (name, colors, nav, favicon, navbar, footer)
    |
    +--- index.mdx ──── URL: /
    +--- export-guide.mdx ──── URL: /export-guide
    +--- csv-reference.mdx ──── URL: /csv-reference
    +--- import-guide.mdx ──── URL: /import-guide
    +--- troubleshooting.mdx ──── URL: /troubleshooting
    |
    +--- logo/light.svg ──── served as-is (user replaces)
    +--- logo/dark.svg ──── served as-is (user replaces)
    +--- favicon.png ──── served as-is (user provides)
```

### Recommended Project Structure After Phase 1

```
/                         # repo root
├── docs.json             # site config (the primary edit target)
├── favicon.png           # user-provided PNG (replaces favicon.svg)
├── index.mdx             # home page placeholder → URL: /
├── export-guide.mdx      # export guide stub → URL: /export-guide
├── csv-reference.mdx     # CSV reference stub → URL: /csv-reference
├── import-guide.mdx      # import guide stub → URL: /import-guide
├── troubleshooting.mdx   # troubleshooting stub → URL: /troubleshooting
├── logo/
│   ├── light.svg         # user replaces in-place
│   └── dark.svg          # user replaces in-place
└── snippets/
    └── snippet-intro.mdx # untouched (Mintlify starter snippet — harmless)
```

Files deleted in this phase: `favicon.svg`, `quickstart.mdx`, `development.mdx`, `essentials/` (6 files), `ai-tools/` (3 files), `api-reference/` (6 files), `images/` (3 files).

**Note on `snippets/`:** The `snippets/snippet-intro.mdx` file is not registered in navigation and poses no risk. It can be left in place for Phase 1 to reduce scope. [VERIFIED: filesystem scan]

**Note on `navigation.global.anchors`:** The current `docs.json` has a `navigation.global.anchors` block with Mintlify's own Documentation and Blog links. These should be removed as they link to Mintlify's own site, not MatrixRates content. The `global` key can be removed entirely or replaced with an empty object. [ASSUMED — no explicit decision in CONTEXT.md; planner should include this cleanup]

---

## docs.json Schema — Verified Field Reference

All fields below are verified against Mintlify's official docs schema. [CITED: mintlify.com/docs/organize/settings-reference]

### colors block

```json
"colors": {
  "primary": "#hex",   // Required. Used for emphasis/accents in light mode.
  "light": "#hex",     // Optional. Used for emphasis/accents in DARK mode.
  "dark": "#hex"       // Optional. Used for buttons and hover states across both modes.
}
```

**Role clarification (verified):**
- `colors.primary` — accent color in light mode (highlighted text, section headers, active nav items)
- `colors.light` — accent color when user is in dark mode (same role as primary, but for dark backgrounds)
- `colors.dark` — button color across both light and dark modes

**Hex constraint:** Must match `^#([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$` — six-character hex only. [CITED: mintlify.com/docs/organize/settings-reference]

### Recommended color values for `#2dd4bf` (teal) primary

| Field | Value | Derivation | Confidence |
|-------|-------|-----------|------------|
| `colors.primary` | `#2dd4bf` | Locked decision D-04 | HIGH |
| `colors.light` | `#5eead4` | +30% lighter — readable on dark backgrounds; corresponds to Tailwind teal-300 | MEDIUM |
| `colors.dark` | `#0d9488` | -15% darker — strong contrast for buttons; corresponds to Tailwind teal-600 | MEDIUM |

These values follow the same lightened/darkened pattern used in official Mintlify examples (e.g., `primary: #0D9373`, `light: #55D799`, `dark: #0D9373`). [CITED: github.com/mintlify/docs settings-appearance.mdx]

### theme field

Valid values: `mint`, `maple`, `palm`, `willow`, `linden`, `almond`, `aspen`, `sequoia`, `luma` [CITED: mintlify.com/docs/organize/settings-reference]

**Recommendation for this project:** Keep `"mint"` (Claude's Discretion). Rationale:
- "Maple" is documented as "modern, clean aesthetics perfect for AI and SaaS products" and is the only theme explicitly designed for SaaS. It would be a reasonable upgrade.
- "Mint" is the classic theme and is battle-tested. The starter kit uses it. No visual regression risk.
- For a non-technical merchant audience, both themes are appropriate. Maple is slightly more modern but the difference is cosmetic.
- **Recommendation:** Keep `"mint"` to minimize risk in Phase 1. Theme can be changed later as a single-field edit with no other consequences.

### favicon field

```json
"favicon": "/favicon.png"
```

Supports string path or object with `light`/`dark` variants. String form is correct for single favicon. Mintlify auto-resizes the image. PNG is fully supported — no SVG requirement. [CITED: mintlify.com/docs/organize/settings-reference]

### navbar block

```json
"navbar": {
  "links": [
    {
      "label": "Support",
      "href": "mailto:support@dojoapps.co.uk"
    }
  ]
  // "primary" key omitted entirely — removes the CTA button
}
```

To remove the primary button (D-08): omit the `"primary"` key entirely. Setting it to `null` is not documented as valid. [ASSUMED — confirmed by schema reference showing it as optional, but null-vs-omit behavior not explicitly tested]

### footer removal

To strip the footer entirely (D-09): remove the `"footer"` key from `docs.json`. The footer block is fully optional. [CITED: mintlify.com/docs/organize/settings-reference — footer is Optional]

### navigation structure (tabs + groups)

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
}
```

**Key facts:**
- Page paths use NO `.mdx` extension in navigation entries [CITED: hackmamba.io Mintlify migration guide — "drop the .mdx extension"]
- `"index"` maps to the root URL `/` [CITED: multiple sources confirm index.mdx is root page]
- `"export-guide"` maps to URL `/export-guide` — kebab-case path becomes kebab-case URL
- `navigation.global.anchors` block should be removed (currently points to Mintlify's own docs/blog)

### contextual block

```json
"contextual": {
  "options": ["copy", "view", "chatgpt", "claude", "perplexity"]
}
```

Valid option strings (verified full list): `"assistant"`, `"copy"`, `"view"`, `"chatgpt"`, `"claude"`, `"perplexity"`, `"grok"`, `"aistudio"`, `"devin"`, `"windsurf"`, `"mcp"`, `"cursor"`, `"vscode"` [CITED: mintlify.com/docs/organize/settings-reference]

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Color derivation for light/dark variants | Custom color calculation | Use Tailwind teal palette values as reference | Mintlify renders these — visual verification is the only test |
| Navigation URL routing | Custom redirect rules | Mintlify's native path-to-URL resolution | Automatic: file path = URL slug, no config needed |
| Favicon format conversion | Custom image processing | Provide PNG directly | Mintlify auto-resizes |
| Build/deploy pipeline | Custom CI/CD | Mintlify GitHub App auto-deploy | Push to main = deploy; no pipeline needed |

---

## File Inventory — What Exists Now vs What Changes

### Current state (verified by filesystem scan)

```
KEEP (modified):
  docs.json                    ← primary edit target
  index.mdx                    ← replace content with stub
  logo/light.svg               ← user replaces in-place (path stays)
  logo/dark.svg                ← user replaces in-place (path stays)
  snippets/snippet-intro.mdx   ← harmless, not in nav — leave as-is
  LICENSE                      ← leave as-is
  README.md                    ← leave as-is

ADD:
  favicon.png                  ← user provides this file
  export-guide.mdx             ← new stub
  csv-reference.mdx            ← new stub
  import-guide.mdx             ← new stub
  troubleshooting.mdx          ← new stub

DELETE:
  favicon.svg                  ← replaced by favicon.png
  quickstart.mdx               ← starter kit page
  development.mdx              ← starter kit page
  essentials/code.mdx          ← starter kit
  essentials/images.mdx        ← starter kit
  essentials/markdown.mdx      ← starter kit
  essentials/navigation.mdx    ← starter kit
  essentials/reusable-snippets.mdx ← starter kit
  essentials/settings.mdx      ← starter kit
  ai-tools/claude-code.mdx     ← starter kit
  ai-tools/cursor.mdx          ← starter kit
  ai-tools/windsurf.mdx        ← starter kit
  api-reference/introduction.mdx      ← starter kit
  api-reference/openapi.json          ← starter kit
  api-reference/endpoint/create.mdx  ← starter kit
  api-reference/endpoint/delete.mdx  ← starter kit
  api-reference/endpoint/get.mdx     ← starter kit
  api-reference/endpoint/webhook.mdx ← starter kit
  images/checks-passed.png     ← starter kit hero image
  images/hero-dark.png         ← starter kit hero image
  images/hero-light.png        ← starter kit hero image
```

Total files to delete: 21 files across 5 directories/paths.

---

## MDX Stub Template

Minimal valid MDX page. Body content is optional — Mintlify generates a title from the path if omitted, but explicit `title` is best practice. [CITED: mintlify.com/docs/organize/pages]

```mdx
---
title: "Export Guide"
description: "How to export your MatrixRates shipping rate table as a CSV."
---

This page is coming soon.
```

**Variation per page:**

| File | title | description |
|------|-------|-------------|
| `index.mdx` | `"MatrixRates Import/Export"` | `"Documentation for the MatrixRates Import/Export Shopify app."` |
| `export-guide.mdx` | `"Export Guide"` | `"How to download your shipping rate table as a CSV file."` |
| `csv-reference.mdx` | `"CSV Reference"` | `"Column-by-column reference for the MatrixRates CSV format."` |
| `import-guide.mdx` | `"Import Guide"` | `"How to upload a rate table CSV to MatrixRates."` |
| `troubleshooting.mdx` | `"Troubleshooting"` | `"Fix common import errors in MatrixRates."` |

Body content: a single line like `"This guide is coming soon."` is sufficient. It prevents an empty-body render and is non-embarrassing if the page goes live. [ASSUMED — minimal content behavior not explicitly documented, but pattern confirmed across Mintlify examples]

---

## Common Pitfalls

### Pitfall 1: Deleting files before updating docs.json navigation
**What goes wrong:** Mintlify shows broken navigation entries or throws build warnings for registered pages that have no corresponding MDX file.
**Why it happens:** `docs.json` still references deleted paths.
**How to avoid:** Always update `docs.json` navigation FIRST, removing all deleted page paths, then delete the files. Do both in the same commit.
**Warning signs:** `mint broken-links` reports errors; live site shows broken sidebar entries.

### Pitfall 2: Including .mdx extension in navigation page paths
**What goes wrong:** Mintlify fails to resolve the page or creates malformed URLs.
**Why it happens:** Navigation entries must be file paths without extension: `"export-guide"` not `"export-guide.mdx"`.
**How to avoid:** All entries in `navigation.tabs[].groups[].pages[]` are extension-free paths.
**Warning signs:** Pages 404 even though the MDX file exists.

### Pitfall 3: Forgetting to remove `navigation.global.anchors`
**What goes wrong:** The live site still shows "Documentation" and "Blog" links pointing to Mintlify's own site in the sidebar anchors section.
**Why it happens:** The current `docs.json` has a `navigation.global.anchors` block with Mintlify's own URLs. It's not removed by just replacing `navigation.tabs`.
**How to avoid:** Remove the entire `navigation.global` key (or set `anchors` to an empty array `[]`).
**Warning signs:** Sidebar shows "Documentation" and "Blog" anchor links pointing to mintlify.com.

### Pitfall 4: Hex color format validation
**What goes wrong:** docs.json fails schema validation if color value is not a 6-character hex.
**Why it happens:** Mintlify schema requires `^#([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$` — no shorthand beyond 3-char, no rgba, no named colors.
**How to avoid:** Always use 6-character hex codes starting with `#`. Confirmed: `#2dd4bf` is valid.
**Warning signs:** Mintlify build/deploy errors mentioning color validation.

### Pitfall 5: Leaving navbar.primary key with null value
**What goes wrong:** Unpredictable behavior — schema expects either a valid object or the key to be absent.
**Why it happens:** Setting `"primary": null` instead of removing the key entirely.
**How to avoid:** Completely remove the `"primary"` key from the `"navbar"` object (D-08).
**Warning signs:** Navbar shows unexpected button or layout glitch.

### Pitfall 6: favicon.svg left in repo with updated docs.json pointing to favicon.png
**What goes wrong:** If `favicon.svg` is still present but `docs.json` now references `/favicon.png`, the old SVG is an orphan but causes no error. The real risk is if `favicon.png` is NOT yet placed in the repo — the favicon will be missing in the browser tab.
**Why it happens:** User forgets to place the PNG before pushing.
**How to avoid:** Either (a) add a favicon placeholder step that the planner must block on user action, or (b) create a task that notes "favicon.png must be supplied by user before this task completes."
**Warning signs:** Browser tab shows no icon or the generic Mintlify leaf icon.

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| `mint.json` config file | `docs.json` config file | February 2025 | All config is now in `docs.json`; `mint.json` is deprecated |
| Single `navigation` array | `navigation.tabs` / `navigation.groups` / `navigation.anchors` nested structure | 2024-2025 | More flexible hierarchy; current repo already uses new format |

**Note:** The current `docs.json` already uses the new `docs.json` schema (not the old `mint.json` format). No migration needed. [VERIFIED: `docs.json` has `"$schema": "https://mintlify.com/docs.json"`]

---

## Assumptions Log

| # | Claim | Section | Risk if Wrong |
|---|-------|---------|---------------|
| A1 | Omitting `navbar.primary` key entirely removes the button (vs setting to null) | docs.json Schema | Low — worst case the button shows unexpectedly; trivial to fix |
| A2 | Placeholder stub with title frontmatter + one line of body text will render without error | MDX Stub Template | Low — Mintlify auto-generates title from path if frontmatter missing; stubs are fail-safe |
| A3 | `snippets/snippet-intro.mdx` can be left in place without causing build errors (not in nav) | File Inventory | Low — it is not referenced in navigation and Mintlify ignores unregistered files |
| A4 | `navigation.global` key can be removed entirely without breaking the navigation structure | Common Pitfalls | Low — `global` is optional per schema; removing it returns clean sidebar |
| A5 | Color values `#5eead4` (light) and `#0d9488` (dark) will render acceptably for the teal brand | Color derivation | Medium — visual rendering only verifiable in browser; alternatives are Tailwind teal-300/600 |

---

## Open Questions

1. **Favicon file dependency**
   - What we know: docs.json will reference `/favicon.png`. The user confirmed they will supply a PNG.
   - What's unclear: When exactly the user will provide the file — Phase 1 cannot be fully complete until `favicon.png` is in the repo root.
   - Recommendation: The plan should include a task that blocks on "user places favicon.png in repo root" — this is an explicit human action step, not something the executor can do.

2. **Logo file dependency**
   - What we know: `logo/light.svg` and `logo/dark.svg` paths stay unchanged (D-05). User replaces files in-place.
   - What's unclear: Same timing question as favicon.
   - Recommendation: Include a human-action task: "User replaces logo/light.svg and logo/dark.svg with MatrixRates brand files."

3. **`navigation.global.anchors` cleanup scope**
   - What we know: Current docs.json has Mintlify's own Documentation and Blog links in `navigation.global.anchors`.
   - What's unclear: No explicit decision in CONTEXT.md on whether to remove these vs replace with MatrixRates-specific anchors.
   - Recommendation: Remove the entire `navigation.global` block. These Mintlify self-referential links are clearly starter kit content.

---

## Environment Availability

| Dependency | Required By | Available | Version | Fallback |
|------------|------------|-----------|---------|----------|
| Git | All file changes | ✓ | System git | — |
| `mint` CLI (local preview) | Validation step | Unknown — not checked | — | Push to main and verify live site |
| Node.js v19+ | `mint dev` | Unknown | — | Use GitHub auto-deploy path |

**Missing dependencies with fallback:**
- `mint` CLI: If not installed, the executor can push to main and verify the live Mintlify deploy instead of local preview. Phase success criteria can be validated on the live URL.

---

## Validation Architecture

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Manual verification + `mint broken-links` CLI |
| Config file | None — no automated test suite for static doc sites |
| Quick run command | `mint broken-links` (if mint CLI available) |
| Full suite command | Manual checklist against Phase 1 success criteria (see below) |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| SITE-01 | docs.json has correct name, support link, no API reference tab, brand color | Manual inspect | `cat docs.json \| grep -E '"name"\|"primary"\|support'` | ✅ docs.json exists |
| SITE-02 | Starter kit dirs/files deleted — none reachable | Manual inspect | `ls essentials/ ai-tools/ api-reference/ 2>&1` | N/A — checking absence |
| SITE-03 | Logo and favicon paths correct in docs.json and files exist at paths | Manual inspect | `ls logo/ favicon.png` | ✅ logo/ exists; favicon.png Wave 0 |
| SITE-04 | All 5 page paths registered in docs.json navigation, MDX files exist | Manual inspect | `grep -E "export-guide\|csv-reference\|import-guide\|troubleshooting" docs.json` | ✅ docs.json exists |

### Success Criteria Checklist (from ROADMAP.md)

After Phase 1 execution, ALL five must be true:

1. Live docs URL shows `MatrixRates Import/Export` name and `#2dd4bf` teal — no Mintlify green, no "Mint Starter Kit" text
2. MatrixRates logo and favicon appear in browser tab and navbar — not the Mintlify leaf icon
3. All starter kit directories deleted; no boilerplate pages reachable via any URL
4. `docs.json` declares all 5 page paths before any content phase — every future cross-link target registered
5. Support email `support@dojoapps.co.uk` in navbar; no API Reference tab in site navigation

### Sampling Rate

- **Per task commit:** `grep` commands above to confirm specific changes
- **Phase gate:** Full 5-point success criteria checklist against live or local Mintlify site before closing Phase 1

### Wave 0 Gaps

- [ ] `favicon.png` — must be placed in repo root by user before SITE-03 can pass (human action, not code)
- [ ] `logo/light.svg`, `logo/dark.svg` — must be replaced by user with MatrixRates brand files (human action)

*(If favicon.png is not yet available, SITE-03 remains partially blocked. The docs.json path update and old favicon.svg deletion CAN proceed; the favicon.png placement is a separate human-action task.)*

---

## Security Domain

This phase involves only static documentation files (MDX content, JSON config). There is no authentication, no user data, no API endpoints, and no secrets. ASVS categories V2–V6 do not apply. The only security-adjacent consideration is:

- No secrets or credentials should be committed (support email is public by design; no API keys or tokens are involved)
- `docs.json` is a public file — confirm it contains no private URLs or credentials before pushing

---

## Sources

### Primary (HIGH confidence)

- [Mintlify docs.json settings reference](https://mintlify.com/docs/organize/settings-reference) — colors schema, theme values, navbar, footer, contextual options, favicon
- [Mintlify Context7 `/mintlify/docs`](https://context7.com/mintlify/docs) — navigation structure, MDX frontmatter, tabs/groups schema
- [Mintlify themes docs](https://mintlify.com/docs/customize/themes) — full theme list with descriptions
- [Mintlify blog: next chapter of themes](https://mintlify.com/blog/the-next-chapter-of-mintlify-themes) — theme use case descriptions

### Secondary (MEDIUM confidence)

- [Hackmamba Mintlify migration guide](https://hackmamba.io/technical-documentation/mintlify-documentation-migration-guide/) — page path URL mapping, "drop .mdx extension" rule
- [Mintlify pages docs](https://mintlify.com/docs/organize/pages) — frontmatter minimums, title auto-generation
- [Mintlify starter kit settings page](https://starter.mintlify.com/essentials/settings) — color role descriptions (primary/light/dark)

### Tertiary (LOW confidence — training knowledge only)

- Tailwind CSS teal palette used as reference for `#5eead4` (teal-300) and `#0d9488` (teal-600) color derivation

---

## Metadata

**Confidence breakdown:**
- docs.json schema: HIGH — verified against official settings-reference page and Context7
- Navigation path resolution: HIGH — confirmed by multiple sources including official examples
- Color derivation: MEDIUM — Tailwind palette values are reasonable but visual rendering must be verified
- MDX stub minimum: MEDIUM — derived from official frontmatter docs; empty body is likely fine but not explicitly tested
- File inventory: HIGH — verified by direct filesystem scan

**Research date:** 2026-04-22
**Valid until:** 2026-05-22 (Mintlify schema is stable; themes/features evolve monthly)
