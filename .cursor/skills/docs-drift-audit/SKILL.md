---
name: docs-drift-audit
description: Compares repo facts against documentation and emits a structured drift report, read-only by default. Use when the user says docs drift, drift audit, are docs current, or asks whether docs match the repo.
---

# Docs Drift Audit

Compare repo facts against documentation and report drift. Default behavior is read-only; safe fixes require an explicit start-of-run override.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/docs-drift-audit/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default target: `docs/internal/drift/audit-YYYY-MM-DD.md` when `docs/internal/` exists, otherwise `docs/drift/audit-YYYY-MM-DD.md`.
- Same-day numbering rule:
  - First artifact for a date has no suffix.
  - When a second artifact lands the same day, rename the first to use `_01` and create the new one as `_02`. Subsequent ones use `_03`, `_04`, ...
  - Date format is hyphenated (`YYYY-MM-DD`). No mixed dots/hyphens.
- Existing files: read-only mode never modifies docs. Auto-apply mode reads, summarizes, asks once, merges approved file edits, uses add-only for directories, and never deletes user content.
- Thin orchestration: end with the Next hints below.

## Default Read-Only Mode
- Walk repo facts such as manifests, CLI help, module layout, commands, public/private docs, and paired files.
- Compare against README, `agent_index.md`, `modules/README.md`, changelogs, and other relevant docs.
- Emit a structured report showing `Expected`, `Observed`, and `Suggested fix` as fenced diffs.
- Do not apply diffs in default mode.

## Auto-Apply Mode
Triggered only when the user starts the run with `apply safe fixes` or equivalent.

Apply only low-risk mechanical edits after confirmation:
- Stale CLI command strings unambiguously derivable from help output.
- Broken relative links with one clear replacement target.
- Version strings already pinned elsewhere in the repo.

Never auto-apply subjective edits such as rewording, reordering, or "make it nicer".

## Report Template
````md
# Docs Drift Audit - YYYY-MM-DD

## Scope
TODO

## Summary
TODO

## Drift Findings
### TODO
- Expected: TODO
- Observed: TODO
- Suggested fix:
```diff
TODO
```

## Safe Auto-Apply Candidates
- TODO

## Follow-Ups
- TODO
````

## Next
- Use **slice-summary** if remediation should be tracked as a campaign.
- Use **changelog-discipline** if applied fixes change user-visible behavior.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
