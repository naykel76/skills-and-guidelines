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
- Keep everything needed to execute the task inside the skill file
- If external docs are referenced, name them explicitly
- Keep under 500 lines — split large reference content into separate files

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
````
