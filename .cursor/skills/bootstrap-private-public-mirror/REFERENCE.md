# bootstrap-private-public-mirror — reference

Long heuristics, tables, and templates. The agent materializes these into the **target** repo; paths here are relative to repo root unless noted.

---

## Example Phase 1 plan fragment (concrete)

```text
Detected mode: CONVERT (override: none)
Public remote: git@github.com:Org/App.git  [Y/n]
Default branch: main

Phase ordering (verbatim):
Phase 1 Detect & plan        (read-only)
Phase 2 Docs split           [CONVERT only]
Phase 6 Link repair          [skipped if no moves]
Phase 3 Public stubs
Phase 4 Private augmentation
Phase 5 Publisher + whitelist doc
Phase 7 AGENTS.md policy     [opt-in]
Phase 8 Dry-run validation   [Y/N at end]

AGENTS.md: hard-private (default). Override: start run with "make AGENTS.md public".

Whitelist proposal: (table: path | classification | reason)
Forbidden patterns: (default list from Phase 5 section below)
```

---

## Phase 2 — Classification heuristics

**Default PUBLIC** (case-insensitive filename match):  
`RUNBOOK`, `INSTALL`, `API`, `USAGE`, `tutorial`, `guide`, `sample`, `example`, `api[-_ ]reference`, `getting-started`.

**Default PRIVATE** (case-insensitive path/filename match):  
`threat[-_ ]model`, `security[-_ ]review`, `incident`, `postmortem`, `runbook[-_]internal`, `adr/`, `docs/agents/`, `docs/modules/`, `docs/internal/`, `project_spec`, `project_status`.

**SPLIT:** Doc links to both public-safe and private-only references (flag for user; sub-flow: user marks sections → two files).

**Unknown:** Propose **PUBLIC** with `?` for review.

**Example table shape (plan output):**

| From | To | Class | Note |
|------|-----|-------|------|
| docs/RUNBOOK.md | docs/public/RUNBOOK.md | PUBLIC | name |
| docs/architecture.md | docs/internal/architecture.md | PRIVATE | name |
| docs/notes.md | docs/public/notes.md | ? | unknown → PUBLIC |
| docs/big-design.md | (split) | SPLIT | mixed |

---

## Phase 3 — README.public.md filter

**DROP sections** whose headings match (case-insensitive):

`^#+\s*(maintainer | internal | private | threat\s*model | security\s*review | incident | publish | mirror | developer\s*setup | local\s*dev | contributing\s*\(internal\) | agent\s*notes)`

**DROP** paragraphs / list items / blockquotes containing **private signals:**

- Resolved private repo URL
- Paths under `docs/internal/`
- `scripts/publish.ps1`
- `scripts/clean-publish-clones.ps1`
- `.cursor/`
- `AGENTS.md`

**KEEP** everything else verbatim.

**APPEND** a short **License** section if a LICENSE file exists (`LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING`) and README lacks one.

---

## Phase 3 — CHANGELOG.public.md filter

- Keep Keep-a-Changelog header + boilerplate.
- Keep release section titles + dates.
- Keep `## [Unreleased]` structure.
- **DROP** bullets matching the same private-signal patterns as README.

---

## Phase 5 — Whitelist build order (heuristic)

Single ordered source list; derive `$WhitelistPaths`, `$PathRenames`, `$ForbiddenPatterns` together.

1. **Build manifests** (public if present): `pyproject.toml`, `package.json` + `package-lock.json`, `Cargo.toml` + `Cargo.lock`, `go.mod` + `go.sum`, `pom.xml`, `build.gradle*`, `Gemfile` + `Gemfile.lock`, etc.
2. **Source dirs** (public if present): `src/`, `lib/`, `app/`, `cmd/`, `pkg/`, plus package dir implied by manifest (e.g. Python flat `<name>/`). When ambiguous, follow manifest.
3. **Tests** (public if present): `tests/`, `test/`, `__tests__/`, `spec/`, `e2e/`.
4. **`docs/public/`** always (after Phase 2).
5. **LICENSE** (one of `LICENSE`, `LICENSE.md`, `LICENSE.txt`, `COPYING`).
6. **`README.public.md`**, **`CHANGELOG.public.md`**.
7. **`.gitignore.public`** — whitelisted at **runtime** in script via `Test-Path` (not always in static path list if absent).

**Default exclusions** (user may opt in per repo): `scripts/`, `tools/`, `internal/`, `private/`, `.github/`, `.cursor/`, `AGENTS.md`, anything under `docs/internal/`, `Makefile`, `Dockerfile*`.

**Default `$ForbiddenPatterns`:**

```text
^\.cursor/(?!skills/)
^\.claude/
^AGENTS\.md$
^docs/internal/
^docs/AGENTS\.md$
^\.env$
^secrets/
\.pem$
\.key$
^\.coverage$
^scripts/publish\.ps1$
^scripts/clean-publish-clones\.ps1$
^temp_prompt
```

Use `^\.cursor/` instead of `(?!skills/)` only when the repo does **not** publish `.cursor/skills/` packs. When **`also write bash publisher`:** append `^scripts/publish\.sh$` and include both PowerShell publisher scripts plus `publish.sh` in forbidden (defense-in-depth).

**`$PathRenames` must always include:**

- `README.public.md:README.md`
- `CHANGELOG.public.md:CHANGELOG.md`
- `docs/public/:docs/`
- `.gitignore.public:.gitignore` (conditional at runtime via `Test-Path`)

**Phase 5 N — targeted re-prompt**

1. Phase 5 was rejected.
2. **(a) Revise** — reopen picker/heuristics (default).
3. **(b) Abort** — stop; Phases 1–4 and 6 outputs kept; nothing further.
4. **(c) Skip-degraded** — continue without publisher reference; final summary: **partial; publisher pending**.

**Interactive picker (on N before revise):** mark each top-level entry PUBLIC / PRIVATE / rename; re-render table; final Y/N. Support **add X** / **remove X** one-liners.

---

## Phase 5 — Publisher PowerShell (`scripts/publish.ps1`)

Agent fills `<DETECTED_PUBLIC_URL>`, `<DEFAULT_BRANCH>`, `$WhitelistPaths`, `$PathRenames`, `$ForbiddenPatterns`, and helper bodies. **Never** leave `<REPLACE_ME>` for remote.

**Temp clone path (must match cleanup script):** after `Get-RepoRoot`, set  
`$repoLeaf = (Split-Path $RepoRoot -Leaf)`  
`$work = Join-Path $env:TEMP ($repoLeaf + '-publish-' + [DateTimeOffset]::UtcNow.ToUnixTimeSeconds())`  
The sibling **`scripts/clean-publish-clones.ps1`** must default its filter to **`$repoLeaf + '-publish-*'`** (same stem).

**Required tail behavior (after filter-repo, forbidden scan, file list print):**

1. **`$ErrorActionPreference = 'Stop'`** stays in effect for the whole script.
2. **`-Push` block:** `Push-Location $work`, then **idempotent `public` remote** — **never** `git remote remove public` unconditionally and **never** rely on `2>$null` alone to hide git stderr (PowerShell still surfaces native stderr as error). **Required:**

```powershell
if ($Push) {
    Push-Location $work
    try {
        $remotes = @(& $git remote 2>&1 | ForEach-Object { $_.ToString().Trim() } | Where-Object { $_ })
        if ('public' -in $remotes) {
            & $git remote remove public
            if ($LASTEXITCODE -ne 0) { throw "git remote remove public failed." }
        }
        & $git remote add public $PublicRemote
        if ($LASTEXITCODE -ne 0) { throw "git remote add public failed." }
        & $git push --force public "HEAD:refs/heads/$Branch"
        if ($LASTEXITCODE -ne 0) { throw "git push failed." }
        if ($PushTags) {
            & $git push --force public --tags
            if ($LASTEXITCODE -ne 0) { throw "git push --tags failed." }
        }
    }
    finally {
        Pop-Location
    }
    Write-Host "Push complete."
    try {
        Remove-Item -Recurse -Force -LiteralPath $work
        Write-Host ('Done. Temp clone removed: ' + $work)
    }
    catch {
        Write-Warning ('Failed to remove temp clone at ' + $work + ': ' + $_.Exception.Message + ' - delete manually.')
    }
}
else {
    Write-Host ('Done. Temp clone left at ' + $work + ' - delete manually if desired.')
}
```

**Why:** `git filter-repo` removes `origin` from the temp clone; **`public`** does not exist until **`git remote add`**. A blind **`git remote remove public`** errors **No such remote: 'public'**; under **`Stop`** that aborts the script.

**Rest of script (unchanged expectations):** `#Requires -Version 5.1`, `Set-StrictMode -Version 3.0`, `Assert-CleanWorkingTree`, `Find-GitExe`, `Find-FilterRepo`, `Invoke-FilterRepo`, `Test-ForbiddenPaths`, `Invoke-WhitelistDocCrossCheck` reading **private** `docs/internal/publish_whitelist_tree.md`, optional gitleaks, throw if `-PushTags` without `-Push`, clone with `--no-local`, forbidden scan **before** push.

**Remote naming:** **`public`** is only the **temporary clone** remote name pointing at **`$PublicRemote`** (e.g. `https://github.com/org/repo.git`). The GitHub **repo slug** is not the git remote name.

---

## Phase 5 — `scripts/clean-publish-clones.ps1` (required sibling)

Emit alongside **`publish.ps1`** unless the user opted out with **`no cleanup script`** at run start. Match **skills-dev** behavior: `#Requires -Version 5.1`, **`Set-StrictMode -Version 3.0`**, **`$ErrorActionPreference = 'Stop'`**, **dry-run by default**, **`-Force`** performs deletes, **`.git` subdirectory required** on each candidate, **`-TempRoot`** defaults to **`$env:TEMP`**, filter directories whose names match the same **`<repoLeaf>-publish-*`** pattern as **`publish.ps1`** (expose **`-ClonePattern`** defaulting to `"$repoLeaf-publish-*"` where `$repoLeaf` is filled at generation time to match **`publish.ps1`**). List size/age before delete; **`exit 1`** if any delete fails under `-Force`. Include comment-based help with `.EXAMPLE` lines using **`powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\clean-publish-clones.ps1`**.

**Private ASCII tree (`publish_whitelist_tree.md`):** under private-only scripts, show both:

```text
->   scripts/publish.ps1
->   scripts/clean-publish-clones.ps1
```

---

## `scripts/publish.ps1` — structural checklist (quick)

- `Find-Tool` / helpers, `git clone --no-local`, `git filter-repo`, post-filter forbidden scan, cross-check, gitleaks optional.
- Print **Mode**, **Target remote**, **Target branch**, **Files that would be published**.
- Dry-run default; **`-Push`** / **`-PushTags`** as in PowerShell block above.

---

## `scripts/publish.sh` (optional override)

Same lists, renames, forbidden patterns, dirty-tree check, temp clone, `git filter-repo`, post-filter forbidden scan, optional gitleaks, dry-run default, `-Push` / `-PushTags` semantics aligned with PowerShell (including **idempotent `public` remote** and **post-success temp clone removal**). Add **`^scripts/publish\.ps1$`**, **`^scripts/clean-publish-clones\.ps1$`**, and **`^scripts/publish\.sh$`** to `$ForbiddenPatterns` when this file exists.

---

## `docs/internal/publish_whitelist_tree.md` — required sections

1. Title + purpose paragraph.
2. Source-of-truth note: `scripts/publish.ps1` holds `$WhitelistPaths` / `$PathRenames` / `$ForbiddenPatterns`; this doc is the maintainer-facing ASCII map.
3. Legend: `==>` PUBLIC | `->` PRIVATE | `=>` rename | `--` optional.
4. Private repository tree (ASCII; expand publish-relevant paths). **Must** include **`->`** lines for **`scripts/publish.ps1`** and **`scripts/clean-publish-clones.ps1`** (private-only; defense-in-depth matches `$ForbiddenPatterns`).
5. After-filter public tree shape (ASCII).
6. **`## Script anchor`** — section title must contain the exact substring **`Script anchor`** and stay stable (**DO NOT RENAME**). Cross-check parser locks onto this heading; content may live in fenced blocks per script implementation. Mapping table rows:
   - `$WhitelistPaths` → `==> PUBLIC` rows
   - `$PathRenames` → `=> rename` rows
   - `$ForbiddenPatterns` → defense-in-depth pattern list
7. Dependency licenses (optional maintainer-filled rows).
8. **`## Maintenance`** footer:

```markdown
## Maintenance

- [ ] After editing scripts/publish.ps1, refresh ASCII trees + script anchor mapping table + dependency licenses.
- [ ] After editing this doc, verify the script's $WhitelistPaths / $PathRenames / $ForbiddenPatterns still match.
- [ ] When in doubt, run a dry run: `powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\publish.ps1` (or `pwsh -File scripts/publish.ps1` if `pwsh` is on PATH). Prune old temp clones with `scripts\clean-publish-clones.ps1` (dry-run lists; `-Force` deletes).
```

When **bash publisher** exists, add:

```markdown
- [ ] Both publishers must be updated together; drift between them is a confidentiality risk.
```

---

## Phase 8 — Tool install hints

Use verbatim when tools missing:

```text
git-filter-repo not found. Install: pip install git-filter-repo
pwsh not found. Install: brew install --cask powershell / apt install powershell / built-in on Windows
```

**Commands to print on skip or in summary:**

```text
powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\publish.ps1
powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\publish.ps1 -Push
powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\publish.ps1 -Push -PushTags
pwsh -File scripts/publish.ps1
pwsh -File scripts/publish.ps1 -Push
pwsh -File scripts/publish.ps1 -Push -PushTags
```

Point to `docs/internal/publish_whitelist_tree.md` for publish boundary.

---

## Dry-run failure → suggested phase

| Symptom | Suggest |
|---------|---------|
| Cross-check mismatch | Re-run Phase 5; align lists and doc anchor |
| Forbidden-pattern hit | Review content; Phase 7 override / whitelist mistakes |
| git-filter-repo error | Install tool or fix path/rename args |
| Parse failure | Regenerate whitelist doc via skill Phase 5 |

Offer **Phase 8 only** after user fixes (no full redo of 1–7 required).

---

## Same-day log naming (`docs/internal/`)

Align with pack same-day rule for files:

- First log: `bootstrap-dry-run-YYYY-MM-DD.log`
- Second run same day: rename existing to `bootstrap-dry-run-YYYY-MM-DD_01.log`, write new as `bootstrap-dry-run-YYYY-MM-DD_02.log`; continue `_03`, …

Path is under `docs/internal/` (forbidden from publish by default patterns).
