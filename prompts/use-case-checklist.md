Inspect the codebase and produce a **human-tester checklist** of all testable
use cases in the application. The goal is to give a clear picture of what the
app does and provide a practical reference for manual test execution.

This is about app behaviour, not testing tools or frameworks.

**Output format**

Group items by feature area (for example: Auth, Lessons, Settings). Within each
group, use markdown checkboxes in this format:

```md
## Feature Area
- [ ] Actor can action — expected outcome  _(unverifiable)_
```

Each item must name the actor (Admin, Student, Guest, etc.) so role and
permission coverage is explicit. Mark any item that cannot be confirmed from the
codebase alone with `_(unverifiable)_` — for example, email delivery or
third-party integrations.

**Coverage**

* Cover all actors and their permissions for each feature
* Include meaningful edge cases: empty states, validation errors, and
  access-denied scenarios
* One line per item: enough detail to act on, no prose
* No duplicate items across groups
* Describe observable behaviour only, not implementation details

Save the output to `use-case-checklist-{agent}-{timestamp}.md` in the project root.
