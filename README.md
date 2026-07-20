# Loop Engineering

Loop Engineering is a private monorepo that groups a set of published CLI and audit tools under `tools/*`.

This fork keeps the repository root focused on documentation, package boundaries, and GitHub workflow maintenance. The root package is **not** intended for publication. Install the individual packages instead.

## Monorepo boundaries

- `package.json` at the repository root defines the private workspace and shared maintenance scripts.
- `tools/loop-audit` is the GitHub-facing audit package. It is the only audit-oriented package in this fork that is expected to talk to the GitHub API directly.
- `tools/goal-audit` is a downstream readiness checker. It should consume local files or pre-fetched audit context and must not expand the fork's GitHub permission surface.
- Other packages under `tools/*` remain independently publishable CLI utilities with their own manifests and release flows.

## Audit-oriented packages

| Package | Intended scope | GitHub boundary |
| --- | --- | --- |
| `@cobusgreyling/loop-audit` | Fetch minimal repository metadata for readiness analysis | Direct GitHub API access with a minimal-permission token contract |
| `@cobusgreyling/goal-audit` | Evaluate goal readiness from local inputs or pre-fetched audit output | No direct GitHub authentication or token handling |

## Publishing and maintenance

- The repository root is private on purpose and should remain a workspace shell only.
- Published artifacts should come from the package directories under `tools/*`.
- GitHub-facing metadata in package manifests should point to this fork so release automation, issue links, and repository provenance stay aligned.
- Audit packages should document whether they require GitHub credentials or rely on upstream pre-fetched context.

## Safety posture

This fork preserves a minimal-permission posture for audit workflows:

- prefer GitHub API-only metadata collection where remote access is required
- keep token ownership isolated to the package that actually performs GitHub calls
- do not broaden package scope from documentation or metadata changes alone
- avoid introducing root-level execution paths that imply the private workspace is publishable

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

Use each package README and manifest as the source of truth for package-specific commands, publishing details, and environment requirements.
