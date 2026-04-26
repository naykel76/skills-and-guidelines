---
name: nk-laravel-best-practices
description: >-
  Use this skill whenever writing, reviewing, or refactoring Laravel PHP code
  in Naykel applications. This includes Naykel-specific routing, model, and
  architectural patterns that extend or override general Laravel best practices.
  Do not wait for an explicit request. If Laravel backend code is being created,
  reviewed, or modified in a Naykel application, this skill applies.
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
