<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.13.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.13.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Three workflow files reference external actions or reusable workflows using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced ref is updated maliciously.

- `.github/workflows/commit-check.yml`: `uses: actions/checkout@v7.0.1` (tag, not SHA)
- `.github/workflows/pre-commit.yml`: `uses: commit-check/.github/.github/workflows/pre-commit.yml@main` (branch)
- `.github/workflows/release-drafter.yml`: `uses: commit-check/.github/.github/workflows/release-drafter.yml@main` (branch)

Locations:

- `.github/workflows/commit-check.yml:17`
- `.github/workflows/pre-commit.yml:9`
- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`, which calls a reusable workflow) also has no job-level `permissions:` key. Without explicit permissions, the workflow runs with the default token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned-uses findings by replacing mutable refs with full 40-character commit SHAs: actions/checkout@v7.0.1 → @3d3c42e5aac5ba805825da76410c181273ba90b1 in commit-check.yml; commit-check/.github reusable workflows @main → @977da5e776d047ec52fc4950909dd0e08e411f32 in both pre-commit.yml and release-drafter.yml. Added top-level `permissions: contents: read` to release-drafter.yml to fix the missing-permissions finding. Original tag/branch names preserved as inline comments.

