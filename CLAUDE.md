# MatrixRates Import/Export Docs — Project Guide

## Project

Mintlify documentation site for MatrixRates Import/Export — a Shopify app that lets merchants import/export shipping rate tables via CSV. Audience: non-technical Shopify store owners.

**Core value:** A merchant can import their first rate table successfully — and diagnose any failure themselves — without ever contacting support.

Planning docs: `.planning/`

## GSD Workflow

This project uses GSD (Get Shit Done) for structured execution.

**Key files:**
- `.planning/PROJECT.md` — project context and requirements
- `.planning/ROADMAP.md` — 6-phase execution plan
- `.planning/STATE.md` — current progress and blockers
- `.planning/REQUIREMENTS.md` — 20 v1 requirements with traceability
- `.planning/research/` — stack, features, architecture, and pitfalls research

**Workflow commands:**
- `/gsd-plan-phase N` — create a detailed plan for phase N
- `/gsd-execute-phase N` — execute a planned phase
- `/gsd-progress` — check current state

**Mode:** YOLO (auto-approve within phases)
**Granularity:** Fine (many focused phases)
**Parallelization:** Enabled

## Phase Overview

| Phase | Name | Status |
|-------|------|--------|
| 1 | Site Rebrand & Scaffold | Not started |
| 2 | Export Guide | Not started |
| 3 | CSV Reference | Not started |
| 4 | Import Guide | Not started |
| 5 | Troubleshooting Guide | Not started |
| 6 | Home Page | Not started |

**Next:** `/gsd-plan-phase 1`

## Tech Stack

- **Platform:** Mintlify (MDX content, `docs.json` config, GitHub auto-deploy)
- **Content format:** MDX files — markdown + Mintlify JSX components
- **Config:** `docs.json` at the repo root — all navigation, branding, and site settings
- **Dev:** `mint dev` (requires Node.js v19+, `npm i -g mint`)
- **Deploy:** Push to main → Mintlify GitHub App auto-deploys

## Content Principles

- **Audience is non-technical merchants** — plain language, numbered steps, screenshots over code
- **Action-Result framing** — every step states what the merchant does AND what they'll see next
- **Error-Cause-Fix** — every troubleshooting entry: verbatim error, plain-English cause, numbered fix
- **Warnings before irreversible steps** — `<Warning>` component immediately before, not at the top
- **No technical jargon** — avoid "parse", "delimiter", "schema"; use "column", "file", "value"

## Key Mintlify Components

```
<Steps>         — numbered walkthroughs (import/export guides)
<Frame>         — screenshots (required — never raw <img>)
<Warning>       — before irreversible steps
<Note> / <Tip>  — supplementary callouts
<Accordion>     — troubleshooting entries
<Card> / <CardGroup> — "What to do next" sections
```

## Active Blockers

Before planning/executing specific phases, these must be resolved:

- **Phase 1:** Brand assets (logo SVG light/dark, favicon, primary hex color)
- **Phase 3:** Exact CSV column specification from the app
- **Phase 4:** MatrixRates app screenshots
- **Phase 5:** Verbatim error message strings from the app

## Support Contact

`support@dojoapps.co.uk`
