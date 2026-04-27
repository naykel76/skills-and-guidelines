---
name: skill-creation
description: >-
  Use this skill whenever creating, updating, or reviewing a skill file. Do not
  wait for an explicit request — if a skill is being created or edited, this
  skill applies.
---

## Two Patterns — Pick One

A skill is either a **Standalone Skill** or a **Routed Skill**. These are mutually
exclusive. Never mix them.

**Decision rule:** Does this skill cover multiple distinct sub-topics, each
needing their own instruction file? If yes → Routed Skill. If no → Standalone Skill.

When in doubt, start with a Standalone Skill.

## Pattern 1: Standalone Skill

A single `SKILL.md` file. No `rules/` directory.

Use when the skill covers one focused topic that fits in a single file.

### Structure

```bash +code
skill-name/
└── SKILL.md
```

### Template

```yaml +code
---
name: skill-name
description: >-
  Use this skill whenever [condition]. Do not wait for an explicit
  request — if [condition], this skill applies.
---

## Rules

- Directive one
- Directive two

## Workflow

1. Step one
2. Step two
```

---

## Pattern 2: Routed Skill

A `SKILL.md` that acts as a router, plus a `rules/` directory containing one
file per sub-topic.

Use when the skill covers multiple distinct sub-topics that would bloat a single
file, and all sub-topics share the same trigger condition.

### Structure

```bash +code
skill-name/
├── SKILL.md
└── rules/
    ├── topic-one.md
    └── topic-two.md
```

### Rules for the `rules/` directory

- Each file covers one focused sub-topic
- Name files in kebab-case matching the sub-topic (e.g. `file-uploads.md`)
- Do not load all rule files by default — the agent reads only the relevant
  file based on the current task
- Do not reference a rule file until it exists, or create it in the same change

### Template

```yaml +code
---
name: skill-name
description: >-
  Use this skill whenever [condition]. Do not wait for an explicit
  request — if [condition], this skill applies.
---

# Skill Title

One-line context for what this skill governs.

## Non-Negotiable Constraint

State the single most important rule here — the one that must be followed
regardless of which rule file is read. Omit this section if no such rule exists.

## Quick Reference

Read the relevant rule file before proceeding. Summaries below are for
navigation only — the rule files take precedence.

### Topic One → `rules/topic-one.md`

- Key directive
- Key directive

### Topic Two → `rules/topic-two.md`

- Key directive
- Key directive
```

---

## Frontmatter (both patterns)

- `name` — kebab-case, matches the folder name
- `description` — trigger mechanism, not a summary
  - Always use YAML block scalar (`>-`) — never write as an inline string
  - Write as "Use this skill whenever [condition]..."
  - End with "Do not wait for an explicit request — if [condition], this skill
    applies." unless the skill is only valid when explicitly triggered
  - No implementation notes or process guidance — trigger conditions only

## Body (both patterns)

- Write directives, not descriptions — tell the agent what to do, not what the
  skill is about
- Keep trigger conditions and non-negotiable constraints in `SKILL.md`
- If external docs are referenced, name them explicitly with their path
