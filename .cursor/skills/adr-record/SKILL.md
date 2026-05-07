---
name: adr-record
description: Creates append-only Architectural Decision Records with stable numbering and supersedes chains. Use when the user says adr, decision record, record this decision, or wants an architecture decision captured.
---

# ADR Record

Create one Architectural Decision Record per call. Each ADR captures one decision in a stable, append-only format with a supersedes chain.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/adr-record/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default folder: `docs/internal/adr/` when `docs/internal/` exists, otherwise `docs/adr/`.
- Existing files: read first, summarize changes, ask once. Defaults: merge for files and add-only for directories; replace only on explicit request. Never delete user content.
- Thin orchestration: end with the Next hints below.

## Filename
- Use `NNNN-title-kebab.md`.
- `NNNN` is the next unused 4-digit zero-padded sequence in the ADR folder.
- Example: `0007-pin-cryptography-version.md`.

## Required Sections
1. `# ADR NNNN - <Title>`
2. `Status: Proposed | Accepted | Deprecated | Superseded`
3. `Date: YYYY-MM-DD`
4. `Context` - problem and constraints.
5. `Options considered` - include rejected options.
6. `Decision` - one paragraph describing what we are doing.
7. `Consequences` - positive, negative, neutral.
8. `Supersedes: <ADR id, if any>`
9. `Superseded by: <ADR id, if any>` - filled later.

## Hard Rules
- Compute the next number by scanning the resolved ADR folder.
- Never edit an Accepted ADR body to change a decision.
- To replace an Accepted decision, create a new ADR with `Status: Accepted` and `Supersedes: NNNN`, then update only the old ADR's `Superseded by:` line.
- Refuse to overwrite an existing ADR body without explicit confirmation.

## Template
```md
# ADR NNNN - <Title>

Status: Proposed
Date: YYYY-MM-DD

## Context
TODO

## Options Considered
- TODO

## Decision
TODO

## Consequences
- Positive: TODO
- Negative: TODO
- Neutral: TODO

Supersedes: none
Superseded by: none
```

## Next
- **maintain-recommended-libraries** can be the source memo behind an ADR.
- **vertical-slice-planner** can plan implementation slices for an Accepted ADR.
