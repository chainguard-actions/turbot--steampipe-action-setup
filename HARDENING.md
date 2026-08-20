<!-- markdownlint-disable -->

# Hardening Report: turbot--steampipe-action-setup/v1.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **turbot--steampipe-action-setup/v1.5.0** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in check-dist.yml use mutable tag refs instead of pinned full-length commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved. Failing references: `actions/checkout@v4` (line 23), `actions/setup-node@v3` (line 26), `actions/upload-artifact@v3` (line 46).

Locations:

- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:26`
- `.github/workflows/check-dist.yml:46`

### unpinned-uses (severity: high)

All `uses:` references in codeql-analysis.yml use mutable tag refs instead of pinned full-length commit SHAs. Failing references: `actions/checkout@v4` (line 39), `github/codeql-action/init@v2` (line 43), `github/codeql-action/autobuild@v2` (line 55), `github/codeql-action/analyze@v2` (line 65).

Locations:

- `.github/workflows/codeql-analysis.yml:39`
- `.github/workflows/codeql-analysis.yml:43`
- `.github/workflows/codeql-analysis.yml:55`
- `.github/workflows/codeql-analysis.yml:65`

### unpinned-uses (severity: high)

All `uses:` references in test.yml use mutable tag refs instead of pinned full-length commit SHAs. Failing references: `actions/checkout@v4` (line 14, line 21).

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:21`

### missing-permissions (severity: medium)

check-dist.yml has no top-level `permissions:` key and the `check-dist` job also has no job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository default (typically `write` for all scopes on private repos), granting broader access than necessary.

Locations:

- `.github/workflows/check-dist.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level `permissions:` key and neither the `units` job nor the `test` job has a job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository default, granting broader access than necessary.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 findings across 3 workflow files:

1. check-dist.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610, actions/upload-artifact@v3 → @ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5. Added top-level `permissions: contents: read`.

2. codeql-analysis.yml: Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262, github/codeql-action/init@v2, autobuild@v2, and analyze@v2 → all @b8d3b6e8af63cde30bdc382c0bc28114f4346c88. This file already had job-level permissions so no permissions block was added.

3. test.yml: Pinned both instances of actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262. Added top-level `permissions: contents: read`.

