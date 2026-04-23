---
phase: 05-troubleshooting-guide
plan: 05-01
subsystem: documentation
tags: [troubleshooting, errors, support]
requires: [TROUBLE-01, TROUBLE-02, TROUBLE-03]
provides: [Complete troubleshooting reference]
affects: [troubleshooting.mdx]
tech-stack: [Mintlify, MDX]
key-files: [troubleshooting.mdx]
decisions:
  - Included a privacy note in the support section to warn merchants about sensitive data in CSVs (T-05-01).
  - Used verbatim error strings as Accordion titles to maximize searchability (T-05-02).
metrics:
  duration: 15m
  completed_date: "2026-04-23"
---

# Phase 05 Plan 01: Troubleshooting Guide Summary

## One-liner
Implemented a comprehensive Troubleshooting Guide with common pre-import mistakes, verbatim error recovery steps, and a support contact section.

## Key Changes

### Content
- **Troubleshooting Guide:** Replaced the "coming soon" stub with full content.
- **Common Mistakes:** Documented file format (.csv vs .xlsx), missing columns, and invalid data types.
- **Error Reference:** Created an `AccordionGroup` with verbatim error messages from the MatrixRates app, providing the cause and numbered fix steps for each.
- **Support Section:** Added a "Still stuck?" exit hatch with a `mailto` link to support and a privacy warning.

### Formatting
- Used Mintlify `<Accordion>` components for the error reference to keep the page clean.
- Used a Mintlify `<Card>` for the primary support contact action.
- Added HTML comments as placeholders for future screenshots.

## Deviations from Plan

None - plan executed exactly as written.

## Known Stubs

| File | Line | Reason |
|------|------|--------|
| troubleshooting.mdx | 44 | Placeholder for screenshot of "Invalid header" error. |
| troubleshooting.mdx | 55 | Placeholder for screenshot of "Missing required header" error. |
| troubleshooting.mdx | 64 | Placeholder for screenshot of "Invalid rate_type value" error. |
| troubleshooting.mdx | 73 | Placeholder for screenshot of "Price must be a number" error. |
| troubleshooting.mdx | 82 | Placeholder for screenshot of "Weight values cannot be negative" error. |

## Self-Check: PASSED
- [x] Troubleshooting guide contains verbatim error headings.
- [x] Cause and fix steps provided for each error.
- [x] Support contact block with mailto link is present.
- [x] Commits are atomic and descriptive.
