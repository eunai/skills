---
name: readme
description: Maintains the private and public README pair for anipyrenamer. Use when editing README.md or README.public.md, or when a change affects install, configuration, quick-start, documentation map, licensing, or public/private publication guidance.
---

# README Pair

Maintain `README.md` and `README.public.md` together.

## Scope

- `README.md` is the private maintainer landing page for the canonical dev repo.
- `README.public.md` is the consumer-facing source that publishes as `README.md`.
- Do not add agent-only trigger text or hidden instructions to either README.
- Do not paste long sections from `docs/internal/project-spec.md` into a README; link to the authoritative source instead.

## Workflow

1. Read both README files before editing either one.
2. Check whether the change affects the public mirror. If yes, update `README.public.md` with consumer-safe wording.
3. Keep `README.md` aligned with the private repo model, maintainer commands, internal docs map, and public stub links.
4. Keep `README.public.md` free of private-only paths such as `AGENTS.md`, `CONTEXT.md`, `.agents/`, `.claude/`, `.cursor/`, `docs/agents/`, and `docs/internal/`.
5. Run `pytest -q tests/test_docs_contracts.py` after editing either README.

## Checklist

- Identity: project name plus a short plain-language description of audience and problem solved.
- Requirements/install/config: Python 3.13+, install commands, `.env`, `.env.example`, and cache path summary.
- Quick start: copy-pastable CLI examples including dry-run, apply, and common flags.
- Documentation map: private README links to spec, runbook, and module specs; public README links only to published docs.
- Changelog links: private README distinguishes internal `CHANGELOG.md` from public `CHANGELOG.public.md`; public README points readers at the published changelog path.
- License: keep MIT license references accurate.

## Validation

The authoritative gate is `tests/test_docs_contracts.py`. If this skill and the test disagree, update the skill or test in the same change so the contract stays single-source.
