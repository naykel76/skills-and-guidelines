---
name: livewire-index-component
description: >-
  Use this skill whenever building a Livewire Index component to list, search,
  sort, or paginate a resource collection. Do not wait for an explicit request
  — if an index or listing component is being created, this skill applies.
---

## Related skills

- `livewire-resource-config` — owns index config decisions and button mappings
- `livewire-form-component` — used alongside index for create/edit modals

## Rules

- Invoke this skill before reading any project files.
- Read the resource config before building. If index config is incomplete,
  follow the incomplete-config workflow in `livewire-resource-config`.
- Preserve existing local UI component variants — do not replace them with
  `x-gt-resource-action` unless explicitly asked.
- Do not treat visual or component normalization as an implicit task.
- When rendering a `show` action, target the public route — do not reuse the
  admin route prefix. Pass the public prefix explicitly.

## Formatting defaults

| Column | Alignment |
| ------ | --------- |
| status | center    |
| date   | center    |

## Table markup

- Use inline attributes when the header tag is easy to scan on one line.
- Wrap attributes to a second line for sortable headers with `wire:click`,
  `:direction`, or alignment attributes.
- Keep the visible column label on the same line as the closing `>`.

## Base structure

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

## Search

Add the `Searchable` trait when config has `searchableFields`.

```php +code
protected function query($query)
{
    $query = $this->applySearch($query);
    return $query;
}
```

## Sort

Add the `Sortable` trait when config has `sortColumn`.

```php +code
protected function query($query)
{
    $query = $this->applySorting($query);
    return $query;
}
```

## Combined search and sort

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

## Pagination refresh after modal save

Add a `model-saved` listener that resets the page when the index is paginated
and receives saves from a modal. Skip only if preserving the current page is
explicitly required.

```php +code
use Livewire\Attributes\On;

#[On('model-saved')]
public function refreshList(): void
{
    $this->resetPage();
}
```
