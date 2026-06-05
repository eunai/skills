---
name: changelog
description: Maintains the private and public changelog pair for anipyrenamer. Use when editing CHANGELOG.md or CHANGELOG.public.md, preparing release notes, or deciding whether a change belongs in internal maintainer history or public product history.
---

# Changelog Pair

Maintain `CHANGELOG.md` and `CHANGELOG.public.md` as separate records.

## Scope

- `CHANGELOG.md` is the private maintainer changelog for AI-assisted work, internal documentation, agent workflow, audits, reviews, and repo-governance changes.
- `CHANGELOG.public.md` is the source for the public user-facing changelog; it publishes as `CHANGELOG.md` in the public repo and should describe product/code/runtime changes.
- The split applies moving forward only. Do not rewrite old releases just to fit the new ownership model.
- If one change has both internal workflow impact and user-facing product impact, write separately worded entries in both files.

## Workflow

1. Read both changelogs before editing either one.
2. Add entries under `## [Unreleased]` only unless preparing a release.
3. Use Keep a Changelog sections only: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.
4. Keep entries curated for humans; do not dump raw commits.
5. Keep public entries free of private paths, AI-agent workflow detail, internal reviews, and slice/audit provenance.
6. Keep private entries focused on maintainer process and documentation governance; product/runtime changes belong in `CHANGELOG.public.md`.
7. Run `pytest -q tests/test_docs_contracts.py` after editing either changelog.

## Placement Rules

- Put AI-agent instructions, skill changes, docs contract changes, internal audits, reviews, and maintainer workflow changes in `CHANGELOG.md`.
- Put CLI behavior, runtime behavior, user-facing fixes, dependencies that affect users, install/config changes, and public documentation changes in `CHANGELOG.public.md`.
- Do not mention `src/`, `tests/`, CLI flags, or runtime behavior in new private Unreleased bullets.
- Do not mention `.agents/`, `.claude/`, `AGENTS.md`, `docs/internal/`, reviews, slices, Claude, Codex, Cursor, or agent workflow in new public Unreleased bullets.

## Validation

The authoritative gate is `tests/test_docs_contracts.py`. Existing Unreleased bullets from before the split are grandfathered there; new bullets must follow this skill.
