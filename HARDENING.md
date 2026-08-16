<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.8.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

actions/checkout is referenced at the mutable tag @v6 instead of a pinned 40-character SHA digest. This allows supply-chain attacks if the tag is moved.

Locations:

- `.github/workflows/commit-check.yml:10`
- `.github/workflows/release.yaml:22`
- `.github/workflows/used-by.yml:13`

### unpinned-uses (severity: high)

Reusable workflow referenced at the mutable branch ref @main instead of a pinned 40-character SHA digest: commit-check/.github/.github/workflows/pre-commit.yml@main

Locations:

- `.github/workflows/pre-commit.yml:10`

### unpinned-uses (severity: high)

Reusable workflow referenced at the mutable branch ref @main instead of a pinned 40-character SHA digest: commit-check/.github/.github/workflows/release-drafter.yml@main

Locations:

- `.github/workflows/release-drafter.yml:12`

### unpinned-uses (severity: high)

Third-party actions referenced at mutable version tags instead of pinned 40-character SHA digests: shenxianpeng/used-by@v0.1.5 and peter-evans/create-pull-request@v8.

Locations:

- `.github/workflows/used-by.yml:14`
- `.github/workflows/used-by.yml:18`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and its only job (draft-release) delegates to a reusable workflow via `uses:` with no job-level `permissions:` key either. Without an explicit permissions block the job inherits the default (potentially write) token permissions.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action/workflow references to full 40-character SHA digests: actions/checkout@v6 → df4cb1c069e1874edd31b4311f1884172cec0e10 (in commit-check.yml, release.yaml, used-by.yml); shenxianpeng/used-by@v0.1.5 → 4c0501cd4b6370c275b50d46400538ae90b8e7a9 (used-by.yml); peter-evans/create-pull-request@v8 → 5f6978faf089d4d20b00c7766989d076bb2fc7f1 (used-by.yml); commit-check/.github reusable workflows @main → fcbd5e3e6cd0c675d58191af2c27ed25ff04a274 (pre-commit.yml and release-drafter.yml). Added `permissions: {}` top-level block to release-drafter.yml to prevent inheriting default write token permissions.

