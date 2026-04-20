---
name: livewire-form-object
description: >-
  Use this skill whenever building a Livewire Form Object to centralize form
  data, validation, and state management separate from the UI component. Do
  not wait for an explicit request — if a form object is being created or
  modified, this skill applies.
---

## Related skills

- `laravel-database-design` — schema nullability and constraints drive validation
- `livewire-form-component` — consumes the form object and wires actions
- `livewire-resource-config` — source of truth for config-driven behavior

## Rules

- Invoke this skill before reading any project files.
- Use Gotime patterns first: `BaseForm`, `Formable`, `Crudable`, and resource
  config.
- Fall back to raw Livewire only when Gotime has no suitable pattern.
- Do not define button or action behavior here — that belongs to
  `livewire-resource-config`.

## Setup

```bash +code
php artisan livewire:form ModelFormObject
```

```php +code
namespace App\Livewire\Forms;

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

## Traits

- `Formable` — provides form state via `editing` and initializes properties
  from the model via `setFormProperties()`.
- `Crudable` — handles validate + persist, including upload-aware saves when
  storage config is present.
- Use both together for standard form objects. Override save behavior only
  when a genuinely non-standard persistence case exists.

## Validation

- Use `#[Validate(...)]` on form properties.
- Property types must match the model/data shape.
- Foreign keys should use `exists:table,id`.
- `required` / `nullable` validation must match database nullability. Resolve
  any conflict before implementing.

## `setFormProperties()` caveat

- Converts `null` to `''` for `string` properties.
- For nullable string or date fields, set explicit values in `init()` after
  calling `setFormProperties()`.
- With native `date`/`datetime` casts and `HasFormattedDates`, assign date
  fields as formatted strings in `init()`.

## Overrides

- Apply local overrides only when behavior requires it.
- Document the reason at the override point in `init()` or the component
  method.
