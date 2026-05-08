---
name: vertical-slice-planner
description: Turns an issue, PRD, or rough idea into thin independently shippable vertical slices in a Markdown plan. Use when the user says slice planner, plan slices, break this into slices, or asks for a vertical-slice plan.
---

# Vertical Slice Planner

Break an issue, PRD, or rough idea into thin, independently shippable vertical slices. Default output is a Markdown plan file whose slices map to future **slice-summary** campaign folders.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/vertical-slice-planner/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default folder: `docs/internal/plans/` when `docs/internal/` exists, otherwise `docs/plans/`.
- Existing files: read first, summarize changes, ask once. Defaults: merge for files, add-only for directories, replace only on explicit request. Never delete user content.
- Thin orchestration: end with the Next hints below.

## Filename And Numbering
- Filename: `YYYY-MM-DD-<slug>-slice-plan.md`.
- Same-day numbering rule:
  - First artifact for a date has no suffix.
  - When a second artifact lands the same day, rename the first to use `_01` and create the new one as `_02`. Subsequent ones use `_03`, `_04`, ...
  - Date format is hyphenated (`YYYY-MM-DD`). No mixed dots/hyphens.
- When renaming a same-day plan, run link repair against the docs root for the renamed file.

## Required Sections
1. Context: one paragraph plus issue, PRD, or chat reference.
2. Goal: what done means for the whole plan.
3. Out of scope: explicit non-goals.
4. Slice rows: each includes planned campaign folder basename, scope, acceptance checks, test plan sketch, risks/unknowns.
5. Suggested order and dependency notes.

## Write Gate
Before writing:
1. Compute target path from docs-root heuristic and filename rules.
2. Print target path and a one-screen outline of slice rows.
3. Ask exactly one Y/N question: `Write this plan to <path>?`
4. On Y, write using merge-safe rules. On N, revise based on feedback or stop; write nothing.

## Override
- If the user says `abstract`, `plan only`, or `do not write a file`, produce the plan in chat and skip the write gate.

## Template
```md
# <Title> Slice Plan

## Context
TODO

## Goal
TODO

## Out Of Scope
- TODO

## Slices
| Campaign Folder | Scope | Acceptance Checks | Test Plan | Risks / Unknowns |
| --- | --- | --- | --- | --- |
| YYYY-MM-DD-<slug> | TODO | TODO | TODO | TODO |

## Suggested Order
TODO
```

## Next
- Use **slice-summary** to execute one slice and produce `slice_summary.md`.
- Use **project-status-doc** so campaign rows appear in the rolling status.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
