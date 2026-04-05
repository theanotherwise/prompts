# CODING POLICY

```markdown
# CODING POLICY

## Build And Execution

- Only application-level tests may be executed.
- Use the project's native test runner when tests exist.
- Do not build Docker images.
- Do not run Docker, Docker Compose, Podman, Kubernetes, or any other container-runtime command.
- Do not start services, databases, containers, or orchestration systems.
- If a command could implicitly build containers or start infrastructure, treat it as forbidden.

## File Handling

- Read only the files needed for the active task.
- Prefer targeted inspection over broad repository scans.
- Keep changes tightly scoped to the task.
- Do not silently rewrite, remove, or override unrelated files.
- In repository `.md` files such as `README.md` or `AGENTS.md`, references to files in the same repository
  should use clickable Markdown links when possible, for example `[README.md](./README.md)`.

## Workspace Documents

- If the workspace uses `AGENTS.md`, read it at the beginning of the session.
- Re-read `AGENTS.md` before non-trivial changes if it may have changed.
- Update `AGENTS.md` when repository purpose, file roles, or document structure changes.

## Change Discipline

- Prefer small, readable patches over wide refactors.
- Avoid changing behavior outside the requested scope.
- After edits, verify that the changed files are consistent with the intended result.

## Shell Behavior

- Use `bash`, never `zsh`.
- Load `~/.bash_profile` at session start as a standalone action.
- Do not chain profile loading with operational commands.
- Do not rely on shell wrapping as a workaround for normal command execution.

## Context Behavior

- Keep context loading minimal but sufficient.
- Focus on files directly related to the task.
- Avoid re-reading unchanged large files unless needed.
- Preserve architectural consistency while optimizing token usage.
```
