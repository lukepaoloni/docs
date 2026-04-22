# Architecture

**Analysis Date:** 2026-04-22

## Pattern Overview

**Overall:** Static documentation site using Mintlify's content-as-configuration model

**Key Characteristics:**
- All content is MDX (Markdown + JSX) files rendered by the Mintlify platform
- There is no application code, server logic, or custom build pipeline — Mintlify handles all rendering
- Navigation, theming, and site structure are entirely driven by `docs.json`
- API documentation is generated from an OpenAPI specification (`api-reference/openapi.json`)
- Reusable content is composed via the `snippets/` directory using MDX imports

## Layers

**Configuration Layer:**
- Purpose: Defines the entire site structure, navigation, theming, branding, and integrations
- Location: `docs.json` (root)
- Contains: Navigation tabs, groups, page paths, colors, logos, navbar, footer, contextual options
- Depends on: Nothing — this is the root authority
- Used by: Mintlify CLI and the hosted Mintlify platform to assemble the site

**Content Layer:**
- Purpose: The documentation pages authored in MDX
- Location: Root-level `.mdx` files (`index.mdx`, `quickstart.mdx`, `development.mdx`) and topic subdirectories (`essentials/`, `ai-tools/`, `api-reference/`)
- Contains: MDX pages using Mintlify's built-in component library (Card, Accordion, Steps, Note, etc.)
- Depends on: `docs.json` (for navigation registration), `snippets/` (for shared content)
- Used by: Mintlify renderer to produce final HTML pages

**Reusable Snippets Layer:**
- Purpose: Shared content fragments imported into multiple pages to avoid duplication
- Location: `snippets/`
- Contains: `.mdx` files that export default components or named variables
- Depends on: Nothing
- Used by: Content layer pages via MDX `import` statements

**API Reference Layer:**
- Purpose: API documentation generated automatically from an OpenAPI specification
- Location: `api-reference/`
- Contains: `openapi.json` (OpenAPI 3.1.0 spec), `introduction.mdx` (overview page), `endpoint/*.mdx` (stub pages referencing OpenAPI operations)
- Depends on: `openapi.json` for auto-generating endpoint pages
- Used by: Mintlify API playground renderer

**Static Assets Layer:**
- Purpose: Images, logos, and favicon used within content and site chrome
- Location: `images/`, `logo/`, `favicon.svg`
- Contains: `.png` hero images, `.svg` logos (light/dark variants), `favicon.svg`
- Depends on: Nothing
- Used by: MDX pages and `docs.json` configuration

## Data Flow

**Page Rendering:**

1. Mintlify CLI (`mint dev`) or hosted platform reads `docs.json` to resolve navigation structure
2. Each page path in `docs.json` maps to a corresponding `.mdx` file (e.g., `"quickstart"` → `quickstart.mdx`)
3. MDX files are compiled: Mintlify component imports are resolved and JSX is rendered to HTML
4. Pages that import from `snippets/` have those fragments inlined at compile time
5. Final pages are served at `http://localhost:3000` (dev) or the hosted Mintlify domain (production)

**API Reference Rendering:**

1. `docs.json` navigation references paths like `"api-reference/endpoint/get"`
2. Each endpoint stub file (e.g., `get.mdx`) contains only a frontmatter `openapi` field (e.g., `openapi: 'GET /plants'`)
3. Mintlify matches that operation to `api-reference/openapi.json` and auto-generates the playground UI

**Deployment:**

1. Changes are committed and pushed to GitHub
2. The Mintlify GitHub app detects the push and triggers a deployment
3. The hosted site is updated automatically — no manual build step required

**State Management:**
- There is no client-side application state. All content is static. Mintlify's contextual AI assistant (ChatGPT, Claude, Perplexity, MCP integrations) is configured in `docs.json` under `"contextual"` and handled entirely by the Mintlify platform.

## Key Abstractions

**docs.json (Site Configuration):**
- Purpose: Single source of truth for site identity, navigation, branding, and feature flags
- Location: `docs.json`
- Pattern: Declarative JSON config; page paths are relative strings matching `.mdx` file paths without the extension

**MDX Page:**
- Purpose: A documentation page combining Markdown prose with Mintlify JSX components
- Examples: `index.mdx`, `quickstart.mdx`, `essentials/markdown.mdx`, `ai-tools/cursor.mdx`
- Pattern: YAML frontmatter (`title`, `description`, `icon`) followed by MDX body using Mintlify components

**Snippet:**
- Purpose: A reusable MDX fragment imported as a React component into other pages
- Examples: `snippets/snippet-intro.mdx`
- Pattern: `.mdx` file in `snippets/` that either uses default export or named exports; imported with `import ComponentName from '/snippets/file.mdx'`

**OpenAPI Endpoint Stub:**
- Purpose: A minimal MDX file that delegates page content entirely to the OpenAPI spec
- Examples: `api-reference/endpoint/get.mdx`, `api-reference/endpoint/create.mdx`
- Pattern: Frontmatter only — `title` plus an `openapi` field specifying the HTTP method and path

## Entry Points

**Site Root:**
- Location: `index.mdx`
- Triggers: Visitor lands on `/` or Mintlify resolves the first page in navigation
- Responsibilities: Renders the introduction/welcome page with links to key sections

**Configuration Entry:**
- Location: `docs.json`
- Triggers: Every Mintlify CLI command or hosted deployment
- Responsibilities: Declares all navigation tabs, page groups, branding, integrations, and site-level settings

**API Documentation Entry:**
- Location: `api-reference/introduction.mdx`
- Triggers: User navigates to the "API reference" tab
- Responsibilities: Explains API authentication and links to the OpenAPI spec

## Error Handling

**Strategy:** Not applicable — this is a static content repository with no custom runtime code

**Patterns:**
- Broken internal links can be validated with `mint broken-links` (CLI command)
- Deployment errors are surfaced in the Mintlify GitHub app checks

## Cross-Cutting Concerns

**Theming:** Defined centrally in `docs.json` under `"colors"`, `"logo"`, `"favicon"`, `"theme"` — applied globally by Mintlify
**Navigation:** Fully declarative in `docs.json` under `"navigation"` — no per-page nav logic required
**AI/Contextual integrations:** Configured in `docs.json` under `"contextual"` — options include `copy`, `view`, `chatgpt`, `claude`, `perplexity`, `mcp`, `cursor`, `vscode`

---

*Architecture analysis: 2026-04-22*
