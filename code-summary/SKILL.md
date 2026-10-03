---
name: code-summary
description: Summarize code structure, inputs, outputs, assumptions, and dependencies.
---

**Only use this skill when explicitly invoked by the user.**

## Detail level (`--detail`)

- `surface`: High-level explanation of the methodological setup and behavior. Do not include code-level details.
- `medium`: Explain the methodological setup, important implementation choices, major assumptions, and key considerations. Use code details only when useful.
- `deep`: Detailed synthesis organized around inputs, outputs, major operations, dependencies, implementation choices, assumptions, and relevant edge cases.

Default: `medium`.

## Examples

The detail-level definitions above are sufficient for routine use. Read [the worked examples](references/example.md) only when uncertain about the intended depth or the distinction between modes. Adapt the structure and length to the code being summarized.
