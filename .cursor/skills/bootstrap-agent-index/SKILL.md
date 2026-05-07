---
name: bootstrap-agent-index
description: Creates or updates a maintainer-facing agent_index.md that is the canonical start-here entrypoint for AI agents and contributors. Use when the user says bootstrap agent index, create agent index, refresh maintainer entrypoint, or asks for a maintainer navigation index.
disable-model-invocation: true
---

# Bootstrap Agent Index

Create or update a maintainer-facing `agent_index.md` that is the canonical "start here" document for AI agents and contributors in this repo.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/bootstrap-agent-index/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default target: `docs/internal/agent_index.md` when `docs/internal/` exists, otherwise `docs/agent_index.md`.
- Existing files: read the existing file first, summarize what would change vs the scaffold, then ask once. Default to merge for files and add-only for directories. Replace only on explicit request, and never delete user content.
- Do not invent doc paths, commands, architecture rules, or decisions. Use TODO placeholders for anything that requires repo-specific judgment.

## Start Detection
1. Resolve the target path from the override or docs-root heuristic.
2. Read the repo tree for real navigation targets:
   - Spec docs such as `project_spec.md`, `design.md`, or equivalent.
   - Module docs such as `modules/README.md`.
   - Agent playbooks such as `agents/README.md`.
   - Recommended-library or ADR docs.
   - Publish-boundary docs such as `publish_whitelist_tree.md` and matching publisher scripts, if present.
3. Derive authoritative commands from real files only:
   - Python: `pyproject.toml`, `requirements*.txt`, `tox.ini`, `noxfile.py`.
   - JavaScript/TypeScript: `package.json` scripts.
   - Make or task runners: `Makefile`, `justfile`, `Taskfile.yml`, or similar.
4. If the target already exists, compare it to the required section scaffold and report the intended merge before writing.

## Required Document Shape
Write these sections in this order:
1. `Quick Navigation` - links to real docs only: spec, modules doc, agent playbooks, recommended libraries, ADRs, and publish boundary if relevant.
2. `Commands` - authoritative install, run, and test commands derived from repo files. Mark unknown commands as TODO instead of guessing.
3. `Agent Boundaries` - `ALWAYS`, `ASK FIRST`, and `NEVER` subsections with TODO prompts for repo-specific rules.
4. `Decision Gates` - choices that must remain ask-first until pinned.
5. `Where Do I Look?` - task-to-doc map using only existing or newly confirmed paths.
6. `Project Spec Summary TOC` - high-signal map of the spec doc if one exists; otherwise a short TODO stub.
7. `Drift Watch` - paired files or docs that must update together.
8. `Architecture Invariants` - enforced or expected invariants, using TODOs where judgment is needed.

## Scaffold Template
Use this base for a new file, filling only facts discovered from the repo:

```md
# Agent Index
Maintainer-facing start-here guide for AI agents and contributors.

## Quick Navigation
- Project spec / modules / playbooks / libraries / publish boundary: TODO
## Commands
- Install / Run / Test: TODO
## Agent Boundaries
- ALWAYS: TODO
- ASK FIRST: TODO
- NEVER: TODO
## Decision Gates
- TODO: Choices that remain ask-first until pinned.
## Where Do I Look?
| Task | Start here |
| --- | --- |
| Understand scope / modules / libraries / publish boundary | TODO |
## Project Spec Summary TOC
TODO: Add high-signal spec map, or note that no spec exists yet.
## Drift Watch
- TODO: Pair docs and files that must change together.
## Architecture Invariants
- TODO: Record enforced or expected invariants.
```

## Merge Behavior
- For a missing target, propose the scaffold path and write only after any needed directory confirmation.
- For an existing target, preserve all user content. Merge by filling empty sections, adding missing required sections, and appending new discovered links or drift-watch rows.
- If replacing seems cleaner, ask explicitly and wait for the user to request replace. Never replace by default.
- After writing, summarize links filled, TODOs left, and commands discovered.

## AGENTS.md Handling
Handle this only after the `agent_index.md` write or merge step:
- If repo-root `AGENTS.md` is missing, offer a minimal stub pointing at the resolved `agent_index.md`, but create it only with consent.
- If repo-root `AGENTS.md` exists, append a `Maintainer navigation` block only after explicit confirmation.
- Never wholesale replace `AGENTS.md`.

## Next
- Run **bootstrap-modules-readme** for module and dependency boundaries.
- Run **bootstrap-agent-playbooks** to create per-topic guides under `agents/`.
- Run **maintain-recommended-libraries** for the trade-off memo this index links.
