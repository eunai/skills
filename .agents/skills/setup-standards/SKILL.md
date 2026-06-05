---
name: setup-standards
description: Bind a repository to the commit-standards skill by writing an invoke-before-commit pointer into the repo's agent-read source-of-truth doc (AGENTS.md, CLAUDE.md, etc.). Use when the user wants to set up, install, or enforce commit message standards in a repo, or invokes /setup-standards. Not for writing individual commit messages (use commit-standards).
---

# Setup Standards

Bind the current repository to the `commit-standards` skill: write a pointer into the repo's source-of-truth doc so any agent or human about to commit is told to invoke `commit-standards` first. One-time, per-repo setup.

This skill writes a **pointer only**. It does not capture repo-local commit rules and does not install git hooks — `commit-standards` enforces softly, and hard hooks are out of scope.

## Workflow

1. **Scan** the repo for candidate source-of-truth docs, in priority order:
   1. `AGENTS.md`
   2. `CLAUDE.md`
   3. `.github/copilot-instructions.md`, `.cursor/rules/*`
   4. `CONTRIBUTING.md`
   5. `.gitmessage`

   Agent-read files outrank human-read files — the pointer's main audience is agents that will not invoke the skill on their own.

2. **Present** what you found as **plain text** (never `AskUserQuestion`): list the candidates that exist, **highlight the recommended one** (the highest-priority agent-read file present) with a one-line reason, and let the user pick or redirect. If none exist, recommend creating `AGENTS.md`.

3. **Write** the pointer into the single chosen doc, formatted for that doc's type — a `## Commit messages` markdown section for `.md` files, or `#`-prefixed comment lines for `.gitmessage`:

   > ## Commit messages
   >
   > Before running `git commit`, invoke the `commit-standards` skill to author or validate the message against Conventional Commits, plus any commit rules stated in this file. Do this for every commit.

4. **Upsert — never duplicate:**
   - No existing pointer → insert the section.
   - Pointer present and current → no-op; report "already bound."
   - Pointer present but stale → update it in place.
   - A *different*, non-ours commit instruction already exists → surface it in plain text and let the user choose **replace / keep both / leave it**.

## Do not

- Do not write individual commit messages — that is `commit-standards`.
- Do not fan the pointer into multiple docs; write one canonical home and re-run for another.
- Do not install git hooks or capture overlay rules — out of scope for this skill.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
