---
phase: "01"
plan: "01-02"
phase_name: "Site Rebrand & Scaffold"
subsystem: docs.json, MDX stubs
tags: [configuration, branding, mintlify]
dependency_graph:
  requires:
    - "01-01"
  provides:
    - SITE-01
    - SITE-02
    - SITE-04
  affects:
    - docs.json navigation
    - all MDX content pages
tech_stack:
  added: []
  patterns: []
key_files:
  created:
    - index.mdx
    - export-guide.mdx
    - csv-reference.mdx
    - import-guide.mdx
    - troubleshooting.mdx
  modified:
    - docs.json
decisions: []
metrics:
  duration: ""
  completed_date: "2026-04-23"
---

# Phase 1 Plan 1-2 Summary: Site Rebrand & Scaffold

## One-Liner

Scaffolded Mintlify site with MatrixRates branding and placeholder MDX stubs for all navigation paths.

## What Was Built

- **docs.json**: Updated with MatrixRates branding (name, teal color #2dd4bf), single Guides tab with all 5 page paths registered, support email in navbar
- **MDX placeholders**: Created 5 minimal stub files for index, export-guide, csv-reference, import-guide, troubleshooting

## Key Changes

1. **docs.json (Plan 01-01)**
   - name: "MatrixRates Import/Export"
   - colors.primary: "#2dd4bf"
   - favicon: "/favicon.png"
   - Single "Guides" tab with registered pages
   - Navbar with support@dojoapps.co.uk link

2. **Starter Kit Deletion (Plan 01-02)**
   - Removed: essentials/, ai-tools/, api-reference/, images/ directories
   - Removed: quickstart.mdx, development.mdx, favicon.svg

3. **Placeholder MDX Stubs (Plan 01-02)**
   - index.mdx, export-guide.mdx, csv-reference.mdx, import-guide.mdx, troubleshooting.mdx

## Verification

- docs.json is valid JSON ✓
- All 5 navigation paths have corresponding .mdx files ✓
- No "Mint Starter Kit" references remain ✓

## Deviation

**None** - plan executed exactly as written.

## Threat Flags

| Flag | File | Description |
|------|------|-------------|
| none | - | No security-relevant surface changes |

---

## Self-Check: PASSED

- docs.json: FOUND (verified valid JSON)
- MDX stubs: FOUND (all 5 files)
- Commits: FOUND (0447bc1, 5e877f0)