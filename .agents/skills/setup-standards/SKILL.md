---
name: setup-standards
description: Binds a repository to selected commit, README, and changelog standards by writing invoke-before-work pointers into the repo's agent-read source-of-truth doc. Use when the user wants to set up or enforce repository standards, or invokes /setup-standards.
---

# Setup Standards

Bind the current repository to one or more standards packs. Write concise
pointers into one source-of-truth doc so agents invoke the relevant skill before
committing or editing documentation.

This skill writes pointers only. It does not copy the standards into the target
repo or install git hooks.

## Workflow

1. **Choose standards first.** Before scanning or editing, present this
   plain-text menu and wait for the user's selection:

   ```text
   Which standards do you want to add?
   (a) Commit message standards
   (b) README standards
   (c) Changelog standards
   (d) All standards
   ```

   Accept `a`, `b`, `c`, or `d`. Do not infer a selection or use a UI question
   tool.

2. **Scan** the repo for candidate source-of-truth docs, in priority order:
   1. `AGENTS.md`
   2. `CLAUDE.md`
   3. `.github/copilot-instructions.md`, `.cursor/rules/*`
   4. `CONTRIBUTING.md`
   5. `.gitmessage`

   Agent-read files outrank human-read files because agents are the primary
   audience.

3. **Choose the target.** In plain text, list existing candidates, mark the
   highest-priority agent-read file as recommended with a one-line reason, and
   wait for the user to choose or redirect. If none exist, recommend creating
   `AGENTS.md`.

4. **Write** only the selected pointers into the single chosen doc. For
   Markdown files, use these sections:

   ```markdown
   ## Commit messages

   Before running `git commit`, invoke the `commit-standards` skill to author or validate the message against Conventional Commits, plus any commit rules stated in this file. Do this for every commit.

   ## README maintenance

   Before editing a README, invoke the `readme` skill and follow any README rules stated in this file.

   ## Changelog maintenance

   Before editing a changelog or release notes, invoke the `changelog` skill and follow any changelog rules stated in this file.
   ```

   For `.gitmessage`, write the selected text as `#`-prefixed comments.

5. **Upsert each selected pointer; never duplicate.**
   - Insert a missing pointer.
   - Leave a current pointer unchanged and report it as already bound.
   - Update a stale pointer in place.
   - Leave unselected standards unchanged.
   - If a different instruction conflicts with a selected pointer, show it in
     plain text and let the user choose `replace`, `keep both`, or `leave it`.

## Do not

- Do not write individual commit messages; use `commit-standards`.
- Do not edit README or changelog content; use `readme` or `changelog`.
- Do not fan pointers into multiple docs; write one canonical home and re-run
  for another.
- Do not install hooks or copy full standards into the target doc.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
