# AGENTS.md

## Project Purpose

This repository stores copy-ready prompt documents for AI agent rules, custom instructions, system prompts, and similar configuration surfaces.
Its purpose is to keep reusable prompt policies in a small, explicit, and easy-to-copy form.

The repository is documentation-only.
It does not contain application source code, runtime services, deployment manifests, or infrastructure definitions.

## Architectural Overview

The repository has three active documents:

- `AGENTS.md`: repository governance and repository-purpose document.
- `README.md`: file index for the repository.
- `CODING.md`: coding policy document intended to be copied into AI rules or custom instructions.

`AGENTS.md` describes what this repository is for and what each document is responsible for.
`CODING.md` contains the actual coding policy.

## Functional Structure

- Governance layer: `AGENTS.md`
- Index layer: `README.md`
- Prompt-policy layer: `CODING.md`

## File Structure Explanation

- `AGENTS.md`
  Explains the purpose of the repository and the role of each document.
- `README.md`
  Lists the files in the repository and links to them.
- `CODING.md`
  Contains instructions for coding work in a copy-ready format.

No other repository files are required for the current state.

## Module and Document Responsibilities

- `AGENTS.md`
  Describes what this repository is, why it exists, and what `README.md` and `CODING.md` are for.
- `README.md`
  Provides a clickable index of the files in the repository.
- `CODING.md`
  Provides the actual coding instructions that are meant to be copied and reused.

## Execution Model

- Runtime: none
- Build: none
- Infrastructure: none

The repository exists only to store and maintain prompt documents.

## Explicit Constraints and Policies

- `AGENTS.md` must explain the repository purpose and document roles.
- `AGENTS.md` must not duplicate the full coding policy stored in `CODING.md`.
- `CODING.md` is where coding instructions belong.
- `README.md` should stay a simple clickable file index.
- Read `AGENTS.md` at the beginning of every session.
- Re-read `AGENTS.md` before non-trivial changes if it may have changed.
- Update `AGENTS.md` whenever repository purpose, file roles, or document structure changes.

## Conflict Handling

- If a document role changes, update `AGENTS.md` in the same task.
- If a new prompt-policy document is added, document its role in `AGENTS.md` and `README.md`.
- `AGENTS.md` must always remain present in the repository.

## Change Control

- Keep `AGENTS.md` focused on repository description, not duplicated policy content.
- Keep `CODING.md` focused on copy-ready coding rules.
- Keep `README.md` focused on indexing and linking the repository files.
