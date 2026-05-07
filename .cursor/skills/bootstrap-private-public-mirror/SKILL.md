---
name: bootstrap-private-public-mirror
description: Phased bootstrap for a private repo that publishes a sanitized public mirror via git filter-repo—docs split, public stubs, publish.ps1, whitelist doc, link repair, optional dry-run. Use when the user says bootstrap private public mirror, set up public mirror, create publish script, or split docs internal public.
disable-model-invocation: true
---

# Bootstrap private–public mirror

**Purpose.** Set up a private repo to publish a sanitized public mirror using `git filter-repo`. In one phased run (unless validate-only): `docs/internal/` + `docs/public/` split, `README.public.md`, `CHANGELOG.public.md`, optional `.gitignore.public`, a private `README.md` **Publishing** hint, `scripts/publish.ps1`, and `docs/internal/publish_whitelist_tree.md`. **This is the only pack skill that moves existing files.** Idempotent on re-runs (detect partial state, offer gap-fill). **Validate-only** skips Phases 2–7 and jumps to Phase 8 when `scripts/publish.ps1` and `docs/internal/publish_whitelist_tree.md` already exist.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Self-contained runtime rules (embed behavior)

- Skill folder: `.cursor/skills/bootstrap-private-public-mirror/` (this file + optional `REFERENCE.md`). No absolute paths from other repos. Use **REFERENCE.md** for long templates, heuristic tables, and script/doc skeletons the agent must materialize in the **target** repo.
- **Docs-root heuristic (use exactly this wording when explaining defaults to the user):**
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- **Existing files (merge-safe):** Read → summarize changes vs template → ask once. Defaults: **merge** for files (fill empty sections, append dated blocks), **add-only** for directories. **Replace** only on explicit user request. Never delete user content.
- **Hard rules (never violate):**
  - Only this skill moves existing docs; always **`git mv`** (preserve history). Never delete originals to “make room.”
  - Public remote URL appears only in `scripts/publish.ps1` default parameter and the whitelist doc intro—not in source, tests, or fixtures.
  - Forbidden-pattern scan runs **after** `filter-repo`, **before** push; abort on any match; never relax.
  - Cross-check is opt-out via `-SkipDocCheck` only; never silently skip.
  - Log start-of-run overrides in Phase 1 plan output: `force fresh`, `force convert`, `make AGENTS.md public`, `also write bash publisher`, `include code`.

## Prerequisites

Target repo should have `.cursor/skills/write-a-skill/SKILL.md` for consistency with the pack; this skill does not depend on it at runtime.

## Trigger phrases

- “bootstrap private public mirror”
- “set up public mirror”
- “create publish script”
- “split docs internal public”

## Idempotency and validate-only

On skill **start**, if **both** `scripts/publish.ps1` and `docs/internal/publish_whitelist_tree.md` exist:

1. Phase 1 detect reports **partial bootstrap detected**.
2. Offer **validate-only**: user confirms → skip Phases 2–7 and execute **Phase 8** only (dry-run gate and optional run).

If only one exists, run Phase 1 fully and offer gap-fill for missing artifacts per phase rules.

## Run mode (auto-detect + confirm)

- **CONVERT** if any: `docs/` has content; `README.md` non-trivial; `CHANGELOG.md` non-trivial; or repo has commits beyond an initial commit.
- **FRESH** otherwise.

Phase 1 prints detected mode with a **single Y/N**. Honor start-of-run overrides: **force fresh** / **force convert** (log in plan).

## Public remote URL

1. `git remote -v`. If `origin` URL ends in `-dev`, `-private`, or `-internal`, propose sibling by stripping suffix (e.g. `LedgerTUI-dev` → `LedgerTUI`). **Single Y/N.**
2. If detection fails, **ask once** for the URL.
3. **Refuse to finish past Phase 5** without a confirmed URL.
4. Write URL into script as `$PublicRemote` default; **never** use `<REPLACE_ME>`.
5. Script header `.NOTES` (or equivalent comment block): `Repository: <private_url> -> <public_url>`.
6. Default branch: `git symbolic-ref refs/remotes/origin/HEAD`; fallback `main`.

## Phase ordering

**Phase 1’s plan output must print this list verbatim** (logical order; execution follows the “Execution order” section):

```text
Phase 1 Detect & plan        (read-only)
Phase 2 Docs split           [CONVERT only]
Phase 6 Link repair          [skipped if no moves]
Phase 3 Public stubs
Phase 4 Private augmentation
Phase 5 Publisher + whitelist doc
Phase 7 AGENTS.md policy     [opt-in]
Phase 8 Dry-run validation   [Y/N at end]
```

**Execution order:** Phase 1 → (Phase 2 if CONVERT) → Phase 6 (if path changes from Phase 2; else skip) → Phase 3 → Phase 4 → Phase 5 → Phase 7 → Phase 8.

### Phase 1 — Detect & plan (read-only)

Print: mode (+ overrides), public URL proposal + Y/N, default branch, **Phase 2 migration table** (CONVERT only; see REFERENCE), **Phase 5 whitelist + forbidden-pattern proposal** (see REFERENCE), **AGENTS.md stance** (default hard-private; override named), **phase list verbatim**, **abort/skip contract**:

| Gate | On N |
|------|------|
| Phase 3 | Empty scaffolds; continue |
| Phase 5 | Targeted re-prompt: (a) Revise default, (b) Abort, (c) Skip degraded—no publisher reference; mark summary “partial; publisher pending” |
| Phase 6 | Continue with drift warning; final summary recommends `docs-drift-audit` |
| Phase 7 | Skip consent-gated edits; continue |
| Phase 8 | Skip dry run; print commands for later |
| **Hard abort** | Stop run; **no rollback**—files already written remain |

### Phase 2 — Docs split (CONVERT only)

- Classify each `docs/**/*.md` per REFERENCE heuristics (PUBLIC / PRIVATE / SPLIT / Unknown with `?`).
- Render migration table; user may edit destinations or trigger SPLIT sub-flow (user marks blocks; write two files).
- **Final Y/N before any move.** Use **`git mv`** only. FRESH: skip entirely; Phase 1 states the skip.

### Phase 6 — Link repair

- **After Phase 2, before Phase 3.**
- Search at minimum: `*.md` under `docs/` and repo root, plus `pyproject.toml` / `package.json` / `Cargo.toml` (and equivalents) for doc URLs.
- **Default:** skip `scripts/`, `src/`, `tests/`. Override: **include code** (start-of-run).
- Summarize matches; **batch Y/N** before applying. On N: continue; note stale refs possible.
- FRESH or CONVERT with no moves: **skipped (no path changes)**.

### Phase 3 — Public stubs

Targets: `README.public.md`, `CHANGELOG.public.md`. Optionally create **`.gitignore.public`** when the user wants a public-only ignore file (merge-safe; script renames it via `$PathRenames` when present).

1. Pre-existing: merge-safe rules.
2. Absent + CONVERT: read private file; filter in memory (REFERENCE: filter rules).
3. Absent + FRESH: minimal scaffold.

**HTML banner** (one line at top of each public stub):

```html
<!-- This file mirrors selected content from the private README/CHANGELOG. Edit-source-of-truth lives in the private repo's README.md / CHANGELOG.md unless this stub diverges intentionally. -->
```

**CONVERT:** mandatory unified-diff **Y/N per file**; **edit** → per-bullet `keep N` / `drop N`; **N** → write empty scaffold instead.

### Phase 4 — Private README augmentation

If `README.md` lacks a **Publishing** section, add one. **Embed this substance** (adapt heading level to repo style):

```markdown
## Publishing

Maintainers: install PowerShell 7+ (`brew install --cask powershell`, `apt install powershell`, or built-in on Windows) and run `pwsh -File scripts/publish.ps1` for a dry run. See `docs/internal/publish_whitelist_tree.md` for what publishes vs stays private.
```

Merge-safe if section exists in another form. **Do not** modify private `CHANGELOG.md` in this skill.

### Phase 5 — Publisher + whitelist doc

- Build **one in-memory source list** (whitelist paths, path renames, forbidden patterns); render **both** `scripts/publish.ps1` and `docs/internal/publish_whitelist_tree.md` from it so they cannot drift at write time.
- **Default AGENTS policy:** include `^AGENTS\.md$` and `^docs/AGENTS\.md$` in `$ForbiddenPatterns`; **never** whitelist root `AGENTS.md`.
- **Override `make AGENTS.md public` (start-of-run):** add `AGENTS.md` (or `AGENTS.public.md`→`AGENTS.md`) to whitelist; remove `^AGENTS\.md$` from forbidden; Phase 1 prints **WARNING** about public-safe review; Phase 7 may seed `AGENTS.public.md` with filter pipeline + diff Y/N; whitelist doc gets a short trade-off note.
- **Override `also write bash publisher` (start-of-run):** emit `scripts/publish.sh` with byte-equivalent behavior; add **both** `^scripts/publish\.ps1$` and `^scripts/publish\.sh$` to `$ForbiddenPatterns`; maintenance footer bullet about keeping both in sync (REFERENCE).
- Print **3-column table:** path | classification | reason. **Y** writes; **N** → Phase 5 rejection flow (Revise / Abort / Skip-degraded).

**Script and doc shapes:** follow **REFERENCE.md** exactly for structure, parameters, cross-check behavior, and `publish_whitelist_tree.md` sections including the **Script anchor** heading (**do not rename**—parser locks onto it).

### Phase 7 — AGENTS.md (opt-in, consent-gated)

- If root `AGENTS.md` missing: offer minimal stub linking to `docs/internal/agent_index.md` (or resolved internal index path)—**consent only**.
- If present: **append** “Maintainer navigation” block **only** after explicit user request (same as pack `AGENTS.md` handling).

### Phase 8 — Dry-run validation

- Pre-flight: `git`, `git-filter-repo`, `pwsh` (or `powershell.exe` on Windows) on PATH. If missing: still show gate; recommend **N**; print install hints from REFERENCE.
- Prompt: `Run a dry-run of scripts/publish.ps1 now? [Y/n]`
- **Y:** run `pwsh -File scripts/publish.ps1` (no `-Push`). Capture stdout/stderr. Surface: result line, “Files that would be published” block, cross-check verdict, forbidden hits, gitleaks one-liner if ran. Write log: `docs/internal/bootstrap-dry-run-YYYY-MM-DD.log` (same-day numbering: first file no suffix; on second run same calendar day rename prior log to `_01`, new log `_02`, etc.—align with pack same-day rule).
- **N:** print commands for later; Phase 8 = skipped (user) or skipped (tools missing).
- Dry-run failure: no rollback; suggest which phase to fix (REFERENCE: troubleshooting map).

## Final summary (always)

Emit per-phase status block matching the spec’s “Bootstrap private-public mirror — final summary” shape (done / skipped / aborted labels). Include follow-ups (re-run hints, log path, `docs-drift-audit` if Phase 6 rejected).

## Next (thin orchestration)

- Run **bootstrap-agent-index** so drift-watch includes `docs/internal/publish_whitelist_tree.md` ↔ `scripts/publish.ps1`.
- Run **changelog-discipline** (dual mode once public stubs exist).
- Run **project-status-doc** / **slice-summary** if the bootstrap should appear as a campaign.
- Ongoing: **docs-drift-audit** should treat script ↔ whitelist as a paired check in bootstrapped repos.
