---
name: post-mortem
description: >
  Run a structured post-mortem on a project that just wrapped. Grill the user with one question at a time across the project's full lifecycle (inputs → execution → revisions → handoffs → delivery → tools → process → time/scope → quality → goals → the one change). Write the answers into a dated post-mortem doc at `library/post-mortems/` that links back to the project's doc in `workshop/`, and surface candidate process changes. Use this skill when the user says "post-mortem", "let's debrief [project]", "run the post-mortem on X", "project retro", "wrap-up review", "what did we learn from X", or "review how [project] went". Explicit-trigger only. The skill is interactive — Claude asks, the user answers, Claude writes. Do not synthesize answers the user didn't give. Do not auto-update SOPs or skills; only propose changes at the end.
---

# Post-Mortem

Your job is to grill the user about a project that just wrapped, capture their answers verbatim into a post-mortem doc, and surface what should change in the process going forward. The point is to keep learning across every big project so the same friction doesn't repeat.

## Hard rules

- One question at a time. Wait for the answer before moving on. Do not batch questions.
- Bullet points. No paragraphs longer than 2 lines.
- Follow `.claude/rules/communication-style.md` if present. No em dashes. No vague abstractions. No motivational language. No "great answer" type filler.
- Capture the user's words plainly. Do not rewrite answers into corporate prose.
- Do not invent context. If the user says "I don't know", record that and move on.
- Internal tone. Direct. This is a learning exercise, not a celebration.
- Do not auto-edit any SOPs, skills, or context files based on the post-mortem. Only propose changes at the end and let the user confirm each one.

## Skip mechanic

If the user says any of these mid-grill, jump to the first question of the next section:
- "skip", "skip this"
- "next section", "next"
- "move on", "pass"

When a section is skipped, omit it entirely from the final doc. Do not write a "skipped" placeholder, do not flag it, do not re-ask the skipped questions later. Acknowledge the skip in one short line ("Skipping to [Section Name].") and ask the next section's first question.

If they say "skip the rest" or "wrap it up", jump straight to Step 4 (process changes) and omit any unanswered sections from the final doc.

## Step 1 — Identify the project

Before grilling, confirm:
- Which project is this post-mortem for? (name, type, client/stakeholder)
- Wrap date
- Is there an existing project doc in `workshop/`? If yes, read it for context.

If the user doesn't specify, ask: "Which project are we debriefing?"

## Step 2 — Set up the doc

Create the file at `library/post-mortems/[YYYY-MM-DD]-[project-slug].md` using the structure in Step 5. Write to it as you go so nothing is lost if the session ends early. If the folder doesn't exist, create it.

Link back to the project doc. Before writing, locate the matching file in `workshop/`. Use git log, memory, and `brain/current-priorities.md` to confirm the right slug. If no project doc exists, note that in the header.

Open with a stub:

```markdown
# Post-Mortem: [Project Name]
**Wrapped:** [date]
**Type:** [project type — e.g., long-form video, feature launch, campaign, report, etc.]
**Client/Stakeholder:** [name]
**Reviewed:** [today's date]
**Project doc:** [relative link to workshop/[slug].md, or "none on file"]
```

## Step 3 — The grill

Ask these in order. One question per turn. 18 questions total across 12 sections.

### Overall read

1. Score this project 1 to 10, and in one sentence: what's the headline story for the project?

### Inputs and dependencies

2. When the project landed on your plate, did you have what you needed? Any upstream friction (missing inputs, late assets, unclear scope) that hit you mid-execution?

### Early phase (kickoff / initial execution)

3. How did the opening phase go — did you have a clear sense of structure and direction, and how long did it take vs. what you expected?
4. One thing about your early-phase process this round you want to repeat or change?

### Mid phase (refinement / iteration)

5. Where did the work get stuck the longest, what did you ship knowing it was weak, and what did you try that you'd skip next time?

### Revisions and feedback

6. How many rounds, how many were avoidable, and was the feedback clear or did you have to interpret it?

### Handoffs to other roles

7. Did your handoffs to other people (collaborators, reviewers, downstream owners) go cleanly, or did you have to fix something after the hand-off?
8. Anything in the handoff process or SOPs that needs an update based on what happened?

### Final phase (polish and finalization)

9. Did the final polish land where you wanted, or did anything drift off-standard and need late fixing?

### Delivery / ship

10. Did the final delivery or launch go cleanly, or did something break or force a workaround?
11. Any spec, format, or delivery-system issue that cost you time, and is it worth updating reference docs?

### Tools

12. Which tool helped the most, which got in the way, and what's missing from the toolkit?

### Process and tracking

13. Did your task system reflect reality or did statuses drift, did anything fall through the cracks, and was there a moment you should have flagged something earlier?

### Time and scope

14. How long did the project take vs. what you expected, and did scope shift mid-way?

### Quality

15. Does the final output meet your bar? If not, what's missing?
16. What's one skill this project pushed you on, and one area you still need to sharpen?

### MIT and standards alignment

17. Did this project move any of your quarterly MITs forward, and by how much? Did it hit the relevant brand/quality standard cleanly, or did you fight old habits anywhere?

### The one change

18. If you change only one thing going into the next project, what is it? And what new SOP, checklist, skill, or reference doc would lock that change in?

## Step 4 — Surface process changes

After all questions are answered (or the user calls "wrap it up"), summarize candidate changes in this format:

```
Proposed changes coming out of this post-mortem:

1. [Change] — update [file path] / create new [skill/SOP/reference]
   Reason: [paraphrased from their answer]

2. ...

Apply which of these? (list numbers, or "none")
```

Only edit files for the changes the user confirms by number. For each confirmed change:
- If it's a context update (e.g., `brain/current-priorities.md`, `brain/mit.md`), propose the exact edit before writing.
- If it's a new SOP or reference, propose the filename and 3-bullet outline before writing.
- If it's a new skill, do not create it inside this skill. Note it for a future session.

## Step 5 — Final doc structure

The completed post-mortem doc should look like this:

```markdown
# Post-Mortem: [Project Name]
**Wrapped:** [date]
**Type:** [project type]
**Client/Stakeholder:** [name]
**Reviewed:** [today's date]
**Project doc:** [relative link to workshop/[slug].md, or "none on file"]

## Score and headline
[Q1 — N/10 and one-sentence headline]

## Inputs and dependencies
- [Q2]

## Early phase
- Structure and timing: [Q3]
- Repeat or change: [Q4]

## Mid phase
- [Q5]

## Revisions
- [Q6]

## Handoffs
- Cleanliness: [Q7]
- SOP updates: [Q8]

## Final polish
- [Q9]

## Delivery
- Launch/delivery: [Q10]
- Spec/format issues: [Q11]

## Tools
- [Q12]

## Process and tracking
- [Q13]

## Time and scope
- [Q14]

## Quality
- Meets bar? [Q15]
- Pushed on / still to sharpen: [Q16]

## MIT and standards
- [Q17]

## The one change
- [Q18]

## Process changes confirmed
- [Each confirmed change from Step 4, with the file path that was updated or "noted for future session"]
```

Sections the user skipped should be omitted from this output entirely.

## Step 6 — Close

End the session with:
- Path to the post-mortem doc
- List of files that were updated (if any)
- One-line note on what to revisit in 30 days

No motivational close. No sign-off.

## What this skill is not

- Not a planning skill. Don't propose new projects or scope work.
- Not a therapy skill. Don't probe feelings beyond the score in Q1.
- Not a writer. Don't expand the user's answers into prose. Capture them plainly.
- Not a self-modifying skill. Don't edit SOPs or other skills without explicit per-item confirmation.
