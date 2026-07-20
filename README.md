# Loop Engineering

Loop Engineering is a private monorepo that groups a set of published CLI and audit tools under `tools/*`.

This fork keeps the repository root focused on documentation, package boundaries, and GitHub workflow maintenance. The root package is **not** intended for publication. Install the individual packages instead.

## Monorepo boundaries

- `package.json` at the repository root defines the private workspace and shared maintenance scripts.
- `tools/loop-audit` is the GitHub-facing audit package. It is the only audit-oriented package in this fork that is expected to talk to the GitHub API directly.
- `tools/goal-audit` is a downstream readiness checker. It should consume local files or pre-fetched audit context and must not expand the fork's GitHub permission surface.
- `tools/loop-context` manages reusable context derived from audit output and should follow the same GitHub token, endpoint, and data-minimization contract when it handles GitHub-sourced material.
- Other packages under `tools/*` remain independently publishable CLI utilities with their own manifests and release flows.

## Audit-oriented packages

| Package | Intended scope | GitHub boundary |
| --- | --- | --- |
| `@cobusgreyling/loop-audit` | Fetch minimal repository metadata for readiness analysis | Direct GitHub API access with a minimal-permission token contract |
| `@cobusgreyling/goal-audit` | Evaluate goal readiness from local inputs or pre-fetched audit output | No direct GitHub authentication or token handling |
| `@cobusgreyling/loop-context` | Store or reshape context derived from audit output | Must not broaden authentication, endpoint, or telemetry scope beyond the audit contract |

## GitHub authentication and endpoint contract

For the audit/context workflow, the repository documents one shared GitHub-facing contract even when only one package performs the remote calls directly:

- Prefer a fine-grained personal access token or a GitHub App installation token.
- For fine-grained personal access tokens, the expected repository permission is `Metadata: read` and `Contents: read` only when a workflow explicitly requires repository content access.
- If a classic personal access token is used as a fallback, keep scope to `public_repo` for public repositories or `repo` only when private repository access is required.
- Default to the GitHub.com REST API base URL, `https://api.github.com`.
- Support GitHub Enterprise Server by allowing the API base URL to be supplied explicitly through package-level configuration or environment variables documented by the package manifest.
- Treat endpoint selection as configuration only; documentation or metadata updates must not silently redirect traffic to any other host.

## Telemetry and data-minimization expectations

GitHub-facing audit and context tooling in this fork should keep the existing low-data posture:

- collect only repository metadata needed for readiness analysis or derived context generation
- do not persist raw GitHub tokens, authorization headers, or broader API payloads when derived context is sufficient
- do not add analytics, usage beacons, or extra outbound telemetry through documentation or manifest changes alone
- keep token ownership isolated to the package that actually performs GitHub calls
- preserve local-first and pre-fetched-input workflows where a package can operate without additional network access

## Publishing and maintenance

- The repository root is private on purpose and should remain a workspace shell only.
- Published artifacts should come from the package directories under `tools/*`.
- GitHub-facing metadata in package manifests should point to this fork so release automation, issue links, and repository provenance stay aligned.
- Audit packages should document whether they require GitHub credentials or rely on upstream pre-fetched context.
- GitHub-oriented manifests should state their token expectations, API base URL behavior, and telemetry limits explicitly so audits do not depend on local tribal knowledge.

## Safety posture

This fork preserves a minimal-permission posture for audit workflows:

- prefer GitHub API-only metadata collection where remote access is required
- keep token ownership isolated to the package that actually performs GitHub calls
- do not broaden package scope from documentation or metadata changes alone
- avoid introducing root-level execution paths that imply the private workspace is publishable
- keep GitHub.com as the default endpoint while allowing explicit GHES API base URL configuration where a package already supports it
- avoid adding extra telemetry, credential storage, or hidden outbound integrations

## Workspace packages

The monorepo currently tracks these package areas under `tools/*`:

- `loop-audit`
- `goal-audit`
- `loop-init`
- `loop-cost`
- `loop-sync`
- `loop-context`
- `loop-gate`
- `mcp-server`
- `loop-worktree`

Use each package README and manifest as the source of truth for package-specific runtime and release details.