# POLICIES PROMPT

Paste-ready policy for AI work on application code, infrastructure code, deployment configuration, and production-facing operational changes.

````markdown
# POLICIES

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

## README POLICY

If a project or subproject already contains a README file, AI should update it when the task changes user-facing setup, usage, file roles, workflows, or other information that the README should accurately describe.

README updates should be concise and limited to information that helps future users or maintainers understand the current project.

If a project or subproject does not already contain a README file, AI must not create one unless the user explicitly asks for it.

## READ ONLY POLICY

When AI uses tools to interact with a project, runtime, service, database, cloud account, deployment system, or external platform, the interaction must be read-only by default.
Read-only means inspection actions such as listing, viewing, describing, logging, diffing, planning, templating, linting, or querying data without changing state.

If the user has not already directly asked for the specific mutating action, AI must ask for explicit confirmation before executing it.
A direct request such as "commit these changes", "prepare release", "make a release", "do a release", "make a release + tag", "do a release + tag", "tag this release", or "taguj" is explicit confirmation for the matching Git commit, release, tag, and push steps described below, so AI must not ask for a second confirmation for those same steps.
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

## DEPENDENCIES POLICY

AI must not install, fetch, add, or update dependencies through package-manager commands by default. Commands such as npm add/install, yarn add/install, pnpm add/install, pip install, poetry add, uv add, go get, cargo add, and similar dependency-fetching commands require explicit confirmation.

When a dependency declaration must change, AI should edit the appropriate manifest, lockfile, or configuration file directly and keep the change scoped to the requested task.

This avoids accidentally using local environment credentials, registry tokens, or user-specific package manager state. Dependency installation and fetching should be left to the user, CI/CD, or an explicitly confirmed command.

## SHELL POLICY

Every time AI is about to use a CLI or shell command in a project, it must first determine the active shell with `echo "$0"` or an equivalent read-only check, then choose command syntax appropriate for that shell. If `bash` is detected and `~/.bash_profile` exists, AI must load it as a standalone best-effort step before the CLI command. If `zsh` is detected, AI must do the analogous Zsh startup load before the CLI command, preferring `~/.zprofile` and using `~/.zshrc` when that is where the environment setup lives. This profile loading is required because ready-to-use tools may already be configured there through `PATH` or related environment setup, so AI must not assume a tool is unavailable before loading the relevant shell profile.

If the shell is `bash`, use Bash-compatible syntax. If the shell is `zsh`, use Zsh-compatible syntax. Do not assume Bash features are available in Zsh or Zsh features are available in Bash unless the command explicitly invokes that shell.

If a command needs a specific shell, invoke that shell explicitly, for example with `bash -lc` or `zsh -lc`, and keep the reason tied to command compatibility rather than using shell wrapping as a general workaround.

## VERIFY POLICY

After changes, AI must run the narrowest useful verification that matches the task and does not introduce unnecessary runtime or infrastructure risk.

Useful verification includes reviewing the diff, running relevant tests or checks, checking for conflict markers, checking repository status, and rendering, planning, or validating configuration when applicable.

In the final response, AI must state what was verified and what was not verified.

## DEPS / DEPENDENCY POLICY

For shared dependencies used by multiple microservices, AI must release the shared dependency first, then update all affected consumers to the released version. Releasing means committing and pushing the shared dependency change, creating and pushing the release tag or version, and then bumping every service that consumes it. Python shared dependencies are consumed as GAR packages, while Go and Rust shared dependencies are consumed as tagged Git code.

## RELEASE POLICY

For normal work, AI must stay on the currently checked-out branch unless the user explicitly asks to switch or create a branch. When the user asks AI to commit current changes, "prepare release", "make a release", "do a release", or "make/do a release + tag", that request is already confirmation for the matching Git mutations and AI must not ask for a second confirmation. When the user asks AI to "prepare release", AI must review the current diff and commit the prepared changes without pushing. When the user asks AI to "make a release" or "do a release", AI must first commit uncommitted release changes if needed, then push the current branch without creating a tag unless the user also explicitly asks for a tag.

Before committing, AI must inspect the diff and repository status, ensure the changes match the requested work, and write a clear Markdown commit message with readable paragraphs describing what changed. AI must not reset, rebase, force-push, or rewrite history without explicit confirmation.

The commit message should use Markdown heading structure when useful, starting with `#` for the main summary and `##` sections for grouped changes. AI must push only to the currently checked-out branch and must not create a new branch unless the user explicitly requests one. If the user names `master` or `main`, AI must resolve that against the repository's real branches and default branch before pushing.

## TAG POLICY

When the user asks AI to "make a release + tag", "do a release + tag", "make a release with tag", "do a release with tag", "tag this release", "taguj", or otherwise explicitly asks for a tag as part of the release, AI must inspect existing local and remote tags before pushing. If the repository already has release tags, AI must create the next release tag on the release commit or, for a standalone tag request such as "taguj", on the current `HEAD`, then push the current branch when the branch has release commits to push and always push the newly created tag without asking for a second confirmation. A plain "make a release" or "do a release" request must not create or push a tag.

AI must preserve the repository's current tag format. For simple integer tags such as `v1`, increment the integer, for example `v1` becomes `v2`. For semantic-version tags such as `v1.0.1`, AI must ask whether to bump major, minor, or patch before creating the tag, unless the user already specified the bump. For timestamp/hash tags such as `2026-05-09.22-17-46.07d4e7c`, AI must create the next tag from the current local timestamp and the release commit hash, abbreviated to the same hash length as the existing tag suffix.

If multiple incompatible tag formats exist, or if the next tag cannot be determined confidently from the existing tags and the user's request, AI must stop and ask for the intended tag format or bump. AI must inspect the final diff and repository status before tagging, and must not retag, move, delete, or overwrite existing tags without explicit confirmation.

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
````
