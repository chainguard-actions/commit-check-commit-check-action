<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.7.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.7.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions and reusable workflows using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved or compromised. Failing references:
- commit-check.yml: `actions/checkout@v6`
- pre-commit.yml: `commit-check/.github/.github/workflows/pre-commit.yml@main`
- release-drafter.yml: `commit-check/.github/.github/workflows/release-drafter.yml@main`
- release.yaml: `actions/checkout@v6`
- used-by.yml: `actions/checkout@v6`, `shenxianpeng/used-by@v0.1.5`, `peter-evans/create-pull-request@v8`

Locations:

- `.github/workflows/commit-check.yml:12`
- `.github/workflows/pre-commit.yml:11`
- `.github/workflows/release-drafter.yml:11`
- `.github/workflows/release.yaml:18`
- `.github/workflows/used-by.yml:13`
- `.github/workflows/used-by.yml:14`
- `.github/workflows/used-by.yml:18`

### missing-permissions (severity: medium)

The workflow file `release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A top-level or job-level `permissions:` block with minimal required scopes should be added.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving full 40-character commit SHAs via lookup_action_sha: actions/checkout@v6→d23441a48e516b6c34aea4fa41551a30e30af803, shenxianpeng/used-by@v0.1.5→4c0501cd4b6370c275b50d46400538ae90b8e7a9, peter-evans/create-pull-request@v8→5f6978faf089d4d20b00c7766989d076bb2fc7f1, commit-check/.github@main→303c9315a9d8272aaf72bae4cac7c69da7263b1f. Added top-level permissions block (contents: write, pull-requests: write) to release-drafter.yml to satisfy the missing-permissions finding.

