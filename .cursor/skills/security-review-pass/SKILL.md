---
name: security-review-pass
description: Produces dated internal security review reports with severity-tagged findings and tracker checklists. Use when the user says security review, security pass, audit findings, or asks for a security audit of the repo.
---

# Security Review Pass

Produce a dated, internal-first security review report with severity-tagged findings, safe remediation snippets, and a paste-ready tracker checklist.

If the user specifies a different root or file path at the start of this run, use it for this run only.

## Runtime Rules
- Self-contained: skill folder is `.cursor/skills/security-review-pass/`; do not use absolute paths from another repo.
- Docs-root heuristic:
  - If `docs/internal/` already exists in the target repo, use it.
  - Else if signals of a public/private split exist (`README.public.md`, `CHANGELOG.public.md`, or `AGENTS.md` referencing `docs/internal/`), only create `docs/internal/` after the user confirms in the override step.
  - Otherwise default to `docs/`.
- Default target: `docs/internal/security/review-YYYY-MM-DD.md` when `docs/internal/` exists, otherwise `docs/security/review-YYYY-MM-DD.md`.
- Same-day numbering rule:
  - First artifact for a date has no suffix.
  - When a second artifact lands the same day, rename the first to use `_01` and create the new one as `_02`. Subsequent ones use `_03`, `_04`, ...
  - Date format is hyphenated (`YYYY-MM-DD`). No mixed dots/hyphens.
- Existing files: read first, summarize changes, ask once. Defaults: merge for files; `Findings` are append-only within a report; add-only for directories; replace only on explicit request. Never delete user content.
- Thin orchestration: end with the Next hints below.

## Authoring Behavior
- Read code and cite real file paths and line ranges.
- Never invent vulnerabilities.
- Do not include secret values, raw credentials, or sensitive user data; redact when illustrating.
- Embed safe remediation snippets only when the fix is mechanical and obvious.

## Required Sections
1. Internal banner: maintainers, may contain paths/ticket ids, not for public paste without review.
2. Scope: reviewed and explicitly out of scope.
3. Method: sources walked, tools run, manual checks performed.
4. Findings: title, severity, where, risk, recommendation.
5. Risks not yet investigated: TODO list with rationale.
6. Tracker checklist: paste-ready Markdown checkboxes for each finding.

## Template
```md
# Security Review - YYYY-MM-DD

Audience: maintainers (internal-first). May contain paths and ticket ids. Not for public paste without review / scrub.

## Scope
TODO

## Method
TODO

## Findings
### TODO
- Severity: Critical | High | Medium | Low | Info
- Where: TODO
- Risk: TODO
- Recommendation: TODO

## Risks Not Yet Investigated
- TODO

## Tracker Checklist
- [ ] TODO
```

## Next
- Use **project-status-slice-report** to record remediation work with verification evidence.
- Use **adr-record** for cross-cutting security decisions.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
