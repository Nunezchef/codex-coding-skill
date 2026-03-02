# Codex Coding Skill For Agent Zero

Use Codex CLI as the default coding engine inside [Agent Zero](https://github.com/agent0ai/agent-zero).

This repository packages a single `SKILL.md` that teaches Agent Zero to route all coding work through Codex in the terminal, instead of treating code tasks as ordinary prose tasks.

## What It Does

- Makes Codex CLI the default path for coding tasks
- Uses Agent Zero's `code_execution_tool` to run `codex`
- Teaches a full terminal workflow, not a one-shot prompt
- Covers implementation, debugging, refactoring, testing, and code review
- Adds guidance for interactive follow-up when Codex asks for confirmation or more context

## Intended Environment

This skill is intended for **Agent Zero** users who have:

- Agent Zero running and configured
- Codex CLI available in the runtime environment
- A workflow where Agent Zero can execute terminal commands through `code_execution_tool`

If you are not using Agent Zero, you can still adapt the skill, but the examples and tool names in this repository are written for Agent Zero specifically.

## Repository Contents

- `SKILL.md` — the skill itself
- `README.md` — usage and installation guide
- `LICENSE` — MIT license

## Installation

Copy this repository's `SKILL.md` into your Agent Zero user skills directory.

Example:

```bash
mkdir -p /a0/usr/skills/codex-coding
cp SKILL.md /a0/usr/skills/codex-coding/SKILL.md
```

If your Agent Zero environment uses a different skills path, copy it into the equivalent user skill directory for that installation.

## How To Use

Activate the skill when the user asks for any coding task:

- write code
- edit code
- debug
- refactor
- add tests
- review code
- scaffold a script or app

The skill teaches the agent to:

1. Gather enough repository context first
2. Form a strong Codex prompt
3. Run `codex` through `code_execution_tool`
4. Continue interacting through the terminal if Codex asks follow-up questions
5. Verify results before reporting success

## Example Use Cases

- "Implement this feature in the current repo"
- "Debug this failing test"
- "Refactor this module without changing behavior"
- "Review these changed files for regressions"

## Why This Skill Exists

The default "just run `codex` once" pattern leaves a lot of Codex's value on the table.

This skill is designed to help Agent Zero use Codex as a real terminal coding engine:

- with repository-aware prompts
- with iterative follow-up
- with explicit verification
- with better handling of interactive confirmations

That makes the integration more useful for serious coding workflows.

## Notes

- This repository does not include Codex CLI itself.
- It does not modify Agent Zero core behavior by itself.
- It only provides a reusable Agent Zero skill that you can install into your environment.

## Contributing

Improvements to the skill wording, examples, or Agent Zero-specific guidance are welcome.

If you make the skill more reliable with local models or stronger for repo-scale workflows, that is especially useful.
