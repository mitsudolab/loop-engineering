# GitHub API authentication guidance for audit tools

This repository favors GitHub API-only audit and context workflows for tools that inspect repository readiness without requiring a local checkout of the target repository.

## Environment variable

- Use `GITHUB_TOKEN` for authenticated API access.
- Do not print, persist, or commit token values.
- Do not include raw `Authorization` headers in logs, snapshots, issue bodies, or generated artifacts.

## Preferred token types

Use the least-privilege credential that can access the target repository:

1. Fine-grained personal access token
2. GitHub App installation token
3. Classic personal access token only when the finer-grained options are not available

## Minimum repository permissions

For GitHub-backed audit commands, start with:

- `Metadata: Read`
- `Contents: Read` only when the command needs repository file or default-branch content access

For public repositories, metadata-only access is often enough for repository-level checks. Add `Contents: Read` only when a command explicitly needs file-backed context.

For private repositories, use the same least-privilege model and grant access only to the specific repository being audited.

## Classic token fallback

If a classic personal access token must be used, prefer the smallest scope that matches the repository visibility:

- Public repositories: `public_repo`
- Private repositories: `repo`

Avoid adding unrelated scopes such as workflow administration, package publishing, organization administration, or user account write access.

## Rate-limit expectations

Authenticated GitHub API usage has a much higher primary rate limit than unauthenticated usage, but throttling can still occur.

- Unauthenticated requests are typically limited to about 60 requests per hour.
- Authenticated requests are typically limited to about 5,000 requests per hour per token.
- Secondary rate limits can still apply during bursts, concurrent scans, or repeated polling.

Operational guidance:

- Prefer authenticated requests for repeatable audit runs.
- Reuse fetched metadata when derived findings are sufficient.
- Batch repository inspection conservatively.
- Back off and retry when GitHub signals throttling.
- Avoid collecting full raw payloads when summaries or derived findings are enough.

## Data-handling contract

Audit tooling should not persist or emit:

- raw GitHub tokens
- authorization headers
- full raw API payloads when derived findings are sufficient
- unnecessary personal data

## Scope of this guidance

This document clarifies how contributors should authenticate GitHub-backed audit commands. It does not change runtime behavior, add new token storage, or require local execution against the target repository.