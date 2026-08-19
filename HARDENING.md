<!-- markdownlint-disable -->

# Hardening Report: elgohr--ecr-login-action/1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **elgohr--ecr-login-action/1.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml `runs.image` references a mutable Docker image tag (`docker://lgohr/ecr-login-action:latest`) instead of an immutable SHA digest. This means the action can silently pull a different (potentially malicious) image on each run. Additionally, both workflow files reference actions using mutable branch refs (`@master`) instead of pinned 40-character commit SHAs: `actions/checkout@master` and `elgohr/Publish-Docker-Github-Action@master`.

Locations:

- `action.yml:23`
- `.github/workflows/publish.yml:8`
- `.github/workflows/publish.yml:9`
- `.github/workflows/test.yml:7`

### missing-permissions (severity: medium)

Neither `.github/workflows/publish.yml` nor `.github/workflows/test.yml` declares a top-level `permissions:` key, and no job-level `permissions:` keys are present either. Without explicit permissions, workflows run with the default (often overly broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/publish.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings across 3 files:

1. action.yml (line 23): Pinned `docker://lgohr/ecr-login-action:latest` to immutable digest `docker://lgohr/ecr-login-action:latest@sha256:29302377ae8d5d9aecd8fabb79e060833812bd4d5ba41e6d8e5273b410c58903`.

2. .github/workflows/publish.yml: Added `permissions: {}` top-level block; pinned `actions/checkout@master` → `@61b9e3751b92087fd0b06925ba6dd6314e06f089 # master`; pinned `elgohr/Publish-Docker-Github-Action@master` → `@8217e91c0369a5342a4ef2d612de87492410a666 # master`.

3. .github/workflows/test.yml: Added `permissions: {}` top-level block; pinned `actions/checkout@master` → `@61b9e3751b92087fd0b06925ba6dd6314e06f089 # master`.

