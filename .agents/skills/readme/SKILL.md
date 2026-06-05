---
name: readme
description: Maintains README documents using Make a README and Google README guidance for clear project identity, setup, usage, support, status, documentation links, and license coverage. Use when editing README.md, README.public.md, package READMEs, quick starts, install instructions, usage examples, documentation maps, or contributor-facing README sections.
---

# README

## Quick start

Read the current README and the docs it links to before editing. Keep the README
as the first useful page for its audience: explain what the project is, how to
start, where to get help, and where deeper docs live.

## Workflow

1. Identify the README's audience: public users, private maintainers, package
   consumers, or contributors.
2. Read linked install, usage, support, status, contribution, and license docs
   before changing summaries.
3. Keep examples copyable and small; include expected output when it clarifies
   usage.
4. Link to deeper docs instead of duplicating long specs.
5. Remove stale links, obsolete requirements, and hidden agent-only trigger
   text.
6. Run the repo's README or docs validation if one exists.

## Checklist

- Name and description: self-explaining project name plus a specific statement
  of what the project does.
- Requirements and installation: exact prerequisites, package manager commands,
  environment variables, and platform limits when relevant.
- Usage: minimal working examples before advanced workflows.
- Support: where readers should ask questions or report issues.
- Contributing: whether contributions are accepted and how to start.
- Status: deprecation, maintenance, or not-for-general-release notes when
  relevant.
- Documentation links: user-facing or team-facing docs that belong outside the
  README.
- License: accurate license name and link.

## Standards

- Follow Make a README:
  <https://www.makeareadme.com/>
- Follow Google's README guidance:
  <https://google.github.io/styleguide/docguide/READMEs.html>
- For private/public audience sanitation, use `publish-boundary`.
