---
name: setup-eunai-skills
description: Bootstraps the eunai skill set into a target Cursor repo by installing the canonical write-a-skill helper (eunai-first, Pocock fallback with Cursor adaptation), recommending skills based on the target repo's language and layout, then authoring each confirmed skill via /write-a-skill with adaptations. Use when the user says "set up eunai skills", "install eunai skills", "bootstrap eunai skill set", "/setup-eunai-skills", or asks to add the eunai skills pack to a new or existing Cursor repo.
---

# Setup Eunai Skills

Bootstrap a Cursor repo with the [eunai/skills](https://github.com/eunai/skills) pack in three consent-gated phases. Idempotent on re-runs; reports every fetch URL before downloading; never silently overwrites existing skills.

If the user specifies a different source URL or target path at the start of this run, use it for this run only.

## Phase A — Install canonical `write-a-skill`

Source URLs (report each before fetching):

1. **eunai (preferred):** `https://raw.githubusercontent.com/eunai/skills/main/.cursor/skills/write-a-skill/SKILL.md`
2. **Pocock (fallback):** `https://raw.githubusercontent.com/mattpocock/skills/main/skills/productivity/write-a-skill/SKILL.md`

Fetch order:

1. Try eunai first. On HTTP 200, use as-is (Cursor-shaped, drop-in compatible).
2. On 404, network failure, or empty body, fall back to Pocock and report the fallback explicitly.
3. **If the Pocock fallback fires, adapt the fetched content before writing:**
   - Rewrite path references from `skills/<name>/` to `.cursor/skills/<name>/`.
   - Rewrite Claude / Claude Code references to Cursor (e.g., `.claude/` → `.cursor/`, "Claude Code agent" → "Cursor agent").
   - Set the attribution footer to: `_Source: [mattpocock/skills](https://github.com/mattpocock/skills) (MIT). Adapted for Cursor by [eunai/skills](https://github.com/eunai/skills)._`
4. Write to `.cursor/skills/write-a-skill/SKILL.md`. If a copy already exists, summarize the diff and ask once. Default on conflict: **skip**.

## Phase B — Discover & recommend skills

1. List the eunai catalog at `https://api.github.com/repos/eunai/skills/contents/.cursor/skills?ref=main`. Report the URL first. Each top-level directory entry is one skill.
2. If the catalog is empty (the eunai mirror has not been published yet) or unreachable, stop and ask the user how to proceed (wait, point at a different mirror, or proceed Pocock-only with Phase A's adaptation pipeline applied per skill).
3. Detect target repo signals (read-only):
   - **Language(s):** file extensions, lockfiles, build manifests (`pyproject.toml`, `package.json`, `*.ps1`, `Cargo.toml`, `go.mod`, `pom.xml`, etc.).
   - **Purpose:** layout signals — CLI tool, library, application, monorepo, docs site. Ask the user if ambiguous.
   - **Already installed:** existing `.cursor/skills/<name>/` folders, so recommendations can flag what is already present.
4. Present two grouped lists with checkbox selection:
   - **Recommended** — skills that fit the detected language/purpose, one-line reason per skill.
   - **All available** — the full eunai catalog for reference, with already-installed packs flagged.
5. Default selection is the Recommended list. User confirms or adjusts.

## Phase C — Author each confirmed skill

For each confirmed skill, in order:

1. Fetch the eunai source from `https://raw.githubusercontent.com/eunai/skills/main/.cursor/skills/<name>/SKILL.md` (and any sibling `REFERENCE.md` or `scripts/`). Report each URL before fetching.
2. Invoke `/write-a-skill` with the fetched content as the starting template.
3. Adapt language/tooling references based on Phase B signals (e.g., PowerShell ↔ bash command examples, Python ↔ Node manifest paths). Preserve substantive behavior.
4. Write to `.cursor/skills/<name>/`. If the folder exists, summarize the diff and ask once. Default on conflict: **skip**.
5. Preserve the footer `_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._` in every authored skill.

## Constraints

- **Idempotent.** Re-running on the same repo is safe; conflicts default to skip, never silent overwrite.
- **No silent network calls.** Print every fetch URL before downloading; surface HTTP status codes in the run summary.
- **No external installer.** No `npx`, `pip`, or package-manager calls. Use only Cursor's HTTP and file tools.
- **Branch hardcoded to `main`** for both `eunai/skills` and `mattpocock/skills` raw URLs. Override only if the user supplies a branch at run start.
- **Read-only detection.** Phase B reads target repo files but never modifies anything outside `.cursor/skills/`.

## Authoring discipline

Every skill written in Phase C must follow `.cursor/skills/write-a-skill/SKILL.md` (the one Phase A just installed):

- SKILL.md under 100 lines; overflow into `REFERENCE.md`.
- Frontmatter `description:` — sentence 1 states the capability; sentence 2 begins `Use when ...` with concrete trigger phrases.
- Self-contained: no absolute paths from another repo; embed templates inline.

## Run summary

At the end of every run, print a compact per-phase outcome block: source chosen for Phase A (eunai vs Pocock-fallback) plus whether adaptation was applied; catalog/recommended/confirmed counts for Phase B; per-skill write/skip and footer for Phase C; any failed fetches with status codes.

## Next

- Verify each installed skill loads in Cursor by invoking its trigger phrase in chat.
- After installing doc-shaped skills, run `/docs-drift-audit` if the target repo has documentation that should reference them.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
