---
name: daily-briefing
description: >
  Generate the user's morning briefing: today's calendar, tasks from their PM tool(s) filtered by active working statuses, active project status, and a snapshot of quarterly MITs. Use this skill whenever the user says "good morning", "what's today", "daily briefing", "morning briefing", "what's on deck", "what am I working on today", "give me the rundown", or any similar morning kickoff phrase. Also trigger on bare "briefing" or "morning" when context is clearly start-of-day. The skill produces a single scannable summary so the user can start the day knowing what's due, who they're meeting with, where their top project stands, and whether their MITs are on track.
---

# Daily Briefing

Your job is to give the user a single, scannable view of the day in under 30 seconds of reading. Four sections, in this order, every time. No filler.

## Configuration

Wire these to the user's actual stack on first run. Defaults assume the personal-assistant vault layout (`brain/`, `workshop/`).

| Source | What to wire | Notes |
|---|---|---|
| Calendar | The user's calendar MCP (Google Calendar, Outlook, etc.) | Pull today's events |
| Task system(s) | The user's PM tool(s) — Monday, Asana, Linear, Jira, Notion, Trello | Configure user ID, primary boards/projects, and which statuses count as "active" |
| Project docs | `workshop/[slug].md` for each active project | One file per project |
| MIT file | `brain/mit.md` | Quarterly MITs |

If the user runs the briefing before configuration is set, ask them once which tools they use and remember the answer in `brain/work.md` for future runs.

## Hard rules

- Bullet points only. No paragraphs.
- All times in the user's local timezone.
- If a section has nothing to surface, say "Nothing" on one line. Do not skip the section header.
- Follow `.claude/rules/communication-style.md` if present. No em dashes, no vague abstractions, no inflated verbs.
- Output is internal (for the user). Casual and direct. No greetings, no sign-offs.

## Section 1 — Calendar

Pull today's events from the user's calendar.

Format each event as a single line:
- `9:00a — Event title (attendees if 1:1 or small group)`

Sort chronologically. All-day events go at the top with `[all day]` prefix. Skip past events if the briefing is being run mid-day.

## Section 2 — Tasks

Pull from the user's configured task system(s). Combine into one list, grouped by source if more than one.

For each task system:
- Pull tasks assigned to the user (use the user ID configured above).
- Filter for the statuses the user marked as "active working states" (e.g., on a content workflow: "Needs Revision", "In Progress", "Ready for Review"; on an engineering workflow: "In Progress", "In Review", "Blocked"). If the user hasn't configured these, ask once and remember.
- For tools with multiple boards/projects, only pull from the boards/projects the user marked as primary.

Each task as a single line:
- `TASK: name — [source/board] [status or due date]`

Group by source (in the order the user listed them) if there are 3+ tasks total. Otherwise flat list. Within each group, sort by urgency: blocked / overdue first, then due today, then everything else.

## Section 3 — Active Projects

Read each `workshop/[slug].md` file (skip archived). For each active project, surface one line:
- `Project name — current phase / next step`

Pull current phase from the project doc. If the doc is missing a status section, say so explicitly: `Project name — no status logged`.

## Section 4 — MIT Snapshot

Read `brain/mit.md`. For each MIT, output one line:
- `MIT [number]: [short name] — [progress % if listed, or "no % logged"] — [risk flag if applicable]`

Risk flag triggers if:
- MIT has not been updated in 14+ days (check the "updated" timestamp in mit.md)
- MIT is past its window (e.g., MIT 1 is days 31-60 — flag if today is past day 60)

Use ⚠️ for risk only (this is the one place an emoji is allowed because it's a visual flag, not decoration).

## Output structure

```
# Briefing — [Date]

## Calendar
- [events]

## Tasks
- [tasks]

## Active Projects
- [projects]

## MITs
- [MIT lines]
```

## Length target

The whole briefing should fit in one screen. If Calendar has more than 6 events, group lunch/coffee/personal into a single "+N personal" line. If the combined task list has more than 8 entries, show top 8 by priority (blocked / overdue > due today > upcoming) and append `+N more`.

## When to skip the skill

If the user asks for just one of these (e.g., "what's on my calendar today" or "what tasks do I have"), don't run the full briefing. Pull the one section they asked for.
