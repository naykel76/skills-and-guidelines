---
name: nk-livewire-best-practices
description: >-
  Use this skill whenever creating, modifying, or reviewing Livewire components
  in a Naykel (Gotime) project. Do not wait for an explicit request — if a
  Livewire component is involved, this skill applies.
---

# Naykel Livewire Best Practices

Patterns and conventions for Livewire components built on the Naykel Gotime framework.

## Consistency First

Before applying any rule, check what the application already does. Follow sibling Gotime Livewire components and existing resource config unless they conflict with the request or this skill.

These rules are defaults for when no local pattern exists yet, not overrides.

## Config First

Before building any component, the resource config must exist and be read. All structure — columns, fields, buttons — is derived from config, never inferred from models, migrations, or the prompt.

If config exists, implement exactly as written. If it conflicts with the request, stop and surface the conflict — do not resolve it yourself.

## Quick Reference

Read the relevant rule file before building. Summaries below are defaults only — full rules take precedence.

### 1. Resource Config → `rules/resource-config.md`

- Check for `config/resources.php` before building any component
- If config does not exist, create it before building any component
- Determine buttons from the component types table — ask if the combination isn't listed
- `routePrefix` is required for `index` and `form`; not required for `form-modal`
- Never modify existing config without explicit instruction
- Config conflicts with the request? Stop and surface — never resolve yourself

### 2. Index Component → `rules/index-component.md`

- Extend `BaseIndex`; set `$modelClass` and `configKey()`
- Always include sort — add `Sortable` trait and `applySorting()` to the query
- Build table columns from `index.columns` in config — never infer from the model
- Render `index.buttons` from config exactly — no more, no less
- Only add features beyond the base structure if explicitly requested

### 3. Form Component → `rules/form-component.md`

- Extend `BaseForm`; declare `ModelFormObject $form` and `$modelClass`
- Route-based forms: `saveAndEdit` for save action, link back to index via `$routePrefix`
- Form-modal: add `createTitle`, `editTitlePrefix`, and `titleField` for the modal heading
- Render buttons from config using the form button tokens in `rules/resource-config.md`

### 4. Form Object → `rules/form-object.md`

- Use Gotime traits: `Formable` and `Crudable`
- Name it `ModelFormObject` — never `ModelForm`
- Build properties from `form.fields` in config — never infer from model, migration, or prompt
- Use `#[Validate(...)]` on properties by default
- Only use `rules()` when a unique constraint requires `Rule::unique()->ignore()`
- `required` / `nullable` validation must match database nullability — resolve conflicts before implementing
- `setFormProperties()` converts `null` to `''` for string properties — only override in `init()` for date fields

### 5. File Uploads → `rules/file-uploads.md`

- Add `WithFileUploads` to the **component**, not the form object
- Do not declare `tmpUpload` on the form object — it is provided by `Crudable`
- Add `$storage` to the **form object** to configure disk, column, and path
- Add `imageUrl()` to the **component** to handle preview and fallback

## How to Apply

Read the relevant rule files before responding — do not rely on the Quick Reference summaries alone.

1. Identify the component types being built: index, form, form-modal, form object, file uploads
2. Read `rules/resource-config.md` first — config drives everything
3. Read the relevant component rule files for the task
4. Check sibling files for existing patterns — follow those first per Consistency First
