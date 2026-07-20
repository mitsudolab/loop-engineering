# GitHub Audit API Boundaries

This fork keeps the audit and goal-check flow constrained to a documented GitHub API-only contract.

## Tools in scope

- `@cobusgreyling/loop-audit`: the only audit package in this fork that is expected to authenticate to the GitHub API.
- `@cobusgreyling/goal-audit`: consumes a local working tree or pre-fetched audit outputs and must not require direct GitHub authentication.

## Required environment variables

### `@cobusgreyling/loop-audit`

- Required: `GITHUB_TOKEN`
- Expected token types:
  - fine-grained personal access token
  - GitHub App installation token
- Classic PAT fallback scopes:
  - public repositories: `public_repo`
  - private repositories: `repo`

### `@cobusgreyling/goal-audit`

- Required: none
- Boundary: do not prompt for, store, or emit GitHub credentials directly.

## Minimum repository permissions

For fine-grained PATs and GitHub App installation tokens, prefer the smallest repository permissions that let the audit complete:

- `metadata`: `read`
- `contents`: `read` only when repository content inspection is required for findings

## API-only boundary

The audit flow in this fork is intentionally read-oriented.

Allowed:
- accept `owner/repo` style input for GitHub-backed audits
- fetch repository metadata needed to compute readiness findings
- derive summaries instead of persisting full raw API payloads when summaries are sufficient

Not allowed by contract:
- pushing commits or tags
- creating or updating pull requests, issues, comments, or reviews
- modifying workflows, checks, or repository settings
- storing raw GitHub tokens or authorization headers
- collecting unnecessary personal data

## Goal-check handoff

When `@cobusgreyling/goal-audit` is used in the same operator flow:

- `@cobusgreyling/loop-audit` may provide repository-derived context
- `@cobusgreyling/goal-audit` should work from local files or pre-fetched metadata
- the goal-check step should not expand the GitHub permission surface beyond the audit contract above
