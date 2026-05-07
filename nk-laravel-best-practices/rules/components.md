# Components

## Default + Base + Variant Structure

Component families should follow a default/base/variant pattern where it fits.

- The default component is the normal public entry point.
- The base component holds shared markup, structure, and behaviour.
- Variants build on the base for named presentations or specific use cases.

Default:
```blade
{{-- button/default.blade.php --}}
<x-gt-button.base {{ $attributes->merge(['class' => 'btn']) }}>
    {{ $slot }}
</x-gt-button.base>
```

Base:
```blade
{{-- button/base.blade.php --}}
@aware(['text' => '', 'icon' => false, 'iconPosition' => 'left'])
<button {{ $attributes->merge(['type' => 'button']) }}>
    {{ $slot }}
</button>
```

Variant:
```blade
{{-- button/primary.blade.php --}}
<x-gt-button.base {{ $attributes->merge(['class' => 'btn primary']) }}>
    {{ $slot }}
</x-gt-button.base>
```

Prefer keeping shared logic in the base. Variants should stay thin where
possible, but may add light logic when the component family needs it.

Do not duplicate shared logic in multiple variants. If a variant needs
something the base should support, extend the base instead of forking the
family.

## Blade-only vs PHP Class

Use a PHP class when the component needs computed properties, method logic, or
config resolution (e.g., resolving an icon alias, determining MIME types).

Use Blade-only for structural and presentational components. Most components
are Blade-only. When in doubt, start Blade-only and add a PHP class only when
the template logic becomes awkward.

## Props and Slots

Blade-only components declare props at the top with `@props([])`. Everything
not in `@props` is available via `$attributes`.

```blade
@props(['title', 'maxWidth' => 'md'])
```

Use named slots for flexible composition:

```blade
@if (isset($footer))
    <div {{ $footer->attributes->class(['bx-footer']) }}>
        {{ $footer }}
    </div>
@endif
```

Merge user attributes with defaults rather than overriding them:

```blade
{{ $attributes->merge(['class' => 'btn']) }}
```

## @aware for Shared Props

Use `@aware` in a child component to access props declared on a parent without
passing them explicitly. This is the correct pattern for base components that
need to read props set on their wrapper variant.

```blade
{{-- In base.blade.php --}}
@aware(['icon' => false, 'iconPosition' => 'left'])
```

Do not pass these props manually through every variant — `@aware` exists
precisely to avoid that.


