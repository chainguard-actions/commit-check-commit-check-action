<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.11.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.11.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Unpinned `uses:` references found. `actions/checkout@v7` uses a mutable tag instead of a full 40-character commit SHA, making it vulnerable to supply-chain attacks if the tag is moved.

Locations:

- `.github/workflows/commit-check.yml:17`

### unpinned-uses (severity: high)

Unpinned `uses:` references found. `commit-check/.github/.github/workflows/pre-commit.yml@main` uses a mutable branch ref (`main`) instead of a full 40-character commit SHA.

Locations:

- `.github/workflows/pre-commit.yml:11`

### unpinned-uses (severity: high)

Unpinned `uses:` references found. `commit-check/.github/.github/workflows/release-drafter.yml@main` uses a mutable branch ref (`main`) instead of a full 40-character commit SHA.

Locations:

- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

Workflow has no top-level `permissions:` key and the only job (`draft-release`) also has no `permissions:` key. Without explicit permissions, the job inherits the default (potentially broad) repository permissions.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings:
1. commit-check.yml: Pinned `actions/checkout@v7` to full SHA `3d3c42e5aac5ba805825da76410c181273ba90b1 # v7`
2. pre-commit.yml: Pinned reusable workflow `commit-check/.github/.github/workflows/pre-commit.yml@main` to full SHA `43874c1227544c6e5b7cf1e4718f6ec516cb23f9 # main`
3. release-drafter.yml: Pinned reusable workflow `commit-check/.github/.github/workflows/release-drafter.yml@main` to full SHA `43874c1227544c6e5b7cf1e4718f6ec516cb23f9 # main`
4. release-drafter.yml: Added top-level `permissions:` block with `contents: write` and `pull-requests: read` (minimal permissions for a release drafter workflow that needs to create/update draft releases)

