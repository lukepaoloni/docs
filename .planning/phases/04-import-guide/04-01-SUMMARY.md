---
phase: "04-import-guide"
plan: "01"
subsystem: "Content"
tags: ["import", "guide", "walkthrough", "merchants"]
requires: ["IMPORT-01", "IMPORT-02", "IMPORT-03", "IMPORT-04"]
provides: ["Complete import walkthrough"]
affects: ["import-guide.mdx"]
tech-stack: ["Mintlify", "MDX"]
key-files: ["import-guide.mdx"]
decisions:
  - "Used a <Warning> block at the top to emphasize data overwrite risks"
  - "Included a verification section to guide merchants to Shopify Admin settings"
metrics:
  duration: "10m"
  completed_at: "2026-04-23T06:45:00Z"
---

# Phase 04 Plan 01: Import Guide Summary

Complete production-ready import walkthrough for merchants with safety warnings and verification steps.

## Key Changes

### Content
- **Import Guide:** Replaced the "coming soon" stub with a full guide.
- **Safety First:** Added a high-visibility warning about overwriting rates, including a link to the export guide for backups.
- **Structured Steps:** Used the Mintlify `<Steps>` component to break the import process into 5 manageable actions.
- **Verification:** Added a dedicated section explaining how to confirm the import success within the Shopify Admin shipping settings.
- **Navigation:** Added a `<CardGroup>` at the bottom to direct merchants to testing, troubleshooting, or exporting.

## Deviations from Plan

None - plan executed exactly as written.

## Known Stubs

| File | Line | Reason |
|------|------|--------|
| import-guide.mdx | 35 | Screenshot placeholder for the Import screen. Actual screenshot will be added in a future branding/asset phase. |

## Self-Check: PASSED

1. **Created files exist:** `import-guide.mdx` exists.
2. **Commits exist:** Commit `601745b` contains the changes.
