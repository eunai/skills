---
name: revise-prompt
description: Restructures a draft into a structured, agent-readable prompt with a stable core scaffold and optional sections, then delivers it in one copy-paste code fence plus a short change summary. Use when the user says /revise, revise prompt, rewrite for agents, refactor instructions for another AI, or wants a clearer agent-facing prompt.
---

# Revise prompt (agent-readable)

Turn an informal or messy prompt into something another model or agent can follow reliably.

## Decide: grill or revise immediately

**Grill first (one question at a time; wait for each answer)** when any of these are true:

- The draft is thin: missing goals, constraints, outputs, or success criteria.
- Instructions conflict or sequencing is unclear.
- Critical context is implicit (who/what/tooling/runtime) and would change behavior.
- The user asks for clarification options or `/grill-me` style review.

Otherwise **revise immediately** from the text you have.

## Revision shape (always)

Inside the fenced block, use **this fixed core**, in order, with tight bullets:

1. **Role** — who the executing agent is; authority boundaries.
2. **Task** — what “done” means in one pass.
3. **Constraints** — must/must-not, scope limits, policies, hallucination avoidance.
4. **Inputs** — what is provided (files, URLs, datasets, assumptions).
5. **Outputs** — exact deliverable shape (sections, bullets, code, filenames).
6. **Edge cases** — ambiguity handling, escalation, retries, failures.

Keep the user’s vocabulary where it carries domain meaning; tighten phrasing elsewhere.

### Optional extras (include only when they add clarity)

Examples: **Context**, **Examples**, **Tooling/environment**, **Definition of done**, **Anti-goals**, **Safety/refusals**, **Citation rules**.

## Reply format

1. **Change summary**: 3–8 bullets, plain language (“Added explicit output schema”, “Split conflicting constraints”).
2. A blank line.
3. **Exactly one fenced block** wrapping the **full revised prompt only** — use a generic fence (opening ``` on its **own line**), no language tag required. Nothing outside that block should duplicate the revised prompt verbatim.

Preserve the draft’s natural language unless the user requests a locale switch.

_Source: [eunai/skills](https://github.com/eunai/skills) (MIT)._
