---
name: livewire-resource-config
description: >-
  Use this skill whenever defining, reviewing, or modifying resource
  configuration, scaffolding resource components from config, or before
  modifying any resource-driven implementation. Do not wait for an explicit
  request — if resource config is involved, this skill applies.
---

## Rules

- Invoke this skill before reading any project files.
- Read `config/resources.php` before implementing anything resource-driven.
- Implement what config declares — do not infer behavior from preference.
- If config keys are ambiguous, stop and resolve before proceeding.
- Never change or extend resource config silently — surface any assumptions at
  the end of the task and ask the user to confirm.

## Config shape

- Use the full Eloquent class for `model`.
- `routePrefix` is required for route-based behavior; optional for modal-only
  components.
- Define only keys the resource actually needs.

```php +code
'resource' => [
    'model' => \App\Models\Model::class,
    'routePrefix' => 'admin.resources',
    'index' => [],
    'form' => [],
    'manager' => [],
];
```

## Button tokens

Map config `buttons` values to component methods:

- `save` → `save`
- `save-stay` → `saveAndEdit`
- `save-and-edit` → `saveAndEdit`
- `save-and-close` → `saveAndClose`
- `cancel` → `cancel`

## Button components

Map config `buttons` values to Blade components:

- `create-modal` → `<x-gt-resource-action action="create" :$routePrefix dispatchTo="admin::resource.form-modal" text="Add Resource" />`
- `create` → `<x-gt-resource-action action="create" :$routePrefix />`
- `edit-modal` → `<x-gt-resource-action action="edit" :id="$item->id" dispatchTo="admin::resource.form-modal" />`
- `edit` → `<x-gt-resource-action action="edit" :$routePrefix :id="$item->id" />`
- `show` → targets a public route, not the admin prefix. Pass the public route
  prefix manually:
  `<x-gt-resource-action action="show" routePrefix="resources" :slug="$item->slug" target="_blank" />`
- `delete` → `<x-gt-resource-action action="delete" :id="$item->id" />`

## Reviewing config vs implementation

1. Read the resource config.
2. Compare against the implementation.
3. List mismatches only: resource, section, configured behavior, actual
   implementation, difference.
4. Apply fixes only after approval.

## Incomplete config

- Treat existing config as authoritative.
- If config is missing details and the likely shape is straightforward, proceed
  with sensible assumptions.
- At the end of the task, list any values you assumed (buttons, columns, form
  sections, route behavior) and ask the user to confirm.
- If missing config has multiple valid interpretations or would lock in
  important behavior, stop and ask first.

## Overrides

- Leave existing local component overrides (e.g. `x-gt-button.action`) as-is
  unless asked to migrate.
- If a button is non-standard for the current context, present the current
  code, the recommended replacement, and the reason — then wait for approval
  before changing it.
- Exception: if the code has a note indicating migration is intended (e.g.
  "can be removed/replaced"), treat it as approved.
