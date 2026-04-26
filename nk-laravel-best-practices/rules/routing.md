# Routing

## Admin Edit Routes Must Bind by ID

Admin edit routes must always use `{model:id}` — not `{model}`. This is
explicit, future-proof, and consistent regardless of whether the model overrides
`getRouteKeyName()`. If a slug-based route key is ever added to the model, the
admin route will not break.

`x-gt-resource-action` always passes `:id`, so the route must match.

Incorrect:
```php
Route::livewire('programs/{program}/edit', 'admin::program.form')->name('.programs.edit');
```

Correct:
```php
Route::livewire('programs/{program:id}/edit', 'admin::program.form')->name('.programs.edit');
```

Public-facing routes can continue to use slug-based binding — this rule applies
to admin/back-office edit routes only.
