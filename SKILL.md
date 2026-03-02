---
name: codex-coding
description: Use when the user asks for any coding task including writing, editing, debugging, refactoring, testing, reviewing code, or implementing software changes.
---

# Use Codex CLI For All Coding Tasks

## Overview

For coding work, Codex CLI is the primary execution engine.

When the user asks for any coding task, do not treat it as a normal prose-only task first. Use `code_execution_tool` to drive `codex` in the terminal, keep the interaction going until Codex finishes or blocks, then report the result.

## When To Use

Use this skill for all coding tasks, including:

- Writing new code
- Editing existing files
- Debugging errors
- Refactoring
- Creating or fixing tests
- Reviewing code with file-level findings
- Scaffolding apps, scripts, services, or libraries
- Repo-aware implementation work

Do not use this skill for non-coding conversation, pure brainstorming, translation, or simple factual questions that do not require code changes or code inspection.

## Core Rule

For coding tasks:

1. Gather enough context to form a good prompt.
2. Use `code_execution_tool` to run `codex`.
3. Continue interacting through the terminal as needed.
4. Verify the result before claiming success.

Do not start writing the code directly outside Codex unless:

- Codex is unavailable
- Codex repeatedly fails after prompt refinement
- The user explicitly tells you not to use Codex

## Execution Pattern

### Step 1: Gather Context First

Before invoking Codex, collect the minimum context needed to avoid a weak prompt:

- Current working directory or repo path
- Relevant file names or directories
- The exact change requested
- Constraints (framework, style, no extra dependencies, etc.)
- Verification expectations (tests, lint, build, output)

If the task references an existing codebase, inspect the repository enough to give Codex concrete instructions.

### Step 2: Build A Strong Codex Prompt

Your prompt to Codex should include:

- The exact task
- Relevant repo or file context
- Constraints and non-goals
- Expected verification steps
- What output you want back

Good prompts are specific and operational.

Example:

```text
In /a0/usr/workdir, add a FastAPI endpoint at /health in the existing app.
Modify the current files instead of creating a new server.
Keep the existing style and dependencies.
After editing, run the relevant tests or a quick import check.
Summarize the files changed and any follow-up issues.
```

### Step 3: Run Codex Through `code_execution_tool`

Use `code_execution_tool` with a terminal command like:

```json
{
  "tool_name": "code_execution_tool",
  "tool_args": {
    "runtime": "terminal",
    "code": "cd /a0/usr/workdir && codex \"In this repository, ...\""
  }
}
```

Always prefer:

- `cd` into the target repo first
- one clear natural-language prompt
- explicit repository-aware instructions

### Step 4: Stay In The Codex Loop

After starting Codex, do not assume one command is enough.

Read the terminal output and continue the workflow:

- If Codex asks for confirmation, answer it
- If Codex asks for more context, provide it
- If Codex finishes partially, run a refined follow-up prompt
- If Codex suggests next verification steps, execute them

Treat Codex as an interactive coding session, not a single fire-and-forget command.

### Step 4a: Handle Interactive Follow-Up Explicitly

When Codex prompts in the terminal, answer the prompt directly and keep the session moving.

Common interactive cases:

- Confirmation to proceed with edits
- Confirmation before running tests or commands
- Requests for missing file paths, framework details, or constraints
- Requests to choose between multiple implementation options

Respond with concise, operational answers.

Examples:

```text
Yes, proceed with the edits.
```

```text
Limit the changes to src/api and tests/api only.
```

```text
Do not add dependencies. Reuse the existing libraries already in the repo.
```

```text
Run the targeted test file only, not the full suite.
```

If Codex asks a question that changes scope, safety, or requirements, pause and ask the user instead of guessing.

If Codex gets stuck in a broad or vague prompt loop, stop the current pass and re-run Codex with a narrower, clearer prompt.

### Step 5: Verify Before Reporting Back

After Codex changes code:

- Review the terminal output
- Inspect changed files or diffs when needed
- Run tests, lint, or build checks if the task requires them
- Confirm whether the requested change actually happened

Do not report success just because Codex printed a plausible message.

## Prompt Templates

### Implementing A Feature

```text
In [REPO_PATH], implement the following feature:
[EXACT FEATURE]

Constraints:
- [CONSTRAINT 1]
- [CONSTRAINT 2]

Use the existing code patterns in this repository.
Modify existing files where appropriate.
Run relevant verification after the change.
Return a concise summary of changed files and any remaining issues.
```

### Debugging

```text
In [REPO_PATH], debug and fix this problem:
[ERROR OR FAILURE]

Focus on the root cause, not a workaround.
Inspect the relevant code and make the minimum necessary fix.
Run the most relevant verification command after fixing it.
Summarize the root cause, the fix, and the verification result.
```

### Refactoring

```text
In [REPO_PATH], refactor [TARGET AREA].

Preserve behavior.
Reduce complexity or duplication without changing external behavior.
Avoid unrelated edits.
Run relevant checks after the refactor.
Summarize what changed and any risks to review.
```

### Code Review

```text
In [REPO_PATH], review [FILES OR CHANGE AREA].

Focus on bugs, regressions, security risks, missing tests, and maintainability issues.
Do not rewrite the code unless explicitly asked.
Return concrete findings with file references when possible.
```

## Failure Handling

If Codex does not complete the task:

- Refine the prompt with better file paths, tighter scope, or clearer constraints
- Break the task into smaller Codex calls
- Ask the user a targeted question only if a real blocker remains

If Codex is unavailable or repeatedly fails:

- State the exact blocker
- Explain what was attempted
- Then continue with a fallback approach only if necessary

## Important Notes

- For coding tasks, Codex CLI is the default engine, not an optional helper.
- Prefer repository-aware prompts over generic one-liners.
- Use multiple Codex passes when the task needs iteration.
- Keep the human updated with concrete progress, not vague summaries.
- Verify output before claiming the task is done.
