---
name: maintain-badges
description: Maintains manually updated static Shields.io badges and their repository rules using authoritative local evidence. Use when checking, adding, adopting, updating, removing, or designing README badges, or when repository changes may affect a managed badge.
---

# Maintain Badges

## Quick start

Inspect root `README.md` unless the user or repository instructions name other
documents. Read root `AGENTS.md` when present. Reconcile only approved, managed
static Shields.io badges against their declared local checks.

Use [REFERENCE.md](REFERENCE.md) for badge variants, Shields.io escaping,
proposal content, and the complete `AGENTS.md` maintenance-rule template.

## Modes

- Normal: inspect and automatically reconcile managed badges when their checks
  select one approved state.
- Audit only: when asked to check, audit, review, or report only, make no edits
  and return the same findings as proposed changes.

## Workflow

1. Stop if the in-scope document does not exist. Do not substitute another
   README unless the user names it.
2. Inspect rendered Markdown image badges. Ignore code, inline code, and HTML
   comments. Report rendered HTML badges as unsupported.
3. Classify badges:
   - Managed: a static Shields.io `/badge/<label>-<value>-<color>` image with a
     matching approved rule in root `AGENTS.md`.
   - Unmanaged: a static badge without an approved rule.
   - Dynamic: a service, endpoint, JSON, GitHub, package, coverage, build, or
     other live-data route.
   - Unsupported: a rendered non-Shields static badge or HTML badge.
   - Unknown: classification is uncertain.
4. Leave dynamic, unsupported, unknown, and unmanaged badges unchanged.
5. For each managed badge, read only the rule's named local files and run only
   its named read-only local checks. Follow declared evidence precedence.
6. If checks are decisive, update state drift, malformed markup, style drift,
   and clear accidental duplicates. If evidence conflicts or is ambiguous,
   leave the badge unchanged.
7. Validate the edited badge, its rule, file scope, unchanged dynamic badges,
   and repository-defined documentation checks.
8. Return a concise chat summary of managed, unmanaged, dynamic, unsupported,
   suggested, and blocked findings, omitting empty groups.

## Approval gates

- When no manageable static badges exist, suggest only locally supportable
  `status`, `visibility`, and `version` badges. Do not add them.
- Before adding or adopting a badge, show its exact Markdown, complete rule,
  initial state, color, repository-wide style, evidence, checks, triggers, and
  affected files.
- Add or adopt a badge and its rule atomically after approval. If root
  `AGENTS.md` is missing, ask before creating it.
- Require approval to remove a badge, change its contract, change the shared
  style, migrate an unsupported badge, relocate an existing badge, or create
  supporting evidence.
- Automatic runs may select only among already approved states.
- Report orphaned rules. Do not restore their badges or remove the rules
  without approval.

## Editing rules

- New badges are plain Markdown images with lowercase alt text, label, value,
  state name, and named color.
- Place new badges on one line immediately below the first H1: `status`,
  `visibility`, `version`, then user-requested badges in approval order.
- Use one approved repository-wide style for managed static Shields.io badges.
  Preserve all unrelated badge formatting.
- Keep badge checks local, read-only, and side-effect free by default.
- Preserve dirty-worktree changes. Stop on conflicts in the same badge row or
  maintenance subsection. Never revert unrelated work.
- Edit and validate only. Do not stage, commit, or push.

## Boundaries

- Do not infer status from activity, tests, issues, or unfinished work.
- Do not infer visibility from README wording or a remote URL alone.
- Do not display planned or unreleased versions.
- Do not browse, query hosting APIs, install dependencies, or mutate supporting
  sources without explicit approval.
- Do not rewrite existing README instructions when integrating the invocation
  rule. Add the smallest compatible rule nearby; report conflicts.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
