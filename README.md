# Agent skills

A small collection of agent skills for documentation maintenance and repo hygiene. Each pack lives under `.agents/skills/<name>/` with a `SKILL.md`. MIT licensed.

## Quick start

Copy the pack you want from `.agents/skills/` into the same path in your target repo, then ask your agent to use that skill by name.

For example, copy `.agents/skills/readme/` to your project when you want README maintenance guidance, or copy `.agents/skills/changelog/` when you want changelog standards.

## Versioning

This project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html) for
the published pack collection, starting at `0.1.0` during initial development.
Minor releases add backward-compatible packs or workflows, patch releases fix or
clarify existing packs, and future major releases mark incompatible public pack
behavior or path changes.

## Reference

One-line capability blurbs come from the first sentence of each pack's YAML `description:` in `SKILL.md`.

### Commit standards

* **commit-standards** - Author and validate git commit messages against Conventional Commits, layering any repo-local rules on top.
* **setup-standards** - Binds a repository to selected commit, README, and changelog standards by writing invoke-before-work pointers into the repo's agent-read source-of-truth doc.

### Documentation maintenance

* **changelog** - Maintains changelog documents using Keep a Changelog 1.1.0 and Semantic Versioning.
* **readme** - Maintains README documents using Make a README and Google README guidance for clear project identity, setup, usage, support, status, documentation links, and license coverage.

### Repo hygiene

* **publish-boundary** - Reviews private source-of-truth repos and their generated public repos for sanitation, audience separation, whitelist drift, and pre-push safety.

## License

This project is licensed under the [MIT License](LICENSE).

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release notes.
