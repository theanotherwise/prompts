# AGENTS.md

## Project Purpose

This repository stores prompt documents for AI agent rules, custom instructions, and similar prompt configuration surfaces.

## Architectural Overview

This is a documentation-only repository.

## Prompt File Format

Prompt-oriented files intended for direct paste into agent or custom-instruction surfaces should be stored as paste-ready Markdown snippets in the same format as [CODING.prompt.md](./CODING.prompt.md).
Files following the `*.prompt.md` naming convention should contain the prompt payload as Markdown inside a fenced `markdown` code block.

## Functional Structure

- [AGENTS.md](./AGENTS.md): explains what this repository is for.
- [README.md](./README.md): clickable index of the files in this repository.
- [CODING.prompt.md](./CODING.prompt.md): coding policy stored in the repository's prompt-ready format for agent instructions and custom instruction surfaces.
- [SECURITY_AUDIT.prompt.md](./SECURITY_AUDIT.prompt.md): security review prompt for application assessments focused on auth, data exposure, SSR, edge behavior, and abuse paths.
