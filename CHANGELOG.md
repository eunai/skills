<!-- This file mirrors selected content from the private README/CHANGELOG. Edit-source-of-truth lives in the private repo's README.md / CHANGELOG.md unless this stub diverges intentionally. -->

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- **revise-prompt** skill: restructure drafts into agent-readable prompts (copy-paste fence + change summary); whitelisted for the public mirror.
- Initial public mirror of Cursor agent skills.
- Public README: Quick start (`/setup-eunai-skills` + manual fallback), grouped **Reference** catalog, Acknowledgements, and Changelog pointer.

### Changed

- **revise-prompt**: grill-first vs immediate-revise decision; six-part prompt
  scaffold and optional sections; reply format includes an optional one-line Y/N
  to continue with the revised prompt.
- **project-status-slice-report**: roadmap Mermaid diagrams follow a fixed
  `flowchart LR` house style (node shapes, status classes, colors, edge layout);
  `project_status.md` template fences no longer nest invalid Markdown blocks.
