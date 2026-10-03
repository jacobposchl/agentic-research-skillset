# Agentic Research Skill Set

A lightweight collection of reusable skills for research workflows with coding agents.

The goal is to make common research tasks more consistent and efficient without introducing a large framework. The skill set is intentionally simple and can evolve alongside new workflows, tools, and integrations.

Contributions, modifications, and extensions are welcome.

## Skills

Skills package reusable instructions and context for repeatable tasks.

- **`code-summary`** — explain code at configurable levels of technical detail.
- **`results-summary`** — summarize, verify, and interpret experimental results.
- **`scaffold-code`** — define files, functions, classes, interfaces, and assumptions before implementation.

## Setup

Clone the repository and run:

```
./install_skills.sh
```

The installer links the skills into your Codex skills directory.

## Usage

Invoke a skill directly in your prompt using:

```
${skill}
```


Each skill defines its own options and expected behavior in its `SKILL.md`.