---
name: grill-with-docs
description: Runs a documentation-aware grilling session that challenges a plan against domain language and recorded decisions, updating CONTEXT and ADRs as terms crystallize. Use when the user wants to stress-test a plan against project terminology and documented architecture choices.
---

# Grill with docs

Interview the user relentlessly about every aspect of the plan until you reach a shared understanding. Walk each branch of the design tree, resolving dependencies between decisions one at a time. For each question, give your recommended answer.

- Ask one question at a time; wait for feedback before continuing.
- If a question can be settled by exploring the workspace (code, config, docs), explore it instead of guessing.

## Domain awareness

While exploring, also look for existing documentation.

### File structure

Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

If `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. The map points to where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily — only when you have something to write. If no `CONTEXT.md` exists, create one when the first term is resolved. If no `docs/adr/` exists, create it when the first ADR is needed.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with existing language in `CONTEXT.md`, call it out immediately. Example framing: the glossary defines "cancellation" as X, but they seem to mean Y — which is it?

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. Example: they say "account" — do they mean the Customer or the User?

### Discuss concrete scenarios

When domain relationships are discussed, stress-test with specific scenarios. Invent scenarios that probe edge cases and force precision about boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it.

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` in the same turn. Do not batch; capture as decisions happen. Use the CONTEXT.md structure and rules in [REFERENCE.md](REFERENCE.md).

Keep `CONTEXT.md` decoupled from implementation trivia. Only include terms that matter to domain experts.

### Offer ADRs sparingly

Only offer an ADR when all three are true:

1. **Hard to reverse** — changing your mind later has real cost
2. **Surprising without context** — a future reader will wonder why it was done this way
3. **Result of a real trade-off** — genuine alternatives existed and one was chosen for specific reasons

If any is missing, skip the ADR. Use the ADR template and rules in [REFERENCE.md](REFERENCE.md).

_Source: [mattpocock/skills](https://github.com/mattpocock/skills) (MIT). Adapted for Cursor by [eunai/skills](https://github.com/eunai/skills)._
