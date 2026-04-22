# Form Component

## Rules

- Read the resource config before building.
- Render buttons from config — use the form button tokens in `rules/resource-config.md`.

## Base structure

Shared by both form and form-modal component types.

```php +code
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

## Form (route-based)

```html +code-blade
<div>
    <div class="my flex va-c space-x-1">
        <x-gt-button.primary wire:click="saveAndEdit" wire:dirty.attr.remove="disabled" text="Save Changes" disabled />
        <a href="{{ route($routePrefix . '.index') }}" class="btn dark">Back to list</a>
        <div wire:dirty class="txt-red">Unsaved changes...</div>
    </div>
    <form wire:submit="save">
        {{-- fields from form.fields config --}}
    </form>
</div>
```

## Form modal

Add `createTitle`, `editTitlePrefix`, and `titleField` to the PHP class for the modal heading.

```php +code
new class extends BaseForm
{
    public string $createTitle = 'Create Resource';
    public string $editTitlePrefix = 'Edit';
    public ?string $titleField = 'name';
};
```

```html +code-blade
<x-gt-modal wire:model="showModal">
    <h4>{{ $this->formTitle() }}</h4>
    <form wire:submit="save">
        {{-- fields from form-modal.fields config --}}
        <div class="tar">
            {{-- buttons from form-modal.buttons config — see rules/resource-config.md form button tokens --}}
        </div>
    </form>
</x-gt-modal>
```
