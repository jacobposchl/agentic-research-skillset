---
name: results-summary
description: Summarize, interpret, and critically evaluate experimental results.
---

**Only use this skill when explicitly invoked by the user.**

## Mode (`--mode`)

- `scan`: Inspect the results and surface the most important things the user should know.
- `critique`: Evaluate and stress-test the user's interpretation of the results.

Default: `scan`.

## `scan`

Use when the user wants the agent to inspect results without requiring a prior interpretation.

Prioritize:

- whether the run appears valid
- signs of model, training, data, or evaluation failure
- lack of learning, instability, collapse, leakage, or suspicious metrics
- unexpected or unusual structure in the results
- the strongest positive or negative findings
- discrepancies across splits, sessions, seeds, groups, conditions, or baselines
- patterns that deserve further inspection
- the most useful next analysis or diagnostic

Inspect relevant code, configuration, logs, figures, and result files when needed to correctly understand what was measured.

Do not force a rigid report structure. Lead with the most consequential findings.

Avoid over-interpreting weak, noisy, or under-controlled results.

## `critique`

Use when the user provides an interpretation, hypothesis, or broader analysis they want evaluated.

Assess:

- whether the presented results actually support the interpretation
- which parts of the claim are strongly supported, weakly supported, or unresolved
- relevant assumptions and confounds
- plausible alternative explanations
- whether the analysis or metric could produce the observed result artifactually
- inconsistencies with other available results
- what additional evidence would distinguish competing explanations
- the most informative follow-up analysis or experiment

Use relevant implementation details, experimental setup, prior results, and broader project context when available.

Treat the user's interpretation as a hypothesis to evaluate rather than an assumption to preserve.

## General behavior

- Distinguish observations from interpretations.
- Prefer concrete evidence from the available results.
- State uncertainty when the evidence is ambiguous.
- Do not invent explanations for missing information.
- Be concise and prioritize findings that affect the user's next decision.

## Examples

The mode definitions above are sufficient for routine use. Read [the worked examples](references/example.md) only when uncertain about the distinction between modes or how to calibrate conclusions to the available evidence. Adapt the structure and length to the user's request and results.
