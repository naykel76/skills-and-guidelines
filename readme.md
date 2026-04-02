<!-- markdownlint-disable MD033 MD041 MD045-->
<p align="center"><a href="https://naykel.com.au" target="_blank"><img src="https://avatars0.githubusercontent.com/u/32632005?s=460&u=d1df6f6e0bf29668f8a4845271e9be8c9b96ed83&v=4" width="120"></a></p>

# Naykel Skills and Guidelines

A collection of agent skills and guidelines that extend capabilities across
planning, development, and tooling.

## Skills

| Skill                 | Description                                                                      |
| --------------------- | -------------------------------------------------------------------------------- |
| `grill-me`            | Stress-test a plan or design through structured interviewing                     |
| `markdown-formatting` | Apply markdown conventions including Torchlight code tags and markdownlint rules |
| `skill-creation`      | Rules and structure for creating and updating skill files                        |

## Usage

Skills are installed using [npx skills](https://skills.sh) — an open standard
package manager for agent skills compatible with Claude Code, Cursor, Codex, and
others.

Skills work in two layers — shared skills installed from this repo, and
project-specific skills that live in `.ai/skills/`. The sync script merges both
into the agent directories Claude Code reads.

### 1. Install shared skills

Run this in the project root. Use `--copy` to install real files instead of
symlinks, `--yes` to skip the interactive prompt.

```bash +code
npx skills@latest add naykel76/skills-and-guidelines --copy --yes
```

Skills are copied to all detected agent directories (`.claude/skills/`,
`.agents/skills/`, etc.). Installed skills and their sources are recorded in
`skills-lock.json` — commit this file to keep installs reproducible across
machines.

### 2. Add project-specific skills

Add any project-specific skills to `.ai/skills/`. These are merged on top of the
shared skills — if a skill name matches, the project version wins.

```bash +code
.ai/
└── skills/
    └── my-skill/
        └── SKILL.md
```

### 3. Add guidelines

Copy any needed files from `guidelines/` in this repo into your project's
`.ai/guidelines/` directory. Guidelines are not installed automatically.

```bash +code
.ai/
└── guidelines/
    └── working-with-nathan.md
```

### 4. Run the sync

Merges `.ai/skills/` over the installed skills and compiles `.ai/guidelines/`
into `CLAUDE.md`.

```bash +code
node scripts/ai-sync.js
```

## Updating

Reinstall to pull the latest changes, then re-run the sync to reapply project
overrides:

```bash +code
npx skills@latest add naykel76/skills-and-guidelines --copy --yes
node scripts/ai-sync.js
```

## Installing a single skill

```bash +code
npx skills@latest add naykel76/skills-and-guidelines/grill-me --copy --yes
```
