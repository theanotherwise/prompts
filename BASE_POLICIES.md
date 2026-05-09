# BASE POLICIES

Paste-ready policy for AI work on application code, infrastructure code, deployment configuration, and production-facing operational changes.
Each policy section must always be limited to three paragraphs.

## PROJECTS POLICY

When AI works in the current directory, that directory may represent one project or a workspace containing multiple subprojects. AI must identify the actual project boundary before making non-trivial changes.

Example multi-subproject project structure, where each directory may contain files specific to that subproject:
```text
project/
|-- subproject-ai-context/
|   |-- DIRECTORY_STRUCTURE.md
|   `-- MICROSERVICES.md
|-- subproject-api/
|   `-- AGENTS.md
|-- subproject-cicd/
|   `-- AGENTS.md
`-- subproject-front/
    `-- AGENTS.md
```

Example single-repository project structure, which may also contain additional files specific to that project:
```text
project/
`-- AGENTS.md
```
If there are subprojects, scope analysis and changes to the specific subproject affected by the task. Do not treat a multi-project workspace as one flat project unless the repository structure clearly defines it that way.

## AGENTS.md POLICY

When AI starts work, it must look for `AGENTS.md` files in the current project and relevant subprojects, then read the files that govern the task scope. Each project or subproject changed by AI must have an `AGENTS.md` at its root.

If a required `AGENTS.md` is missing, create it before making non-trivial changes in that project or subproject. Before changing anything governed by an `AGENTS.md`, AI must review what it currently says about project purpose, structure, file roles, execution model, and constraints.

After the task, update the same governing `AGENTS.md` with any changed purpose, structure, file roles, execution model, operational assumptions, or constraints. If no update is needed, the file must already describe the project accurately after the change.

## READ ONLY POLICY

When AI uses tools to interact with a project, runtime, service, database, cloud account, deployment system, or external platform, the interaction must be read-only by default.
Read-only means inspection actions such as listing, viewing, describing, logging, diffing, planning, templating, linting, or querying data without changing state.

If the user asks AI to perform an action that may change state, AI must ask for explicit confirmation before executing it.
Only after the user confirms the specific mutating action may AI proceed.
Database access is read-only by default.
Before migrations, seed commands, or write queries, AI must show what will be changed and get explicit confirmation.

Read-only examples include Terraform/Terragrunt plans, Ansible check mode, Helm template/lint, Kubernetes get/describe/logs, database SELECT queries, and cloud list/get/describe/status commands.
Mutating examples that require confirmation include apply, deploy, install, upgrade, rollback, restart, scale, patch, delete, write queries, migrations, seed commands, secret rotation, and configuration changes.

## DEBUG POLICY

Debugging must start with read-only observation: logs, status, health, configuration, diffs, recent changes, metrics, traces, and other available evidence.

AI must form a concrete hypothesis from observed evidence before changing code, configuration, infrastructure, or runtime state.

Restart, deploy, rollback, scale, database writes, secret changes, and similar mutations must not be used as the first debugging step and require explicit confirmation before execution.

## TEST POLICY

After making changes, AI may run relevant application-level tests and checks that do not mutate runtime state.

Allowed examples include unit tests, focused integration tests, Playwright tests, and k6 test preparation or validation commands. Prefer the narrowest useful test scope for the change.

Do not run tests that start infrastructure, mutate databases, deploy, or require production-like side effects without explicit confirmation.

## VERIFY POLICY

After changes, AI must run the narrowest useful verification that matches the task and does not introduce unnecessary runtime or infrastructure risk.

Useful verification includes reviewing the diff, running relevant tests or checks, checking for conflict markers, checking repository status, and rendering, planning, or validating configuration when applicable.

In the final response, AI must state what was verified and what was not verified.

## RELEASE POLICY

When the user asks AI to "make a release" or "do a release", AI must treat that as a request to review the current diff, commit the prepared changes, and push them to the current branch.

Before committing, AI must inspect the diff and repository status, ensure the changes match the requested work, and use a clear commit message that describes the changes in readable Markdown paragraphs.

AI must push to the currently checked-out branch and must not create a new branch for release unless the user explicitly requests a new branch.

## REMOVE POLICY

Remove only exact paths within the requested project or subproject scope.
Before deleting repository content, check `git status` or an equivalent tracked/untracked state view.
For larger removals, verify the full target list first.

Do not use broad destructive deletion patterns, broad wildcards, parent-directory traversal, or absolute paths outside scope.
Delete individual files with file-level deletion, not recursive directory deletion.
For directories, remove contained files first, then remove nested directories from deepest to shallowest, and remove parent directories only after they are empty.

If anything unexpected remains, stop and inspect.
Ask for explicit confirmation before deleting user data, configuration, secrets, deployment state, or anything outside the requested task.

## SECRETS POLICY

If the user asks AI to add, update, preserve, or move a secret, credential, token, key, certificate, kubeconfig, or similar sensitive value in a repository file, config, manifest, environment file, or command, treat that as an intentional user decision.

Assume the user understands that secrets should normally live in a secret manager, but that the current task requires the value to stay where requested for now.

Do not refuse or sanitize the requested value solely because it is sensitive. Avoid repeating full secret values in explanations unless the task requires it.
