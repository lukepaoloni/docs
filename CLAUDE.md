# MatrixRates Import/Export Docs — Project Guide

## Project

Mintlify documentation site for MatrixRates Import/Export — a Shopify app that lets merchants import/export shipping rates via CSV. Audience: non-technical Shopify store owners.

**Core value:** A merchant can import their first set of rates successfully — and diagnose any failure themselves — without ever contacting support.

**Workspace root:** `/Users/lukepaoloni/Shopify/Apps/docs`
**Planning docs:** `.planning/`

## Working Relationship
- You can push back on ideas—this can lead to better documentation. Cite sources and explain your reasoning when you do so.
- **ALWAYS** ask for clarification rather than making assumptions.
- **NEVER** lie, guess, or make up anything.

## GSD Workflow

This project uses GSD (Get Shit Done) for structured execution.

**Key files:**
- `.planning/PROJECT.md` — project context and requirements
- `.planning/ROADMAP.md` — 6-phase execution plan
- `.planning/STATE.md` — current progress and blockers
- `.planning/REQUIREMENTS.md` — 20 v1 requirements with traceability

**Workflow commands:**
- `/gsd-plan-phase N` — create a detailed plan for phase N
- `/gsd-execute-phase N` — execute a planned phase
- `/gsd-progress` — check current state

**Mode:** YOLO (auto-approve within phases) | **Granularity:** Fine | **Parallelization:** Enabled

## Phase Overview

| Phase | Name | Status |
|-------|------|--------|
| 1 | Site Rebrand & Scaffold | ✓ Complete |
| 2 | Export Guide | ✓ Complete |
| 3 | Spreadsheet Reference | ✓ Complete |
| 4 | Import Guide | ✓ Complete |
| 5 | Troubleshooting Guide | ✓ Complete |
| 6 | Home Page | ✓ Complete |

## Tech Stack & Context
- **Platform:** Mintlify (MDX content, `docs.json` config, GitHub auto-deploy)
- **Format:** MDX files with YAML frontmatter
- **Config:** `docs.json` at the repo root — navigation, theme, branding, and site settings
- **Dev:** `mint dev` (requires Node.js v19+, `npm i -g mint`)
- **Schema:** Refer to the [docs.json schema](https://mintlify.com/docs.json) for configuration

### Frontmatter requirements
- `title`: Clear, descriptive page title
- `description`: Concise summary for SEO/navigation

## Content Strategy & Principles
- **Accuracy First:** Prioritize accuracy and usability. Document just enough for user success.
- **Non-technical Audience:** Use plain language, numbered steps, and screenshots over code.
- **Action-Result Framing:** Every step states what the merchant does AND what they'll see next.
- **Error-Cause-Fix:** Troubleshooting entries must include verbatim error, plain-English cause, and numbered fix.
- **Evergreen Content:** Make content evergreen when possible; avoid duplication.
- **Terminology:** Avoid technical jargon (e.g., use "column" instead of "schema", "file" instead of "parse"). Rename "rate table" to "shipping rates" for merchant clarity.

## Writing Standards
- **Voice:** Second-person voice ("you").
- **Structure:** Prerequisites at start of procedural content.
- **Verification:** Test all code/CSV examples before publishing.
- **Formatting:** Match style of existing pages; include both basic and advanced use cases.
- **Assets:** Alt text on all images; relative paths for internal links.

## Key Mintlify Components
```mdx
<Steps>         — numbered walkthroughs (import/export guides)
<Frame>         — screenshots (required — never raw <img>)
<Warning>       — before irreversible steps
<Note> / <Tip>  — supplementary callouts
<Accordion>     — troubleshooting entries / large reference lists
<Card> / <CardGroup> — "What to do next" sections
<Badge>         — status/plan labels (Syntax: <Badge color="blue">Text</Badge>)
```

## Git Workflow
- **Frequency:** Commit frequently throughout development.
- **Safety:** NEVER use `--no-verify`. NEVER skip or disable pre-commit hooks.
- **Branching:** Create a new branch when no clear branch exists for changes.
- **Cleanup:** Check for uncommitted changes before starting a new task.

## Support Contact
`support@dojoapps.co.uk`
