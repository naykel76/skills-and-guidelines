---
name: livewire-form-object
description: >-
  Use this skill whenever a form component is being created or modified. This
  skill MUST be invoked before reading any project files — invoke it
  immediately when the request mentions a form component, as a form object is
  always required. Do not wait for an explicit request — if a form component
  is involved, this skill applies.
---

## Rules

- Invoke this skill before reading any project files.
- Use Gotime patterns first: `Formable` and `Crudable`.
- Name the form object `ModelFormObject` — not `ModelForm`.
- Build form properties from `form.fields` in config — do not infer fields from the model, migration, or prompt.

## Validation

- Use `#[Validate(...)]` on form properties by default.
- Only use a `rules()` method when unique constraints require `Rule::unique()->ignore()`.
- `#[Validate(...)]` and `rules()` co-exist — use attributes on all other properties and `rules()` only for the unique-constrained fields.
- `rules()` must be `protected`.
- Pass the model instance to `->ignore()`, not the id: `->ignore($this->editing)`.
- Property types must match the model/data shape.
- Foreign keys should use `exists:table,id`.
- `required` / `nullable` validation must match database nullability. Resolve any conflict before implementing.

```php +code
#[Validate('nullable|string|max:255')]
public string $name = '';

public string $code = ''; // validated in rules()

#[Validate('nullable|string|max:255')]
public string $headline = '';

protected function rules(): array
{
    return [
        'code' => ['required', 'string', 'max:255', Rule::unique('widgets', 'code')->ignore($this->editing)],
    ];
}
```

## Overrides

- Apply local overrides only when behavior requires it.
- Document the reason at the override point in `init()` or the component method.

## `setFormProperties()` caveat

- Converts `null` to `''` for `string` properties.
- Declare nullable string properties as `string = ''` — `setFormProperties()` handles the null conversion correctly.
- Only override in `init()` for special cases: date fields that require formatter methods.
- With native `date`/`datetime` casts and `HasFormattedDates`, assign date fields as formatted strings in `init()`.

```php +code
public function init(Model $model): void
{
    $this->editing = $model;
    $this->setFormProperties($this->editing);

    // Date fields need formatter methods — setFormProperties() cannot handle these
    $this->started_at = $model->started_at ? $model->startedAt() : null;
}
```

## Setup

```bash +code
php artisan livewire:form ModelFormObject
```

```php +code
use App\Models\Model;
use Livewire\Form;
use Naykel\Gotime\Traits\Crudable;
use Naykel\Gotime\Traits\Formable;

class ModelFormObject extends Form
{
    use Crudable, Formable;

    public function init(Model $model): void
    {
        $this->editing = $model;
        $this->setFormProperties($this->editing);
    }
}
```
