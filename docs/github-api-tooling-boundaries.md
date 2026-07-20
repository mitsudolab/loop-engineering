# GitHub API tooling boundaries

This fork keeps GitHub integration narrow and explicit. Contributors should treat GitHub access as an opt-in capability for specific packages, not as a default behavior across the monorepo.

## Scope

- `@cobusgreyling/loop-audit` is the primary package allowed to fetch repository data from the GitHub API.
- `@cobusgreyling/loop-context` should prefer local files, exported summaries, or pre-fetched repository metadata instead of making direct GitHub calls.
- Tools that do not need GitHub data should not add token requirements, background syncing, or write-capable API flows.

## Token configuration

Use `GITHUB_TOKEN` only when a tool explicitly documents GitHub API support.

Preferred token choices:
- fine-grained personal access token with read-only repository access
- GitHub App installation token with read-only repository access

Classic PAT fallback:
- public repositories: `public_repo`
- private repositories: `repo`

Token handling rules:
- keep tokens in environment variables only
- do not commit tokens, fixtures, or screenshots that expose credentials
- do not persist tokens in generated files, caches, or package metadata
- do not print token values in logs, examples, or test snapshots

## Outbound API expectations

When GitHub access is enabled, outbound traffic should stay read-only and limited to repository analysis needs.

Allowed patterns:
- resolve `owner/repo` input or a repository URL to repository metadata
- read repository contents only when analysis requires it
- derive findings, summaries, or normalized context from GitHub responses

Out-of-bounds patterns:
- pushing commits or tags
- opening or editing pull requests, issues, or comments
- mutating workflows, settings, secrets, or webhooks
- storing full raw API payloads when a derived summary is sufficient
- expanding token scope requirements for convenience

## Contributor checklist

Before adding or changing GitHub-facing behavior:
- confirm whether the package is an API consumer or a local-context consumer
- document the exact environment variable and minimum permissions required
- list accepted inputs and forbidden actions next to the package
- avoid introducing new write paths, auth storage, or silent network behavior
- prefer passing pre-fetched audit output into downstream tools instead of re-fetching from GitHub

## Package manifests

Package-local boundary manifests live next to the affected tools:
- `tools/loop-audit/github-api-boundary.json`
- `tools/loop-context/github-api-boundary.json`

If a future package needs direct GitHub access, add an equivalent manifest and update this document before changing runtime behavior.
