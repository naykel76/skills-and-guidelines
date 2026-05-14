---
name: mermaid-diagram
description: >-
  Use this skill whenever creating or editing a Mermaid diagram file. Do not
  wait for an explicit request — if a diagram is being created or edited, this
  skill applies.
---

## Files

- Store diagrams in `resources/design/` as `.mmd` files.
- Name files in PascalCase matching the diagram subject (e.g. `CourseCompleteSequenceDiagram.mmd`).

## Scope

- Diagram only the process named — do not expand into related flows unless asked.
- If the scope is ambiguous, grill me before proceeding.

## Sequence Diagrams

- Use `actor` for humans, `participant` for system components.
- Use short aliases on `participant` when the display name is long.
- Use `->>` for calls, `-->>` for returns/responses.
- Use `Note over` for phase labels only when there are distinct phases — omit for single-phase diagrams.
- Do not use semicolons (`;`) inside arrow labels — Mermaid treats them as statement terminators.
