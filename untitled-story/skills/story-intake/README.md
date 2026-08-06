# story-intake — credit and local reference setup

## Credit

This skill is adapted from **"30 to Glory"** by **Ekello Harrid** (GitHub
[FuFicFac](https://github.com/FuFicFac), [portfolio](https://ej-student-portal.vercel.app/)),
published as a gist:
<https://gist.github.com/FuFicFac/f29ecac587c175280d863bb7ef766222>

His design carries the substance of the skill: the 22/4/4 question budget and
the hard stop at 30, the six worksheet-critical coverage zones, the
`COVERED`/`PARTIAL`/`THIN`/`UNRESOLVED` markers, the reserve-question logic, the
`READY`/`NOT_READY` gate, and the four act structures with the
ten-chapters-per-act rule. The three reference documents are his as well.

What is local to this project: retargeting the handoff away from
`book-brain-builder` and into `kb/` + `work/outline/`, the seeding table, the
escape hatch, and the `AGENTS.md` / muse wiring.



`SKILL.md` reads three files from `references/` that are **deliberately not in
git**:

- `references/story-worksheet.md`
- `references/act-structures.md`
- `references/chapter-outline.md`

If you cloned this repo, that directory is empty and the skill will not find
them. That is expected, not a bug.

## Why

Those three files come from the "30 to Glory" StoryCraft intake skill, a secret
gist by FuFicFac:

<https://gist.github.com/FuFicFac/f29ecac587c175280d863bb7ef766222>

The gist itself states no license, copyright notice, or usage terms anywhere in
its text.

The author's portfolio does signal intent, though. At
<https://ej-student-portal.vercel.app/> the gist is listed under a section
headed "Open Source on GitHub", introduced with: "Everything I've published
publicly. Skills, tools, chat interfaces, automation frameworks — if I built it
and opened it up, it's here. Fork it, study it, build on it."

That is a real invitation to reuse and it makes a permission request very
likely to succeed. It is still not a license grant: it does not name terms,
and it does not say the work may be redistributed inside a third-party package
under that package's own license. This repository is Apache-2.0, so committing
the files here would offer someone else's work to the world under an
Apache-2.0 grant they never agreed to.

Holding them out of git is the conservative default, not a judgment that reuse
was unwelcome.

They are therefore kept on local disk only, listed in `.gitignore`, and never
published. The rest of this skill — `SKILL.md`, and the intake wiring in
`AGENTS.md` and `skills/creative-writing-muse/SKILL.md` — is original to this
repo and carries no such restriction.

## Restoring them locally

Copy the three files from wherever you keep your own copy, for example:

```bash
cp "/c/Users/micha/Projects/30 to Glory Skill/references/"*.md \
   untitled-story/skills/story-intake/references/
```

`.gitignore` keeps them untracked, so there is no risk of committing them by
accident.

## If the licensing question gets resolved

The author is Ekello Harrid (GitHub `FuFicFac`), reachable via the portfolio
above, <https://x.com/ekello>, or a comment on the gist itself. Given the
"Fork it, study it, build on it" framing, asking is likely a formality — but it
converts an inference into a stated permission, which is what the Apache-2.0
redistribution actually needs.

If FuFicFac grants permission to redistribute, drop the
`/untitled-story/skills/story-intake/references/` line from `.gitignore`,
commit the files, and add an attribution notice naming the gist as the source.

If permission is declined or never arrives, rewrite the three references in
original prose instead. The underlying craft material — act-structure
percentages, the ten-chapters-per-act heuristic, worksheet field categories —
is common knowledge and not ownable; only the specific expression in the gist
is. A clean rewrite would let the skill ship complete under Apache-2.0.

Until then, intake still runs for anyone who has the files locally, and
degrades to "reference missing" for anyone who does not.
