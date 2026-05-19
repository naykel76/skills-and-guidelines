# Component Docs

Use for named components or component families.

A single-component page documents one component. A multi-component page
covers several closely related components that belong together.

## Single-Component Pages

Use when the page is about one component.

### Structure

1. Shared title and lead
2. `## Basic Usage` — minimum working example
3. `## Properties` — when the component exposes a clear API
4. Additional `##` sections as needed

### Rules

- Show the minimum working usage early.
- Keep the lead focused on purpose, not implementation detail.
- Add sections only when behaviour, states, or variants need explanation.
- Put the properties table near the top when it helps the reader scan the API.

## Multi-Component Pages

Use when the page groups several related components or variants under one topic.

### Structure

1. Shared title and lead for the group
2. Each component becomes a `##` section
3. Repeat the single-component structure inside each one using `###` headings

### Rules

- Only group components that genuinely belong together.
- Keep the same section pattern for each component in the group.
- Do not mix unrelated components just to reduce page count.

## Variants and Families

When a component belongs to a family, document the public usage first.

If needed, explain how the family is organised:

- default component
- base component
- named variants

Only go into architectural detail when the component family structure is part
of what the page needs to explain.

