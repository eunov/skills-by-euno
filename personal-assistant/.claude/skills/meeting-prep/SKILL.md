---
name: meeting-prep
description: >
  Prepare the user for an upcoming meeting by pulling the calendar event, prior meeting notes with the same attendees from their notes tool, and recent Slack/messaging threads with those people. Use this skill whenever the user says "prep me for [meeting/person]", "what's my next meeting", "prep for the [name] call", "meeting prep", "what do I need to know before my [time] meeting", "brief me on the [name] sync", or asks about an upcoming event on their calendar. Also trigger when they name a person and say "I have a call with them" or similar. The skill produces a single-page brief: who, what, prior context, open threads, and a recommended opening move.
---

# Meeting Prep

Your job is to walk the user into a meeting knowing exactly who they're talking to, what the meeting is for, what was said last time, and what's still unresolved.

## Configuration

Wire these to the user's actual stack on first run.

| Source | What to wire |
|---|---|
| Calendar | Their calendar MCP (Google Calendar, Outlook, etc.) |
| Meeting notes | Their notes tool (Granola, Otter, Fathom, Fireflies, etc.) |
| Messaging | Their team messaging tool (Slack, Teams, Discord, etc.) |

If a tool isn't wired up, skip that section gracefully.

## Hard rules

- Bullet points. No paragraphs longer than 2 lines.
- Follow `.claude/rules/communication-style.md` if present. No em dashes, no vague abstractions.
- Internal tone (this is for the user). Direct.
- If a data source returns nothing, say "Nothing found" on one line. Don't pad.

## Step 1 — Identify the meeting

If the user names the meeting ("prep me for the [name] sync"), search calendar by title or attendee.
If they ask for "my next meeting", pull the next upcoming event from their calendar.
If multiple candidates exist, list them and ask which one.

Once identified, extract:
- Title
- Time (user's local timezone)
- Attendees (names + emails)
- Description / agenda from the event body
- Any attached or linked docs

## Step 2 — Prior meeting history

For each attendee other than the user, search the user's meeting notes tool for prior meetings with that person. Surface the **last 1-2 meetings** with each.

For each prior meeting, output:
- Date + title
- 2-3 bullet summary of what was discussed
- Any unresolved action items or open questions

If no prior notes exist for an attendee, say `[Name]: no prior notes`.

## Step 3 — Messaging signal

For each attendee other than the user, scan recent DMs and shared channels in the user's messaging tool.

Surface:
- Any message from that person in the last 7 days
- Any open thread where the user owes a reply
- Any decision or question that landed without a clear close

Format:
- `[Name] — [channel/DM] — [1-line summary of what's unresolved]`

If nothing unresolved, skip the person.

## Step 4 — Recommended opening

Based on what surfaced, suggest:
- The first topic to raise (an open thread, an unresolved question, or a follow-up from last meeting)
- Any decision the user should be ready to make in the meeting

Keep this to 2-3 bullets. This is the most valuable section. If there's nothing to recommend, say so plainly.

## Output structure

```
# Meeting Prep — [Meeting title] @ [time]

## The meeting
- Time: [time, local timezone]
- Attendees: [names]
- Agenda: [from event description, or "none in invite"]
- Linked docs: [titles + links, or "none"]

## Prior history
[Per-attendee meeting note summaries]

## Open threads
[Per-attendee messaging signal, only what's unresolved]

## Walk-in
- [Recommended opening / first topic]
- [Decisions to be ready for]
```

## Length target

A meeting prep should be readable in under 60 seconds. If a section runs long, compress to the top 2-3 items by recency or relevance.

## Edge cases

- **Recurring meeting (e.g., weekly 1:1 or standing sync):** Lean heavily on last meeting's notes. Action items from the prior session are the most valuable signal.
- **First-time meeting with someone:** Skip prior history section, lean on messaging signal and any external context if available. Suggest invoking the `client-researcher` skill if the person is external.
- **No description, no docs, no prior notes:** Say it plainly. "Walking in cold — recommend asking the host what the outcome should be." Don't invent context.
