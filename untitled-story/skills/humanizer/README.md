# humanizer — provenance and local notes

`SKILL.md` is an unmodified copy of **v2.9.1** of
[blader/humanizer](https://github.com/blader/humanizer), MIT licensed,
© 2025 Siqi Chen. The full license text sits beside it in `LICENSE`.

Unlike `story-intake`, there is no licensing question here: MIT permits
redistribution inside this Apache-2.0 repository as long as the license text and
copyright notice travel with it, which is what `LICENSE` is for. Keep it.

## Keeping it in sync

The file is deliberately unmodified so updates stay a straight copy. To update:

```bash
git -C "C:/Users/micha/Projects/humanizer" pull
cp "C:/Users/micha/Projects/humanizer/SKILL.md" \
   untitled-story/skills/humanizer/SKILL.md
```

Do not edit `SKILL.md` in place. Project-specific adjustments belong in this
file or in `AGENTS.md`, so the next update does not silently revert them.

## How it fits this project

Humanizer is a **final polish pass**, not a drafting skill. It rewrites text
wholesale, which cuts across the muse's stance separation — draft before
judging, critique from the reader's experience, revise the highest-impact issue.
Run it only after critique and revision have settled, on prose the author has
otherwise approved.

It sits at a different layer from the two skills it appears to overlap:

- `writing-principles` — narrative failure modes (info-dumping, labeling
  emotions, collapsing ambiguity). No prose-surface tells.
- `llm-writing` — prose-surface tells, but written for documents; it talks about
  sections, disclosure tiers, and sources. Much of it does not apply to fiction.
- `humanizer` — prose-surface tells for general prose, plus a "personality and
  soul" section neither of the others has.

The genuine additions for fiction are inflated symbolism, promotional language,
superficial `-ing` analyses, rule of three, and AI vocabulary.

## Known overlap: em dashes

Both this skill and `writing-principles` ("Punctuation Tells") issue directives
about em dashes. **`writing-principles` governs punctuation for this project** —
it is fiction-specific and defers to the project style file and the author's
voice. Treat humanizer's em-dash rule as already satisfied and do not apply both,
or a polish pass will double-correct punctuation the author chose deliberately.

## Overriding it

Humanizer optimizes for prose that does not read as AI-written. That is not
always the same as prose that serves the story. If a deliberate stylistic
choice — an anaphoric triple, a formal register, a character who genuinely
speaks in inflated abstractions — trips one of its patterns, the author's voice
wins. Flag the tension; do not silently normalize it away.
