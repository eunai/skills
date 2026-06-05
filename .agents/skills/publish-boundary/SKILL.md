---
name: publish-boundary
description: Reviews private source-of-truth repos and their generated public repos for sanitation, audience separation, whitelist drift, and pre-push safety. Use when editing private/public document pairs, publish scripts, whitelists, public-facing docs, catalog entries, or preparing to push a filtered public repo.
---

# Publish Boundary

## Quick start

Before changing or pushing public output, identify:

- The private source-of-truth repo.
- The generated public repo.
- The files renamed or filtered during publish.
- The command that builds or dry-runs the public tree.

Then compare private and public surfaces together, edit the private source first
when possible, and run the repo's publish dry run before any public push.

## Workflow

1. Read the repo glossary and agent instructions for the canonical terms.
2. Find the publish contract: whitelist docs, publish scripts, path renames,
   forbidden patterns, and public catalog rules.
3. Read each affected private/public pair together before editing either file.
4. Keep internal workflow, private paths, unpublished docs, agent-only
   instructions, temp prompts, secrets, and maintainer provenance out of public
   surfaces.
5. Keep public docs useful without pointing readers at private-only paths.
6. If the change alters what publishes, update the whitelist docs and publisher
   together, but ask before expanding the whitelist, renaming public pack paths,
   or changing the public repo remote.
7. Run the repo's dry-run publisher and inspect the produced file list before a
   push.

## Sanitation Checks

- Public output must not include private agent instructions, internal specs,
  private changelogs, hidden workflow docs, `.env`, keys, secrets, coverage
  output, temp prompt files, or publish scripts.
- Public README and changelog sources must use public-facing wording and links.
- Private README and changelog files may mention maintainer workflow, internal
  docs, audits, and publishing commands.
- Public catalog entries should come from pack frontmatter when the repo uses
  that convention; fix frontmatter first, then sync the catalog.
- If a validation script and a prose doc disagree, update both in the same
  change so the publish boundary has one contract.

## Related Skills

- Use `readme` for README structure, clarity, install, usage, support, and
  documentation map standards.
- Use `changelog` for Keep a Changelog and Semantic Versioning release-note
  standards.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
