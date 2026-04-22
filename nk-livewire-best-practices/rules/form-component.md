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

    public function mount(): void
    {
        $this->loadFormConfig($this->configKey());
    }
};
```

## Config and form lifecycle

Config loading and form model initialisation are separate concerns.

- Load config in `mount()` for `BaseForm` components when the component consumes config state such as fields, buttons, route prefix, labels, or other form config.
- Initialise the form model before the component reads, renders, validates, or saves form state.
- Route-based forms usually initialise the model in `mount()` because the form renders immediately.
- `mount()` may be omitted when the component does not consume config state and form state is initialised entirely by `create()` / `edit()`.
- Do not initialise a blank model in `mount()` just for ceremony when `create()` / `edit()` already owns the form state.

Route-based form example:

```php +code
public function mount(Model $model): void
{
    $this->loadFormConfig($this->configKey());

    $model = $model->exists ? $model : new $this->modelClass();
    $this->form->init($model);
}
```

Form-modal example when create/edit actions initialise the form:

```php +code
public function mount(): void
{
    $this->loadFormConfig($this->configKey());
}
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

Use `formTitle()` for modal headings. The base form provides fallback titles from `configKey()`.

Only add title override properties when the fallback title is not specific enough. Do not add `titleField` unless the base form implementation actually uses it.

```php +code
new class extends BaseForm
{
    public string $createTitle = 'Create Webinar';
    public string $editTitlePrefix = 'Edit Webinar';
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