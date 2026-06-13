<!-- markdownlint-disable -->

# Hardening Report: elgohr--ecr-login-action/1.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **elgohr--ecr-login-action/1.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag (`docker://lgohr/ecr-login-action:latest`) instead of a SHA digest. This means the image pulled at runtime can change without notice, enabling supply-chain attacks. The `image:` field under `runs:` should reference a specific immutable SHA digest, e.g. `image: 'docker://lgohr/ecr-login-action@sha256:<64-hex-char-digest>'`.

Locations:

- `action.yml:23`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image reference 'docker://lgohr/ecr-login-action:latest' with the immutable digest 'docker://lgohr/ecr-login-action@sha256:29302377ae8d5d9aecd8fabb79e060833812bd4d5ba41e6d8e5273b410c58903' in action.yml line 23. The original tag is preserved as a comment for readability.

