<!-- markdownlint-disable -->

# Hardening Report: elgohr--ecr-login-action/v3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **elgohr--ecr-login-action/v3** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions by mutable tags or branch names instead of pinned full-length commit SHAs, making the workflows vulnerable to supply-chain attacks if those tags/branches are moved.

- `.github/workflows/assign.yml`: `uses: pozil/auto-assign-issue@v2` (tag)
- `.github/workflows/release.yml`: `uses: actions/checkout@v4` (tag, appears 3 times)
- `.github/workflows/release.yml`: `uses: elgohr/ecr-login-action@main` (branch)

Locations:

- `.github/workflows/assign.yml:8`
- `.github/workflows/release.yml:13`
- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:40`

### missing-permissions (severity: medium)

Two workflow files lack complete permissions coverage:

1. `.github/workflows/assign.yml`: No top-level `permissions:` key and the only job (`auto-assign`) has no job-level `permissions:` key. The workflow uses `secrets.GITHUB_TOKEN` without restricting its scope.

2. `.github/workflows/release.yml`: No top-level `permissions:` key, and the `release` job has no job-level `permissions:` key (only the `test` and `integration-test` jobs define `permissions: contents: read`). The `release` job pushes tags to the repository and uses `secrets.PUBLISH_TOKEN`, but its token permissions are unrestricted.

Locations:

- `.github/workflows/assign.yml:1`
- `.github/workflows/release.yml:34`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. assign.yml:
   - Pinned pozil/auto-assign-issue@v2 to full SHA @7bf9d82c77d45976224660b873fc83e60576c5aa
   - Added top-level `permissions: {}` to deny all by default
   - Added job-level `permissions: issues: write` (minimum needed for issue assignment)

2. release.yml:
   - Pinned actions/checkout@v4 (3 occurrences) to full SHA @34e114876b0b11c390a56381ad16ebd13914f8d5
   - Pinned elgohr/ecr-login-action@main to full SHA @cfc13779e02b4569910002aacd9e24ea086264dc
   - Added top-level `permissions: {}` to deny all by default
   - Added job-level `permissions: contents: write` to the `release` job (needed to push tags)

### Iteration 2

**Fixes applied:** github-env-injection

**Notes:**

Fixed entrypoint.sh lines 19, 21, 22, and 23: introduced four sanitized intermediate variables (safe_username, safe_password, safe_registry, safe_docker_name) using `printf '%s' "$VAR" | tr -d '\n\r'` to strip embedded newlines before writing to $GITHUB_OUTPUT. The ::add-mask:: command was also updated to use safe_password. This prevents a malicious caller from injecting arbitrary key=value pairs into $GITHUB_OUTPUT via newline characters in AWS API responses.

