---
name: bootstrap-agent-playbooks
description: Creates or updates an agents/ folder of short topic playbooks for maintainers and AI agents. Use when the user says bootstrap agent playbooks, create agent guides, set up agents/ folder, or asks for topic-specific agent guidance.
---

# Bootstrap Agent Playbooks

Create or update an `agents/` folder of topic-specific playbooks that explain what to do, what not to do, and where in the spec to look.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/bootstrap-agent-playbooks/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default folder: `docs/internal/agents/` when `docs/internal/` exists, otherwise `docs/agents/`.
- Existing files: read first, summarize changes, ask once. Defaults: merge for files, add-only for directories, replace only on explicit request. Never delete user content.
- Thin orchestration: end with the Next hints below.

## What To Create
- Always propose `agents/README.md` as the index.
- Ask which topic stubs to create; do not create all by default.
- Suggested topics: `PYTHON.md` or `LANGUAGE.md`, `SECURITY.md`, one inferred domain file such as `<DOMAIN>.md`, and `TUI.md`, `FRONTEND.md`, or `API.md` when relevant.

## Authoring Behavior
- Create only the stubs the user confirms.
- Do not invent rules. Leave `Always`, `Ask first`, and `Never` as placeholders when repo rules are not already documented.
- `Spec jump points` must reference real files when possible; otherwise use TODO.
- Never overwrite an existing topic file without explicit confirmation.

## README Template
```md
# Agent Playbooks

These maintainer-facing guides help agents pick the right local rules by task. They are not a replacement for reading code.

## Pick A Guide
| Task | Guide |
| --- | --- |
| TODO | TODO |

## What These Are
- Short operational notes for repeat agent work.

## What These Are Not
- Public documentation unless explicitly reviewed.
```

## Topic Template
```md
# <Topic> Playbook

## Purpose
TODO

## Always
- TODO

## Ask First
- TODO

## Never
- TODO

## Spec Jump Points
- TODO

## Pitfalls
- TODO

## Where To Put New Code
- TODO
```

## Next
- Run **bootstrap-agent-index** so it points at `agents/README.md`.
- Run **bootstrap-modules-readme** so playbook layer references resolve.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
