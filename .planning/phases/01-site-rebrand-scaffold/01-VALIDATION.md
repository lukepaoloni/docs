---
phase: 1
slug: site-rebrand-scaffold
status: draft
nyquist_compliant: false
wave_0_complete: false
created: 2026-04-22
---

# Phase 1 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Manual verification + `mint broken-links` CLI (if available) |
| **Config file** | None — no automated test suite for static doc sites |
| **Quick run command** | `grep -E '"name"\|"primary"\|support' docs.json` |
| **Full suite command** | Manual checklist against Phase 1 success criteria (5 criteria) |
| **Estimated runtime** | ~30 seconds (grep checks) + visual browser verification |

---

## Sampling Rate

- **After every task commit:** Run the grep command for that task's requirement
- **After every plan wave:** Run full grep suite across docs.json
- **Before `/gsd-verify-work`:** Full 5-point checklist must pass (including live site visual check)
- **Max feedback latency:** 30 seconds (automated grep checks)

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Threat Ref | Secure Behavior | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|------------|-----------------|-----------|-------------------|-------------|--------|
| 1-01-01 | 01 | 1 | SITE-01 | — | No secrets in docs.json | manual | `grep -E '"name"\|"primary"\|"support"' docs.json` | ✅ | ⬜ pending |
| 1-01-02 | 01 | 1 | SITE-02 | — | N/A | manual | `ls essentials/ ai-tools/ api-reference/ 2>&1 \| grep "No such"` | ❌ W0 (deleted) | ⬜ pending |
| 1-01-03 | 01 | 1 | SITE-03 | — | N/A | manual | `ls logo/ favicon.png` | ❌ W0 (user action) | ⬜ pending |
| 1-01-04 | 01 | 1 | SITE-04 | — | N/A | manual | `grep -E "export-guide\|csv-reference\|import-guide\|troubleshooting" docs.json` | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] `favicon.png` — must be placed in repo root by user before SITE-03 can fully pass (human action)
- [ ] `logo/light.svg`, `logo/dark.svg` — must be replaced by user with MatrixRates brand files (human action)

*Note: docs.json path updates and favicon.svg deletion CAN proceed without the PNG file. The user's favicon.png placement is a separate human-action task that unblocks SITE-03 completion.*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Site shows teal brand color, not Mintlify green | SITE-01 | Visual rendering only verifiable in browser | Open live docs URL, confirm teal accent color (#2dd4bf range), confirm no green (#16A34A) visible |
| MatrixRates logo appears in navbar | SITE-03 | File content swap requires visual inspection | Open live docs URL, confirm logo SVG renders in navbar (not Mintlify leaf) |
| Favicon appears in browser tab | SITE-03 | Browser tab rendering | Open live docs URL, check browser tab icon is MatrixRates favicon, not Mintlify leaf |
| No starter kit pages reachable | SITE-02 | 404 behavior requires browser test | Navigate to /quickstart, /development, /essentials/settings — each must 404 or redirect |
| Support email in navbar | SITE-01 | Navbar link rendering | Open live docs URL, confirm "Support" link visible in navbar, click confirms mailto:support@dojoapps.co.uk |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references (favicon.png, logo SVGs)
- [ ] No watch-mode flags
- [ ] Feedback latency < 30s
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
