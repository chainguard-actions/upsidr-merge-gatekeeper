<!-- markdownlint-disable -->

# Hardening Report: upsidr--merge-gatekeeper/v1.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upsidr--merge-gatekeeper/v1.1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the tag is moved or the upstream action is compromised. Failing references: build-ci.yaml uses `actions/checkout@v2` and `actions/setup-go@v1`; documentation-ci.yaml uses `actions/checkout@v2`; merge-gatekeeper-latest.yaml uses `actions/checkout@v2`. All should be pinned to a full SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`).

Locations:

- `.github/workflows/build-ci.yaml:26`
- `.github/workflows/build-ci.yaml:30`
- `.github/workflows/documentation-ci.yaml:16`
- `.github/workflows/merge-gatekeeper-latest.yaml:14`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no job within them defines job-level permissions either. Without explicit permissions, workflows run with the default token permissions (which may be overly broad, e.g. `write` on `contents`). Each workflow should declare minimal required permissions, e.g. `permissions: read-all` or specific scopes such as `contents: read`.

Locations:

- `.github/workflows/build-ci.yaml:1`
- `.github/workflows/documentation-ci.yaml:1`
- `.github/workflows/merge-gatekeeper-latest.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 unpinned action references by pinning to full SHA commits with tag comments: actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e and actions/setup-go@v1 → @0caeaed6fd66a828038c2da3c0f662a42862658f. Added top-level permissions blocks to all 3 workflow files with minimal required permissions: build-ci.yaml and documentation-ci.yaml get 'contents: read'; merge-gatekeeper-latest.yaml gets 'contents: read', 'statuses: read', and 'pull-requests: read' to support the merge gatekeeper action's functionality.

