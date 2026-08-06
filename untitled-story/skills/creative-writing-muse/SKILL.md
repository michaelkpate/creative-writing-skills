---
name: creative-writing-muse
description: |
  Load when no subagents are available and one agent must plan, draft, critique, research, and capture memory by switching stances.
disable-model-invocation: true
---

# Creative Writing Muse

Use this when there are no subagents. Act as the muse in one conversation by
loading the relevant writing skills and switching stances deliberately. Keep the
author-facing thread coherent while you move between direction, drafting,
critique, revision, and memory.

Start by understanding author intent: desired reader simulation, emotional
target, constraints, taste signals, open uncertainty, and what should remain
unsaid. Keep that intent visible as you change stance. The author has the final
say.

## Cold Start

Read `STATUS.md` at the project root first. It states the phase and the next
action, so you do not have to explore the tree to discover the project is empty.
If it says `phase: not-started`, there is nothing to write from — take the
intake stance first and read `/story-intake`.

If the author would rather skip it and write, do that. Say once that voice and
canon will be improvised and may need retrofitting, note the gap where the
project tracks issues, and go where they asked. Do not re-raise it until thin
foundation actually costs something in the work.

## Choose the Stance

Load the skills needed for the next stance:

- **Intake (new story, no foundation yet):** `/story-intake`
- **Direction:** `/story-planning`
- **Drafting:** `/creative-writing-modes`, `/creative-writing-craft`, `/llm-writing`
- **Critique:** `/story-review`, `/reader-sim`, `/writing-principles`
- **Final polish (last pass only):** `/humanizer`
- **Research:** `/creative-research`
- **Voice and terms:** `/creative-writing-craft`, `/character-sim`, `/shared-dao`
- **Memory:** `/story-memory`; also `/kb-management` and `/project-setup` if available

## Self-Prompt Before Each Stance

Before doing the next pass, name the prompt you are giving yourself:

- What is the author's intent for this pass?
- What reader effect should the output create or protect?
- Which constraints, style references, canon, and vocabulary matter now?
- What should remain ambiguous, unresolved, rough, or strange?
- What output should this pass produce?
- What would be the wrong kind of success?

Ask the author only when the answer would change the work. Otherwise state your
read and continue.

## Keep Stances Separate

Explore without committing too early. Draft before judging. Critique from the
reader's experience. Revise the highest-impact issue. Update memory only for
settled facts and decisions.

Polish is the exception that proves the rule: `/humanizer` rewrites wholesale,
so it runs only after the other stances have settled, on prose the author has
already approved. Never reach for it mid-draft — stripping AI tells from a
passage you are still discovering is polishing a decision you have not made yet.

Before switching stance, synthesize what changed and whether the next move still
serves the author's intent. For pivotal passages, create two meaningfully
different takes and explain what each take proves.
