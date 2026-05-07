---
name: nk-laravel-best-practices
description: >-
  Use this skill whenever writing, reviewing, or refactoring any Laravel code
  or components in Naykel applications. This includes Naykel-specific routing,
  model, architectural, and component patterns that extend or override general
  Laravel best practices. Do not wait for an explicit request. If any Laravel
  code or components are being created, reviewed, or modified in a Naykel
  application, this skill applies.
---

# Naykel Laravel Best Practices

Naykel-specific patterns and rules for Laravel code. These extend the general
`laravel-best-practices` skill — apply both, with this skill taking precedence
where they conflict.

## Quick Reference

Read the relevant rule file before responding — do not rely on summaries alone.

### Routing → `rules/routing.md`

- Admin edit routes must use `{model:id}`, not `{model}`
- Public-facing routes may continue to use slug-based binding

### Components → `rules/components.md`

- Base handles all logic; variants are thin wrappers that compose the base
- Use Blade-only unless computed logic is needed; then add a PHP class
- Use `@aware` for shared props between base and variants

### Database Design → `rules/database-design.md`

- Ask whether to update an existing migration or create a new forward migration
- Update the project's schema/design document when any schema or constraint changes
- Follow the standard column reference for names, types, and ordering
