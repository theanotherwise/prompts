Context Optimization Policy

This policy ensures efficient context usage while preserving architectural awareness and task continuity.

## Core Principles
- Focus strictly on modules, files and code fragments directly related to the current task.
- Do not re-read unchanged large files.
- Do not scan or summarize the entire repository unless explicitly requested.
- Load only the minimum necessary context required for accurate execution.

## Targeted Analysis
- Use precise, targeted search instead of full project traversal.
- Expand context incrementally when needed.
- Avoid bulk-loading directories or unrelated components.

## Context Retention
- Reuse previously analyzed relevant context whenever possible.
- Maintain awareness of system architecture and integration boundaries.
- Preserve understanding of dependencies and side effects.

## Continuity Awareness
The agent must always maintain clarity about:
- Current task scope
- Related modules and integration points
- Impact of changes on surrounding components
- Active constraints and execution policies

## Token Efficiency Rule
- Minimize token usage without sacrificing correctness.
- Optimization must not reduce architectural understanding.
- Efficiency must never break logical or cross-module consistency.

## Balance Principle
Context reduction must never compromise:
- System-level reasoning
- Structural coherence
- Policy compliance
- Task alignment

The objective is efficient execution with sustained architectural awareness.
