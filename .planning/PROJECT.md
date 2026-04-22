# MatrixRates Import/Export Docs

## What This Is

User guides and knowledge base for MatrixRates Import/Export — a Shopify app that lets merchants import and export their shipping rate tables via CSV. Written for non-technical Shopify store owners who need to set up or modify their shipping rates without contacting support.

## Core Value

A merchant can import their first rate table successfully — and diagnose any failure themselves — without ever contacting support.

## Requirements

### Validated

- ✓ Mintlify documentation site configured and deployable via GitHub — existing
- ✓ Navigation structure (tabs + groups) defined in `docs.json` — existing
- ✓ MDX content authoring with Mintlify components (Cards, Steps, Notes, Accordions) — existing
- ✓ GitHub-based auto-deployment pipeline via Mintlify GitHub App — existing

### Active

- [ ] Import guide — step-by-step walkthrough for merchants uploading a rate table CSV
- [ ] CSV format reference — column definitions, required fields, accepted values, worked examples
- [ ] Troubleshooting guide — common import errors with plain-English causes and fixes
- [ ] Export guide — how to download the current rate table for backup or editing
- [ ] Site rebrand — replace Mintlify starter kit content with MatrixRates branding, navigation, and home page

### Out of Scope

- App settings documentation (beyond import/export) — focused scope for v1
- Billing and account management docs — different problem, different audience moment
- Developer or API documentation — audience is non-technical merchants
- Advanced rate configuration guides — out of scope until core import/export is covered

## Context

- The existing repo is a Mintlify starter kit — all current content (essentials/, ai-tools/, api-reference/) is sample content to be replaced
- There is an existing FAQ on the user's website that may be outdated — should be reviewed as source material during content creation
- MatrixRates ships rate tables use a CSV format with weight/price breakpoints, zones, and carrier-based rules
- The Mintlify site has GitHub auto-deployment already wired up — content changes go live on push

## Constraints

- **Audience**: Non-technical merchants — plain language, screenshots over code, numbered steps
- **Scope**: Import guide + CSV reference + troubleshooting only for v1 — avoid expanding to full app docs
- **Platform**: Mintlify MDX — all content in `.mdx` files, navigation registered in `docs.json`

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Focused scope (import + CSV + troubleshooting) | Covers the 3 top merchant pain points without sprawl | — Pending |
| Non-technical tone throughout | Primary audience is store owners, not developers | — Pending |
| Replace starter kit content entirely | Sample content is Mintlify boilerplate — none of it is relevant | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-04-22 after initialization*
