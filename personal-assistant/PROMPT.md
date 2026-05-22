# Personal Assistant Bootstrap Prompt

Unzip the `personal-assistant` folder somewhere convenient, then open it in either Claude Code or Claude Desktop's Cowork side panel. Copy the prompt below the divider and paste it into a fresh chat with that folder open. Claude will ask ~12 questions and personalize your vault in place.

**Requirements:** Claude Code or Claude Desktop (Cowork). The `personal-assistant` folder unzipped and opened as the workspace.

**Where this works:**
- **Claude Code** — skills auto-activate on the trigger phrases in each `SKILL.md`.
- **Cowork (Claude Desktop)** — skills don't auto-activate, but `CLAUDE.md` tells the assistant to read the matching `SKILL.md` and follow it when you say a trigger phrase. Functionally identical.

---

You are about to personalize a personal assistant vault for the user pasting this prompt. The vault folder structure and all bundled skills are already on disk. Your job is to walk the user through onboarding questions and inject their answers into the existing files. No files are fetched from the internet, nothing is created from scratch — the structure is here, you just personalize it.

## Your job

Walk the user through 12 questions, then personalize the vault. Be casual and direct.

## Communication rules

- One question at a time. Do not stack.
- No em dashes. No motivational filler.
- Two sentences max per turn.
- After each answer, briefly acknowledge it ("Got it.") and ask the next question.

## Step 0 — Verify the vault is on disk

Before asking anything, check the current working directory for the vault structure. Look for:

- A `CLAUDE.md` file at the root
- A context folder containing `me.md` (e.g., `brain/me.md`, or `context/me.md` if the user renamed the folder)

**If found** → Detect which folder names are in use (`brain/` vs `context/`, `library/` vs `references/`, etc.) by inspecting what's on disk. Remember those names — they're the chosen names used throughout the rest of the prompt. Continue to Step 1.

**If NOT found** → Stop and tell the user:

> I don't see the personal-assistant vault structure here. Make sure you unzipped the `personal-assistant` folder and opened that folder in Claude Code (or Claude Desktop's Cowork side panel) before pasting this prompt.

## Step 1 — Welcome

Output this exactly:

> Welcome. Let's personalize your assistant vault. I'll ask you about 12 questions. The more you give me up front, the sharper your assistant will be from day one. Anything you skip can be filled in later. Ready?

Wait for confirmation.

## Step 2 — Questions

Ask each one at a time. Wait for the answer before moving on.

1. What's your full name?
2. What's your role or job title?
3. What timezone are you in?
4. In one sentence, what do you do?
5. What's your #1 priority right now? (the single most important thing you're working on)
6. What do you want to name me, your personal assistant? (optional - leave blank to keep it nameless)
7. What tools do you use daily? (e.g., Slack, Notion, Monday, Asana, Linear, Google Workspace, Adobe)
8. What's your team shape? (solo / small team / large org / freelance / agency)
9. What are your top 1-3 goals for the current quarter? (each in one line)
10. What are your top 1-3 MITs (Most Important Targets) right now? A Most Important Target is a measurable outcome that defines whether the quarter was successful.
11. Anything else important about how you work? (work hours, communication patterns, deep work — optional)
12. One thing your assistant should always remember or always avoid? (optional)

## Step 3 — Personalize the vault

Replace `$VARIABLE` markers with the user's answers. Use today's date in ISO format (YYYY-MM-DD) for date placeholders. **All file paths below use default folder names — if Step 0 detected different folder names (e.g., `context/` instead of `brain/`), substitute the detected names everywhere, both in the file paths and inside file contents (e.g., `@brain/me.md` references inside `CLAUDE.md` become `@<detected-name>/me.md`).**

For each file below, edit the existing file on disk in place:

**`CLAUDE.md`** — already on disk. Apply these edits:
- If the user named their assistant in Q6, rewrite the identity line from "You are the user's executive assistant and second brain." to "You are $ASSISTANT_NAME — the user's executive assistant and second brain." If they left it blank, leave the line as is.
- Replace the "[Fill this in...]" line under **Top priority** with the user's #1 priority.

**`brain/me.md`** — replace the placeholder block with:
```
- **Name:** $NAME
- **Role:** $ROLE
- **Timezone:** $TIMEZONE
- **What I do:** $WHAT_I_DO
- **#1 priority:** $TOP_PRIORITY
```

**`brain/work.md`** — replace the Tools placeholder with $TOOLS_LIST (one tool per bullet). Replace the Notes placeholder with $ANYTHING_ELSE from Q11. If Q11 was skipped, leave the Notes section empty.

**`brain/team.md`** — add a team-shape line right after the header (keep the existing placeholder and the empty table intact):
```
Team shape: $TEAM_SHAPE
```

**`brain/current-priorities.md`** — replace the date and first-priority placeholders:
```
_Last updated: $TODAY_

1. $TOP_PRIORITY
```

**`brain/goals.md`** — replace the `[e.g., Q2 2026]` placeholder with the current quarter. Fill the numbered goal placeholders with $GOALS_LIST from Q9 (one goal per numbered line, up to 3). Leave unused numbered slots as placeholders.

**`brain/mit.md`** — for each MIT the user gave in Q10, replace the `[Short name]` placeholder of the matching MIT section with their stated MIT. Leave the Problem, Hypothesis, and measurement placeholders as-is for the user to fill in later. If they gave fewer than 3 MITs, leave the remaining MIT sections as placeholders.

### Memory entry (optional)

If the user answered question 12, write a short memory note so future sessions honor that preference.

## Step 4 — Skill primer

Output the following block exactly. This is the user's reference for what they have and how to fire it.

> Here are the 9 bundled skills and how to use them. Each one fires when you say a specific phrase. Trigger phrases are casual — use whichever feels natural.
>
> **slop-detector** — catches AI-sounding writing before it ships.
> Say: "run slop detector", "de-slop this", "make this sound human"
>
> **client-researcher** — deep internet research on a person before outreach or a meeting.
> Say: "research [name]", "build a brief on [name]", "prep me for a call with [name]"
>
> **handoff** — captures the current session into a paste-ready handoff doc for a fresh chat.
> Say: "session sync", "handoff", "save this session"
>
> **grill-me** — interviews you relentlessly about a plan until every decision is resolved.
> Say: "grill me", "stress-test this plan", "interview me on this"
>
> **daily-briefing** — morning rundown: today's calendar, tasks, active projects, MIT snapshot.
> Say: "good morning", "what's today", "daily briefing", "what's on deck"
>
> **meeting-prep** — pre-meeting brief from calendar, prior notes, recent threads.
> Say: "prep me for [meeting]", "what's my next meeting", "brief me on the [name] sync"
>
> **mit-checkin** — review-only pulse on your quarterly MITs.
> Say: "MIT check-in", "how am I tracking", "review my MITs"
>
> **post-mortem** — structured debrief of any completed project.
> Say: "post-mortem", "let's debrief [project]", "what did we learn from [project]"
>
> **evening-beer** — end-of-day wrap that updates your priorities, MITs, decisions log, and project files.
> Say: "end of day", "EOD", "wrap up the day", "let's grab a beer to end the day"
>
> **How skills fire:**
> - In Claude Code, they auto-trigger the instant you say a phrase.
> - In Cowork, I read the matching `SKILL.md` and follow it. Once I've read it in a session, it stays loaded for follow-ups.
>
> If a phrase ever doesn't fire the skill, just say "use the [skill-name] skill" and that always works.

## Step 5 — Closing message

Output exactly:

> Vault is personalized.
>
> Top-level folders: brain/, library/, workshop/, ledger/, recipes/, attic/ (or your renamed equivalents).
>
> Quick test: say "good morning" and I'll run the daily-briefing skill. [If the user named their assistant, say "I'm $ASSISTANT_NAME — I get sharper the more you tell me." Otherwise say "I get sharper the more you tell me."]

Then stop.

---

End of bootstrap prompt.
