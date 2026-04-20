---
name: livewire-form-object
description: >-
  Use this skill whenever building a Livewire Form Object to centralize form
  data, validation, and state management separate from the UI component. Do
  not wait for an explicit request — if a form object is being created or
  modified, this skill applies.
---

## Related skills

- `laravel-database-design` (schema constraints drive validation and typing)
- `livewire-form-component` (form object consumption and action wiring)
- `livewire-resource-config` (config-driven behavior and source of truth)

## Contract boundary

- Shared behavior semantics belong to `livewire-resource-config`.
- This skill defines form state, validation, typing, and persistence only.
- Do not define button/token semantics in this skill.

## Rule of precedence

- Use Gotime patterns first: `BaseForm`, `Formable`, `Crudable`, and resource
  config.
- Use raw Livewire-only approaches only when Gotime does not provide a suitable
  pattern.

## Initial setup

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

## Core traits

- `Formable`
  - Provides the form state baseline via `editing`.
  - Supports initializing form properties from the model
    (`setFormProperties(...)`).

- `Crudable`
  - Provides the standard persistence workflow (validate + persist).
  - Handles upload-aware save behavior when storage config is present.

- Use both traits together for standard form objects.
- Override save behavior only when a real non-standard persistence case exists.
- If trait behavior conflicts, use an explicit local override and document why.

## Validation and typing rules

- Use `#[Validate(...)]` on form properties.
- Property types should match the expected model/data shape.
- Foreign keys should use `exists:table,id` where applicable.
- `required` / `nullable` validation must align with database nullability.
- If DB constraints and form validation conflict, resolve the conflict before
  implementation.

## Formable caveat

- `setFormProperties()` converts `null` to `''` for `string` properties.
- For nullable string/date form fields, set explicit values in `init(...)` after
  `setFormProperties(...)`.
- With native `date` / `datetime` casts + `HasFormattedDates`, assign form date
  fields as formatted strings in `init(...)`.

## Exceptions

- Keep Gotime baseline patterns.
- Apply explicit local overrides only when behavior requires it.
- Document overrides at the implementation point (`init(...)` or component
  method).
