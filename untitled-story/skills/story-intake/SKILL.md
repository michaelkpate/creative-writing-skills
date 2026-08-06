---
name: story-intake
description: |
  Cold-start intake for a new story. Load when the project has no foundation yet — a raw idea, brain dump, or premise fragment — and the muse needs to interview the author into a usable story foundation before planning or drafting.
---

# Story Intake — 30 to Glory

Adapted from the "30 to Glory" StoryCraft intake skill by **Ekello Harrid**
([FuFicFac](https://github.com/FuFicFac),
[gist](https://gist.github.com/FuFicFac/f29ecac587c175280d863bb7ef766222)).
The question budget, coverage zones, readiness gate, and act-structure framework
are his design; the handoff into this project's `kb/` and `work/` layout is the
local adaptation. See `README.md` for the full provenance note.

The muse's cold-start stance. Takes a raw fiction idea, brain dump, or premise
fragment and converts it into a story foundation this project can draft from:
four intake artifacts in `work/intake/`, plus a seeded `kb/`.

**Role:** First gate only. Once the foundation exists, hand off to
`/story-planning` and leave this stance behind. Do not run intake twice on a
story that already has one.

## Responsibilities

- Normalize brain dumps, premise fragments, or partial dossiers into usable story signals
- Ask up to 30 author-facing questions following the question budget
- Track coverage against worksheet-critical categories
- Recommend act structure using `references/act-structures.md`
- Produce the four intake artifacts
- Seed `kb/` and `work/outline/` from the bundle
- Return `READY` or `NOT_READY`

## Non-Responsibilities

- Draft chapter or scene prose
- Run editorial rewrite passes
- Silently invent missing canon

## When to Run

Check `STATUS.md` at the project root. Run intake when it says
`phase: not-started`, or `phase: intake` with `intake_complete: false` (a run
that stopped partway — resume it rather than starting over).

At any later phase the story already has a foundation. Read `work/intake/` and
`kb/` instead, and move to the stance the author actually needs. Do not re-run
intake on a story that has one.

**Escape hatch.** The author may decline — "skip it, just write." Honor that
immediately and without argument. Then:

1. Say plainly that the foundation is thin and what that costs: voice, canon,
   and continuity will be improvised and may need retrofitting later.
2. Log the skipped intake to `kb/issues/` so it resurfaces.
3. Proceed to the stance they asked for.

The author has the final say. Offer intake again only when thin foundation
actually causes a problem in the work — not as a recurring nag.

## Question Budget

| Budget | Count | Purpose |
|--------|------:|---------|
| Core | 22 | Baseline story foundation and drafting-critical information |
| Expansion | 4 | Worksheet depth that materially improves foundation quality |
| Reserve | 4 | Repair questions only — use only when synthesis or contradiction is detected |

Hard rule: stop at 30. If key gaps remain, report them explicitly rather than
keep asking.

Ask one question at a time. Let the author's answers redirect what you ask
next — the budget is a ceiling, not a script to read through.

## Worksheet-Critical Coverage Zones

Every intake must materially address these zones (see
`references/story-worksheet.md` for full field definitions):

- **Project foundation:** title, genre, audience, tone, projected length, premise, conflict, stakes
- **Voice and prose:** POV, tense, reader experience goal, prose baseline, anti-slop constraints
- **World:** world overview, unbreakable rules, primary settings, plot-critical lore or tech
- **Character:** protagonist wound, flaw, want, need, emotional arc, major relationship pressure
- **Plot:** beat spine, likely act structure, genre promises, trope obligations
- **Drafting handoff:** chapter-planning constraints, chapter generation instructions

Coverage does not require every worksheet field to be equally deep. It does
require honesty about what is thin.

## Coverage Markers

Rate each zone as: `COVERED` | `PARTIAL` | `THIN` | `UNRESOLVED`

## Reserve Question Logic

Reserve questions may be spent only when:

- The protagonist engine is still vague
- The stakes are not legible
- The world rules are too soft to support drafting
- POV / tense / narration mode is unstable
- The beat spine is too thin to support outline generation
- Synthesis surfaces a contradiction between answers

## Stop Conditions — Emit NOT_READY When:

- Critical contradictions remain after reserve questions are exhausted
- Worksheet-critical sections are still too thin to draft safely
- The budget reaches 30 and core gaps remain

Never fake completion.

`NOT_READY` is not a dead end. Seed everything that *is* settled, record the
gaps in `kb/issues/`, and tell the author which specific zones need another
pass before drafting is safe.

## Act Structure Recommendation

After coverage is assessed, recommend one of four structures from
`references/act-structures.md`:

- **3-Act**: 10–20 chapters. Maximum creative freedom, but vulnerable to saggy middle.
- **4-Act**: 10–40 chapters. Balanced, midpoint-focused, prevents sag.
- **5-Act**: 20–40 chapters. Character-transformation focus, varied act lengths.
- **9-Act**: 30–40+ chapters. Granular commercial pacing, multiple turning points.

Choose based on projected chapter count and genre. Apply the 10-chapter-per-act
rule.

## Output Artifacts

Write four files to `work/intake/`:

1. **`INTAKE_TRANSCRIPT.md`** — raw Q&A record
2. **`INTAKE_SUMMARY.md`** — concise operational summary: premise, voice baseline, act structure recommendation, readiness status
3. **`WORKSHEET_COVERAGE_REPORT.md`** — explicit `COVERED` / `PARTIAL` / `THIN` / `UNRESOLVED` markers per zone
4. **`INTAKE_BUNDLE.md`** — structured foundation record, organized by the coverage zones above

Apply the project's source tagging in all four: untagged text is the author's,
`<AI>...</AI>` marks your suggestions, `<hidden>...</hidden>` marks author-only
notes. Intake is where an author's own words matter most — do not launder their
phrasing into yours.

### INTAKE_BUNDLE.md Format

Structured, consistent, and parseable. Use `snake_case` headers matching
`references/story-worksheet.md`. Include all collected data organized by the
worksheet zones. Flag explicitly where data is thin or unresolved.

## Seeding the Project

The bundle is the source; `kb/` is where it lands. After writing the artifacts,
seed:

| Bundle zone | Seeds |
|---|---|
| voice_and_prose | `kb/styles/` — and fill the **Voice and Style Goals** section in `AGENTS.md`, which ships empty on a fresh project |
| world | `kb/world/` — unbreakable rules also go to `kb/canon/` |
| character | `kb/characters/` — one file per main character |
| plot beat spine + act structure | `work/outline/` |
| thin, unresolved, or skipped zones | `kb/issues/` |
| phase, counts, next action | `STATUS.md` at the project root |

Update `STATUS.md` last, once everything else has landed — it describes the
state, so writing it first would describe a state that does not exist yet. Move
the phase to `foundation` on `READY`, or leave it at `intake` on `NOT_READY`
with the blocking zones named under open questions.

Follow `/story-memory` and `/kb-management` for file conventions.

Seed only what the author settled. An intake answer is a decision; an
`<AI>`-tagged suggestion the author never confirmed is not. Provisional material
stays in `work/intake/` until the author commits to it — do not let it harden
into canon by passing through this table.

If the author supplied prose they admire or wrote themselves, save it to
`kb/samples/` before deriving anything into `kb/styles/`.

## Handoff

Report to the author: readiness status, the coverage table, the recommended act
structure and why, and what was seeded where. Name the thin zones out loud.

Then leave this stance. Direction work goes to `/story-planning`; the outline
seeded in `work/outline/` is a starting spine, not a finished plan.

## References

- `references/story-worksheet.md` — full Story Worksheet Template with all field definitions
- `references/act-structures.md` — act structure guide and decision framework
- `references/chapter-outline.md` — chapter outline template

Reproduced unmodified from Ekello Harrid's gist with his permission — see
`README.md`. If a reference is ever missing, say so plainly rather than
inventing the worksheet fields or act-structure guidance from memory.
