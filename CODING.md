# Coding Instructions

This file is the consolidated agent-instructions document for the repository.
`AGENTS.md` remains the authoritative governance document.

Below is the full content as a single copy-friendly Markdown block.

```markdown
# Coding Instructions

`AGENTS.md` is the authoritative governance document.
This file is a consolidated copy of the active repository rules for agents.

## Current AGENTS.md

# AGENTS.md

## Project Purpose

This repository stores deterministic policy documents for agent behavior in this workspace.
It does not contain application source code, runtime services, deployment manifests, or infrastructure definitions.

## Architectural Overview

The repository is documentation-only.
Its structure has two persistent layers:

- `AGENTS.md`: authoritative governance contract for repository-aware agents.
- `CODING.md`: consolidated human-readable copy of the active repository rules, including a copy of the current `AGENTS.md` content and the operational policy bundle.

`AGENTS.md` is the source of truth.
If `CODING.md` and `AGENTS.md` diverge, `AGENTS.md` prevails.

## Functional Structure

- Governance layer: repository constraints, change control, and conflict handling.
- Execution policy layer: command, test, and infrastructure restrictions.
- Context policy layer: scope control and token-efficiency requirements.
- Distribution layer: a single merged `CODING.md` that mirrors the active policy set for easy reuse and copy/paste.

## File Structure Explanation

- `AGENTS.md`: mandatory repository governance document. Must always exist.
- `CODING.md`: merged reference containing the current `AGENTS.md` content and the repository policy bundle in one copy-friendly Markdown document.

No other repository files are required for the current state.

## Module and Document Responsibilities

- `AGENTS.md`
  Defines repository purpose, document roles, operational constraints, structural rules, and update obligations.
- `CODING.md`
  Provides a single copy-oriented document for humans and downstream tooling that need the current agent instructions in one place.

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
- Requests for a single consolidated document must be implemented as `CODING.md` plus the required `AGENTS.md`.
- If future requests attempt to remove `AGENTS.md`, the request must be rejected or preceded by a governance change that still preserves the mandatory existence rule.

## Change Control

- Any change that introduces, removes, or renames repository files must be reflected here.
- Any change that alters the meaning of the merged policy bundle in `CODING.md` must also update `AGENTS.md`.
- Documentation consolidation is allowed only if `AGENTS.md` remains present and authoritative.

## Repository Policy Bundle

## AGENT Governance Policy

AGENTS.md is the authoritative governance document of this repository.
It defines architecture, structure, responsibilities, constraints, and operational assumptions.

### 0. Mandatory existence rule

- If AGENTS.md does not exist in the repository, you must:
  1. Perform a full repository review.
  2. Analyze architecture, structure, responsibilities, runtime model, build process, and infrastructure aspects.
  3. Create a complete AGENTS.md before making any other modification.
- No code, configuration, or structural changes are allowed before AGENTS.md is created.

### 1. Mandatory startup behavior

- At the beginning of every session, read AGENTS.md before performing any reasoning, analysis, or modification.
- Do not rely on memory or prior context.
- Never assume repository structure without reading AGENTS.md first.

### 2. Continuous change verification

- Before executing any non-trivial action (code change, refactor, rename, delete, infrastructure modification, configuration update, dependency change, structural change), check whether AGENTS.md has changed.
- If it has changed since the last read, re-read it fully.
- Never use a cached version.

### 3. Change reflection requirement

- Any modification that affects architecture, structure, runtime behavior, build process, infrastructure, responsibilities, or constraints must be reflected in AGENTS.md.
- Update AGENTS.md in the same task where the change is introduced.
- Never leave AGENTS.md outdated.

### 4. Structural integrity enforcement

AGENTS.md must always contain:

- Project purpose
- Architectural overview
- Functional structure
- File structure explanation
- Module/service responsibilities
- Execution model (runtime/build/infra)
- Explicit constraints and policies

### 5. Conflict handling

- If a requested change contradicts AGENTS.md, do not proceed silently.
- Explicitly identify the conflict.
- Propose an AGENTS.md update.
- Update AGENTS.md before implementing the conflicting change.

### 6. Post-change validation

After any modification:

- Re-evaluate AGENTS.md.
- Verify it matches the real repository state.
- Ensure new files, removed files, renamed files, and changed behavior are documented.
- If mismatch exists, fix AGENTS.md before finishing.

### 7. Deterministic documentation rule

- AGENTS.md must be technical, precise, and deterministic.
- No marketing language.
- No ambiguity.
- No historical commentary.
- It must describe the current state only.

### 8. Governance strictness

- Never override AGENTS.md implicitly.
- Never introduce structural drift.
- Treat AGENTS.md as a living but controlled governance contract.

### 9. AI Token Optimization Rule

AGENTS.md serves not only as governance documentation but also as a context optimization layer for AI systems.

- Any code change must trigger evaluation of whether updating AGENTS.md would:
  - Reduce future token consumption
  - Improve architectural clarity
  - Make project structure easier to understand
  - Reduce ambiguity for future analysis
  - Improve deterministic reasoning about the repository
- If updating AGENTS.md would materially improve AI context efficiency or reduce the need to re-analyze large portions of the repository, it must be updated in the same task.
- AGENTS.md must act as a compressed architectural map of the project, allowing AI systems to reason about structure without scanning the entire repository.
- If a change introduces new structural patterns, conventions, integration points, or execution flows, AGENTS.md must be updated even if the architecture is not fundamentally altered.

The goal is to minimize unnecessary token usage while maximizing structural clarity and determinism.
This policy is mandatory and must be enforced automatically in every interaction.

## Build Execution Policy

This policy strictly limits what the agent is allowed to execute in the context of build and runtime operations.

### 1. Allowed actions

- The agent is allowed to execute application-level tests only.
- Tests must be executed using the project's native test runner (for example: pytest, go test, npm test, mvn test).
- Test execution must not implicitly trigger container builds or infrastructure startup.

### 2. Explicitly forbidden actions

The agent must never:

- Build Docker images.
- Execute `docker build`.
- Execute `docker compose build`.
- Execute `docker-compose build`.
- Execute `docker compose up`.
- Execute `docker-compose up`.
- Execute `docker run`.
- Execute `docker start`.
- Execute `docker stop`.
- Execute `docker restart`.
- Execute `docker create`.
- Execute any Docker command whatsoever.

### 3. No infrastructure manipulation

- The agent must not start services.
- The agent must not stop services.
- The agent must not restart services.
- The agent must not initialize databases.
- The agent must not start containers.
- The agent must not trigger orchestration systems.

### 4. No implicit builds

- The agent must not execute any command that indirectly triggers image building or container orchestration.
- If a test command would require container startup, the agent must refuse and report the violation.

### 5. Local execution scope

- Only code-level test execution is allowed.
- No environment provisioning.
- No runtime orchestration.
- No dependency container startup.

### 6. Compliance enforcement

- If a requested action requires Docker, Docker Compose, Podman, Kubernetes, or any container runtime, the agent must refuse.
- If uncertainty exists whether a command may trigger containerization, the agent must assume it is forbidden.

### 7. Safety principle

The agent operates in a test-only execution mode:

- Code can be analyzed.
- Code can be modified.
- Tests can be executed.
- Nothing can be built.
- Nothing can be deployed.
- Nothing can be started.

This restriction is absolute and cannot be bypassed.

## Context Optimization Policy

This policy ensures efficient context usage while preserving architectural awareness and task continuity.

### Core Principles

- Focus strictly on modules, files, and code fragments directly related to the current task.
- Do not re-read unchanged large files.
- Do not scan or summarize the entire repository unless explicitly requested.
- Load only the minimum necessary context required for accurate execution.

### Targeted Analysis

- Use precise, targeted search instead of full project traversal.
- Expand context incrementally when needed.
- Avoid bulk-loading directories or unrelated components.

### Context Retention

- Reuse previously analyzed relevant context whenever possible.
- Maintain awareness of system architecture and integration boundaries.
- Preserve understanding of dependencies and side effects.

### Continuity Awareness

The agent must always maintain clarity about:

- Current task scope
- Related modules and integration points
- Impact of changes on surrounding components
- Active constraints and execution policies

### Token Efficiency Rule

- Minimize token usage without sacrificing correctness.
- Optimization must not reduce architectural understanding.
- Efficiency must never break logical or cross-module consistency.

### Balance Principle

Context reduction must never compromise:

- System-level reasoning
- Structural coherence
- Policy compliance
- Task alignment

The objective is efficient execution with sustained architectural awareness.

## Shell Execution Policy

All shell command execution must strictly follow these rules.

### 1. Shell type

- Never use zsh.
- Always operate in bash.

### 2. Profile loading

- At the beginning of every session, load the user's `.bash_profile` as a separate standalone command.
- Do not combine it with any other command.
- Always use `source ~/.bash_profile`.
- Never use dot (`.`) syntax.
- If the user explicitly requests a reload, run `source ~/.bash_profile` again as a separate command.

### 3. Command execution rules

- Never use `bash -c "..."`.
- Never use `bash -lc "..."`.
- Never wrap commands inside shell invocation strings.
- Execute commands directly as a regular interactive bash user would.

### 4. No workarounds

- Do not use `bash -c` as a workaround for environment or chaining.
- Do not simulate execution through subshell wrapping.
- Do not bypass the profile loading rule.

### 5. Separation of concerns

- Do not chain profile loading with execution.
- Do not prepend `source` to operational commands.
- Keep environment initialization and command execution logically separate.

### 6. User-context fidelity

- Commands must behave exactly as if typed manually in a regular bash session.
- Respect user PATH, aliases, and environment variables defined in `.bash_profile`.
- Do not override environment unless explicitly instructed.

### 7. Compliance

- If a requested execution would violate these rules, stop and explain the violation.
- Never silently ignore this policy.

This execution policy is mandatory for all shell interactions.
```
