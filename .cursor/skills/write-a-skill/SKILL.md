---
name: write-a-skill
description: Guides creation of Cursor Agent Skills under `.cursor/skills/` with proper structure, progressive disclosure, and bundled resources. Use when the user wants to create, write, or build a new skill, asks where skills live, or mentions SKILL.md frontmatter.
---

# Writing Skills

Store each skill in **`.cursor/skills/<skill-name>/`** (project) so Cursor loads it as an Agent skill. The YAML **`name`** is the stable id; the **`description`** is the main routing blurb—treat it like metadata for which skill to load, not a summary of every section.

## Process

1. **Gather requirements** - ask user about:
 - What task/domain does the skill cover?
 - What specific use cases should it handle?
 - Does it need executable scripts or just instructions?
 - Any reference materials to include?

2. **Draft the skill** - create:
 - SKILL.md with concise instructions
 - Additional reference files if content exceeds 500 lines
 - Utility scripts if deterministic operations needed

3. **Review with user** - present draft and ask:
 - Does this cover your use cases?
 - Anything missing or unclear?
 - Should any section be more/less detailed?

## Skill Structure

```
.cursor/skills/skill-name/
├── SKILL.md           # Main instructions (required)
├── REFERENCE.md       # Detailed docs (if needed)
├── EXAMPLES.md        # Usage examples (if needed)
└── scripts/           # Utility scripts (if needed)
    └── helper.js
```

## SKILL.md Template

```md
---
name: skill-name
description: Brief description of capability. Use when [specific triggers].
---

# Skill Name

## Quick start

[Minimal working example]

## Workflows

[Step-by-step processes with checklists for complex tasks]

## Advanced features

[Link to separate files: See [REFERENCE.md](REFERENCE.md)]
```

## Description Requirements

The description is **the only thing your agent sees** when deciding which skill to load. It's surfaced in the system prompt alongside all other installed skills. Your agent reads these descriptions and picks the relevant skill based on the user's request.

**Goal**: Give your agent just enough info to know:

1. What capability this skill provides
2. When/why to trigger it (specific keywords, contexts, file types)

**Format**:

- Max 1024 chars
- Write in third person
- First sentence: what it does
- Second sentence: "Use when [specific triggers]"

**Good example**:

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.
```

**Bad example**:

```
Helps with documents.
```

The bad example gives your agent no way to distinguish this from other document skills.

## When to Add Scripts

Add utility scripts when:

- Operation is deterministic (validation, formatting)
- Same code would be generated repeatedly
- Errors need explicit handling

Scripts save tokens and improve reliability vs generated code.

## When to Split Files

Split into separate files when:

- SKILL.md exceeds 100 lines
- Content has distinct domains (finance vs sales schemas)
- Advanced features are rarely needed

**Exception:** This canonical `write-a-skill` SKILL.md may exceed 100 lines without splitting; for other skills, keep SKILL.md lean or move detail to REFERENCE.md.

## Review Checklist

After drafting, verify:

- [ ] Skill folder lives under `.cursor/skills/<skill-name>/`
- [ ] Description includes triggers ("Use when...")
- [ ] SKILL.md under 100 lines (except this canonical write-a-skill)
- [ ] No time-sensitive info
- [ ] Consistent terminology
- [ ] Concrete examples included
- [ ] References one level deep

_Source: [mattpocock/skills](https://github.com/mattpocock/skills) (MIT). Adapted for Cursor by [eunai/skills](https://github.com/eunai/skills)._
