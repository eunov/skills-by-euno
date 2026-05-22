# Personal Assistant

A role-agnostic vault template for a "second brain" you can run in Claude Code or Claude Desktop's Cowork side panel. Unzip this folder, open it, and personalize it to your role.

## What this is

A working folder structure plus a curated set of skills that turn Claude (Code or Cowork) into a persistent executive assistant. The assistant remembers your context, tracks your goals, debriefs your projects, and ships ready-to-paste drafts.

## Two ways to use it

**Hand-edit (simplest)**
1. Open this folder in Claude Code or Cowork.
2. Edit `brain/me.md`, `brain/work.md`, `brain/team.md`, `brain/goals.md`, `brain/mit.md` with your own info. The `[Fill this in...]` placeholders show you where.
3. Update the **Top priority** line at the top of `CLAUDE.md`.
4. Start using it.

**Bootstrap via prompt (guided)**
1. Open `PROMPT.md` in this folder.
2. Copy everything below the first divider.
3. Paste into a fresh Claude Code or Cowork chat (with this folder open in the workspace).
4. Claude asks 12 questions and personalizes the vault for you in place.

## Top-level folder cheat sheet

| Folder | What lives there |
|---|---|
| `brain/` | Who you are: name, role, team, goals, MITs |
| `library/` | SOPs, style guides, EOD logs, meeting notes, post-mortems |
| `workshop/` | Active project workstreams (one doc per project) |
| `ledger/` | Append-only decision log |
| `recipes/` | Reusable formats and templates |
| `attic/` | Old material no longer active |
| `.claude/rules/` | Style rules the assistant always follows |
| `.claude/skills/` | Bundled skills (see below) |

## Bundled skills

| Skill | What it does |
|---|---|
| `slop-detector` | Catches AI-sounding writing before it ships |
| `client-researcher` | Deep research on a person before outreach or a meeting |
| `handoff` | Saves the current session as a paste-ready handoff |
| `grill-me` | Stress-tests a plan by interviewing you on every decision |
| `daily-briefing` | Morning rundown: calendar, tasks, priorities, MITs |
| `meeting-prep` | Pre-meeting brief from calendar, prior notes, and threads |
| `mit-checkin` | Quarterly review of your Most Important Targets |
| `post-mortem` | Structured debrief of any completed project |
| `evening-beer` | End-of-day wrap that updates your context files |

## Notes

- All skills are role-agnostic. The PM-tool-aware skills (`daily-briefing`, `evening-beer`) include a Configuration table at the top — wire in whichever task system, calendar, and messaging tool you use on first run.
- Skills are explicit-trigger only. They stay dormant until you say one of the phrases listed in each `SKILL.md` frontmatter.
- Works in both Claude Code and Claude Desktop's Cowork side panel. In Claude Code, skills auto-trigger on phrases. In Cowork, `CLAUDE.md` tells the assistant to read the matching `SKILL.md` when you say a trigger phrase, so behavior is the same.
