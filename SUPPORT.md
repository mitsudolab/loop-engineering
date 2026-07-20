# Support

## Where to get help

- Use the [GitHub issue tracker](https://github.com/mitsudolab/loop-engineering/issues) for reproducible bugs, documentation gaps, and package metadata corrections.
- Include the package name and version when reporting a problem in a published tool under `tools/*`.
- For security-sensitive reports, avoid posting secrets, tokens, or private repository details in public issues.

## Package discovery

This repository root is a private npm workspace and is **not** a publish target.

Published CLI packages in this fork are maintained from their package directories, including:

- `tools/loop-audit` for GitHub API-backed readiness auditing
- `tools/loop-context` for local-first context management derived from audit output

Repository source and package metadata are published from the monorepo so npm consumers can trace each package back to:

- repository source: `https://github.com/mitsudolab/loop-engineering`
- issue tracker: `https://github.com/mitsudolab/loop-engineering/issues`

## Privacy and network expectations

The most visible audit-oriented packages in this fork are intentionally positioned around a narrow network boundary:

- `@cobusgreyling/loop-audit` is the package expected to talk to the GitHub API directly.
- `@cobusgreyling/loop-context` should stay local-first and should not broaden token handling, outbound endpoints, or telemetry scope beyond the documented audit contract.
- `@cobusgreyling/goal-audit` should work from local files or pre-fetched audit context rather than introducing direct GitHub authentication.

When GitHub access is required, prefer minimal-permission tokens, default to the GitHub REST API, and avoid collecting or transmitting repository data that is not needed for the current audit task.
