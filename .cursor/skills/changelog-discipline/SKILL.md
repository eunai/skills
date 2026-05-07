---
name: changelog-discipline
description: Maintains CHANGELOG.md and optional public changelog stubs using Keep a Changelog and Semantic Versioning. Use when the user says update changelog, release notes, keep a changelog, or asks to prepare release notes.
---

# Changelog Discipline

Maintain `CHANGELOG.md` and, when present, `CHANGELOG.public.md` following Keep a Changelog 1.1.0 and Semantic Versioning.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/changelog-discipline/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Existing files: read first, summarize changes, ask once. Defaults: merge for files by inserting under `[Unreleased]`; add-only for directories; replace only on explicit request. Never delete user content.
- Thin orchestration: end with the Next hint below.

## Start Detection
- `CHANGELOG.md` and `CHANGELOG.public.md` present: DUAL mode.
- Only `CHANGELOG.md`: SINGLE private/root mode.
- Only `CHANGELOG.public.md`: SINGLE public mode.
- Neither exists: offer to create `CHANGELOG.md` only after user consent.
- Do not invent `CHANGELOG.public.md` unless the user opts in during the override step.

## Single Mode Rules
- File begins with `# Changelog` plus Keep a Changelog and SemVer boilerplate.
- `## [Unreleased]` appears first.
- Use only populated subsections: Added, Changed, Deprecated, Removed, Fixed, Security.
- Release headings are `## [X.Y.Z] - YYYY-MM-DD`, newest first.
- Footer reference links compare `[Unreleased]` to the latest tag and are updated when tagging.

## Dual Mode Rules
- `CHANGELOG.md` may carry full maintainer detail.
- `CHANGELOG.public.md` includes only externally relevant bullets.
- Public entries must not include internal paths, employee-only detail, private machinery, secrets, customer data, or local paths.
- When both change in one update, write the private changelog fully and project a filtered subset into the public changelog.

## Authoring Behavior
- Pull recent commit messages since the last tag, if any, as draft input only.
- Ask the user to confirm wording before writing.
- Default to adding entries under `[Unreleased]`, not rewriting old releases.

## Minimal Template
```md
# Changelog

All notable changes to this project will be documented in this file.

The format is based on Keep a Changelog, and this project adheres to Semantic Versioning.

## [Unreleased]
```

## Next
- Use **adr-record** for significant decisions referenced from a changelog entry.
