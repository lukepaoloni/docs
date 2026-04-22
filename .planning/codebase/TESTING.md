# Testing

*Mapped: 2026-04-22 | Focus: quality*

## Overview

This is a documentation-only repository with no application source code. There is **no test framework, no test files, and no CI/CD pipeline** in the current state.

---

## Current State

| Category | Status |
|----------|--------|
| Unit tests | None |
| Integration tests | None |
| E2E tests | None |
| CI/CD pipeline | None |
| Automated link checking | None |
| Spell checking | None |
| MDX syntax validation | None |

---

## Manual Validation

Mintlify provides a local dev server for manual visual validation:

```bash
# Install CLI (one-time)
npm i -g mintlify

# Start local preview
mintlify dev
```

Open `http://localhost:3000` to preview pages before publishing.

---

## Recommended Testing Additions

### 1. Link Validation

Add a CI step to catch broken internal and external links:

```bash
# Example: using lychee link checker
lychee --offline '**/*.mdx' '**/*.json'
```

### 2. MDX Syntax Validation

Mintlify CLI can validate MDX syntax:

```bash
mintlify check
```

### 3. GitHub Actions Pipeline

Suggested `.github/workflows/docs-check.yml`:

```yaml
name: Docs check
on: [pull_request]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm i -g mintlify
      - run: mintlify check
```

---

## Quality Gates (Manual)

Before merging any content PR:

- [ ] `mintlify dev` runs without errors
- [ ] All new pages appear in navigation correctly
- [ ] Code examples are complete and runnable
- [ ] Images display correctly in both light and dark mode
- [ ] No placeholder/lorem ipsum copy
- [ ] Internal links resolve to existing pages

---

*Last updated: 2026-04-22*
