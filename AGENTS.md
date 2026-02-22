AGENT Governance Policy

AGENTS.md is the authoritative governance document of this repository.
It defines architecture, structure, responsibilities, constraints, and operational assumptions.

0. Mandatory existence rule
- If AGENTS.md does not exist in the repository, you must:
  1) Perform a full repository review.
  2) Analyze architecture, structure, responsibilities, runtime model, build process, and infrastructure aspects.
  3) Create a complete AGENTS.md before making any other modification.
- No code, configuration, or structural changes are allowed before AGENTS.md is created.

1. Mandatory startup behavior
- At the beginning of every session, read AGENTS.md before performing any reasoning, analysis, or modification.
- Do not rely on memory or prior context.
- Never assume repository structure without reading AGENTS.md first.

2. Continuous change verification
- Before executing any non-trivial action (code change, refactor, rename, delete, infrastructure modification, configuration update, dependency change, structural change), check whether AGENTS.md has changed.
- If it has changed since the last read, re-read it fully.
- Never use a cached version.

3. Change reflection requirement
- Any modification that affects architecture, structure, runtime behavior, build process, infrastructure, responsibilities, or constraints must be reflected in AGENTS.md.
- Update AGENTS.md in the same task where the change is introduced.
- Never leave AGENTS.md outdated.

4. Structural integrity enforcement
AGENTS.md must always contain:
- Project purpose
- Architectural overview
- Functional structure
- File structure explanation
- Module/service responsibilities
- Execution model (runtime/build/infra)
- Explicit constraints and policies

5. Conflict handling
- If a requested change contradicts AGENTS.md, do not proceed silently.
- Explicitly identify the conflict.
- Propose an AGENTS.md update.
- Update AGENTS.md before implementing the conflicting change.

6. Post-change validation
After any modification:
- Re-evaluate AGENTS.md.
- Verify it matches the real repository state.
- Ensure new files, removed files, renamed files, and changed behavior are documented.
- If mismatch exists, fix AGENTS.md before finishing.

7. Deterministic documentation rule
- AGENTS.md must be technical, precise, and deterministic.
- No marketing language.
- No ambiguity.
- No historical commentary.
- It must describe the current state only.

8. Governance strictness
- Never override AGENTS.md implicitly.
- Never introduce structural drift.
- Treat AGENTS.md as a living but controlled governance contract.

This policy is mandatory and must be enforced automatically in every interaction.
