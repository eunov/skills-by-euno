# skills-by-euno

Claude Code skills I use to run my day. Drop them in your skills directory and they activate on the trigger phrases in each `SKILL.md`.

## What's a skill?

A markdown file with frontmatter that tells Claude Code how to handle a specific recurring task. Triggered by natural language. Stays dormant until you ask.

## Skills

| Skill | Triggers on | What it does |
|---|---|---|
| [`evening-beer`](evening-beer/SKILL.md) | "EOD", "wrap up the day", "let's crack a beer", etc. | Aggregates the day from memory, git, decisions log, and PM tool. Proposes one batched update across EOD log, priorities, goals, decisions, projects, and auto-memory. One approval, then writes everything. |

## Install

```
git clone https://github.com/eunov/skills-by-euno.git ~/.claude/skills/skills-by-euno
```

Or copy individual skill folders into `~/.claude/skills/`. Restart Claude Code and the trigger phrases will work.

## Conventions

- One folder per skill. `SKILL.md` is required, anything else is optional.
- Frontmatter needs `name` and `description`. The description must list trigger phrases so Claude knows when to invoke the skill.
- Explicit triggers only. No skill should run unless the user asks for it by name or trigger phrase.
- Show the full proposal, get one approval, then write. No partial writes.

## Contributing

Open a PR with a new skill folder. Keep the `SKILL.md` direct, no inflated verbs, no motivational filler. Trigger phrases go in the frontmatter description.
