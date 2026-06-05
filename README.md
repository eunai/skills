<!-- This file mirrors selected content from the private README/CHANGELOG. Edit the private repo first unless this public stub intentionally diverges. -->

# Agent skills

A small collection of agent skills for documentation maintenance and repo hygiene. Each pack lives under `.agents/skills/<name>/` with a `SKILL.md`. MIT licensed; the collection is inspired by [Matt Pocock's skills](https://github.com/mattpocock/skills).

## Quick start

Copy the skill folder you want from `.agents/skills/` into the same path in your target repo, then ask your agent to use that skill by name.

For example, copy `.agents/skills/readme/` to your project when you want repo-specific README maintenance guidance, or copy `.agents/skills/changelog/` when you want changelog ownership rules.

## Reference

One-line capability blurbs come from the first sentence of each pack's YAML `description:` in `SKILL.md`.

### Documentation maintenance

* **changelog** - Maintains the private and public changelog pair for anipyrenamer.
* **readme** - Maintains the private and public README pair for anipyrenamer.

## Acknowledgements

This collection is inspired by [Matt Pocock's skills](https://github.com/mattpocock/skills) (MIT).

## License

This project is licensed under the [MIT License](LICENSE).

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release notes.
