# Resource Config

## Rules

- Check if `config/resources.php` exists for the requested component types.
- If config does not exist, create it before building any component.
- Determine buttons from the component types table. If the combination isn't in the table, ask — never assume or infer.
- If config exists, read it and implement from it exactly as written.
- Do not modify existing config without explicit instruction.
- If existing config conflicts with the request, stop and surface it — do not resolve it yourself.

## Workflow

```bash +code
User prompt
    ↓
Identify component types (index, form, form-modal)
    ↓
Does config exist?
    ├── yes → read it → implement exactly as written
    │             ↓
    │         conflicts with request? → stop and surface
    │
    └── no  → determine buttons from component types table
                    ↓
                combination not in table? → ask before proceeding
                    ↓
                columns/fields not specified? → ask: "specify or use sensible defaults?"
                    ↓
                create config (columns, fields, buttons)
                    ↓
                build components from config
```

## Routes

- `routePrefix` is required when `index` or `form` is present.
- `form-modal` does not require a `routePrefix`.
- When `index` or `form` is requested, add routes to `routes/web.php`.

```php +code
Route::prefix('resources')->name('resources')->group(function () {
    Route::livewire('/', 'admin::resource.index')->name('.index');
    Route::livewire('/create', 'admin::resource.form')->name('.create');
    Route::livewire('/{resource:id}/edit', 'admin::resource.form')->name('.edit');
});
```

## Config shape

```php +code
'resource' => [
    'model' => \App\Models\Model::class,
    'routePrefix' => 'admin.resources',

    'index' => [
        'columns' => ['column'],
        'buttons' => ['create', 'edit'],
        'searchableFields' => ['column'], // optional — only include when search is explicitly requested
        'sortColumn' => 'column',
    ],

    'form' => [
        'fields' => ['field'],
        'buttons' => ['save', 'cancel'],
    ],
];
```

## Component types and buttons

The requested component types determine which buttons appear in the config:

| Requested components   | Index buttons                | Form buttons               |
| ---------------------- | ---------------------------- | -------------------------- |
| `index` + `form`       | `create`, `edit`             | `save`, `cancel`           |
| `index` + `form-modal` | `create-modal`, `edit-modal` | `save-and-close`, `cancel` |

## Button components

Map config `buttons` values to Blade components:

- `cancel` → `<x-gt-button wire:click="cancel" class="btn sm" text="CANCEL" />`
- `create-modal` → `<x-gt-resource-action action="create" :$routePrefix dispatchTo="admin::resource.form-modal" text="Add Resource" />`
- `create` → `<x-gt-resource-action action="create" :$routePrefix />`
- `edit-modal` → `<x-gt-resource-action action="edit" :id="$item->id" dispatchTo="admin::resource.form-modal" />`
- `edit` → `<x-gt-resource-action action="edit" :$routePrefix :id="$item->id" />`
- `save-and-close` → `<x-gt-button wire:click="saveAndClose" class="btn primary sm" text="SAVE" />`
- `save-edit` → `<x-gt-button wire:click="saveAndEdit" class="btn primary sm" text="SAVE" />`
- `save-new` → `<x-gt-button wire:click="saveAndNew" class="btn primary sm" text="SAVE" />`
- `save` → `<x-gt-button type="submit" class="btn primary sm" text="SAVE" />`
- `delete` → `<x-gt-resource-action action="delete" :id="$item->id" />`

The `delete` token also requires a modal companion placed after pagination (or at the end of the component). The `WithFormActions` trait provides `selectedId`:

```html +code-blade
<x-gt-modal.delete wire:model="selectedId" :$selectedId />
```
