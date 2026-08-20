<!-- markdownlint-disable -->

# Hardening Report: upsidr--merge-gatekeeper/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **upsidr--merge-gatekeeper/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files use tag-based (mutable) `uses:` references instead of pinned full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

- `.github/workflows/build-ci.yaml`: `actions/checkout@v2` (line 22), `actions/setup-go@v1` (line 25)
- `.github/workflows/documentation-ci.yaml`: `actions/checkout@v2` (line 14), `actions/setup-go@v3` (line 18)
- `.github/workflows/merge-gatekeeper-latest.yaml`: `actions/checkout@v2` (line 11)

All should be replaced with their full SHA digest equivalents, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/build-ci.yaml:22`
- `.github/workflows/build-ci.yaml:25`
- `.github/workflows/documentation-ci.yaml:14`
- `.github/workflows/documentation-ci.yaml:18`
- `.github/workflows/merge-gatekeeper-latest.yaml:11`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and none of the individual jobs define job-level `permissions:` blocks. Without explicit permissions, workflows run with the default (often broad) token permissions, violating the principle of least privilege.

- `.github/workflows/build-ci.yaml`: no top-level or job-level permissions
- `.github/workflows/documentation-ci.yaml`: no top-level or job-level permissions
- `.github/workflows/merge-gatekeeper-latest.yaml`: no top-level or job-level permissions

Each workflow should declare minimal required permissions, e.g. `permissions: read-all` at the top level and then grant only what each job needs.

Locations:

- `.github/workflows/build-ci.yaml:1`
- `.github/workflows/documentation-ci.yaml:1`
- `.github/workflows/merge-gatekeeper-latest.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references across 3 workflow files by replacing tag-based references with full 40-character SHA commit hashes (preserving the original tag in a comment). Added top-level `permissions: {}` to all 3 workflow files to deny all permissions by default, and added minimal job-level permissions (contents: read for build and documentation jobs; contents: read + pull-requests: read for the merge-gatekeeper job which uses GITHUB_TOKEN to interact with PRs).

