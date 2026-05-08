---
name: project-status-slice-report
description: Creates and updates a slice-based `project_status.md` plus per-slice `slice_report/` entries with required verification evidence. Use when the user mentions project status, roadmap/slices, slice reports, or updating status after tests pass.
disable-model-invocation: true
---

# Project Status + Slice Report

Creates and maintains two documentation artifacts **in any repo**:

- **`project_status.md`**: rolling slice roadmap with Mermaid diagram + slice table + decision log + update rules.
- **`slice_report/`**: a per-slice shipped report created **only when a slice is marked `done`**.

See detailed rules and templates in [REFERENCE.md](REFERENCE.md).

## Quick start

1. Decide the docs root:
   - If `docs/internal/` exists, use `docs/internal/`
   - Else use `docs/`
2. Ensure `project_status.md` exists (create from template if missing).
3. Update the slice table **exactly as directed by the user** (don’t infer status).
4. Sync the Mermaid diagram to the slice table using the **canonical `flowchart LR` style** (node shapes, `:::done` / `:::milestone`, `classDef` colors, edge layout)—see [REFERENCE.md](REFERENCE.md#mermaid-roadmap-canonical-style).
5. If a slice transitioned to `done`, create exactly one slice report entry for it.

## Workflows

### Create `project_status.md` (if missing)

- Create `project_status.md` using the reference template.
- Keep slice IDs stable (`S1`, `S2`, …), allow `D1` decision diamonds and `M1` milestones.
- Add a slice table with **Slice | Status | Description** and an **acceptance signal** per slice.

### Update status (user-driven)

- Apply only the status edits the user specifies.
- Never renumber slices; mark dropped slices as `cancelled`.
- Keep the Mermaid diagram in sync with the slice table: same nodes/status classes and branching order; follow [REFERENCE.md](REFERENCE.md#mermaid-roadmap-canonical-style).

### Create a slice report (only when slice becomes `done`)

- Write under `slice_report/<NNN>-<kebab-slug>/S<N> - <title>.md`.
- Include: What was done, Decisions, What shipped, How verified (commands + pass counts), Git commit (subject + SHA lookup), Notes/follow-ups.

## Advanced features

- Detailed detection rules, guardrails/redaction, and templates: see [REFERENCE.md](REFERENCE.md).
- Example outputs: see [EXAMPLES.md](EXAMPLES.md).

_Source: [mattpocock/skills](https://github.com/mattpocock/skills) (MIT). Adapted for Cursor by [eunai/skills](https://github.com/eunai/skills)._
