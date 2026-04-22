---
name: skill-creation
description: >-
  Use this skill whenever creating, updating, or reviewing a skill file. Do not
  wait for an explicit request — if a skill is being created or edited, this
  skill applies.
---

## Frontmatter

- `name` — kebab-case, matches the folder name
- `description` — trigger mechanism, not a summary
  - Always use YAML block scalar (`>-`) — never write the description as an
    inline string
  - Write as "Use this skill whenever [condition]..."
  - End with "Do not wait for an explicit request — if [condition], this skill
    applies." unless the skill is only valid when explicitly triggered by the
    user
  - No implementation notes or process guidance — trigger conditions only

## Body

- Write directives, not descriptions — tell the agent what to do, not what the
  skill is about
- Keep routing, trigger conditions, and non-negotiable constraints in `SKILL.md`;
  put subtopic-specific instructions in `rules/` files
- If external docs are referenced, name them explicitly
- Keep under 500 lines — split large reference content into separate files

## Rules Directory

Use a `rules/` subdirectory when a skill covers multiple distinct sub-topics
that would otherwise bloat the main `SKILL.md`.

- Each file in `rules/` should cover one focused topic
- Name files in kebab-case matching the sub-topic (e.g. `file-uploads.md`)
- Do not reference a rule file until it exists, or create it in the same change
- Do not load all rule files by default — instruct the agent to read only the
  relevant file(s) based on the current task
- Reference rule files explicitly in `SKILL.md` using a routing section

### Quick Reference section format

Add a `## Quick Reference` section to `SKILL.md` that maps each sub-topic to its rule file with a short summary of key directives:

````markdown
## Quick Reference

### File Uploads → `rules/file-uploads.md`

- Add upload handling to the component, not the form object
- Configure storage in the form object

### Form Components → `rules/form-component.md`

- Read resource config before building
- Render buttons from config

### Index Components → `rules/index-component.md`

- Build table columns from config
- Render row actions from config
````

### When to use rules vs separate skills

- Use `rules/` when sub-topics share the same trigger condition and would
  always be activated by the same kind of task
- Keep separate skills when each sub-topic has a meaningfully different trigger
  condition that benefits from its own `description` field

## Example

````yaml +code
---
name: skill-name
description: >-
  Use this skill whenever [condition]. Do not wait for an explicit
  request — if [condition], this skill applies.
---

## Section

- Directive one
- Directive two

## Quick Reference

### Topic One → `rules/topic-one.md`

- Directive one
- Directive two

### Topic Two → `rules/topic-two.md`

- Directive three
- Directive four
````
