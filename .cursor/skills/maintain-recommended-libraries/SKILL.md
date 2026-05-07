---
name: maintain-recommended-libraries
description: Creates or updates a decision-oriented recommended_libraries.md memo of library candidates and trade-offs. Use when the user says recommended libraries, library trade-offs, stack memo, or asks to compare implementation dependencies.
---

# Maintain Recommended Libraries

Create or update `recommended_libraries.md`, a non-final decision memo for library candidates, trade-offs, testability, and runtime stability.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/maintain-recommended-libraries/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default target: `docs/internal/recommended_libraries.md` when `docs/internal/` exists, otherwise `docs/recommended_libraries.md`.
- Existing files: read first, summarize changes, ask once. Defaults: merge for files and append dated blocks to `Agent Notes`; add-only for directories; replace only on explicit request. Never delete user content.
- Thin orchestration: end with the Next hint below.

## Authoring Behavior
- Detect language and existing dependencies from manifests such as `pyproject.toml`, `requirements.txt`, `package.json`, `Cargo.toml`, or equivalents.
- Seed candidate concerns only from visible repo needs. Do not invent a stack.
- Mark undecided choices as `candidate / not finalized`.
- The agent may propose a non-binding recommendation sketch, but user confirmation is required before it is treated as decided.

## Required Sections
1. Purpose paragraph with "no library choices are finalized here" disclaimer.
2. Hard constraints for stack choices: scale, stability, correctness, typing gate; user fills repo-specific values.
3. Candidate table: concern | candidates | rationale.
4. Expanded rationale by concern: trade-offs, risks/deps, testability, stability.
5. Optional `X vs Y` deep dives for high-stakes comparisons.
6. Non-binding stack recommendation sketch.
7. Agent Notes: dated append-only subsections.

## Template
```md
# Recommended Libraries

This memo captures evaluation context. No library choices are finalized here.

## Hard Constraints
- Scale: TODO
- Stability: TODO
- Correctness: TODO
- Typing gate: TODO

## Candidates
| Concern | Candidates | Rationale |
| --- | --- | --- |
| TODO | TODO | candidate / not finalized |

## Rationale By Concern
### TODO
- Trade-offs: TODO
- Risks / deps: TODO
- Testability: TODO
- Runtime stability: TODO

## Non-Binding Recommendation Sketch
TODO

## Agent Notes
### YYYY-MM-DD
- TODO
```

## Next
- **adr-record** converts a hardened decision from this memo into a permanent ADR.
