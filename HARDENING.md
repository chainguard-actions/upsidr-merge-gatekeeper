<!-- markdownlint-disable -->

# Hardening Report: upsidr--merge-gatekeeper/v1.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upsidr--merge-gatekeeper/v1.2.1** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Actions are referenced by mutable tag rather than a full 40-character commit SHA, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing references:
- actions/checkout@v2 (line 26)
- actions/setup-go@v1 (line 29)

Locations:

- `.github/workflows/build-ci.yaml:26`
- `.github/workflows/build-ci.yaml:29`

### unpinned-uses (severity: high)

Actions are referenced by mutable tag rather than a full 40-character commit SHA, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing references:
- actions/checkout@v2 (line 17)
- actions/setup-go@v3 (line 21)

Locations:

- `.github/workflows/documentation-ci.yaml:17`
- `.github/workflows/documentation-ci.yaml:21`

### unpinned-uses (severity: high)

Action is referenced by mutable tag rather than a full 40-character commit SHA, making the workflow vulnerable to supply-chain attacks if the tag is moved. Failing reference:
- actions/checkout@v2 (line 12)

Locations:

- `.github/workflows/merge-gatekeeper-latest.yaml:12`

### missing-permissions (severity: medium)

The workflow has no top-level `permissions:` key and the single job also has no `permissions:` key. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Add a top-level `permissions: {}` block and grant only the specific scopes required.

Locations:

- `.github/workflows/build-ci.yaml:1`

### missing-permissions (severity: medium)

The workflow has no top-level `permissions:` key and the single job also has no `permissions:` key. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Add a top-level `permissions: {}` block and grant only the specific scopes required.

Locations:

- `.github/workflows/documentation-ci.yaml:1`

### missing-permissions (severity: medium)

The workflow has no top-level `permissions:` key and the single job also has no `permissions:` key. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions. Add a top-level `permissions: {}` block and grant only the specific scopes required.

Locations:

- `.github/workflows/merge-gatekeeper-latest.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. build-ci.yaml: Pinned actions/checkout@v2 to SHA 0717577d45739eb3c851188b29f50ed6c0b2194e and actions/setup-go@v1 to SHA 0caeaed6fd66a828038c2da3c0f662a42862658f. Added top-level `permissions: {}`.

2. documentation-ci.yaml: Pinned actions/checkout@v2 to SHA 0717577d45739eb3c851188b29f50ed6c0b2194e and actions/setup-go@v3 to SHA be3c94b385c4f180051c996d336f57a34c397495. Added top-level `permissions: {}`.

3. merge-gatekeeper-latest.yaml: Pinned actions/checkout@v2 to SHA 0717577d45739eb3c851188b29f50ed6c0b2194e. Added top-level `permissions: {}`. The local action reference (`uses: ./`) was left unchanged as it is not a remote action reference.

