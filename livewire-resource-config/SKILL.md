---
name: livewire-resource-config
description: >-
    Define resource configuration in config/resources.php as the contract layer.
    This contract can drive component implementation, scaffolding, and testing.
---

## When to use me

- Defining or changing resource behavior contracts
- Creating or updating component config (any component type)
- Scaffolding/generating resource components from config
- Building tests/assertions from configured behavior
- Before modifying any resource-driven implementation

## Foundation

- Follow architecture priority from `naykel-laravel`.
- This skill assumes `naykel-laravel` is also loaded.

## What this is

- `config/resources.php` is a contract layer.
- The contract is open-ended and may be consumed by anything in the system.
- Do not couple config meaning to one specific component type.
- Implementations consume the contract; they should not redefine contract rules.
- Livewire/Blade examples are reference implementations, not a restriction on
  where the contract can be consumed.

## Core contract rules

- Do not infer behavior by preference; implement what config declares.
- Shared behavior definitions belong here (single source), not duplicated in
  other skills.
- If contract keys/tokens are ambiguous, stop and resolve before implementation.
- When resource config is incomplete but the task can still be completed with
  narrow, sensible assumptions, you may proceed using best judgment.
- If you do proceed with assumptions, explicitly summarize the config values you
  assumed at the end of the task and prompt the user to review/sign off on
  them.
- Never silently change or extend resource config without surfacing those
  assumptions in your final handoff.

## Base contract shape

- Always use full Eloquent model class for `model`.
- `routePrefix` is required for route-based behavior and optional for modal-only
  behavior.
- Define only keys needed by the resource.
- If behavior is shared, define it in config once.
- Preserve local overrides unless explicitly asked to migrate.

```php +code
'resource' => [
    'model' => \App\Models\Model::class,
    'routePrefix' => 'admin.resources',
    // section keys are open-ended and contract-driven
    'index' => [],
    'form' => [],
    'manager' => [],
];
```

## Button tokens (contract-level)

- Tokens are agnostic behavior markers, not tied to one component type.
- Wherever a section declares `buttons`, use the token mapping below.
- If token is unknown, stop and confirm before implementation.
- Flow: config declares tokens; implementations render corresponding
  code/methods.

## Action token mapping (single source)

- `save` -> `save`
- `save-stay` -> `saveAndEdit`
- `save-and-edit` -> `saveAndEdit`
- `save-and-close` -> `saveAndClose`
- `cancel` -> `cancel`

## Button code mapping (single source)

- `create-modal` -> `<x-gt-resource-action action="create" :$routePrefix dispatchTo="admin::resource.form-modal" text="Add Resource" />`
- `create` -> `<x-gt-resource-action action="create" :$routePrefix />`
- `edit-modal` -> `<x-gt-resource-action action="edit" :id="$item->id" dispatchTo="admin::resource.form-modal" />`
- `edit` -> `<x-gt-resource-action action="edit" :$routePrefix :id="$item->id" />`
- `show` -> public-facing route. Do not assume the admin `routePrefix`.
  Provide the public route prefix manually (typically the resource route prefix
  without `admin.`), for example:
  `<x-gt-resource-action action="show" routePrefix="resources" :slug="$item->slug" target="_blank" />`
- `delete` -> `<x-gt-resource-action action="delete" :id="$item->id" />`

## Review workflow

1. Read the resource config first.
2. Compare implementation against config contract.
3. Produce mismatch list only:
   - resource
   - section/context
   - configured behavior
   - actual implementation
   - difference
4. Apply fixes only after approval.

## Incomplete config workflow

- Default: treat existing config as authoritative.
- If the config is missing details needed to finish the resource and the likely
  contract shape is straightforward, complete the implementation with sensible
  assumptions rather than stopping the task mid-flow.
- At the end of the task, include a short config review prompt covering any
  values you added or assumed (for example `buttons`, `columns`, `form`
  sections, or route-driven behavior).
- If the missing config has multiple materially different interpretations or the
  assumption would lock in important behavior, stop and ask before proceeding.

## Local override rule

- If a view already uses a local override component (for example
  `x-gt-button.action`), leave it as-is unless explicitly asked to migrate.
- Do not perform automatic normalization/refactor passes.

## Non-standard button prompt rule

- If a button implementation is non-standard for the current resource config
  context, do not auto-refactor.
- First present:
  - current button code
  - recommended standard replacement
  - reason for replacement
- Apply only after explicit approval.
- Exception: if the code contains an explicit local note indicating migration is
  intended (for example "can be removed/replaced"), treat it as approved.
