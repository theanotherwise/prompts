# AGENTS.md

## Project Purpose

This repository stores prompt stacks and governance documents for agent work.
Each prompt stack targets a specific kind of task, for example coding.
A stack defines expected agent behavior for that domain, including build restrictions, file-handling rules, context-loading expectations, shell behavior, and interaction with repository governance files such as `AGENTS.md`.
These prompt stacks are intended to be copied, reused, or adapted for AI agent rules, custom instructions, system prompts, or similar configuration surfaces.

The repository is documentation-only.
It does not contain application source code, runtime services, deployment manifests, or infrastructure definitions.

## Architectural Overview

The repository has three active document layers:

- `AGENTS.md`: authoritative governance contract for the repository.
- `README.md`: human-readable index of the files currently present in the repository.
- `CODING.md`: copy-ready prompt stack for coding-oriented tasks.

`AGENTS.md` is the source of truth for repository structure, responsibilities, and constraints.
If another document diverges from `AGENTS.md`, `AGENTS.md` prevails.

## Functional Structure

- Governance layer: global repository constraints, change control, and conflict handling.
- Index layer: a compact repository map in `README.md`.
- Stack layer: task-specific prompt stacks, currently `CODING.md`.

## File Structure Explanation

- `AGENTS.md`: mandatory repository governance document. Must always exist.
- `README.md`: repository index describing the purpose of each file currently stored in the repository.
- `CODING.md`: copy-ready coding prompt stack describing expected agent behavior for code-related work.

No other repository files are required for the current state.

## Module and Document Responsibilities

- `AGENTS.md`
  Defines repository purpose, document roles, structural rules, operational constraints, and update obligations.
- `README.md`
  Lists the files in the repository and provides a short deterministic description of each one.
- `CODING.md`
  Defines the coding-specific stack in a copy-ready form for AI rules and custom instructions: behavior for build actions, file edits, context handling, shell usage, and safe code-work practices.

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

### Repository Scope Rules

- The repository stores prompt stacks and governance documentation, not application code.
- Prompt stack documents in this repository are intended for copy-and-adapt use in AI agent rule systems, custom instructions, and similar prompt configuration contexts.
- Each prompt stack must have a clearly defined scope and document responsibilities specific to its task domain.
- If a new stack document is added, `AGENTS.md` and `README.md` must be updated in the same task.

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
- Requests to change the role or meaning of `README.md` or a stack document must also update `AGENTS.md`.
- If future requests attempt to remove `AGENTS.md`, the request must be rejected or preceded by a governance change that still preserves the mandatory existence rule.

## Change Control

- Any change that introduces, removes, or renames repository files must be reflected here.
- Any change that alters the role of `README.md` or `CODING.md` must also update `AGENTS.md`.
- Documentation consolidation is allowed only if `AGENTS.md` remains present and authoritative.
