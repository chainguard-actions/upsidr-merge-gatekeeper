<!-- markdownlint-disable -->

# Hardening Report: upsidr--merge-gatekeeper/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upsidr--merge-gatekeeper/v1.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag refs instead of full 40-character commit SHA pins. If the tag is moved (e.g. by a supply-chain compromise), the workflow will silently execute different code. Affected references: actions/checkout@v2 and actions/setup-go@v1 in build-ci.yaml; actions/checkout@v2 in documentation-ci.yaml; actions/checkout@v2 in merge-gatekeeper-latest.yaml.

Locations:

- `.github/workflows/build-ci.yaml:23`
- `.github/workflows/build-ci.yaml:26`
- `.github/workflows/documentation-ci.yaml:14`
- `.github/workflows/merge-gatekeeper-latest.yaml:12`

### missing-permissions (severity: medium)

None of the workflow files declare a top-level or job-level `permissions:` block. Without explicit permissions, workflows run with the default token permissions (which may include write access to repository contents, pull requests, etc.), violating the principle of least privilege. All three workflow files are affected: build-ci.yaml, documentation-ci.yaml, and merge-gatekeeper-latest.yaml.

Locations:

- `.github/workflows/build-ci.yaml:1`
- `.github/workflows/documentation-ci.yaml:1`
- `.github/workflows/merge-gatekeeper-latest.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files: (1) Pinned actions/checkout@v2 to SHA 0717577d45739eb3c851188b29f50ed6c0b2194e and actions/setup-go@v1 to SHA 0caeaed6fd66a828038c2da3c0f662a42862658f in all affected locations, preserving the original tag in a comment. (2) Added top-level `permissions: {}` to all three workflow files to deny all permissions by default, and added job-level `permissions: contents: read` (minimum needed for checkout). The merge-gatekeeper job also gets `statuses: read` since it uses GITHUB_TOKEN to check PR statuses.

