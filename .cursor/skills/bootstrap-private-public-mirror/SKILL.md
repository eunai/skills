---
name: bootstrap-private-public-mirror
description: Sets up a private repo to publish a sanitized public mirror via git filter-repo, including docs split, public stubs, scripts/publish.ps1 and scripts/clean-publish-clones.ps1, whitelist doc, link repair, and dry-run validation. Use when the user says bootstrap private public mirror, set up public mirror, create publish script, or split docs internal public.
disable-model-invocation: true
---

# Bootstrap Private-Public Mirror

Set up a private repo to publish a sanitized public mirror using `git filter-repo`. In one phased run, produce the `docs/internal/` + `docs/public/` split, `README.public.md`, `CHANGELOG.public.md`, optional `.gitignore.public`, a private `README.md` Publishing hint, **`scripts/publish.ps1`**, **`scripts/clean-publish-clones.ps1`**, and `docs/internal/publish_whitelist_tree.md`.

If the user specifies a different root or file path at the start of this run, use it for this run only.

Use `REFERENCE.md` for the detailed heuristics, examples, script skeletons, whitelist/doc shapes, and troubleshooting tables. This `SKILL.md` stays lean per `write-a-skill`; do not change runtime behavior by omitting reference details.

## Runtime Rules

- Self-contained: skill folder is `.cursor/skills/bootstrap-private-public-mirror/`; do not use absolute paths from another repo.
- Docs-root heuristic (use exactly this wording when explaining defaults to the user):
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Existing files: read, summarize changes vs template, then ask once. Default to merge for files, add-only for directories, replace only on explicit request, and never delete user content.
- Thin orchestration: end with the Next hints below.

## Hard Rules

- This is the only pack skill that moves existing files. Always use `git mv` to preserve history; never delete originals to make room.
- Public remote URL appears only in `scripts/publish.ps1` parameters and the whitelist doc intro, never in source, tests, or fixtures.
- Forbidden-pattern scan runs after `filter-repo` and before push; abort on any match; never relax this.
- Cross-check is opt-out with `-SkipDocCheck`; never silently disable it.
- Detect start-of-run overrides and print them in Phase 1: `force fresh`, `force convert`, `make AGENTS.md public`, `also write bash publisher`, `include code`, or **`no cleanup script`** (only explicit opt-out skips emitting `scripts/clean-publish-clones.ps1`).
- Hard abort stops the run without rollback. Files already written remain, and the final summary is still emitted.
- **Phase 5 default deliverables:** emit **`scripts/publish.ps1`** and **`scripts/clean-publish-clones.ps1`** together. **`publish_whitelist_tree.md`** Script anchor **`### Forbidden patterns`** must list **`^scripts/publish\.ps1$`** and **`^scripts/clean-publish-clones\.ps1$`** (defense-in-depth, line-for-line in sync with `$ForbiddenPatterns` in the script). Private ASCII tree must show **`->`** both `scripts/publish.ps1` and **`scripts/clean-publish-clones.ps1`**.
- **Generated `publish.ps1` `-Push` block:** never `git remote remove public` unconditionally (and never rely on `2>$null` to hide failure). **Required pattern:** `$remotes = @(& $git remote 2>&1 | ForEach-Object { $_.ToString().Trim() } | Where-Object { $_ })`; **only** run `git remote remove public` when **`'public' -in $remotes`**; then `git remote add public $PublicRemote` with **`$LASTEXITCODE`** checks; then `git push`. **Why:** a fresh `git filter-repo` temp clone has no `public` remote yet; git writes to stderr and **`$ErrorActionPreference = 'Stop'`** turns that into a terminating error. After a **successful** `-Push`, **`Remove-Item -Recurse -Force`** on the temp clone inside `try/catch`; on failure **`Write-Warning`** (do not throw). Dry-run (no `-Push`) must still end with **temp clone left … delete manually** (and optional pointer to `clean-publish-clones.ps1`). Full script shape: **`REFERENCE.md` → Phase 5 — Publisher PowerShell**.

## Start Detection

- Validate-only: if both `scripts/publish.ps1` and `docs/internal/publish_whitelist_tree.md` exist, Phase 1 reports partial bootstrap detected and offers to skip Phases 2-7 and go straight to Phase 8.
- Run mode: CONVERT if `docs/` has content, `README.md` or `CHANGELOG.md` is non-trivial, or repo has commits beyond initial commit; FRESH otherwise. Confirm detected mode once.
- Public remote: inspect `git remote -v`; if origin ends in `-dev`, `-private`, or `-internal`, propose the sibling URL by stripping the suffix. Ask once if detection fails. Refuse to finish past Phase 5 without a confirmed URL.
- Default branch: detect with `git symbolic-ref refs/remotes/origin/HEAD`; fall back to `main`.

## Phase Order

Phase 1 plan output must print this list verbatim:

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

Execution order: Phase 1 -> Phase 2 when CONVERT -> Phase 6 when paths moved -> Phase 3 -> Phase 4 -> Phase 5 -> Phase 7 -> Phase 8.

## Phase Behavior

- Phase 1: read-only plan. Print detected mode, public URL proposal and Y/N, default branch, Phase 2 migration proposal, Phase 5 whitelist/forbidden-pattern proposal, AGENTS.md stance, phase order, and abort/skip contract. Use the concrete plan fragment in `REFERENCE.md`.
- Phase 2: CONVERT only. Classify each `docs/**/*.md` as PUBLIC, PRIVATE, SPLIT, or unknown-to-PUBLIC with `?`; render the migration table; allow user edits and split sub-flow; final Y/N before any `git mv`.
- Phase 6: after Phase 2 and before Phase 3. Search docs/root markdown plus project metadata for moved doc links; skip `scripts/`, `src/`, and `tests/` unless `include code` was requested; batch Y/N before applying. Skip when no path changes.
- Phase 3: create or merge `README.public.md` and `CHANGELOG.public.md` using the filter rules and one-line HTML banner in `REFERENCE.md`; optionally create `.gitignore.public`. In CONVERT, show a mandatory unified diff per file. Treat **`scripts/clean-publish-clones.ps1`** like **`scripts/publish.ps1`** in private-signal DROP rules for public stubs.
- Phase 4: add the private `README.md` Publishing section if missing, adapted to local style. Include **one line** for **`scripts\clean-publish-clones.ps1`**: dry-run lists temp `*-publish-*` dirs with `.git`; add **`-Force`** to delete. Do not modify private `CHANGELOG.md`.
- Phase 5: build one in-memory source list for whitelist paths, path renames, and forbidden patterns, then render **`scripts/publish.ps1`**, **`scripts/clean-publish-clones.ps1`**, and **`docs/internal/publish_whitelist_tree.md`** with Script anchor blocks **line-for-line** identical to the script arrays. On rejection, use the targeted Revise / Abort / Skip-degraded prompt.
- Phase 7: keep AGENTS.md hard-private by default. Missing root `AGENTS.md` may get a minimal internal-index stub only with consent; existing `AGENTS.md` is untouched unless the user explicitly asks for a Maintainer navigation append. Honor the start-only `make AGENTS.md public` override exactly as in `REFERENCE.md`.
- Phase 8: offer dry-run validation after tool preflight. On Y, run `pwsh -File scripts/publish.ps1` or `powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\publish.ps1`, capture output, surface the required result lines, and write `docs/internal/bootstrap-dry-run-YYYY-MM-DD.log` using the pack same-day numbering rule. On N, print later-run and publish commands.

## Publisher Requirements

- **`scripts/publish.ps1`:** `#Requires -Version 5.1`, `Set-StrictMode -Version 3.0`, `$ErrorActionPreference = 'Stop'`, temp clone under **`$env:TEMP`** using the same **`<repoLeaf>-publish-<unixStamp>`** basename the cleanup script filters (derive `<repoLeaf>` from the private repo root folder name). Idempotent **`public`** remote handling, **`git remote add`** exit checks, **post-success `-Push`** temp clone removal with warning-only failure, default **`$PublicRemote`** as the user-confirmed URL (typically `https://github.com/<org>/<public-repo>.git` — the **GitHub repo slug is not the git remote name**; the temp clone uses remote name **`public`**). Never leave `<REPLACE_ME>` for remote.
- **`scripts/clean-publish-clones.ps1`:** `#Requires -Version 5.1`, strict mode, dry-run default, **`-Force`** deletes, **`.git`** sanity check, **`-TempRoot`** default **`$env:TEMP`**, **`-ClonePattern`** default aligned with publish clone naming (`<repoLeaf>-publish-*`). See **`REFERENCE.md` → Phase 5 — clean-publish-clones.ps1** for the full template.
- `$PathRenames` always include `README.public.md:README.md`, `CHANGELOG.public.md:CHANGELOG.md`, `docs/public/:docs/`, and conditional `.gitignore.public:.gitignore`.
- The script clones with `--no-local`, runs `git filter-repo`, scans forbidden patterns before push, runs the whitelist doc cross-check by default, optionally runs `gitleaks`, and only pushes when `-Push` is set.
- The whitelist doc must include a stable `Script anchor` heading for parser cross-checks, plus maintenance checklist. Do not rename the anchor.
- If `also write bash publisher` is requested at run start, emit `scripts/publish.sh` with equivalent behavior and add both publisher scripts to forbidden patterns.

## Final Summary

Always emit the Prompt I1 per-phase status block: Phase 1, Phase 2, Phase 6, Phase 3, Phase 4, Phase 5, Phase 7, Phase 8, with done / skipped / aborted / partial labels and follow-ups. Include dry-run log path when Phase 8 ran and recommend `docs-drift-audit` if Phase 6 was rejected.

## Next

- Run **bootstrap-agent-index** so drift-watch includes `docs/internal/publish_whitelist_tree.md` <-> `scripts/publish.ps1` (and optionally `clean-publish-clones.ps1` in maintainer docs).
- Run **changelog-discipline** (dual mode once public stubs exist).
- Run **project-status-doc** / **slice-summary** if the bootstrap should appear as a campaign.
- Ongoing: **docs-drift-audit** should treat script <-> whitelist as a paired check in bootstrapped repos.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
