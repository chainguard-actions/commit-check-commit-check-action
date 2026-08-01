<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.12.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.12.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions or reusable workflows using mutable tags or branch names instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks:
- `.github/workflows/commit-check.yml` line 19: `uses: actions/checkout@v7.0.1` (tag, not SHA)
- `.github/workflows/pre-commit.yml` line 9: `uses: commit-check/.github/.github/workflows/pre-commit.yml@main` (branch ref)
- `.github/workflows/release-drafter.yml` line 10: `uses: commit-check/.github/.github/workflows/release-drafter.yml@main` (branch ref)

Locations:

- `.github/workflows/commit-check.yml:19`
- `.github/workflows/pre-commit.yml:9`
- `.github/workflows/release-drafter.yml:10`

### missing-permissions (severity: medium)

`.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its only job (`draft-release`) also has no `permissions:` key. Without explicit permissions, the job inherits the default token permissions (which may be broad), violating the principle of least privilege.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three unpinned-uses findings by replacing mutable tag/branch references with full 40-character commit SHAs (preserving original refs as comments): (1) actions/checkout@v7.0.1 → @3d3c42e5aac5ba805825da76410c181273ba90b1 in commit-check.yml; (2) commit-check/.github pre-commit.yml@main → @2ba725751f2fe99f0679c168dcb9d1c75ff41138 in pre-commit.yml; (3) commit-check/.github release-drafter.yml@main → @2ba725751f2fe99f0679c168dcb9d1c75ff41138 in release-drafter.yml. Fixed missing-permissions by adding a top-level permissions block to release-drafter.yml with contents: write (required to create/update draft releases) and pull-requests: read (minimal access for release drafter to read PR metadata).

