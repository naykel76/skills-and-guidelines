# File Uploads

## Rules

- Add `WithFileUploads` to the **component**, not the form object.
- Do not declare `tmpUpload` on the form object — it is provided by `Crudable`.

<!-- these need to be reviewed -->
- Add `$storage` to the **form object** to configure disk, column, and path.
- Add `imageUrl()` to the **component** to handle preview and fallback.

## Component setup

```php +code
use Livewire\WithFileUploads;

new class extends BaseForm
{
    use WithFileUploads;
    // ...
};
```

## Form object storage config

Add to the form object class body (not inside a method):

```php +code
public array $storage = [
    'disk'     => 'public',         // default: 'public'
    'dbColumn' => 'image_path',     // default: 'file_path'
    'path'     => 'uploads/images', // default: '' (root)
];
```

## Blade

```html +code-blade
<img src="{{ $this->imageUrl() }}" alt="{{ $form->name ?? null }}">
<x-gt-filepond wire:model="form.tmpUpload" />
```

## Model URL helper

Add to the model when image preview is needed:

```php +code
public function featuredImageUrl(): string
{
    return $this->image_path
        ? Storage::disk('media')->url($this->image_path)
        : url('/svg/placeholder.svg');
}
```
