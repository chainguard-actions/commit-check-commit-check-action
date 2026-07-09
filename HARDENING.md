<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.11.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **commit-check--commit-check-action/v2.11.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions/reusable workflows by mutable tags or branch names instead of pinned full-length SHA commits:
- .github/workflows/commit-check.yml: `uses: actions/checkout@v7` (tag, not SHA)
- .github/workflows/pre-commit.yml: `uses: commit-check/.github/.github/workflows/pre-commit.yml@main` (branch, not SHA)
- .github/workflows/release-drafter.yml: `uses: commit-check/.github/.github/workflows/release-drafter.yml@main` (branch, not SHA)

Locations:

- `.github/workflows/commit-check.yml:19`
- `.github/workflows/pre-commit.yml:11`
- `.github/workflows/release-drafter.yml:11`

### missing-permissions (severity: medium)

The workflow file release-drafter.yml has no top-level `permissions:` key and its only job (`draft-release`) also has no job-level `permissions:` key. This means the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned action references: (1) actions/checkout@v7 → pinned to SHA 9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0 in commit-check.yml; (2) commit-check/.github reusable workflow @main → pinned to SHA 4a68c86b2edf1d31c5752bfeaf66b5425df8352e in both pre-commit.yml and release-drafter.yml. Added top-level permissions block to release-drafter.yml with contents: write and pull-requests: read (minimal permissions needed for a release drafter workflow).

