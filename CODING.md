# CODING STACK

This stack describes how an agent should behave when the task is related to code, files, tests, and coding workflow.

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
- Keep changes tightly scoped to the task.
- Do not silently rewrite, remove, or override unrelated files.
- When instructions for the workspace use a governance file, re-read it before non-trivial changes if it may have changed.
- When a structural or policy change is made, update the corresponding governance documentation in the same task.

### Change Discipline

- If a requested change conflicts with active project rules, identify the conflict before proceeding.
- After edits, verify that documentation and file layout still match the actual current state.

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
