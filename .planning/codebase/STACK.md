# Technology Stack

**Analysis Date:** 2026-04-22

## Languages

**Primary:**
- MDX (Markdown + JSX) - All documentation content pages (`*.mdx` files throughout root and subdirectories)

**Secondary:**
- JSON - Site configuration (`docs.json`, `api-reference/openapi.json`)
- SVG - Logo and favicon assets (`logo/dark.svg`, `logo/light.svg`, `favicon.svg`)

## Runtime

**Environment:**
- Node.js v19 or higher (required by Mintlify CLI per `development.mdx`)

**Package Manager:**
- npm - Used to install the Mintlify CLI globally (`npm i -g mint`)
- Lockfile: Not present (no local `package.json` — CLI is installed globally, not as a project dependency)

## Frameworks

**Core:**
- Mintlify - Documentation platform that renders MDX content, handles navigation, theming, and deployment. Version tracked by the globally-installed `mint` CLI.

**Build/Dev:**
- Mintlify CLI (`mint`) - Local preview server at `http://localhost:3000`, link validation, and deployment trigger
  - Install: `npm i -g mint`
  - Dev: `mint dev`
  - Dev on custom port: `mint dev --port <PORT>`
  - Link validation: `mint broken-links`
  - Update CLI: `npm mint update`

**Testing:**
- Not applicable - Content-only repository with no test framework

## Key Dependencies

**Critical:**
- Mintlify platform - The entire rendering, hosting, and deployment pipeline is provided by Mintlify. The repo is content only; all build infrastructure is external.

**Infrastructure:**
- GitHub + Mintlify GitHub App - Automatic deployment triggered on push to default branch. App installed via `https://dashboard.mintlify.com/settings/organization/github-app`.

## Configuration

**Site Configuration:**
- `docs.json` - Primary config file. Controls theme, name, colors, navigation tabs/groups/pages, logo, navbar links, footer socials, and contextual AI options.
  - Schema: `https://mintlify.com/docs.json`
  - Theme: `mint`
  - Primary color: `#16A34A`
  - Contextual AI options: copy, view, chatgpt, claude, perplexity, mcp, cursor, vscode

**API Reference:**
- `api-reference/openapi.json` - OpenAPI 3.1.0 specification (sample Plant Store API). Used by Mintlify to auto-generate the API reference playground pages.

**Environment:**
- No environment variables required locally — all configuration is in `docs.json` and MDX frontmatter
- No `.env` files present

## Platform Requirements

**Development:**
- Node.js v19+
- `mint` CLI installed globally via npm
- Run `mint dev` from the directory containing `docs.json`
- Local preview: `http://localhost:3000`

**Production:**
- Hosted on Mintlify's platform
- Deployments triggered automatically via GitHub push through the Mintlify GitHub App
- Dashboard: `https://dashboard.mintlify.com`

---

*Stack analysis: 2026-04-22*
