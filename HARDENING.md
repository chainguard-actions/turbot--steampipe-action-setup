<!-- markdownlint-disable -->

# Hardening Report: turbot--steampipe-action-setup/v1.5.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **turbot--steampipe-action-setup/v1.5.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

.github/workflows/check-dist.yml:
  - uses: actions/checkout@v4
  - uses: actions/setup-node@v3
  - uses: actions/upload-artifact@v3

.github/workflows/codeql-analysis.yml:
  - uses: actions/checkout@v4
  - uses: github/codeql-action/init@v2
  - uses: github/codeql-action/autobuild@v2
  - uses: github/codeql-action/analyze@v2

.github/workflows/test.yml:
  - uses: actions/checkout@v4 (appears twice)

All of these should be pinned to their full commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:40`
- `.github/workflows/codeql-analysis.yml:35`
- `.github/workflows/codeql-analysis.yml:39`
- `.github/workflows/codeql-analysis.yml:47`
- `.github/workflows/codeql-analysis.yml:56`
- `.github/workflows/test.yml:11`
- `.github/workflows/test.yml:17`

### missing-permissions (severity: medium)

check-dist.yml has no top-level `permissions:` key and its only job (`check-dist`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). A minimal permissions block such as `permissions: contents: read` should be added.

Locations:

- `.github/workflows/check-dist.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level `permissions:` key and neither of its jobs (`units`, `test`) has a job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal permissions block such as `permissions: contents: read` should be added.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings across three workflow files:

1. **unpinned-uses** — Pinned all action references to full commit SHAs:
   - `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4` (in all three files)
   - `actions/setup-node@v3` → `@3235b876344d2a9aa001b8d1453c930bba69e610 # v3` (check-dist.yml)
   - `actions/upload-artifact@v3` → `@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3` (check-dist.yml)
   - `github/codeql-action/init@v2` → `@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (codeql-analysis.yml)
   - `github/codeql-action/autobuild@v2` → `@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (codeql-analysis.yml)
   - `github/codeql-action/analyze@v2` → `@b8d3b6e8af63cde30bdc382c0bc28114f4346c88 # v2` (codeql-analysis.yml)

2. **missing-permissions** — Added top-level `permissions: contents: read` to both `check-dist.yml` and `test.yml`. The `codeql-analysis.yml` already had appropriate job-level permissions (actions: read, contents: read, security-events: write) so no change was needed there.

