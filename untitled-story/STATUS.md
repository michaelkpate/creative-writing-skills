---
phase: not-started
updated: 2026-08-06
chapters_in_story: 0
drafts_in_work: 0
intake_complete: false
---

# Story Status

**Read this before exploring the project.** It answers "where is this story?" so
you do not have to walk fifteen directories to find out they are empty.

## Phase

**`not-started`** — no intake has been run. There is no story yet.

The sequence: `not-started` → `intake` → `foundation` → `outline` → `drafting`
→ `revision` → `done`. Phases overlap in practice; record the furthest one the
work has actually reached, not the one currently being touched.

## What exists

| Area | State |
|---|---|
| `work/intake/` | empty — intake not run |
| `kb/` | empty — no canon, characters, world, timeline, or style files |
| `work/outline/` | empty |
| `work/brainstorm/` | empty |
| `work/drafts/` | empty |
| `work/critique-reports/` | empty |
| `story/` | empty — no chapters |

## Next action

Run intake — read `skills/story-intake/SKILL.md`. The author may decline and
write instead; if so, log the skipped intake to `kb/issues/` and update the
phase here to reflect what is actually happening.

## Open questions

None yet. Once intake runs, thin or unresolved coverage zones belong in
`kb/issues/`, with a one-line pointer here if they block progress.

---

## Maintaining this file

**Update it whenever you update `kb/`.** Same trigger as the memory discipline
in `AGENTS.md`: after a decision, chapter, or revision settles. A status file
nobody updates becomes a status file nobody can trust, and then everyone walks
the directories again.

**Do not put story facts here.** Names, canon, plot, character state, and
timeline live in `kb/`. This file holds phase, counts, pointers, and the next
action. Duplicating facts guarantees the two will disagree.

**If it disagrees with the tree, the tree wins.** The frontmatter counts exist
so a single `ls` can catch drift cheaply. If `story/` holds six chapters and
this says zero, the file is stale: correct it, then carry on. Never reason from
a count you have not confirmed when the number actually matters.

**Keep it short.** It is read at the start of every session. If it grows past a
screen, the detail belongs in `kb/` and this should point at it instead.
