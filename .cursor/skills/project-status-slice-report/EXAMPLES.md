## Examples

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

