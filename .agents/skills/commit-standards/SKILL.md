---
name: commit-standards
description: Author and validate git commit messages against Conventional Commits, layering any repo-local rules on top. Use when about to commit, when writing or fixing a commit message, or when the user invokes /commit-standards or asks about commit message conventions. Not for changelog entries (use the changelog skill) or for browsing git history.
---

# Commit Standards

Author or validate a git commit message against Conventional Commits, then layer any repo-local rules on top.

## When this fires

- A user or agent is **about to commit** with no message drafted yet → **generate**.
- A commit message already exists (drafted by a human or agent) → **validate and correct**.
- Someone invokes `/commit-standards` or asks about commit conventions → answer from this standard.

## The standard (base)

```
type(scope)!: subject

body

footers
```

- **type** (required) — one of the 11: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.
- **scope** (optional) — free-form, in parentheses, e.g. `feat(parser):`. A repo overlay may enumerate or forbid scopes.
- **`!`** (optional) — placed before the colon, marks a breaking change.
- **subject** (required) — imperative mood, lowercase, no trailing period, ≤50 chars target / 72 hard.
- **body** (optional) — what and why, blank line after the subject, wrapped ~72 cols.
- **footers** (optional) — `BREAKING CHANGE: …`, trailers like `Co-Authored-By: …`, issue refs.

A breaking change is signalled by `!` before the colon **and/or** a `BREAKING CHANGE:` footer.

See [REFERENCE.md](REFERENCE.md) for type meanings, footer rules, body conventions, the `revert` format, and worked examples.

## Repo-local overlay

Apply the base above, then layer repo-local rules on top, overriding **only** the dimension they touch:

- Mandated trailers (e.g. `Co-Authored-By: …`) → always append them.
- Enumerated or forbidden scopes → restrict the scope accordingly.
- Tighter subject limits or extra types → use the repo's.

Source these rules **ambiently** — from what you already know about this repo (its `AGENTS.md`/`CLAUDE.md`, the harness instructions, this conversation). Do not hunt for a config file. The base always applies; the overlay never removes it, only refines it.

## Workflow

1. **Generate** (no message yet): read the staged diff, pick the single best `type`, infer `scope` if obvious, write an imperative subject, add body/footers when the change warrants them, apply the overlay, then commit.
2. **Validate** (message exists): check it against the standard + overlay. If already correct, commit as-is. If not, **show before → after**, then commit the corrected message. Do not silently rewrite; do not block waiting for approval.

## Do not

- Do not write changelog entries — that is the `changelog` skill's job.
- Do not install git hooks or bind a repo — that is the `setup-standards` skill's job.
