# External Integrations

**Analysis Date:** 2026-04-22

## APIs & External Services

**Documentation Platform:**
- Mintlify - Renders, hosts, and serves the documentation site
  - Dashboard: `https://dashboard.mintlify.com`
  - Docs: `https://mintlify.com/docs`
  - Auth: Managed via Mintlify account (no local credentials)

**Contextual AI Integrations (configured in `docs.json`):**
The site exposes contextual AI assistant buttons to end users. These are platform-level integrations managed by Mintlify, not by this repo:
- ChatGPT (`chatgpt`)
- Claude (`claude`)
- Perplexity (`perplexity`)
- MCP (`mcp`)
- Cursor (`cursor`)
- VS Code (`vscode`)

## Data Storage

**Databases:**
- None - Content-only static documentation site. No database.

**File Storage:**
- Local filesystem only - Images stored in `images/`, logos in `logo/`, SVG favicon in `favicon.svg`. All committed to the git repository.

**Caching:**
- None managed by this repo - Mintlify platform handles CDN and caching.

## Authentication & Identity

**Auth Provider:**
- Bearer token authentication is documented in the sample API reference (`api-reference/openapi.json`) as an example only. It is not implemented in this repository.
- Mintlify dashboard access is authenticated via the Mintlify platform itself.

## Monitoring & Observability

**Error Tracking:**
- None

**Logs:**
- None managed by this repo - Mintlify platform logs are accessible via the dashboard.

## CI/CD & Deployment

**Hosting:**
- Mintlify platform (managed hosting, no self-hosted infrastructure)

**CI Pipeline:**
- Mintlify GitHub App - Installed from `https://dashboard.mintlify.com/settings/organization/github-app`
  - Monitors the default branch
  - Auto-deploys on every push
  - Runs deployment checks visible in the GitHub PR/commit status (see `images/checks-passed.png`)
  - No GitHub Actions workflows are present in this repository

## Environment Configuration

**Required env vars:**
- None - This repository has no environment variables. All configuration is in `docs.json`.

**Secrets location:**
- No secrets present. The Mintlify platform handles all auth and deployment tokens externally.

## Webhooks & Callbacks

**Incoming (documented sample only — not implemented in this repo):**
- `POST /plant/webhook` — Defined in `api-reference/openapi.json` as an OpenAPI 3.1.0 webhook example. This is illustrative content for documentation purposes only.

**Outgoing:**
- None

## External Links Referenced

**Mintlify properties (navbar and footer anchors in `docs.json`):**
- Documentation: `https://mintlify.com/docs`
- Blog: `https://mintlify.com/blog`
- Support email: `hi@mintlify.com`
- GitHub: `https://github.com/mintlify`
- Twitter/X: `https://x.com/mintlify`
- LinkedIn: `https://linkedin.com/company/mintlify`
- Customer showcase: `https://mintlify.com/customers`
- Community: `https://mintlify.com/community`

---

*Integration audit: 2026-04-22*
