---
name: evening-beer
description: >
  Run an end-of-day wrap on your work. Aggregate everything touched today (12:01am → now) from Claude Code memory, your decisions log, git history, your project-management tool, and file diffs. Propose a single batched update across a dated EOD log, your priorities file, your MIT file, your decisions log, active project docs, and Claude Code auto-memory. One approval, then write everything. Use this skill when the user says any of: "end of day", "EOD", "wrap up the day", "log today", "log the day", "daily wrap", "close out the day", "let's crack open that beer", "let's grab an evening beer", "let's grab a beer to end the day", or similar beer-themed end-of-day phrasing. Explicit-trigger only. Coexists with the handoff skill — handoff is mid-session context capture, EOD is the daily roll-up that updates live state files.
---

# Evening Beer

A daily wrap-up skill. Aggregates the day, proposes updates to your live state files, gets one approval, then writes everything in one pass. Opens with a fresh beer one-liner each run.

## Configuration

Defaults assume the personal-assistant vault layout (`brain/`, `library/`, `ledger/`, `workshop/`). If you renamed folders during setup or are running this skill in a different layout, swap the paths below to match your vault.

| Purpose | Default path | What goes here |
|---|---|---|
| EOD log folder | `library/eod-logs/YYYY-MM-DD.md` | The dated wrap file the skill creates |
| Priorities file | `brain/current-priorities.md` | Your current list of active priorities |
| MIT / goals file | `brain/mit.md` | Quarterly Most Important Targets |
| Decisions log | `ledger/log.md` | Append-only log of meaningful decisions |
| Project docs | `workshop/[slug].md` | One file per active project |

## Style

- Bullet points. No paragraphs longer than 2 lines.
- Follow `.claude/rules/communication-style.md` if present. No em dashes, no inflated verbs (leverage, unlock, optimize, activate, etc.), no motivational language, no AI authority openers ("the truth is", "here's the thing").
- Internal tone. Direct. Capture what happened plainly. Do not pad.
- Dates in YYYY-MM-DD. Times in the user's local timezone.

## Hard rules

- Capture the user's day plainly from the sources. Do not invent activity. If a source is empty, say "nothing logged" — do not pad.
- Single approval pattern: show the full proposal, get one yes, then write. Do not write any file before approval.

## Step 0 — Crack one open

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
- **File diffs** — `git diff --name-only HEAD@{00:00}` to catch uncommitted changes in the brain, library, and workshop folders.
- **Calendar (past events only)** — today's events that already happened, for meeting context.

If a source returns nothing, mark it as "nothing logged" in your working notes. Do not fail the skill if one source is empty.

## Step 2 — Build the working draft

Synthesize the aggregated data into a working draft in chat (do not write to disk yet). Group by:

- **Worked on** — projects or workstreams that moved today
- **Decisions made today** — net-new decisions, not already in the decisions log
- **Status moves** — PM tool or project doc status transitions
- **MIT touches** — which MITs were moved by today's work, and by how much (best estimate, the user will confirm)
- **Wins** — things that landed (shipped, approved, completed)
- **Open threads** — what's hot for tomorrow, blocked items, follow-ups

## Step 3 — Propose the full update

Show one consolidated proposal in this format:

```
## EOD proposal — [YYYY-MM-DD]

### New file: library/eod-logs/YYYY-MM-DD.md
[show full content of the proposed EOD log, using the template in Step 5]

### Updates to brain/current-priorities.md
[show diff: what gets added, removed, reordered, moved to Recently Finished]

### Updates to brain/mit.md
[show diff per MIT that moved: progress %, last updated date, note]
[if no MITs moved, write "No MIT changes."]

### Appends to ledger/log.md
[show each new decision line in the format: [YYYY-MM-DD] DECISION: ... | REASONING: ... | CONTEXT: ...]
[if nothing to add, write "No new decisions."]

### Updates to workshop/[slug].md
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

1. Create `library/eod-logs/YYYY-MM-DD.md` (create the folder if it doesn't exist)
2. Update `brain/current-priorities.md`
3. Update `brain/mit.md`
4. Append to `ledger/log.md`
5. Update any `workshop/[slug].md` files
6. Write auto-memory entries (one file per memory, plus an index entry in the project's `MEMORY.md`)

After writing, output a clean list of paths touched. No re-summary of content.

## Step 5 — EOD log template

The dated file at `library/eod-logs/YYYY-MM-DD.md`:

```markdown
# EOD Log — YYYY-MM-DD

## Snapshot
[2 sentences: how the day went, what shipped, what's hot for tomorrow]

## Worked on
- [project or workstream] — [what happened in one line]
- ...

## Decisions made today
- [decision summary] → see ledger/log.md
- [decision summary]

## Status moves
- [item] moved X → Y ([source])

## MIT progress
- MIT 1: [movement, or "no change"]
- MIT 2: [movement, or "no change"]
- MIT 3: [movement, or "no change"]

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

**EOD already ran today.** If `library/eod-logs/YYYY-MM-DD.md` already exists, ask: "EOD log exists for today. Append (merge new activity), replace (rewrite from scratch), or skip?"

**Mid-day trigger.** If the user runs it before end of business, note "Partial day — ran at [time]" in the snapshot and proceed normally. Don't refuse.

**Project wrapped today.** If a project hit a "complete" status today, flag at the end of Step 3: "Project [name] wrapped today — want to trigger the `post-mortem` skill next?" Don't auto-run post-mortem.

**Memory writes — handle with care.** Auto-memory writes persist across all future Claude Code sessions. For each proposed memory entry, show the name, type, and full content in the proposal so the user can spot anything that shouldn't persist. They can `skip auto-memory` to bypass that section entirely.

## What this skill is not

- Not a post-mortem. EOD is daily; post-mortem is per-project. They can chain but aren't the same job.
- Not a handoff. Handoff is a mid-session RAM dump; EOD is a daily multi-file update.
- Not a planner. Don't propose new projects, new MITs, or scope changes. Capture what happened, update live state.
- Not a writer. Don't expand the day's activity into prose. Bullets, plain language, what actually happened.
- Not self-modifying. Don't edit this skill or any other skills based on the day's activity. Note candidates for a future session.
