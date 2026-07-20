# Package Boundaries

This repository is a private monorepo workspace for the Loop Engineering toolchain.

## Workspace boundary

- The repository root is a private workspace manifest used to coordinate development across packages in `tools/*`.
- Do not install the repository root package as an application dependency.
- Prefer the published npm packages under the `@cobusgreyling/*` scope when consuming Loop Engineering tools.

## Published package boundary

Each package under `tools/*` owns its own README, build output, and release metadata.

High-impact CLI packages include:

- `@cobusgreyling/loop-audit`: backend-oriented repository readiness auditing with a GitHub API-only contract and minimal-permission token handling.
- `@cobusgreyling/loop-cost`: standalone CLI for estimating token budgets and daily operating costs from bundled registry data.
- `@cobusgreyling/loop-init`, `@cobusgreyling/loop-sync`, `@cobusgreyling/loop-context`, `@cobusgreyling/loop-worktree`, and related tools: publish independently and should be consumed package-by-package.

## Supported usage

Use the published packages when you need:

- a standalone CLI installed from npm
- package-level README guidance and release notes
- backend-oriented tooling that keeps repository access narrow and explicit

## Unsupported usage

Avoid treating this repository as:

- a single installable root package
- a browser SDK distribution
- a signal that every workspace is safe to embed into frontend code without review

## Fork note

This fork keeps the release surface focused on explicit package metadata, clear monorepo boundaries, and GitHub-oriented audit contracts. Review each package manifest before publishing or automation changes so repository URLs, token expectations, and supported execution modes stay aligned with the fork.