---
name: code-review
description: >-
  Use this skill whenever the user asks for a code review, package review, or
  architectural audit. Delivers severity-ranked findings with evidence and
  fixes, not general advice. Always saves the review as a markdown file and
  returns a concise summary in chat.
---

## Intent

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
  Record findings and proposed fixes in the review report, not the status file.
- Update coverage only when supported by verification and documentation.
  Working in an application does not establish package review completion.
- Do not remove findings without approval; raise entries that appear resolved.
- Discuss proposed implementation changes before making them unless already
  authorised. When changes are authorised, verify them, update the relevant
  documentation, and then update coverage.

## Output

Save a dated review report in the application root using the format below.
When a package status file is specified, use it only for coverage tracking under
the Package Review Procedure above. A status file does not replace the report.

Filename: `code-review-{agent}-YYMMDD.md`. If that name exists, append `-2`,
`-3`, etc.

Return a concise summary in chat with the report path. Do not paste the full
report into chat unless the user explicitly asks.

Do not use inline review directives (e.g. `::code-comment`) unless the user
explicitly requests them.

### Package/Component Reports

Use this structure:

1. **Recommendation**. State the recommended approach and immediate next step
   in a short paragraph. Choose an approach supported by the review; identify
   any decision that genuinely needs the user's answer.
2. **What the Application Can Rely On**. State the verified capabilities and
   relevant conditions for using them.
3. **What Needs Fixing Now**. Prioritise current-use blockers in dependency
   order, independently of severity. Put prerequisites first and identify the
   dependent work, including dependencies across layers. Say what to change
   and where it belongs; reference findings instead of repeating their evidence.
   If nothing blocks use, say so.
4. **What Can Wait**. Identify deferred defects and review work that the current
   use does not need. Distinguish unverified capabilities from known defects.
5. **Findings**.
6. **Verification Limits**.

Keep the three decision sections even when an answer is "none". Do not add
sprint planning, effort estimates or generic risk ratings to component reports.

Optionally include **Checked and Cleared** after the findings when a plausible
concern was investigated and resolved. Record the conclusion and supporting
evidence briefly so the concern is not raised again; do not inventory routine
checks.

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
