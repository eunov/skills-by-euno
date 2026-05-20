---
name: evening-beer
description: >
  Run an end-of-day wrap on your work. Aggregate everything touched today (12:01am → now) from Claude Code memory, your decisions log, git history, your project-management tool, and file diffs. Propose a single batched update across a dated EOD log, your priorities file, your goals file, your decisions log, active project docs, and Claude Code auto-memory. One approval, then write everything. Use this skill when the user says any of: "end of day", "EOD", "wrap up the day", "log today", "log the day", "daily wrap", "close out the day", "let's crack open that beer", "let's grab an evening beer", "let's grab a beer to end the day", or similar beer-themed end-of-day phrasing. Explicit-trigger only.
---

# Evening Beer

A daily wrap-up skill. Aggregates the day, proposes updates to your live state files, gets one approval, then writes everything in one pass. Opens with a fresh beer one-liner each run.

## Configuration

Customize these paths to match your project. Defaults assume a flat layout in your project root. If your repo uses different paths, swap them in everywhere they appear below. If none of these files exist yet, see **First-run setup** below — the skill will bootstrap them for you.

| Purpose | Default path | What goes here |
|---|---|---|
| EOD log folder | `eod-logs/YYYY-MM-DD.md` | The dated wrap file the skill creates |
| Priorities file | `priorities.md` | Your current list of active priorities |
| Goals file | `goals.md` | Quarterly or longer-term targets |
| Decisions log | `decisions/log.md` | Append-only log of meaningful decisions |
| Project docs | `projects/[slug].md` | One file per active project |

### First-run setup

If NONE of `priorities.md`, `goals.md`, `decisions/log.md`, or any `projects/*.md` exist in the project root when the skill runs, do a short bootstrap before Step 0. If any one of them exists, skip the bootstrap and proceed normally.

**Opener (one line):**
"Looks like this is your first evening beer. Quick setup before we crack one open. (yes / skip)"

**If yes, ask these questions in sequence, one at a time. Short answers expected:**

1. "Top 3–5 priorities right now? (one per line)" → write `priorities.md` with a `# Priorities` header and the lines beneath as bullets.
2. "Goals this quarter or year? (one per line, include a target/metric if you've got one)" → write `goals.md` with a `# Goals` header and the lines beneath as bullets.
3. "Active projects? (slug + one-line description per project, one per line)" → create one `projects/[slug].md` stub per entry, each with a `# [Slug]` header and the one-line description underneath.

**Always create regardless of answers** (so the bootstrap does not re-trigger next run):

- `decisions/log.md` with a `# Decisions` header.
- `eod-logs/` folder (empty).

**Skip path:** if the user says "skip" at the opener, still create empty stub files (`priorities.md`, `goals.md`, `decisions/log.md`) with just headers, plus the `eod-logs/` folder. Do not create any `projects/` files — by skipping, the user has not declared any projects.

**After writing:** output the created paths as a short bullet list, then proceed straight into Step 0 with the normal opener.

**Style for the bootstrap:**

- Questions one line each. No preamble paragraphs.
- Same style rules as the rest of the skill: no em dashes, no inflated verbs, no motivational language.
- Capture the user's answers literally into the seed files. Do not rewrite or expand into prose.

## Style

- Bullet points. No paragraphs longer than 2 lines.
- No em dashes. No inflated verbs (leverage, unlock, optimize, activate, etc.). No motivational language. No AI authority openers ("the truth is", "here's the thing").
- Internal tone. Direct. Capture what happened plainly. Do not pad.
- Dates in YYYY-MM-DD. Times in the user's local timezone.

## Hard rules

- Capture the user's day plainly from the sources. Do not invent activity. If a source is empty, say "nothing logged" — do not pad.
- Single approval pattern: show the full proposal, get one yes, then write. Do not write any file before approval.

## Step 0 — Crack one open

Before generating the opener, check the configured paths. If none of `priorities.md`, `goals.md`, `decisions/log.md`, or any `projects/*.md` exist, run **First-run setup** (see Configuration) first, then come back here.

Generate a fresh one-line opener every run. Do not reuse the same line twice. The four examples below are style references, not a pick list — write a new one in their spirit.

**Constraints:**
- One line. Two short sentences max. Then go straight to Step 1.
- Pick one beer brand to name: Modelo, Corona, Coors Light, or Michelob Ultra. Rotate which brand you pick across runs.
- Dry, self-aware, slightly self-deprecating humor. Not corporate cheer. Not motivational.
- Follows the Style rules above. No em dashes. No inflated verbs.
- The opener riffs on the beer (taste, branding, marketing, price, the person drinking it, the situation around cracking it open). Then a beat that transitions into the EOD wrap.

**Style references (do not reuse verbatim):**
- "Cracking open a Modelo. Let's see what we got done today."
- "Corona's open, lime is missing, life goes on. Walking through the day."
- "Coors Light's cold and cracked. Closing out the day."
- "Michelob Ultra in hand because someone around here cares about macros. EOD time."

**Other angles to riff on (for variety):**
- The price (Modelo's expensive now, Coors Light is the budget pick)
- The marketing (Michelob's "superior light beer," Corona's beach commercials)
- The taste (Modelo's golden, Corona's basically water with citrus)
- The situation (cracking it on a Tuesday, drinking warm beer, last one in the fridge)
- Self-aware AI humor (the AI doesn't actually drink, the metaphorical beer, etc.)

After the opener line, proceed straight to Step 1. No second paragraph. No "ready?" question. No emoji.

## Step 1 — Aggregate the day

Pull from these sources, all filtered to today's date (00:00 local → now):

- **Claude Code memory** — observations dated today. Use the available memory search tool to pull today's entries.
- **Decisions log** — entries in the user's decisions log dated today.
- **Git** — `git log --since="00:00" --pretty=format:"%h %s"` in the repo root.
- **The user's project-management tool** — Monday, Linear, Jira, Asana, Notion, Trello, or whatever they use. Pull status changes today on tasks assigned to them. Wire the integration in here.
- **File diffs** — `git diff --name-only HEAD@{00:00}` to catch uncommitted changes in the project, priorities, and reference folders.
- **Calendar (past events only)** — today's events that already happened, for meeting context.

If a source returns nothing, mark it as "nothing logged" in your working notes. Do not fail the skill if one source is empty.

## Step 2 — Build the working draft

Synthesize the aggregated data into a working draft in chat (do not write to disk yet). Group by:

- **Worked on** — projects or workstreams that moved today
- **Decisions made today** — net-new decisions, not already in the decisions log
- **Status moves** — PM tool or project doc status transitions
- **Goal touches** — which goals were moved by today's work, and by how much (best estimate, the user will confirm)
- **Wins** — things that landed (shipped, approved, completed)
- **Open threads** — what's hot for tomorrow, blocked items, follow-ups

## Step 3 — Propose the full update

Show one consolidated proposal in this format:

```
## EOD proposal — [YYYY-MM-DD]

### New file: eod-logs/YYYY-MM-DD.md
[show full content of the proposed EOD log, using the template in Step 5]

### Updates to priorities.md
[show diff: what gets added, removed, reordered, moved to Recently Finished]

### Updates to goals.md
[show diff per goal that moved: progress %, last updated date, note]
[if no goals moved, write "No goal changes."]

### Appends to decisions/log.md
[show each new decision line in the format: [YYYY-MM-DD] DECISION: ... | REASONING: ... | CONTEXT: ...]
[if nothing to add, write "No new decisions."]

### Updates to projects/[slug].md
[for each active project that moved today, show the proposed edit]
[if no project doc changes, write "No project doc changes."]

### Auto-memory writes
[for each persistent memory worth saving: name, type, one-line content]
[if nothing memory-worthy, write "No memory updates."]

---

Approve all? (yes / edit / skip [item])
```

If the user says **yes**, write everything in Step 4.
If they say **edit [item]**, walk through that item, apply the change, re-show the proposal.
If they say **skip [item]**, drop that item from the write set.
If they say **no**, stop and ask what to redo.

## Step 4 — Write everything

In a single pass, in this order:

1. Create `eod-logs/YYYY-MM-DD.md` (create the folder if it doesn't exist)
2. Update `priorities.md`
3. Update `goals.md`
4. Append to `decisions/log.md`
5. Update any `projects/[slug].md` files
6. Write auto-memory entries (one file per memory, plus an index entry in the project's `MEMORY.md`)

After writing, output a clean list of paths touched. No re-summary of content.

## Step 5 — EOD log template

The dated file at `eod-logs/YYYY-MM-DD.md`:

```markdown
# EOD Log — YYYY-MM-DD

## Snapshot
[2 sentences: how the day went, what shipped, what's hot for tomorrow]

## Worked on
- [project or workstream] — [what happened in one line]
- ...

## Decisions made today
- [decision summary] → see decisions/log.md
- [decision summary]

## Status moves
- [item] moved X → Y ([source])

## Goal progress
- [goal name]: [movement, or "no change"]

## Wins
- [things that landed]

## Open threads / tomorrow
- [what's hot for tomorrow]
- [blocked items]

## Files updated by this EOD
- [list of paths]
```

Omit any section that's empty. Do not write "Nothing" placeholders in the final EOD log.

## Edge cases

**No activity today.** Say: "Nothing logged from any source today. Want to describe the day manually, or skip the EOD?" If the user describes manually, treat the input as the source and proceed to Step 3.

**EOD already ran today.** If `eod-logs/YYYY-MM-DD.md` already exists, ask: "EOD log exists for today. Append (merge new activity), replace (rewrite from scratch), or skip?"

**Mid-day trigger.** If the user runs it before end of business, note "Partial day — ran at [time]" in the snapshot and proceed normally. Don't refuse.

**Project wrapped today.** If a project hit a "complete" status today, flag it in the EOD log under Open threads so the user can decide what to do next (retrospective, archive, announce, etc.).

**Memory writes — handle with care.** Auto-memory writes persist across all future Claude Code sessions. For each proposed memory entry, show the name, type, and full content in the proposal so the user can spot anything that shouldn't persist. They can `skip auto-memory` to bypass that section entirely.

## What this skill is not

- Not a planner. Don't propose new projects, new goals, or scope changes. Capture what happened, update live state.
- Not a writer. Don't expand the day's activity into prose. Bullets, plain language, what actually happened.
- Not self-modifying. Don't edit this skill or any other skills based on the day's activity. Note candidates for a future session.
