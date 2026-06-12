# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Added the `maintain-badges` skill for evidence-backed static Shields.io badge
  maintenance and repository-specific badge rules.
- Added status, visibility, and released-version badges to the public README.

### Changed

- Expanded `setup-standards` so users can choose commit, README, changelog, or
  all standards before binding the selected guidance to a repository.

## [0.1.0] - 2026-06-05

### Added

- Public catalog entries for the `commit-standards`, `setup-standards`, and
  `publish-boundary` skills.
- Public catalog entries for the `readme` and `changelog` documentation
  maintenance skills.
- Public Semantic Versioning guidance for the published pack collection.

### Changed

- Updated the public repo contract to publish `.agents/skills/` packs instead
  of the previous skill portfolio layout.
- Updated the published `changelog` and `readme` packs to follow general
  changelog and README standards.

### Removed

- Removed the legacy project-status and slice-report skill catalog references.
