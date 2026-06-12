# Maintain Badges Reference

Depth behind [SKILL.md](SKILL.md). Static badge format:
<https://shields.io/badges/static-badge>.

## Static Shields.io format

Use a literal static route:

```md
![label: value](https://img.shields.io/badge/label-value-color)
```

Only `/badge/<label>-<value>-<color>` with literal components is manageable.
Dynamic service routes remain out of scope.

Use plain Markdown images with lowercase descriptive alt text. Do not wrap a
new badge in a link unless the user requests it or approves an authoritative
local destination; record any approved link target in the badge rule.

Escape path content using Shields.io rules:

- Space: `%20`
- Literal hyphen: `--`
- Literal underscore: `__`
- Other special characters: URL-encode

For example, released version `1.0.0-rc.1` is encoded as
`1.0.0--rc.1`.

## Repository-wide styles

Ask for one style when adding or adopting the first managed badge:

- `flat`: default; omit the `style` query parameter
- `flat-square`: `?style=flat-square`
- `plastic`: `?style=plastic`
- `for-the-badge`: `?style=for-the-badge`
- `social`: `?style=social`

The style rule applies only to managed static Shields.io badges. A style change
requires approval and updates all managed badges atomically. V1 does not manage
logos, logo colors, or custom label colors.

## Project Status template

Use conceptual name `Project Status` and rendered label `status`. Only one state
appears per document.

| State | Meaning | Color |
| --- | --- | --- |
| `in development` | Actively being built; behavior or interfaces may change. | `yellow` |
| `active` | Released and receiving normal feature and maintenance work. | `brightgreen` |
| `maintenance` | Supported primarily through fixes and essential updates. | `orange` |
| `deprecated` | Available but discouraged, with migration or replacement guidance. | `red` |
| `archived` | No longer maintained; effectively read-only. | `lightgrey` |

```md
![status: in development](https://img.shields.io/badge/status-in%20development-yellow)
![status: active](https://img.shields.io/badge/status-active-brightgreen)
![status: maintenance](https://img.shields.io/badge/status-maintenance-orange)
![status: deprecated](https://img.shields.io/badge/status-deprecated-red)
![status: archived](https://img.shields.io/badge/status-archived-lightgrey)
```

Require an explicit durable status declaration. Prefer a project specification,
roadmap, manifest, or root `AGENTS.md`. If none exists, propose adding the
declaration to root `AGENTS.md`; never infer status from activity.

## Visibility template

Only one state appears per document.

```md
![visibility: private](https://img.shields.io/badge/visibility-private-red)
![visibility: public](https://img.shields.io/badge/visibility-public-brightgreen)
![visibility: internal](https://img.shields.io/badge/visibility-internal-orange)
![visibility: private source | public repo](https://img.shields.io/badge/visibility-private%20source%20%7C%20public%20repo-blue)
```

Prefer explicit policy in root `AGENTS.md` or a project specification. Local
remotes and approved hosting metadata may corroborate but do not override
policy. Do not infer visibility from a remote URL alone.

## Version template

Display the latest released SemVer exactly, without a leading `v`, in blue:

```md
![version: 0.1.0](https://img.shields.io/badge/version-0.1.0-blue)
```

Released prereleases such as `1.0.0-rc.1` are allowed. Preserve authoritative
build metadata when the displayed release includes it. Ignore `Unreleased`,
planned versions, and placeholders. The repository must designate one primary
version source; named corroborating sources must agree.

## Proposal requirements

Before asking for approval, show:

- Exact Markdown and placement.
- Complete shared policy and per-badge subsection.
- Initial state, named color, and repository-wide style.
- Primary evidence, corroborating evidence, and precedence.
- Read-only local checks and trigger surfaces.
- Files that would change.

Several badges may be approved as one batch. Keep each contract separate and
apply only the explicitly approved subset.

## Placement and migration

- Root `README.md` is the default and package-level READMEs are not discovered
  automatically.
- When the user names an alternate document, record its exact path and use its
  first H1 for placement. Do not also edit root `README.md` unless requested or
  required by repository instructions.
- Put managed badges on one line separated by single spaces in this order:
  `status`, `visibility`, `version`, then custom badges in approval order.
- Do not relocate existing badges without approval. Recommend relocation when
  the top badge row is not a suitable location.
- If a badge row becomes unwieldy, recommend a layout change rather than
  applying one automatically.
- Report rendered non-Shields static badges as unsupported and offer an
  approval-gated migration to equivalent static Shields.io markup and a
  maintenance rule.

## `AGENTS.md` template

Use normal capitalization for prose and `## Badge maintenance`. Keep badge
identifier headings and badge-facing data lowercase.

```md
## Badge maintenance

- Scope: inspect root `README.md` unless the user or repository instructions
  name another document.
- Invoke `maintain-badges` before completing any change that may affect a
  managed badge's checks, and before completing any static badge addition,
  removal, or contract change.
- Style: all managed static Shields.io badges use `<approved style>`.
- Dynamic badges, unsupported badges, unknown badges, and unmanaged static
  badges are out of scope and must remain unchanged.
- Preserve existing README instructions. Add only the smallest compatible
  invocation rule nearby.

### <lowercase badge label>

- Badge: `<conceptual badge identity and purpose>`
- Location: `<relative document path and placement>`
- Format: `<exact approved Markdown or parameterized approved form>`
- States: `<approved lowercase values and named colors>`
- Authoritative evidence: `<primary source, corroboration, and precedence>`
- Checks: `<exact read-only local checks selecting each state>`
- Update rule: `<automatic state reconciliation and approval boundaries>`
- Invoke when: `<files, commands, or policy changes that affect this badge>`
- Out of scope: `<dynamic routes, unsupported parameters, or unrelated data>`
```

When labels collide across documents, qualify headings with the relative path:

```md
### status — README.md
### status — docs/product.md
```

The `Badge` and `Location` fields are the definitive identity. Preserve extra
human-authored notes and unknown fields. Report conflicts instead of rewriting
them.

## Complete compact example

```md
## Badge maintenance

- Scope: inspect root `README.md`.
- Invoke `maintain-badges` before completing changes to `README.md`,
  `CHANGELOG.md`, or the version source, and before any static badge change.
- Style: all managed static Shields.io badges use `flat` with no `style`
  parameter.
- Dynamic, unsupported, unknown, and unmanaged badges are out of scope and
  remain unchanged.

### version

- Badge: latest released Semantic Versioning version.
- Location: `README.md`, in the badge row immediately below the first H1.
- Format: `![version: <semver>](https://img.shields.io/badge/version-<escaped semver>-blue)`.
- States: any released SemVer value; color `blue`.
- Authoritative evidence: primary `CHANGELOG.md` latest released heading;
  local Git tag is optional corroboration.
- Checks: ignore `Unreleased`; parse the latest released heading; require valid
  SemVer; when a matching local tag exists, require agreement.
- Update rule: reconcile automatically when checks are decisive; report drift
  and leave unchanged on conflict.
- Invoke when: editing `README.md`, `CHANGELOG.md`, release tags, or release
  policy.
- Out of scope: planned versions, `Unreleased`, remote release APIs, and dynamic
  version badges.
```
