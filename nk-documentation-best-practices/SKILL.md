---
name: nk-documentation-best-practices
description: >-
  Use this skill whenever creating or updating documentation across any Naykel
  project. Do not wait for an explicit request — if documentation is being
  created or updated, this skill applies.
---

# NK Documentation Best Practices

Shared documentation rules across all Naykel projects. Project-specific skills
extend this one — apply both, with the project skill taking precedence where
they conflict.

## Review Workflow

- Mark every new heading with `(review)` until its content is confirmed.
- When you update a section carrying `(review)`, ask: "[Section name] has been
  updated. Is this section complete?"
- On confirmation: remove the `(review)` tag, update the project status file,
  and remove `(wip)` from the nav entry if the doc is now fully complete.
- Leave `(review)` on any headings that are still unresolved.
- When all `(review)` tags are gone from a doc, it is considered complete.
- When closing documentation work, call out which headings or docs still carry
  `(review)` tags.

## Common Rules

- **One doc, one job.** If a page explains the mental model AND lists all
  properties AND shows a quickstart, split it.
- **Code-first.** Add prose only when behaviour is non-obvious or context is
  needed.

## Document Structure

- Every doc starts with a title and a short lead.
- The lead should say what the page covers and what the reader gets from it.
- Keep it concrete and free of filler.
- Do not add a separate `## Introduction` section.

## Nav Management

- Only add docs to nav after the doc or section has been reviewed and agreed.
- When starting work on a doc not yet in the nav, add it in its real category
  with `(wip)` suffixed to the name so it is accessible during development.
- Keep nav labels and sections minimal. Add structure back only as docs are
  reviewed and agreed.

## Quick Reference

Read the relevant rule file before responding — do not rely on summaries alone.

### Component Docs → `rules/component-docs.md`

- Single-component pages
- Multi-component pages
- Variants and component families
