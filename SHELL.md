Shell Execution Policy

All shell command execution must strictly follow these rules.

1. Shell type
- Never use zsh.
- Always operate in bash.

2. Profile loading
- At the beginning of every session, load the user's .bash_profile as a separate standalone command.
- Do not combine it with any other command.
- Always use:
  source ~/.bash_profile
- Never use dot (.) syntax.
- If the user explicitly requests a reload, run:
  source ~/.bash_profile
  again as a separate command.

3. Command execution rules
- Never use: bash -c "..."
- Never use bash -lc "..."
- Never wrap commands inside shell invocation strings.
- Execute commands directly as a regular interactive bash user would.

4. No workarounds
- Do not use bash -c as a workaround for environment or chaining.
- Do not simulate execution through subshell wrapping.
- Do not bypass the profile loading rule.

5. Separation of concerns
- Do not chain profile loading with execution.
- Do not prepend source to operational commands.
- Keep environment initialization and command execution logically separate.

6. User-context fidelity
- Commands must behave exactly as if typed manually in a regular bash session.
- Respect user PATH, aliases, environment variables defined in .bash_profile.
- Do not override environment unless explicitly instructed.

7. Compliance
- If a requested execution would violate these rules, stop and explain the violation.
- Never silently ignore this policy.

This execution policy is mandatory for all shell interactions.
