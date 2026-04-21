# Naykel Laravel Conventions

## Core rules

- Follow Laravel conventions first.
- Use explicit types (properties, params, returns).
- Prefer early returns over nested branching.
- Use migration `up()` only.
- Keep pricing/amount fields nullable without default `0`.

## Architecture priority

- Gotime is a foundational application package in this codebase (Livewire base
  components, shared traits, UI components, file handling, casts, markdown
  extensions, and supporting services).
- Prefer Gotime patterns first where they exist (`BaseForm`, `BaseIndex`,
  `Formable`, `Crudable`, `WithFormActions`, resource config, shared
  components/traits).
- Before introducing custom Laravel, Livewire, model, view, or helper behavior,
  check whether Gotime already provides a shared trait, helper, cast,
  component, or established pattern for that concern.
- Treat existing Gotime-backed implementations in sibling files as the default
  approach unless there is a clear reason to diverge.
- Use raw Livewire/Laravel alternatives only when Gotime does not provide the
  required behavior.

## Shared model helpers

- `HasFormattedDates` is an example of the broader Gotime-first rule above.
- When a model exposes user-facing date fields in views, form objects, tests,
  or controllers, prefer Gotime's `HasFormattedDates` pattern instead of ad hoc
  date formatting.
- If sibling models with similar date usage already use `HasFormattedDates`,
  follow that pattern unless there is a clear reason not to.
- Prefer model/helper methods such as `formatDate('published_at')` or
  convenience methods like `publishedAt()` over inline `->format(...)` calls in
  Blade and other callers.
