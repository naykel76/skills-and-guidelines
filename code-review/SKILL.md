---
name: code-review
description: >-
  Use this skill when the user requests a dedicated code review, package review,
  or architectural audit. Do not use it for routine checks, walkthroughs, or
  verification performed while developing a component or updating documentation.
---

## Intent

- Treat incidental checks during ongoing work as part of that work, even when
  the user uses the word "review".
- Perform analysis only unless the user explicitly asks for edits.
- Prioritize actionable, evidence-based findings over general advice.

## Communication Style

- Use plain language. Avoid jargon, hedging, consultancy tone, and explanatory
  filler.
- Do not pad findings with soft language such as "makes it hard to reason
  about", "the only real decision is", or similar framing.
- Do not offer alternate report versions unless the user explicitly asks.

## Scope

- Infer the target path, package, component, or changed files from the request.
  Ask only if the scope is genuinely unclear.
- Identify the review mode:
    - `Package/component review` for a bounded subsystem
    - `Application audit` for broad maintainability or architecture review
- For a package review, treat the package as a bounded unit and aim for deeper,
  more confident coverage within that boundary.
- For an application review or a large repo, prioritise entry points,
  boundaries, changed files, and the highest-risk areas. Do not imply full
  file-by-file coverage unless you actually performed it, and state coverage
  limits only when they materially affect confidence in the findings.
- Report only exclusions and verification limits that materially affect trust
  in the conclusions. Keep them to two lines where possible; omit the section
  when there are none. Do not repeat the request, inventory examined files,
  list versions, or describe the investigation process.
- Respect user-provided exclusions exactly, and note them before reviewing.
- If exclusions conflict with a requested output section, remove the section and
  note why.

## Explore Before Judging

- Read directory structure first, then key entry points and supporting files.
- Avoid conclusions from a single file when behaviour spans multiple files.
- Check surrounding usage before calling something broken, dead, duplicated,
  or inconsistent.

## False-Positive Guard Rails

- Read `CLAUDE.md`, `AGENTS.md`, or a similar conventions file at the project
  root before reviewing.
- Follow local project guidance before general framework preferences. Treat
  documented local patterns, overrides, and conventions that differ from
  framework or package defaults as intentional unless there is direct evidence
  they are causing risk.
- If something looks suspicious but may be intentional, present it as a question
  or assumption, not a defect.
- Do not turn preference differences into findings.
- Do not flag missing abstraction, config-driven behavior, or refactors unless
  there is a concrete maintenance, correctness, or reuse problem.
- Do not flag missing null guards or defensive checks on internal methods when
  the invalid input is already blocked at the calling boundary. Trace the call
  graph before raising the finding. Guards belong at the boundary (Livewire
  actions, controllers, public API entry points); internal methods should trust
  their callers.

## Core Dimensions

1. Architecture
2. Readability and clarity
3. Maintainability
4. Correctness and edge cases
5. Performance
6. Security

- Treat performance as a review dimension only when it is relevant to the
  request or there is concrete evidence of waste, scale risk, or unnecessary
  work.
- Treat security as a review dimension only when it is relevant to the request
  or there is concrete evidence of exposure, trust-boundary failure, or unsafe
  data handling.

## Evidence Standard

- Prefer exact file and line references when available.
- If line references are not practical, use file plus class, method, or view
  fragment.
- Describe the observed behaviour or failure mode, not just the code smell.
- Mention missing or weak tests when they materially increase risk.
- For cross-file issues, cite the smallest set of files needed to prove the
  point.

## Finding Discipline

- All findings must use the finding format below. Do not switch between formats
  for different dimensions or severity levels.
- Do not report low-value or repetitive findings that do not materially change
  risk, behaviour, or maintainability.
- Distinguish broken behaviour from unfinished build work and unconfirmed
  requirements. A defect violates an established contract or requirement;
  an incomplete example or planned feature alone is not a defect. Put build
  work in actions and unresolved intent in questions or deferred decisions.
- Only report issues you can defend with concrete code evidence or a clear
  behavioural risk.
- Prefer root-cause fixes over style-only commentary.
- Weaknesses first. Strengths are optional and brief.
- Keep `Impact` to one sentence maximum.
- Keep `Fix` to one sentence maximum unless the issue is `Critical` and a longer
  recommendation is necessary to avoid ambiguity.
- Only include a `Trade-off` when there is a real downside, decision, or
  implementation risk that materially affects the recommendation.
- Recommendations should say what to change. Do not drift into design coaching
  unless the implementation choice is the actual issue.
- Order findings by severity, then by confidence.

## Severity and Finding Format

Severity must be exactly one of: `Critical`, `High`, `Medium`, `Low`.

State the triggering conditions and whether the affected path is active. Rate
impact under those conditions; do not present hypothetical activation as a
current failure.

- `Critical`: active or near-certain impact to data integrity, auth boundaries,
  or production availability.
- `High`: credible exploit or major correctness risk with meaningful
  business/user impact.
- `Medium`: real defect with bounded impact or required preconditions.
- `Low`: minor risk, hard-to-trigger edge case, or low-impact maintainability
  concern.

Use this format for every finding:

```
### {Severity} — {One-sentence description}
- **Impact:** ...
- **Evidence:** ...
- **Fix:** ...
- **Trade-off:** ...  ← only when a real downside or decision matters
```

## Package Review Procedure

Use this procedure when the review identifies a package status file. It is a
shared working convention across packages, not a requirement to complete every
package capability before use.

- Establish the required behaviour and relevant reference example.
- Check the maintained source, application overrides, directly related pieces,
  documentation, and existing status entries.
- Separate issues blocking the current use from review work that can wait.
- Use the package's coverage table and status key. `✅` means confirmed against
  source and documented; `review` means documented but not yet confirmed;
  `n/a` means not needed; blank means review coverage has not started, not that
  an implementation is absent.
- Keep coverage entries as a lasting record of verified work and documentation.
  Record findings and proposed fixes in chat or an authorised report, not the status file.
- Update coverage only when supported by verification and documentation.
  Working in an application does not establish package review completion.
- Do not remove findings without approval; raise entries that appear resolved.
- Discuss proposed implementation changes before making them unless already
  authorised. When changes are authorised, verify them, update the relevant
  documentation, and then update coverage.

## Output

Report findings in chat by default. Save only when the user requests or confirms
it. If a report would be useful, ask whether to save it after presenting findings.

Save authorised reports in the application root. When continuing the same
review, prefer updating its report over creating another for each step.

Filename: `{agent}-{target}-YYMMDD.md`. Use a short, descriptive kebab-case
review target, for example `codex-navbar-261028.md`. If that name exists,
append `-2`, `-3`, etc.

When a report is saved, return a concise summary in chat with its path.

Do not use inline review directives (e.g. `::code-comment`) unless the user
explicitly requests them.

### Package/Component Reports

Use these sections:

1. **Next Actions**. Give one ordered list of concrete actions in dependency
   order, naming the owning package or application. Distinguish repairs from
   planned build work and decisions needing confirmation. Reference findings
   for evidence; do not repeat it here. Base the sequence on the agreed workflow
   and requirements, not merely on which draft files exist.
2. **Findings**. Record confirmed defects using the finding format above.
3. **Deferred Work**. List specific defects, review targets, or decisions that
   can wait. Reference an existing finding rather than describing it again.
4. **Verification Limits**. State only unchecked behaviour or failed checks
   that materially affect the recommendation. Do not narrate the investigation.

If there are no defects, say so under Findings. Omit Deferred Work and
Verification Limits when there is nothing material to report. Do not add
separate recommendation, capability inventory, resolved-history, sprint-planning,
or generic risk-rating sections. Mention a verified capability only where it
supports an action or explains why a proposed change is unnecessary.

Before saving, check that every action follows the agreed requirements, every
finding establishes a defect under stated conditions, and each fact is stated
once. Keep information only when it changes an action, priority, or confidence
in the result. Updating an existing report does not require a new report.

### Broad Review Reports

Use this structure:

1. **Findings**.
2. **Top Architectural Risks**. Up to 5.
3. **Top Refactor Priorities**. Up to 5.
4. **Next Sprint Action Plan**. 3–7 items, highest value first. For each item:
    - **Action:** specific change to make
    - **Why now:** immediate benefit or risk reduction
    - **Effort:** `S`, `M`, or `L`
    - **First step:** exact starting task
5. **Maintainability Risk:** `Low`, `Moderate`, or `High` with reasoning.
6. **Cross-Boundary Coupling Risk:** `Low`, `Moderate`, or `High` with reasoning.
7. **Verification Limits**.

## No-Finding Reviews

- If no findings are discovered, say so explicitly.
- Still mention meaningful residual risks, unreviewed areas, or testing gaps
  in prose — do not use the finding format for these.
- Do not pad the response with generic best practices.
