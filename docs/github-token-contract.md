# GitHub Token Contract

This fork keeps GitHub-facing CLI behavior intentionally narrow for repository audit and context workflows.

## Applies to

- `@cobusgreyling/loop-audit`
- `@cobusgreyling/loop-context`

These tools are expected to operate in a GitHub API-only mode for repository analysis. They should accept repository coordinates such as `owner/repo`, use token-backed GitHub API access, and avoid local checkout execution by default.

## Required token input

- Preferred environment variable: `GITHUB_TOKEN`
- Fine-grained personal access token is preferred
- Repository access should be limited to the target repository whenever possible

## Minimum permissions

Use the smallest token that can satisfy metadata collection.

- `Metadata: Read` is required
- `Contents: Read` is allowed only when a tool needs repository file or tree metadata to complete analysis
- Do not require write permissions for the audit/context path
- Do not require issue, pull request, actions, administration, or webhook permissions for the audit/context path

## Allowed GitHub usage

The audit/context path may:

- resolve repository identity and default branch
- inspect repository metadata needed for analysis
- read file and tree metadata only when required for repository readiness or context derivation
- derive compact summaries, findings, and structured analysis from returned GitHub data

## Prohibited persistence and output

The audit/context path must not persist or emit:

- raw GitHub tokens
- authorization headers
- copied secret values from environment variables or GitHub responses
- full raw API payloads when derived summaries are sufficient
- unnecessary personal data from contributors, committers, or repository activity

## Logging rules

- redact tokens and auth headers from logs, errors, and test output
- prefer repository coordinates and endpoint names over dumping response bodies
- keep persisted artifacts limited to derived findings, summaries, and non-sensitive metadata required for reproducibility

## Root workspace expectations

The repository root is documentation and metadata coordination for this contract. It is not the preferred execution entrypoint for broader local-repository automation in this fork.

## Contributor guidance

When adding or changing GitHub-integrated CLI behavior:

- keep the permission contract at or below `Metadata: Read` plus optional `Contents: Read`
- document any new GitHub endpoint usage before release
- avoid adding token storage, replay logs, or broad response snapshots
- treat this document as the source of truth for the GitHub API-only audit/context boundary
