# skills-by-euno

A collection of Claude Code skills built to boost productivity in a role-agnostic environment. Whether you're an engineer, PM, operator, founder, or analyst — these skills are designed to plug into your workflow without assuming what you do for a living.

## What's a skill?

A skill is a self-contained markdown file (`SKILL.md`) that teaches Claude Code how to handle a specific recurring task. Skills activate on natural-language triggers, run a defined procedure, and stay out of your way the rest of the time.

## Available skills

| Skill | What it does |
|---|---|
| [`evening-beer`](evening-beer/SKILL.md) | End-of-day wrap. Aggregates the day from memory, git, decisions log, and your PM tool. One approval, then writes everything in a single pass. |

## Install

Drop any skill folder into your Claude Code skills directory:

```
~/.claude/skills/
```

Or clone the whole repo into your skills directory:

```
git clone https://github.com/eunov/skills-by-euno.git ~/.claude/skills/skills-by-euno
```

Restart Claude Code and the skills will be discoverable by their trigger phrases.

## Philosophy

- **Role-agnostic.** No assumptions about job title, stack, or tooling. Configure paths and integrations per project.
- **Explicit triggers.** Skills only activate when you ask. No surprise behavior.
- **One approval, then write.** Skills propose changes in full before touching files. You stay in control.
- **Plain language.** No motivational filler. No inflated verbs. Capture what happened, do the work.

## Contributing

Open a PR with a new skill folder. Each skill needs a `SKILL.md` with frontmatter (`name`, `description`) and clear trigger phrases in the description so Claude knows when to invoke it.
