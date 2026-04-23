---
phase: "02"
plan: "01"
subsystem: docs
tags:
  - export
  - guide
  - mintlify
dependency_graph:
  requires:
    - "01-site-rebrand-scaffold"
  provides:
    - "export-guide"
    - "EXPORT-01"
    - "EXPORT-02"
  affects:
    - "import-guide"
    - "csv-reference"
tech_stack:
  added:
    - Mintlify Steps component
    - Mintlify CardGroup component
    - Mintlify Frame and Tip components
  patterns:
    - Numbered walkthrough with action-result framing
    - What to do next cards
key_files:
  created: []
  modified:
    - "export-guide.mdx"
decisions:
  - "D-03: URL slugs are kebab-case - /export-guide"
metrics:
  duration_minutes: 2
  completed_date: "2026-04-23T06:03:00Z"
---

# Phase 2 Plan 1: Export Guide Summary

## Overview

Created the complete Export Guide page with a numbered walkthrough and What to do next cards, enabling merchants to download their shipping rate table as a CSV file without external assistance.

## What Was Built

- **3-step numbered walkthrough** using `<Steps>` component
- Each step has clear action + expected result in plain language
- `<Tip>` block for download issues
- **4-card What to do next section** with cross-links to import-guide, csv-reference

## Key Decisions Made

- Kept walkthrough to 3 steps (optimal for common export flow)
- Added 4th "Learn about the CSV format" card to drive traffic to csv-reference
- Used Lucide icons: edit-3, folder, users, file-text

## Deviation Documentation

**None** — Plan executed exactly as written.

## Auth Gates

**None** — No authentication required for documentation pages.

## Threat Flags

| Flag | File | Description |
|------|------|-------------|
| none | export-guide.mdx | Static documentation, no security surface |

## Known Stubs

**None** — All content is production-ready.

---

## Self-Check: PASSED

- [x] export-guide.mdx exists and is not placeholder stub
- [x] File contains `<Steps>` with 3 steps
- [x] File contains `<CardGroup>` with 4 cards
- [x] At least one card links to /import-guide
- [x] No remaining "coming soon" or placeholder text
- [x] docs.json navigation includes export-guide (from Phase 1)
- [x] Commit fd83402 created