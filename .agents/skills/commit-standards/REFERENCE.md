# Commit Standards — Reference

Depth behind [SKILL.md](SKILL.md). Canonical source: Conventional Commits v1.0.0 — <https://www.conventionalcommits.org/en/v1.0.0/>.

## Type meanings

- **feat** — a new feature (user-facing capability).
- **fix** — a bug fix.
- **docs** — documentation only.
- **style** — formatting, whitespace, semicolons; no change to code behavior.
- **refactor** — a code change that neither fixes a bug nor adds a feature.
- **perf** — a change that improves performance.
- **test** — adding or correcting tests.
- **build** — build system or external dependencies (npm, pip, etc.).
- **ci** — CI configuration and scripts.
- **chore** — maintenance that touches neither src nor tests.
- **revert** — reverts a previous commit (see below).

## Subject rules

- Imperative mood: "add", not "added" / "adds". It should read as "this commit will _<subject>_".
- Lowercase first word; no trailing period.
- ≤50 chars target, 72 hard limit.

## Body

- Separate from the subject by one blank line.
- Explain *what* and *why*, not *how*. Wrap ~72 cols.
- Optional; omit for trivial changes.

## Footers

- One per line, in a block after a blank line following the body.
- `BREAKING CHANGE: <description>` — describes the break; equivalent to `!` in the header. Use both for emphasis.
- Trailers: `Co-Authored-By: Name <email>`, `Reviewed-by: …`, `Refs: #123`, `Closes #123`.
- A repo overlay often mandates specific trailers — always include those.

## Breaking changes

Two interchangeable signals; use either or both:

```
feat(api)!: drop v1 endpoints
```

```
feat(api): drop v1 endpoints

BREAKING CHANGE: v1 endpoints removed; clients must migrate to v2.
```

## Revert format

```
revert: <subject of the reverted commit>

This reverts commit <hash>.
```

## Examples

```
feat(parser): add array literal support
```

```
fix: prevent crash on empty config

The loader assumed a non-empty map. Guard the lookup and default to an
empty map instead.

Refs: #482
```

```
refactor(auth)!: replace session cookies with JWTs

BREAKING CHANGE: existing sessions are invalidated; users must re-login.
Co-Authored-By: Claude <noreply@anthropic.com>
```
