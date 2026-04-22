# Code Conventions

*Mapped: 2026-04-22 | Focus: quality*

## Overview

This is a documentation-only repository (no application source code). "Conventions" apply to MDX content authoring and docs.json configuration.

---

## File Format

Every `.mdx` page **must** begin with YAML frontmatter:

```yaml
---
title: "Clear, specific, keyword-rich title"
description: "Concise description explaining page purpose and value"
---
```

Optional frontmatter fields:
- `icon: "icon-name"` — used on navigation items and cards
- `sidebarTitle: "Short title"` — overrides nav label without changing page title

---

## Writing Style

| Rule | Example |
|------|---------|
| Second person for instructions | "You can configure..." not "Users can configure..." |
| Active voice | "Run the command" not "The command should be run" |
| Present tense for current state | "The API returns..." not "The API will return..." |
| Define jargon on first use | "MDX (Markdown with JSX components)..." |
| Parallel structure in lists | All items start with same grammatical form |
| Inverted pyramid | Most important info first |

---

## Mintlify Component Usage

Components come from Mintlify's built-in library — no imports needed.

### When to use each component

| Component | Use for |
|-----------|---------|
| `<Note>` | Supplementary helpful info |
| `<Tip>` | Best practices, shortcuts |
| `<Warning>` | Destructive actions, breaking changes |
| `<Info>` | Neutral context, announcements |
| `<Check>` | Success confirmations |
| `<Steps>` | Sequential procedures |
| `<Tabs>` | Platform-specific or alternative content |
| `<CodeGroup>` | Same concept in multiple languages |
| `<Accordion>` | Progressive disclosure |
| `<Card>` | Navigation cards, feature highlights |
| `<Columns>` / `<CardGroup>` | Grid layouts |
| `<ParamField>` | API request parameters |
| `<ResponseField>` | API response fields |
| `<RequestExample>` / `<ResponseExample>` | API call/response pairs |
| `<Frame>` | Wrapping images |

---

## Code Examples

- Always complete and runnable — copy-paste should work
- Realistic data (not `foo`/`bar` placeholders)
- **Never include real API keys or secrets**
- Show error handling where relevant
- Specify language identifier on fenced code blocks:

```bash
# Good: language specified
```javascript config.js
const x = 1;
```

---

## Navigation (docs.json)

- Route paths omit file extension: `"essentials/navigation"` not `"essentials/navigation.mdx"`
- Navigation must be updated in `docs.json` whenever a page is added or removed — Mintlify does not auto-discover pages
- Group names use title case: `"Getting started"`, `"Writing content"`

---

## Snippets

- Reusable copy lives in `snippets/*.mdx`
- Import with `<Snippet file="snippet-intro.mdx" />`
- Keep snippets atomic — one concept per file
- Do not duplicate content that belongs in a snippet

---

## Images

- Wrap all images in `<Frame>` component
- Always include descriptive `alt` text
- Store in `images/` directory
- Reference as absolute path: `/images/filename.png`

---

## Heading Hierarchy

- Pages start at `##` (H2) — the page `title` frontmatter serves as H1
- Never skip heading levels (H2 → H3, not H2 → H4)
- Use descriptive, keyword-rich headings for navigation and SEO

---

*Last updated: 2026-04-22*
