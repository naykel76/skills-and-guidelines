---
name: livewire-index-component
description: >-
    Build Livewire Index components that display and manage resource collections.
    Handles listing, searching, filtering, sorting, pagination, and actions on
    records. Activates when creating resource listing pages, data tables, or browse
    interfaces.
---

## When to use me

- Creating a listing/index page for a resource
- Building the main entry point for browsing records
- User asks for searchable, sortable, or paginated lists

## Foundation

- Follow architecture priority from `naykel-laravel`.
- Use `livewire-resource-config` for shared config semantics and button mappings.

## Related skills

- `livewire-resource-config`
- `livewire-form-component`

## Implementation

### Guard rails

- Preserve existing local UI component variants in index views.
- If a project uses local action components (for example `x-gt-button.action`),
  do not replace them with `x-gt-resource-action` unless explicitly requested.
- Do not treat visual/component normalization as an implicit task.
- Treat `livewire-resource-config` as the owner of index contract decisions. If
  index config is incomplete, follow its incomplete-config workflow rather than
  inventing undocumented behavior silently.
- When rendering a `show` action from config, assume it targets a public-facing
  route and pass the public route prefix explicitly rather than reusing the
  admin resource prefix.

### Formatting defaults

| Column | Alignment | Notes |
| ------ | --------- | ----- |
| status | center    |       |
| date   | center    |       |

### Table markup readability

- Prefer inline Blade attributes when a table header tag remains easy to scan.
- Wrap attributes onto a second line when a table header becomes harder to read
  on one line, especially for sortable headers with `wire:click`,
  `:direction`, or alignment attributes.
- Keep the visible column label on the same line as the closing `>` where
  practical.

### Base structure

```php +code
use App\Models\Model;
use Naykel\Gotime\Livewire\BaseIndex;

new class extends BaseIndex
{
    protected string $modelClass = Model::class;

    protected function configKey(): string
    {
        return 'resource';
    }

    protected function query($query)
    {
        return $query;
    }
}
```

## Features

### Search

- If config has `searchableFields`, add `Searchable` trait.
- Apply search in `query()`.

```php +code
protected function query($query)
{
    $query = $this->applySearch($query);
    return $query;
}
```

### Sort

- If config has `sortColumn`, add `Sortable` trait.
- Apply sorting in `query()`.

```php +code
protected function query($query)
{
    $query = $this->applySorting($query);
    return $query;
}
```

### Combining features

```php +code
use Naykel\Gotime\Traits\Searchable;
use Naykel\Gotime\Traits\Sortable;

new class extends BaseIndex
{
    use Searchable, Sortable;

    protected function query($query)
    {
        $query = $this->applySorting($query);
        $query = $this->applySearch($query);

        return $query;
    }
}
```

### Pagination refresh after modal save

- If the index is paginated and receives `model-saved` from modal create/edit,
  add a refresh listener that calls `resetPage()`.
- This avoids stale/empty pages after create/edit mutations.
- Exception: only skip this if preserving current page is explicitly required.

```php +code
use Livewire\Attributes\On;

#[On('model-saved')]
public function refreshList(): void
{
    $this->resetPage();
}
```
