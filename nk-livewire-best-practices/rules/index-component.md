# Index Component

## Rules

- Read the resource config before building.
- Always include sort — add the `Sortable` trait and `applySorting()` to the query.
- Only add features beyond the base structure if explicitly requested.
- Build the table columns from `index.columns` in config — do not infer columns from the model.
- Read `index.buttons` from config and render each button in the table row actions cell using the button components defined in `rules/resource-config.md` — every button in config must appear in the blade, no more, no less.

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

## Sort

Add the `Sortable` trait when sorting is required.

```php +code
use Naykel\Gotime\Traits\Sortable;

new class extends BaseIndex
{
    use Sortable;

    protected function query($query)
    {
        return $this->applySorting($query);
    }
}
```

## Search

Add the `Searchable` trait when searching is required.

```php +code
protected function query($query)
{
    return $this->applySearch($query);
}
```

## Combining methods

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

## Blade template

```html +code-blade
<div>
    <!-- create button will be placed here. still some refinement needed -->
    <x-gt-table>
        <x-slot:thead>
            <tr>
                <x-gt-table.th wire:click="sortBy('column')" sortable :direction="$this->getSortDirection('column')"> Column </x-gt-table.th>
                <x-gt-table.th> </x-gt-table.th>
            </tr>
        </x-slot:thead>
        <x-slot:tbody>
            @forelse($this->items as $item)
                <tr wire:key="{{ $item->id }}">
                    <td>{{ $item->column }}</td>
                    <td class="whitespace-nowrap">
                        <!-- Render buttons in the actions cell exactly as defined in `index.buttons` config. -->
                    </td>
                </tr>
            @empty
                <tr>
                    <td class="tac pxy" colspan="10">No records found...</td>
                </tr>
            @endforelse
        </x-slot:tbody>
    </x-gt-table>
    {{ $this->items->links('gotime::pagination.livewire') }}

    {{-- Include when form-modal is requested --}}
    <livewire:admin::resource.form-modal />
</div>
```
