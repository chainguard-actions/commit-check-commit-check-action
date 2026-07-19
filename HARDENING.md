<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.11.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.11.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions or reusable workflows using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced ref is updated maliciously.

- `.github/workflows/commit-check.yml`: `uses: actions/checkout@v7` (tag, not SHA)
- `.github/workflows/pre-commit.yml`: `uses: commit-check/.github/.github/workflows/pre-commit.yml@main` (branch, not SHA)
- `.github/workflows/release-drafter.yml`: `uses: commit-check/.github/.github/workflows/release-drafter.yml@main` (branch, not SHA)

Locations:

- `.github/workflows/commit-check.yml:18`
- `.github/workflows/pre-commit.yml:10`
- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned workflow references by resolving their full 40-character commit SHAs: (1) actions/checkout@v7 → @9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0 in commit-check.yml; (2) commit-check/.github pre-commit and release-drafter reusable workflows both pinned to @fcbd5e3e6cd0c675d58191af2c27ed25ff04a274 (the current HEAD of main). Added a top-level permissions block to release-drafter.yml with contents: write (required for drafting releases) and pull-requests: read (minimal access for reading PR metadata used by release drafter).

