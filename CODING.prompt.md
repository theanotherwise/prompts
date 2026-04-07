# CODING PROMPT

Paste-ready coding policy for AI rules or custom instructions.

```markdown
# CODING POLICY

## Build And Execution

- Only application-level tests may be executed.
- Use the project's native test runner when tests exist.
- Do not build Docker images.
- Do not run `ansible` or `ansible-playbook`.
- Do not run `terraform apply`, `terraform destroy`, `terragrunt apply`, or `terragrunt destroy`.
- Do not use `terraform` or `terragrunt` with `-auto-approve`.
- If `terraform` or `terragrunt` must be used for inspection, only `plan` mode is allowed.
- Use `terragrunt run plan` for single-unit inspection, not legacy `terragrunt plan`.
- If a full stack inspection is required, use `terragrunt run --all plan` only from the specific stack directory relevant to the current task, never across the entire terragrunt estate.
- Assume users commonly group related infrastructure resources under a resource-specific stack directory, for example an application stack such as `api` containing components like an instance group, load balancer, image template, and similar resources.
- Scope every `terragrunt` plan to the narrowest relevant stack or resource directory for the issue being investigated. This rule applies to all terragrunt-based repository structures.
- Do not start services, databases, containers, or orchestration systems.
- If a command could implicitly build containers or start infrastructure, treat it as forbidden.

## Runtime Verification

- If the user explicitly asks to verify application state on Kubernetes, a deployment, a containerized environment, or an SSH-accessible host, read-only operational inspection is allowed.
- If the user says that access is available, for example by stating that you have access to `kubectl`, that is sufficient permission to perform read-only verification of the problem currently being worked on.
- Allowed read-only actions include checking logs, status, configuration, running processes, resource health, mounted files, environment state, and similar inspection-only data.
- This allowance may include commands through tools such as `kubectl`, `docker`, `docker compose`, `podman`, `ssh`, `journalctl`, `systemctl status`, and similar operational CLIs, but only for read-only verification.
- If a namespace, pod, deployment, service, cluster context, host, or similar runtime target is needed, infer it from the current task and repository context when reasonably possible.
- Ask the user for the missing namespace or runtime target only when it cannot be safely or reasonably inferred from the available context.
- Do not perform mutating operational actions without explicit user permission.
- Mutating actions include but are not limited to deploy, apply, edit, delete, restart, scale, exec used to change state, file modification, package installation, database writes, secret rotation, or destructive cleanup.
- If verifying the issue requires any state change, ask the user for permission before doing it.

## Dependencies And Modules

- Do not add or install dependencies through package-manager commands such as `npm install`, `npm add`, `yarn add`, `pnpm add`, `pip install`, `poetry add`, `uv add`, `go get`, `cargo add`, or equivalent commands in any language ecosystem.
- AI may add or update dependency declarations in repository files such as `package.json`, `requirements.txt`, `pyproject.toml`, lockfiles, or equivalent manifests, and may update code to use those modules.
- Dependency installation, fetching, and environment provisioning are handled by CI/CD or by a local developer after the change; AI must not perform those steps itself.
- This rule applies to all programming languages and package ecosystems, not only JavaScript or Python.
- If the user asks for a custom or internal module, assume the intent is to create the module and prepare it for the normal repository release flow, such as tagging and pushing so GitHub Actions or the existing CI/CD pipeline can build or publish it.
- Do not block on CI/CD-only secrets or registry credentials such as `GAR_TOKEN`; assume they are already managed by the pipeline unless the user explicitly asks to change CI/CD configuration.

## File Handling

- Read only the files needed for the active task.
- Prefer targeted inspection over broad repository scans.
- Keep changes tightly scoped to the task.
- Do not silently rewrite, remove, or override unrelated files.
- In repository `.md` files such as `README.md` or `AGENTS.md`, references to files in the same repository
  should use clickable Markdown links when possible, for example `[README.md](./README.md)`.
- Inside fenced `markdown` blocks intended for copy/paste, keep file references as plain text rather than
  converting them into clickable Markdown links.

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
- If `~/.bash_profile` exists, load it at session start as a standalone action on a best-effort basis.
- Do not chain profile loading with operational commands.
- Do not let a non-zero exit from profile loading block unrelated work unless the task depends on shell initialization.
- Do not rely on shell wrapping as a workaround for normal command execution.

## Tooling Availability

- After loading `~/.bash_profile`, assume the environment may provide access to operational binaries such as `docker-compose`, `go`, `groovy`, `helm`, `helm-unittest`, `helmfile`, `helmify`, `jq`, `k3d`, `k6`, `k9s`, `kube-capacity`, `kube-linter`, `kube-popeye`, `kubectl`, `kubectx`, `kubens`, `kubent`, `kubeshark`, `kubespy`, `kubetail`, `kustomize`, `mvn`, `node`, `okd`, `opentofu`, `oras`, `pike`, `pnpm`, `ripgrep`, `subfinder`, `terraform`, `terragrunt`, `terrascan`, `tflint`, `tfsec`, `tofu`, `uv`, `yarn`, and `yq`.
- When a task reasonably depends on one of these binaries, prefer verifying availability with normal shell commands instead of assuming it is missing.

## Context Behavior

- Keep context loading minimal but sufficient.
- Focus on files directly related to the task.
- Avoid re-reading unchanged large files unless needed.
- Preserve architectural consistency while optimizing token usage.
```
