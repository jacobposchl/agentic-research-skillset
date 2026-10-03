---
name: scaffold-code
description: Design and create a code scaffold for user verification before substantive implementation.
---

**Only use this skill when explicitly invoked by the user.**

## Purpose

Use this skill when the user has a rough idea of what should be implemented and wants to articulate the structure before writing the full implementation.

Adapt the scaffold to the requested scope, including folder-, file-, class-, or function-level changes.

## Workflow

### Phase 1 — Design

Before modifying code:

1. Inspect relevant existing code, project structure, and `AGENTS.md` instructions.
2. Determine the structure needed for the requested implementation.
3. Present a concise proposed scaffold containing, when relevant:
   - file/module tree
   - functions and classes
   - responsibility of each component
   - major inputs and outputs
   - interfaces and data flow between components
   - important assumptions or unresolved design choices
4. Include a simple diagram when it improves understanding.
5. Do not write or modify project files.
6. Stop and wait for explicit user approval or requested revisions.

Do not proceed to Phase 2 without approval.

### Phase 2 — Scaffold

After the user approves the design:

1. Create only the approved files and code structure.
2. Add appropriate:
   - function and method signatures
   - class definitions
   - docstrings
   - type hints when appropriate
   - TODOs or concise comments describing intended logic
3. For each substantive component, make the intended:
   - inputs
   - outputs
   - behavior
   - assumptions
   clear from the scaffold.
4. Preserve existing project conventions and `AGENTS.md` instructions.
5. Do not implement substantive algorithmic logic unless explicitly requested.
6. Stop after the scaffold has been created.

## General behavior

- Prefer the simplest structure that cleanly represents the requested implementation.
- Avoid introducing unnecessary abstractions, files, classes, or dependencies.
- Preserve compatibility with the surrounding codebase.
- Surface ambiguous design decisions during Phase 1 rather than silently choosing them.
- Keep diagrams and explanations focused on structure and flow rather than implementation details.
- Treat later implementation of the scaffold as a separate task.