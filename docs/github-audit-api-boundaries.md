# GitHub API-only audit boundaries

This fork supports a single repository-audit path for contributors who only have GitHub API access.

## Supported audit workflow

1. Provide the target repository as `owner/repo` to `@cobusgreyling/loop-audit`.
2. Provide a GitHub token explicitly through the environment variable `GITHUB_TOKEN`.
3. Fetch only the repository metadata and contents that are required for readiness and boundary review.
4. Pass the resulting audit context to follow-on tools such as `@cobusgreyling/goal-audit`.

This keeps repository analysis inside an API-backed contract and avoids depending on local execution inside the target repository.

## Token contract

- Required environment variable: `GITHUB_TOKEN`
- Preferred token types:
  - fine-grained personal access token
  - GitHub App installation token
- Minimum permissions:
  - repository metadata: read
  - repository contents: read only when the audit step needs file-level inspection

If a classic personal access token must be used, keep the scope as small as possible:

- public repositories: `public_repo`
- private repositories: `repo`

Do not persist tokens in files, generated artifacts, prompts, or package metadata.

## Tool boundaries in this fork

### `@cobusgreyling/loop-audit`

`@cobusgreyling/loop-audit` is the only package in this fork that should directly depend on GitHub API authentication for repository auditing.

It may:

- accept `owner/repo` identifiers
- read `GITHUB_TOKEN` from the environment
- call GitHub APIs needed for repository readiness review
- emit structured audit context for downstream analysis

It must not:

- require contributors to run code inside the target repository
- store GitHub credentials locally
- broaden token permissions beyond the documented audit need

### `@cobusgreyling/goal-audit`

`@cobusgreyling/goal-audit` is downstream-only.

It may:

- consume a local working tree
- consume pre-fetched audit metadata
- reuse audit context produced by `@cobusgreyling/loop-audit`

It must not:

- request `GITHUB_TOKEN` directly
- add new GitHub authentication requirements
- expand the fork's permission surface for goal checks

## Contributor guidance for forks

When using this fork safely:

- set `GITHUB_TOKEN` only for the audit step that needs GitHub API access
- prefer ephemeral or installation-scoped credentials over long-lived broad tokens
- keep downstream tools environment-agnostic unless they are the documented GitHub API entrypoint
- treat audit outputs as derived context, not as a place to store secrets

## Non-goals for this contract

This document does not authorize:

- local execution hooks inside third-party repositories
- automatic credential discovery from developer machines
- hidden fallback authentication paths
- expansion of runtime engine behavior beyond repository analysis metadata collection
