<!-- markdownlint-disable -->

# Hardening Report: upsidr--merge-gatekeeper/v1.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upsidr--merge-gatekeeper/v1.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHA hashes. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Failing references:
- .github/workflows/build-ci.yaml: `uses: actions/checkout@v2` and `uses: actions/setup-go@v1`
- .github/workflows/documentation-ci.yaml: `uses: actions/checkout@v2`
- .github/workflows/merge-gatekeeper-latest.yaml: `uses: actions/checkout@v2`
All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/build-ci.yaml:27`
- `.github/workflows/build-ci.yaml:30`
- `.github/workflows/documentation-ci.yaml:14`
- `.github/workflows/merge-gatekeeper-latest.yaml:12`

### missing-permissions (severity: medium)

None of the three workflow files declare a top-level `permissions:` block, and no job within them declares job-level permissions either. Without explicit permissions, workflows run with the default (often broad) token permissions, violating the principle of least privilege. Each workflow should declare the minimal required permissions (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/build-ci.yaml:1`
- `.github/workflows/documentation-ci.yaml:1`
- `.github/workflows/merge-gatekeeper-latest.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files: (1) Pinned all unpinned action references to full 40-character commit SHAs - actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e in all three files, actions/setup-go@v1 → @0caeaed6fd66a828038c2da3c0f662a42862658f in build-ci.yaml. (2) Added top-level permissions blocks to all three workflow files with minimal required permissions: build-ci.yaml and documentation-ci.yaml get 'contents: read', merge-gatekeeper-latest.yaml gets 'contents: read', 'statuses: read', and 'checks: read' to support the merge gatekeeper action's needs.

