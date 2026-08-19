<!-- markdownlint-disable -->

# Hardening Report: elgohr--ecr-login-action/v2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **elgohr--ecr-login-action/v2** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

assign.yml has no top-level `permissions:` key and the only job (`auto-assign`) also has no job-level `permissions:` key. This means the job runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/assign.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` key, and the `release` job has no job-level `permissions:` key (unlike the `test` and `integration-test` jobs which do). The `release` job performs a `git push --force` to a protected branch and should have explicit minimal permissions.

Locations:

- `.github/workflows/release.yml:30`

### unpinned-uses (severity: high)

assign.yml references `pozil/auto-assign-issue@v1` — a mutable tag, not a pinned 40-character commit SHA. This is vulnerable to supply-chain attacks if the tag is moved.

Locations:

- `.github/workflows/assign.yml:8`

### unpinned-uses (severity: high)

release.yml references multiple actions by mutable tag or branch rather than pinned 40-character commit SHAs: `actions/checkout@v3` (lines 13, 22, 36) and `elgohr/ecr-login-action@main` (line 23). These are vulnerable to supply-chain attacks.

Locations:

- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:36`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions, unpinned-uses

**Notes:**

Fixed all four findings across two workflow files:

1. assign.yml:
   - Added top-level `permissions: {}` to deny all default permissions
   - Added job-level `permissions: issues: write` to the `auto-assign` job (minimum needed for issue assignment)
   - Pinned `pozil/auto-assign-issue@v1` → `@d11e715efc663fe323c3d8d4d3cbbfdddd539baf # v1`

2. release.yml:
   - Added top-level `permissions: {}` to deny all default permissions
   - Added job-level `permissions: contents: write` to the `release` job (needed for git push --force to protected branch)
   - Pinned `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3` (all 3 occurrences)
   - Pinned `elgohr/ecr-login-action@main` → `@cfc13779e02b4569910002aacd9e24ea086264dc # main`
   - The `test` and `integration-test` jobs already had `permissions: contents: read` and were left unchanged

