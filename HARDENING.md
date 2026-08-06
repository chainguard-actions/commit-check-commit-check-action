<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.13.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.13.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Three workflow files reference actions/workflows by mutable tag or branch name instead of a full 40-character commit SHA, making them vulnerable to supply-chain attacks if the referenced ref is force-pushed or hijacked:
- `.github/workflows/commit-check.yml`: `uses: actions/checkout@v7.0.1` (version tag, not a SHA)
- `.github/workflows/pre-commit.yml`: `uses: commit-check/.github/.github/workflows/pre-commit.yml@main` (branch ref)
- `.github/workflows/release-drafter.yml`: `uses: commit-check/.github/.github/workflows/release-drafter.yml@main` (branch ref)

Locations:

- `.github/workflows/commit-check.yml:17`
- `.github/workflows/pre-commit.yml:11`
- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. Without an explicit permissions block the workflow inherits the repository's default token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned-uses findings by resolving mutable tags/branches to full 40-character commit SHAs: actions/checkout@v7.0.1 → SHA 3d3c42e5aac5ba805825da76410c181273ba90b1, and both commit-check/.github reusable workflows @main → SHA 977da5e776d047ec52fc4950909dd0e08e411f32. Added a top-level permissions block to release-drafter.yml with contents: write and pull-requests: write, which are the minimum permissions required for a release drafter workflow to create/update draft releases and manage PR labels.

