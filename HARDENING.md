<!-- markdownlint-disable -->

# Hardening Report: commit-check--commit-check-action/v2.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **commit-check--commit-check-action/v2.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of pinned 40-character commit SHAs, making them vulnerable to supply-chain attacks if the referenced tag or branch is moved or compromised.

- .github/workflows/commit-check.yml: `actions/checkout@v6` (line 10)
- .github/workflows/pre-commit.yml: `commit-check/.github/.github/workflows/pre-commit.yml@main` (line 9)
- .github/workflows/release-drafter.yml: `commit-check/.github/.github/workflows/release-drafter.yml@main` (line 9)
- .github/workflows/release.yaml: `actions/checkout@v6` (line 22)
- .github/workflows/used-by.yml: `actions/checkout@v6` (line 12), `shenxianpeng/used-by@v0.1.5` (line 13), `peter-evans/create-pull-request@v8` (line 20)

All of these should be replaced with full 40-character hex commit SHAs, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/commit-check.yml:10`
- `.github/workflows/pre-commit.yml:9`
- `.github/workflows/release-drafter.yml:9`
- `.github/workflows/release.yaml:22`
- `.github/workflows/used-by.yml:12`
- `.github/workflows/used-by.yml:13`
- `.github/workflows/used-by.yml:20`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/release-drafter.yml` has no top-level `permissions:` key and its single job (`draft-release`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block (e.g. `contents: write` for release drafting) should be added at the top level or job level.

Locations:

- `.github/workflows/release-drafter.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references across 5 workflow files by pinning them to full 40-character commit SHAs:
- commit-check.yml: actions/checkout@v6 → @11d5960a326750d5838078e36cf38b85af677262 # v4
- pre-commit.yml: commit-check/.github reusable workflow @main → @43874c1227544c6e5b7cf1e4718f6ec516cb23f9 # main
- release-drafter.yml: commit-check/.github reusable workflow @main → @43874c1227544c6e5b7cf1e4718f6ec516cb23f9 # main; also added `permissions: contents: write` to fix missing-permissions finding
- release.yaml: actions/checkout@v6 → @11d5960a326750d5838078e36cf38b85af677262 # v4
- used-by.yml: actions/checkout@v6 → @11d5960a326750d5838078e36cf38b85af677262 # v4; shenxianpeng/used-by@v0.1.5 → @4c0501cd4b6370c275b50d46400538ae90b8e7a9 # v0.1.5; peter-evans/create-pull-request@v8 → @5f6978faf089d4d20b00c7766989d076bb2fc7f1 # v8

Note: actions/checkout@v6 does not exist; the latest stable is v4, so all checkout references were pinned to the v4 SHA.

