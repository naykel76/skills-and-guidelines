---
name: livewire-form-component
description: >-
  Use this skill whenever creating or editing a Livewire form component for
  data entry, create/edit interfaces, or modal forms. Do not wait for an
  explicit request — if a form component is being built or modified, this
  skill applies.
---

## Related skills

- `livewire-resource-config` — owns button mappings and config behavior
- `livewire-form-object` — owns form state, validation, and persistence

## Rules

- Invoke this skill before reading any project files.
- Read the resource config before building. If config is incomplete, follow
  the incomplete-config workflow in `livewire-resource-config`.
- Resolve configured actions in the component, not with direct config calls
  in Blade.
- Do not redefine button or action behavior here — that belongs to
  `livewire-resource-config`.

## Layout defaults

| Form area                | Default                            | Notes                                                |
| ------------------------ | ---------------------------------- | ---------------------------------------------------- |
| primary text inputs      | left-aligned in content column     | Use for title/name/body-driving fields               |
| status/toggle controls   | sidebar or grouped utility section | Keep publish/release/category-style toggles together |
| media/upload preview     | centered                           | Match existing preview-card patterns                 |
| actions/footer           | right-aligned                      | Keep save/cancel actions grouped consistently        |
| helper/secondary content | separate section or accordion      | Avoid mixing with primary identity fields            |

## Base structure

Extend `BaseForm`. It provides config access, form actions, and the standard
form lifecycle.

```php +code
<?php

use Naykel\Gotime\Livewire\BaseForm;
use App\Livewire\Forms\ModelFormObject;
use App\Models\Model;

new class extends BaseForm
{
    public ModelFormObject $form;
    protected string $modelClass = Model::class;

    protected function configKey(): string
    {
        return 'resource';
    }
};
```

## Modal forms

- Use a single modal controlled by `showModal`.
- For delete/confirm flows, use `selectedId` alongside `showModal`.
- Use `BaseForm::formTitle()` for modal headings.
- Use `wire:submit="save"` as the form submit action.

```php +code
new class extends BaseForm
{
    public string $createTitle = 'Create Resource';
    public string $editTitlePrefix = 'Edit';
    public ?string $titleField = 'title';
};
```

```html +code-blade
<x-gt-modal wire:model="showModal">
    <h4>{{ $this->formTitle() }}</h4>
    <form wire:submit="save">
        <x-gt-input wire:model="form.title" label="title" />
        <div class="tar">
            <x-gt-button wire:click="cancel" class="btn sm" text="CANCEL" />
            <x-gt-button wire:click="saveAndClose" class="btn primary sm" text="SAVE" />
        </div>
    </form>
</x-gt-modal>
```
