# GitHub API Repository Metadata Ingestion Contract

This fork prefers a single safe-default ingestion mode for repository audit and context workflows.

## Preferred input

- Repository coordinate: `owner/repo`
- Authentication: token-backed GitHub API access
- Execution model: remote metadata fetch only

## Required behavior

Tools that participate in repository audit or context collection should treat the GitHub API path as the default and keep the request surface narrow.

- Accept `owner/repo` instead of assuming a local checkout.
- Use a GitHub token only for authenticated API access.
- Fetch only repository metadata needed for analysis.
- Keep the root workspace positioned as documentation and package coordination, not as a local execution entrypoint.

## Metadata scope

The intended scope is repository-level metadata that supports readiness and context analysis without cloning or modifying the target repository.

Examples of acceptable metadata include:

- repository name and visibility
- default branch name
- description, topics, and homepage
- archived, disabled, and fork status
- primary language and language summary
- open issues and pull request counts when exposed through repository metadata queries
- stargazer, watcher, and fork counts
- recent update timestamps

## Non-goals

This contract does not authorize broader inspection paths.

- No local checkout execution as the default path
- No generated asset updates tied to remote inspection
- No non-GitHub integrations for repository ingestion
- No token storage changes or credential broadening
- No mutation of the target repository during analysis

## Package-level guidance

Package metadata and documentation should describe this contract consistently.

- `@cobusgreyling/loop-audit` should present `owner/repo` plus token-backed GitHub API access as its primary ingestion mode.
- `@cobusgreyling/loop-context` should describe remote repository metadata as the preferred context input for audit-oriented flows in this fork.
- Future README updates should point users to this contract instead of implying local execution is required.
