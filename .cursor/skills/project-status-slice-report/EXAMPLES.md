## Examples

### Example: Mermaid roadmap (house style)

Diagrams use `flowchart LR`, quoted slice labels, `:::done` on finished work, double-parenthesis milestones, optional `V1` horizon node, the two fixed `classDef` lines, and multi-line edge chains when the spine is long:

```mermaid
flowchart LR
  S1["S1 Slice A summary"]:::done
  S2["S2 Slice B summary"]
  M1(("First usable core")):::done

  S1 --> S2 --> M1

  classDef done fill:#1f6f3a,color:#fff,stroke:#0f3,stroke-width:1px;
  classDef milestone fill:#264653,color:#fff,stroke:#a0c8d8,stroke-width:1px;
```

### Example: slice report path

If the slice is `S9 — Import flow TUI (picker preview commit)`, create:

- `docs/internal/slice_report/009-import-flow-tui/S9 - Import flow TUI (picker preview commit).md`

If the repo has no `docs/internal/`, create:

- `docs/slice_report/009-import-flow-tui/S9 - Import flow TUI (picker preview commit).md`

### Example: verification evidence

Include concrete commands and outcomes:

- `pytest tests/integration/test_import_flow_tui.py`: **1 passed**.
- `pytest`: **38 passed**.

### Example: commit SHA lookup

- **Subject:** `Wire import flow preview and commit`
- **SHA lookup:** `git log -1 --format=%H --grep="Wire import flow"`

