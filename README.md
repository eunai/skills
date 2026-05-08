<!-- This file mirrors selected content from the private README/CHANGELOG. Edit-source-of-truth lives in the private repo's README.md / CHANGELOG.md unless this stub diverges intentionally. -->

# Cursor agent skills

Cursor Agent Skills for documentation hygiene, planning, security passes, private-to-public publishing, and agent alignment. Each pack lives under `.cursor/skills/<name>/` with a `SKILL.md` (and optional `REFERENCE.md`). MIT licensed; some packs trace to [Matt Pocock's skills](https://github.com/mattpocock/skills) (see Acknowledgements).

## Quick start

**Recommended — `/setup-eunai-skills`:** Copy the [`setup-eunai-skills`](.cursor/skills/setup-eunai-skills/) folder into your repo at `.cursor/skills/setup-eunai-skills/`, then ask your agent to run **`/setup-eunai-skills`** (or describe that intent in chat). The skill installs the canonical `write-a-skill` helper, recommends packs for your repo, and authors adapted copies.

The orchestrator lists available packs from the public mirror **[eunai/skills](https://github.com/eunai/skills)** (`.cursor/skills/` on `main`). If that listing is empty or you are offline, use the manual path below.

**Fallback — manual copy:** Copy any folder from [`.cursor/skills/`](.cursor/skills/) into your project at `.cursor/skills/<same-folder-name>/`. Cursor loads project skills from there. Open each pack's `SKILL.md` for triggers and behavior.

## Reference

One-line capability blurbs are the first sentence of each pack's YAML `description:` (see `SKILL.md` frontmatter). Groups are ordered bootstrap → docs → design → authoring → output style; packs are alphabetical within each group.

### Bootstrap & repo scaffolding

* **bootstrap-agent-index** — Creates or updates a maintainer-facing agent_index.md that is the canonical start-here entrypoint for AI agents and contributors.
* **bootstrap-agent-playbooks** — Creates or updates an agents/ folder of short topic playbooks for maintainers and AI agents.
* **bootstrap-modules-readme** — Creates or updates modules/README.md describing package boundaries, allowed dependency edges, and forbidden edges.
* **bootstrap-private-public-mirror** — Sets up a private repo to publish a sanitized public mirror via git filter-repo, including docs split, public stubs, publisher script, whitelist doc, link repair, and dry-run validation.
* **setup-eunai-skills** — Bootstraps the eunai skill set into a target Cursor repo by installing the canonical write-a-skill helper (eunai-first, Pocock fallback with Cursor adaptation), recommending skills based on the target repo's language and layout, then authoring each confirmed skill via /write-a-skill with adaptations.

### Doc & process discipline

* **adr-record** — Creates append-only Architectural Decision Records with stable numbering and supersedes chains.
* **changelog-discipline** — Maintains CHANGELOG.md and optional public changelog stubs using Keep a Changelog and Semantic Versioning.
* **docs-drift-audit** — Compares repo facts against documentation and emits a structured drift report, read-only by default.
* **maintain-recommended-libraries** — Creates or updates a decision-oriented recommended_libraries.md memo of library candidates and trade-offs.
* **project-status-slice-report** — Creates and updates a slice-based `project_status.md` plus per-slice `slice_report/` entries with required verification evidence. Roadmap diagrams use a fixed `flowchart LR` Mermaid house style with edge lines capped at four `-->` per source line and bridge-node continuations (see the pack’s `REFERENCE.md`).
* **security-review-pass** — Produces dated internal security review reports with severity-tagged findings and tracker checklists.
* **vertical-slice-planner** — Turns an issue, PRD, or rough idea into thin independently shippable vertical slices in a Markdown plan.

### Design conversation

* **grill-me** — Guides the agent to stress-test a plan or design through relentless, one-question-at-a-time interviews until decisions align.
* **grill-with-docs** — Runs a documentation-aware grilling session that challenges a plan against domain language and recorded decisions, updating CONTEXT and ADRs as terms crystallize.

### Skill authoring

* **revise-prompt** — Restructures a draft into a structured, agent-readable prompt with a stable core scaffold and optional sections, then delivers it in one copy-paste code fence plus a short change summary, optionally ending with a one-line Y/N offer to execute the revised prompt next.
* **write-a-skill** — Guides creation of Cursor Agent Skills under `.cursor/skills/` with proper structure, progressive disclosure, and bundled resources.

### Agent output style

* **caveman** — Configures the agent for ultra-compressed replies that drop filler while preserving full technical accuracy.

## Acknowledgements

This collection is inspired by [Matt Pocock's skills](https://github.com/mattpocock/skills) (MIT). Packs derived from that work — **caveman**, **grill-me**, **grill-with-docs**, and **write-a-skill** — carry a Pocock source line and a note that they were adapted for Cursor in each `SKILL.md` footer. Other packs were authored here and carry an [eunai/skills](https://github.com/eunai/skills) (MIT) footer. Per-skill footers are the detailed source of truth.

## License

This project is licensed under the [MIT License](LICENSE).

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release notes.
