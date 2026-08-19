<!-- markdownlint-disable -->

# Hardening Report: elgohr--ecr-login-action/0.0.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **elgohr--ecr-login-action/0.0.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses `actions/checkout@master`, which is pinned to a mutable branch name rather than an immutable 40-character commit SHA. This means the action could be silently updated (or compromised) without any change to the workflow file, creating a supply-chain risk.

Locations:

- `.github/workflows/test.yml:7`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/test.yml` has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents). A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed .github/workflows/test.yml: (1) Pinned `actions/checkout@master` to the full commit SHA `61b9e3751b92087fd0b06925ba6dd6314e06f089` with a `# master` comment for readability. (2) Added a top-level `permissions: contents: read` block to restrict the workflow token to the minimum required for a checkout-and-build workflow.

