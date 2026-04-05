# AGENTS.md

## Project Purpose

This repository stores deterministic policy documents for agent behavior in this workspace.
It does not contain application source code, runtime services, deployment manifests, or infrastructure definitions.

## Architectural Overview

The repository is documentation-only.
Its structure has two persistent layers:

- `AGENTS.md`: authoritative governance contract for repository-aware agents.
- `README.md`: consolidated human-readable copy of all operational rules, formatted as a single copy-friendly Markdown block.

`AGENTS.md` is the source of truth.
If `README.md` and `AGENTS.md` diverge, `AGENTS.md` prevails.

## Functional Structure

- Governance layer: repository constraints, change control, and conflict handling.
- Execution policy layer: command, test, and infrastructure restrictions.
- Context policy layer: scope control and token-efficiency requirements.
- Distribution layer: a single merged `README.md` that mirrors the active policy set for easy reuse.

## File Structure Explanation

- `AGENTS.md`: mandatory repository governance document. Must always exist.
- `README.md`: merged reference containing governance, build, context, and shell policies in one Markdown block.

No other repository files are required for the current state.

## Module and Document Responsibilities

- `AGENTS.md`
  Defines repository purpose, document roles, operational constraints, and update obligations.
- `README.md`
  Provides a single copy-oriented document for humans and downstream tooling that need the full policy bundle in one place.

## Execution Model

- Runtime: none. The repository has no executable application runtime.
- Build: none. The repository has no build pipeline artifacts in its current state.
- Infrastructure: none. The repository must not start services, containers, or orchestration systems.
- Allowed execution scope: file inspection, file editing, and native test execution only if future repository contents add tests that do not trigger infrastructure startup.

## Explicit Constraints and Policies

### Governance Rules

- Read `AGENTS.md` at the beginning of every session before reasoning, analysis, or modification.
- Before any non-trivial action, verify whether `AGENTS.md` has changed and re-read it if needed.
- Reflect any structural, behavioral, responsibility, or policy change in `AGENTS.md` in the same task.
- Do not proceed silently when a request conflicts with repository governance. Identify the conflict first.
- After every modification, verify that `AGENTS.md` matches the real repository state.
- Keep repository documentation technical, precise, deterministic, and limited to the current state.

### Build and Infrastructure Rules

- Only application-level tests are allowed, and only through the native test runner when such tests exist.
- Docker, Docker Compose, Podman, Kubernetes, and any container-runtime command are forbidden.
- Do not start, stop, restart, provision, or orchestrate infrastructure.
- If a command could implicitly build containers or start infrastructure, treat it as forbidden.

### Context Rules

- Load only the minimum repository context required for the active task.
- Use targeted inspection instead of broad repository scans unless explicitly requested.
- Reuse already known relevant context when safe to do so.
- Preserve system-level consistency while optimizing token usage.

### Shell Rules

- Use `bash`, never `zsh`.
- Load `~/.bash_profile` as a standalone command at session start.
- Do not chain profile loading with operational commands.
- Do not use `bash -c`, `bash -lc`, or similar shell wrapping as a workaround.
- Execute commands as directly as possible within the available environment.

## Conflict Handling

- The repository cannot be reduced to a single Markdown file because `AGENTS.md` is mandatory.
- Requests for a single consolidated document must be implemented as `README.md` plus the required `AGENTS.md`.
- If future requests attempt to remove `AGENTS.md`, the request must be rejected or preceded by a governance change that still preserves the mandatory existence rule.

## Change Control

- Any change that introduces, removes, or renames repository files must be reflected here.
- Any change that alters the meaning of the merged policy bundle in `README.md` must also update `AGENTS.md`.
- Documentation consolidation is allowed only if `AGENTS.md` remains present and authoritative.
