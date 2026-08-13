<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.15.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.15.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions or reusable workflows by mutable tags or branch names instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks.

- `.github/workflows/commit-check.yml`: `uses: actions/checkout@v7.0.1` (tag, not SHA)
- `.github/workflows/pre-commit.yml`: `uses: commit-check/.github/.github/workflows/pre-commit.yml@main` (branch name)
- `.github/workflows/release-drafter.yml`: `uses: commit-check/.github/.github/workflows/release-drafter.yml@main` (branch name)

Locations:

- `.github/workflows/commit-check.yml:19`
- `.github/workflows/pre-commit.yml:12`
- `.github/workflows/release-drafter.yml:12`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its single job (which calls a reusable workflow via `uses:`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned-uses findings by resolving full commit SHAs: actions/checkout@v7.0.1 → @3d3c42e5aac5ba805825da76410c181273ba90b1 in commit-check.yml; commit-check/.github reusable workflows @main → @8104e2a764949a4c021ab0380bd08c6af7c047a5 in both pre-commit.yml and release-drafter.yml. Added `permissions: {}` top-level block to release-drafter.yml to fix the missing-permissions finding. Original tag/branch names preserved as inline comments.

