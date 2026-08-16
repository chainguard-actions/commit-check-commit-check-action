<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.6.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.6.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions and reusable workflows using mutable tags or branch names instead of pinned 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is updated with malicious code.

Failing references:
- `.github/workflows/commit-check.yml`: `actions/checkout@v6` (tag)
- `.github/workflows/pre-commit.yml`: `commit-check/.github/.github/workflows/pre-commit.yml@main` (branch)
- `.github/workflows/release-drafter.yml`: `commit-check/.github/.github/workflows/release-drafter.yml@main` (branch)
- `.github/workflows/release.yaml`: `actions/checkout@v6` (tag)
- `.github/workflows/used-by.yml`: `actions/checkout@v6` (tag), `shenxianpeng/used-by@v0.1.5` (tag), `peter-evans/create-pull-request@v8` (tag)

Locations:

- `.github/workflows/commit-check.yml:10`
- `.github/workflows/pre-commit.yml:9`
- `.github/workflows/release-drafter.yml:11`
- `.github/workflows/release.yaml:22`
- `.github/workflows/used-by.yml:14`
- `.github/workflows/used-by.yml:15`
- `.github/workflows/used-by.yml:20`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository default token permissions, which may be overly broad. A top-level or job-level `permissions:` block with minimal required scopes should be added.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all 7 unpinned action/workflow references to full 40-character commit SHAs with original tags preserved as comments: actions/checkout@v6→d23441a, shenxianpeng/used-by@v0.1.5→4c0501c, peter-evans/create-pull-request@v8→5f6978f, and both commit-check/.github reusable workflows @main→43874c1. Added `permissions: {}` top-level block to release-drafter.yml to satisfy the missing-permissions finding.

