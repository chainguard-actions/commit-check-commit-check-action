<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.10.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.10.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions/reusable workflows using mutable tags or branch names instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to a malicious commit.

Failing references:
- `.github/workflows/commit-check.yml` line 18: `actions/checkout@v7` (tag)
- `.github/workflows/pre-commit.yml` line 8: `commit-check/.github/.github/workflows/pre-commit.yml@main` (branch)
- `.github/workflows/release-drafter.yml` line 10: `commit-check/.github/.github/workflows/release-drafter.yml@main` (branch)
- `.github/workflows/release.yaml` line 19: `actions/checkout@v7` (tag)
- `.github/workflows/used-by.yml` line 15: `actions/checkout@v7` (tag)
- `.github/workflows/used-by.yml` line 16: `shenxianpeng/used-by@v0.1.5` (tag)

Note: `peter-evans/create-pull-request@5f6978faf089d4d20b00c7766989d076bb2fc7f1` in used-by.yml is correctly pinned to a SHA.

Locations:

- `.github/workflows/commit-check.yml:18`
- `.github/workflows/pre-commit.yml:8`
- `.github/workflows/release-drafter.yml:10`
- `.github/workflows/release.yaml:19`
- `.github/workflows/used-by.yml:15`
- `.github/workflows/used-by.yml:16`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal permissions block (e.g. `permissions: contents: write` or `permissions: {}`) should be added.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action/workflow references by resolving them to full 40-character commit SHAs using lookup_action_sha: actions/checkout@v7 → SHA 3d3c42e5..., shenxianpeng/used-by@v0.1.5 → SHA 4c0501cd..., and commit-check/.github@main → SHA 43874c12.... Added a top-level `permissions: contents: write` block to release-drafter.yml to satisfy the missing-permissions finding (contents: write is the minimal permission needed for a release drafter to create/update draft releases).

