---
name: livewire-form-component
description: >-
    Build Livewire Form components for creating and editing records. Displays
    structured input fields, handles user input and submission. Activates when
    creating data entry forms, edit interfaces, or input screens.
---

## When to use me

- Creating form interfaces for user interaction
- Building create/edit/profile forms
- Any component that renders a form (full-page or modal)

## Related skills

- livewire-resource-config
- livewire-form-object

## Contract boundary

- `livewire-resource-config` owns shared config semantics and button mappings.
- This skill implements form structure and behavior from that contract.
- Do not redefine token semantics here.
- Resolve configured actions in component/base layer, not direct config calls in
  Blade.
- If form config is incomplete, follow `livewire-resource-config`'s
  incomplete-config workflow and surface any assumptions for review at the end
  of the task.

## Formatting defaults (review)

| Form area                | Default formatting                 | Notes                                                |
| ------------------------ | ---------------------------------- | ---------------------------------------------------- |
| primary text inputs      | left-aligned in content column     | Use for title/name/body-driving fields               |
| status/toggle controls   | sidebar or grouped utility section | Keep publish/release/category-style toggles together |
| media/upload preview     | centered                           | Match existing preview-card patterns                 |
| actions/footer           | right-aligned                      | Keep save/cancel actions grouped consistently        |
| helper/secondary content | separate section or accordion      | Avoid mixing with primary identity fields            |

## Implementation

### Base structure

All form components extend `Naykel\Gotime\Livewire\BaseForm`  `BaseForm`
provides `HasConfig`, `WithFormActions`, and the standard form lifecycle.

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

## Modal

- Use a single modal controlled by `showModal`.
- For delete/confirm workflows, use `selectedId` alongside `showModal`.
- Use `BaseForm::formTitle()` for modal headings instead of inline title logic.
- Use form submit baseline `wire:submit="save"`.

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
