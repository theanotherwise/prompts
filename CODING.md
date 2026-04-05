# CODING STACK

This file defines the coding-oriented prompt stack for the repository.
It describes how an agent should behave when the task is related to code, repository files, tests, and coding workflow.
`AGENTS.md` remains the authoritative governance document.

## Scope

Use this stack when the task involves code changes, code review, repository inspection, tests, debugging, refactors, or implementation work.

## Behavior Model

### Build Behavior

- Only application-level tests may be executed.
- Use the project's native test runner when tests exist.
- Do not build Docker images.
- Do not run Docker, Docker Compose, Podman, Kubernetes, or any other container-runtime command.
- Do not start services, databases, containers, or orchestration systems.
- If a command could implicitly build containers or start infrastructure, treat it as forbidden.

### File Handling Behavior

- Read only the files needed for the active task.
- Prefer targeted inspection over broad repository scans.
- Re-read `AGENTS.md` before non-trivial changes if it may have changed.
- When a structural or policy change is made, update `AGENTS.md` in the same task.
- When a new stack file is added or a repository document changes role, update `README.md` and `AGENTS.md` together.
- Never silently remove or replace governance files.

### Governance Awareness

- `AGENTS.md` is always authoritative.
- If `CODING.md` conflicts with `AGENTS.md`, follow `AGENTS.md`.
- If a requested change conflicts with governance, identify the conflict before proceeding.
- After edits, verify that repository documents still match the actual file layout and current behavior.

### Shell Behavior

- Use `bash`, never `zsh`.
- Load `~/.bash_profile` at session start as a standalone action.
- Do not chain profile loading with operational commands.
- Do not rely on shell wrapping as a workaround for normal command execution.

### Context Behavior

- Keep context loading minimal but sufficient.
- Focus on files directly related to the task.
- Avoid re-reading unchanged large files unless needed.
- Preserve architectural consistency while optimizing token usage.

## Repository Relationships

- `AGENTS.md`
  Global governance for the repository.
- `README.md`
  File index for the repository.
- `CODING.md`
  Coding-specific operating stack.
