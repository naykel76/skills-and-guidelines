# Naykel Livewire Conventions

## Component types

- `Index`: list and manage collections
- `Form`: create/edit a single record
- `Form-Modal`: form rendered inside a modal
- `Manager`: route-level coordinator for one resource
- `Editor`: coordinated multi-part editing workflow

## Core rules

- Generate components with `php artisan make:livewire`.
- If `config/livewire.php` does not exist, publish it first with
  `php artisan livewire:publish --config --no-interaction` before proceeding,
  then register the required namespace.
- Before registering or using a namespace, read `config/livewire.php` to check
  what namespaces and paths already exist.
- Namespaces must be registered in `config/livewire.php` under
  `component_namespaces` before use. Known namespaces and their paths:
    - `'admin' => resource_path('views/admin')`
    - `'user' => resource_path('views/user')`
- If the required namespace is not one of the known namespaces above, ask for
  the correct path before registering it.
- Name components as `{namespace}::{resource}.{type}`.
- Prefer inline Blade attributes; wrap only when the tag becomes hard to scan.

## Method ordering

Always follow this order inside any component class:

1. `configKey()` — always first
2. Event listeners (`#[On(...)]` methods)
3. `query()` — always last

## Guard rails

- If naming or structure is ambiguous, stop and confirm.
- Avoid unrequested namespace changes or broad renames.
- Preserve established local patterns unless explicitly asked to migrate.
- Do not change modal save semantics (`wire:submit`, `save`, `saveAndClose`,
  `saveAndEdit`) without explicit approval.

## Examples

- `admin::course.index`
- `admin::course.manager`
- `admin::lesson.quiz-form-modal`
