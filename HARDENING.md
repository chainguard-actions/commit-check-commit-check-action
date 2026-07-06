<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action--/v2.11.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **commit-check--commit-check-action--/v2.11.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses actions/checkout@v7, which is pinned to a mutable tag rather than a full 40-character commit SHA. This allows supply-chain attacks if the tag is moved.

Locations:

- `.github/workflows/commit-check.yml:18`

### unpinned-uses (severity: high)

The workflow uses commit-check/.github/.github/workflows/pre-commit.yml@main, which is pinned to a mutable branch name rather than a full 40-character commit SHA. This allows supply-chain attacks if the branch is updated.

Locations:

- `.github/workflows/pre-commit.yml:11`

### unpinned-uses (severity: high)

The workflow uses commit-check/.github/.github/workflows/release-drafter.yml@main, which is pinned to a mutable branch name rather than a full 40-character commit SHA. This allows supply-chain attacks if the branch is updated.

Locations:

- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

The workflow file has no top-level permissions block and the single job (draft-release) also has no job-level permissions block. Without explicit permissions, the workflow inherits the default repository permissions, which may be overly broad.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings:
1. commit-check.yml: Pinned actions/checkout@v7 to full SHA 9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0 # v7
2. pre-commit.yml: Pinned reusable workflow commit-check/.github/.github/workflows/pre-commit.yml@main to full SHA 57e126f26f0e9efaf4de8960d60612b0756b7557 # main
3. release-drafter.yml: Pinned reusable workflow commit-check/.github/.github/workflows/release-drafter.yml@main to full SHA 57e126f26f0e9efaf4de8960d60612b0756b7557 # main
4. release-drafter.yml: Added top-level permissions block with contents: write and pull-requests: write (minimum permissions needed for a release drafter to create/update draft releases and manage PR labels)

