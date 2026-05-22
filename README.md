# skills-by-euno

A collection of Claude Code skills and a personal assistant vault template. Drop the skills into your skills directory and they activate on the trigger phrases in each `SKILL.md`, or use the `personal-assistant/` template as a starting point for your own Claude Code second brain.

## What's a skill?

A markdown file with frontmatter that tells Claude Code how to handle a specific recurring task. Triggered by natural language. Stays dormant until you ask.

## Skills

| Skill | Triggers on | What it does |
|---|---|---|
| [`evening-beer`](evening-beer/SKILL.md) | "EOD", "wrap up the day", "let's crack a beer", etc. | Aggregates the day from memory, git, decisions log, and PM tool. Proposes one batched update across EOD log, priorities, goals, decisions, projects, and auto-memory. One approval, then writes everything. |

## Personal assistant vault

[`personal-assistant/`](personal-assistant/) is a drop-in second-brain template for Claude Code (also works inside Cowork). It's the same vault layout used to run a real workday: context files for who you are and what you're working on, a workshop for active projects, a library for SOPs and meeting notes, and a ledger for decisions.

Bootstrap it by pointing Claude Code at the folder and pasting [`PROMPT.md`](personal-assistant/PROMPT.md). The bootstrap walks you through a short interview, fills in your context files, and primes the bundled skills so they work for your stack on day one.

**Bundled skills** (all explicit-trigger):

| Skill | What it does |
|---|---|
| `daily-briefing` | Morning rundown: today's calendar, active tasks from your PM tool, project status, MIT snapshot. |
| `meeting-prep` | Pre-meeting brief: event details, prior meeting notes with the same attendees, unresolved threads, recommended opening. |
| `mit-checkin` | Walks your quarterly MITs one at a time, flags stale or at-risk items, confirms updates before writing. |
| `post-mortem` | Grill-style debrief after a project wraps. One question at a time across the full lifecycle. Writes a dated doc and surfaces process changes. |
| `evening-beer` | End-of-day wrap. Same skill as the standalone above, wired into the vault layout. |
| `client-researcher` | Deep internet research on a person before a meeting or outreach. Produces a structured brief with hooks. |
| `slop-detector` | Rewrites outbound copy to sound human. Strips inflated verbs, AI openers, vague abstractions. |
| `handoff` | Compacts the current session into a handoff doc so a fresh agent can pick up the work. |
| `grill-me` | Socratic questioning skill for thinking through ideas one question at a time. |

The vault ships as a template — every `brain/` file is a placeholder ready for the bootstrap prompt to fill in. No personal data baked in.

## Install

```
git clone https://github.com/eunov/skills-by-euno.git ~/.claude/skills/skills-by-euno
```

Or copy individual skill folders into `~/.claude/skills/`. For the personal-assistant vault, copy the whole [`personal-assistant/`](personal-assistant/) folder anywhere on your machine and point Claude Code at it. Restart Claude Code and the trigger phrases will work.

## Conventions

- One folder per skill. `SKILL.md` is required, anything else is optional.
- Frontmatter needs `name` and `description`. The description must list trigger phrases so Claude knows when to invoke the skill.
- Explicit triggers only. No skill should run unless the user asks for it by name or trigger phrase.
- Show the full proposal, get one approval, then write. No partial writes.

## Contributing

Open a PR with a new skill folder. Keep the `SKILL.md` direct, no inflated verbs, no motivational filler. Trigger phrases go in the frontmatter description.
