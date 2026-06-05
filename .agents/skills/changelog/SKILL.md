---
name: changelog
description: Maintains changelog documents using Keep a Changelog 1.1.0 and Semantic Versioning. Use when editing CHANGELOG.md, CHANGELOG.public.md, release notes, Unreleased sections, version headings, or deciding how a change maps to SemVer impact.
---

# Changelog

## Quick start

Read the existing changelog before editing it. Add human-curated notable changes
under `## [Unreleased]` unless preparing a release, and group entries with Keep
a Changelog section names.

## Workflow

1. Read the relevant changelog document and nearby release history.
2. Decide whether the change is notable to that changelog's audience.
3. Add concise bullets under `## [Unreleased]`.
4. Use only these section names: `Added`, `Changed`, `Deprecated`, `Removed`,
   `Fixed`, and `Security`.
5. Keep entries human-readable; do not paste raw commit subjects or diffs.
6. When preparing a release, move applicable `Unreleased` bullets into a new
   version section, use an ISO date such as `2026-06-05`, and keep latest
   releases first.
7. Run the repo's changelog or docs validation if one exists.

## SemVer Mapping

- `MAJOR`: incompatible public API or behavior changes.
- `MINOR`: backward-compatible public API additions, new functionality, or
  newly deprecated public API.
- `PATCH`: backward-compatible bug fixes.
- `0.y.z`: initial-development versions may change more freely, but still
  explain user-visible breakage clearly.

For breaking changes, make the impact explicit in the changelog entry and in
the release version choice.

## Standards

- Follow Keep a Changelog 1.1.0:
  <https://keepachangelog.com/en/1.1.0/>
- Follow Semantic Versioning 2.0.0:
  <https://semver.org/spec/v2.0.0.html>
- For private/public audience sanitation, use `publish-boundary`.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
