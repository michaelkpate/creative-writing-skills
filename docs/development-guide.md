# Development Guide: creative-writing-skills

Reference for extending or maintaining the package, derived from the repo
architecture investigation (2026-07-15). This is a development guide, not an
implementation plan — when picking up a backlog item (§11), write a scoped
plan for that item first.

Current package version at time of writing: **0.5.6**.

## Goal

Deliver a Mars source package of composable creative-writing **agents** and
**skills** for Meridian and Claude-family harnesses, plus a Claude-native
`cw/` distribution (marketplace plugin + `.skill` zips).

Product thesis:

- one strong author-facing muse
- a small specialist worker set
- durable story context in `kb/`
- craft methodology in skills

Not: many near-duplicate writer agents.

---

## 1. Product surfaces

| Surface | Path | Consumers |
|---|---|---|
| Mars source | `agents/`, `skills/`, `mars.toml` | Meridian (`mars add` / `mars sync`) |
| Claude plugin | `cw/agents/`, `cw/skills/`, `cw/.claude-plugin/plugin.json` | Claude Code, Cowork, claude.ai marketplace |
| Skill zips | built from `cw/skills/` → `zips/*.skill` | Manual Claude.ai upload / GitHub Releases |
| Bootstrap | `bootstrap/project-setup/BOOTSTRAP.md` | First-time Meridian project setup |

**Source of truth rule:** edit `agents/` and `skills/` first, then resync or adapt `cw/`. CI fails on drift.

---

## 2. Package model

### `mars.toml`

- package name + version
- Depends on `meridian-base` (`>=0.8.0, <0.9.0`) and local `meridian-prompter`
- Emits native harness targets: `.claude`, `.cursor`, `.codex`, `.opencode`
- `agent_emission = "always"`

Repo is both:

- a **publisher** of agents/skills
- a **consumer** of Meridian-base shared skills (`llm-writing`, `shared-dao`, `grill-with-docs`, etc.)

### Primary commands

```bash
meridian mars check
meridian mars sync
meridian -C "$PWD" mars version patch --push
```

When nested under a Meridian session, always pass `-C` to the package root so check/version/sync target this package, not the session control root.

---

## 3. Architecture

### Agents = thin runners

Spawned processes with model/tool/policy frontmatter and a short body.

| Agent | Role |
|---|---|
| `muse` | Author-facing coordinator: intent, routing, synthesis |
| `writer` | All production prose modes (fresh / revise / bridge / alternate / polish) |
| `critic` | Focused adversarial craft critique |
| `editor` | Holistic book-editor memo and priority |
| `reader-sim` | Experiential first-time reader response |
| `continuity-checker` | Broad canon / terminology contradiction pass |
| `brainstormer` | Divergent option generation |
| `outliner` | Arc / chapter / beat structure after direction is chosen |
| `character-sim` | In-character voice / relationship testing |
| `style-creator` | Style-reference extraction from samples |
| `web-researcher` | Research support |

Knowledge capture is **not** a bundled agent: muse routes it to meridian-base's
`kb-lead` loaded with `story-memory`, or applies `story-memory` directly in
harnesses without a kb-lead subagent. (A former `chronicler` agent was
consolidated away; stale doc references were cleaned up 2026-07-15 on branch
`fix/chronicler-doc-drift`.)

### Skills = methodology loaded into context

| Skill | Purpose |
|---|---|
| `story-planning` | Direction, brainstorming, outline, architecture |
| `creative-writing-modes` | Pen-on-paper production modes |
| `creative-writing-craft` | Prose, scene, style, genre technique + resources |
| `story-review` | Editorial / dev / line / copy / proof / critique / reader-signal |
| `story-memory` | Context selection, fact extraction, artifacts, issues |
| `writing-principles` | Reader rewards + AI fiction failure modes |
| `writing-staffing` | How to compose teams and attach extra skills |
| `reader-sim` / `character-sim` | Skill-only simulation modes |
| `creative-writing-muse` | Single-agent muse fallback when no subagents exist |
| `creative-research` | Research support for creative work |

Notes:

- `creative-writing-muse` is for Claude.ai / skills-only. The **`muse` agent does not load it**.
- Agents own Product-Lead-style routing directly: capture intent, craft specialist prompts, synthesize, speak back to the author.

### Production loop

```text
muse → writer → critic | editor | reader-sim | continuity-checker → writer
// decisions settle → knowledge update (story-memory / kb-lead)
```

### User project layout

```text
my-story/
├── AGENTS.md / CLAUDE.md
├── story/                 # author manuscript space
├── work/
│   ├── outline/
│   ├── drafts/
│   ├── critique-reports/
│   └── brainstorm/
└── kb/
    ├── styles/
    ├── characters/
    ├── world/
    ├── timeline/
    ├── canon/
    └── issues/
```

---

## 4. Dual distribution: Mars source vs `cw/`

### Source paths

- `skills/<name>/SKILL.md` (+ optional `resources/`)
- `agents/<name>.md`
- optional `agents/<name>.author-variant`

### Claude tree build / lint

`scripts/sync_cw_skills.py` classifies components:

**GENERATED** (refreshed from temporary Mars consumer `.claude/` output on `--apply`):

- skills: `character-sim`, `creative-research`, `creative-writing-craft`, `creative-writing-modes`, `creative-writing-muse`, `reader-sim`, `story-memory`, `story-planning`, `story-review`, `writing-principles`, plus selected base skills (`information-hierarchy`, `intent-modeling`, `knowledge-layers`, `llm-writing`, `qi-layer`, `reflect`)
- agents: `brainstormer`, `character-sim`, `continuity-checker`, `critic`, `editor`, `muse`, `outliner`, `reader-sim`, `style-creator`, `web-researcher`, `writer`

**MANUAL** (lint only, never overwritten):

- `writing-staffing`
- `grill-with-docs`
- `shared-dao`
- `structured-artifact`
- `md-validation`
- `zoom-out`
- `kb-management`
- `project-setup`

Mars lowers harness frontmatter to Claude format. `cw/` must use Claude vocab (`name` + `description`, maybe `disable-model-invocation`), never Mars-only keys (`type`, `model-invocable`, `effort`, etc.). Frontmatter is transformed; **agent body text passes through verbatim** — so body prose must read correctly in every harness (see design rule 7).

Script modes:

```bash
python scripts/sync_cw_skills.py            # full check: build + drift + lint
python scripts/sync_cw_skills.py --lint     # CI / pre-commit
python scripts/sync_cw_skills.py --apply    # regenerate GENERATED components
python scripts/sync_cw_skills.py --list     # print classification
```

`--apply` may need network access to resolve git dependencies.

### Plugin packaging

- Marketplace catalog: `.claude-plugin/marketplace.json` → source `./cw`
- Plugin manifest: `cw/.claude-plugin/plugin.json` (**required** for marketplace add-from-GitHub)
- Version triple sync:
  - `mars.toml`
  - `.claude-plugin/marketplace.json` → `metadata.version`
  - `cw/.claude-plugin/plugin.json` → `version`

The sync script can re-sync versions across these files.

---

## 5. Implementation recipes

### A. Add or change a skill

1. Create/edit `skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`; Mars may also use `type`, `model-invocable`, etc.).
2. Put deep material under `skills/<name>/resources/` and load only the needed resource.
3. Keep skills **self-contained** (no hard skill-to-skill dependencies).
4. Wire into agents via `skills:` frontmatter (`load` `/` `available` or flat list).
5. Ship in Claude distribution by either:
   - adding the skill name to `GENERATED_SKILLS` in `scripts/sync_cw_skills.py`, or
   - hand-maintaining under `cw/skills/` and listing in `MANUAL_SKILLS`.
6. Run `python scripts/sync_cw_skills.py --apply`, then the validation matrix (§5F).
7. Update README skill table and `CHANGELOG.md`.

### B. Add or change an agent

1. Create `agents/<name>.md` with Mars frontmatter:
   - `name`, `description` (selection signal: when/why)
   - `model` + optional `model-policies`
   - `skills` / `subagents` / tools / sandbox / approval as needed
2. Keep body thin: role + stance + output contract. Methodology lives in skills. Body prose must stay harness-neutral (rule 7).
3. Update `skills/writing-staffing/SKILL.md` with spawn guidance:
   - extra `--skills`
   - files to attach via `-f`
   - when to fan out
4. If muse should route to it, add to muse `subagents:`.
5. Add to `GENERATED_AGENTS` (or `MANUAL_AGENTS`) and sync `cw/`.
6. Run the validation matrix (§5F).

Reference patterns:

- orchestrator thinness: `agents/muse.md`
- prose worker: `agents/writer.md`
- staffing: `skills/writing-staffing/SKILL.md`
- Claude lowered view: `cw/agents/writer.md`

### C. Extend craft content

Most package work is content engineering:

```text
skills/creative-writing-craft/resources/
skills/story-review/resources/
skills/story-planning/resources/
skills/writing-principles/resources/
skills/story-memory/resources/
```

After edits to GENERATED skills, always re-run `--apply`.

### D. Bootstrap / first-time project setup

- Meridian: `bootstrap/project-setup/BOOTSTRAP.md`
  - create `AGENTS.md`
  - `CLAUDE.md` containing `@AGENTS.md`
  - kb dirs, work dirs, optional style-creator
- Claude-only: `cw/skills/project-setup` → `/creative-writing-skills:project-setup`

### E. Release

Preferred:

```bash
meridian -C "$PWD" mars version patch --push
```

`scripts/release.sh` is deprecated (bumps only `mars.toml`; macOS-only `sed -i ''`).

Release constraints:

- tag `vX.Y.Z` must match `mars.toml` version
- CI builds `zips/*.skill` and attaches them to GitHub Release
- enable pre-commit once per clone if desired:

```bash
git config core.hooksPath .githooks
```

Pre-commit runs `sync_cw_skills.py --lint` + `claude plugins validate cw`.

### F. Validation matrix (done definition)

```bash
meridian mars check
claude plugins validate .claude-plugin/marketplace.json
claude plugins validate cw
python scripts/sync_cw_skills.py --lint
python scripts/create_skill_zips.py
```

Run full sync check (with Mars build) when changing GENERATED components:

```bash
python scripts/sync_cw_skills.py
```

Note: this matrix is structural. It does not test whether a skill produces
good behavior in a live session — behavioral validation is an open gap (§11).

---

## 6. Design rules that protect quality

1. **Agents thin, skills deep.** Agent bodies remain stance shells.
2. **Descriptions as selection signals.** When/why to choose; no internal mechanics clutter in public descriptions.
3. **One prose writer.** All modes live under `creative-writing-modes`, not split writer agents.
4. **Separate critique modes.** Critic ≠ editor ≠ reader-sim ≠ continuity-checker.
5. **Source tagging (brainstorms):** untagged = user; `<AI>...</AI>` = AI suggestion; `<hidden>...</hidden>` = author-only.
6. **Style guides are directive AI instructions** (imperative form + examples), not human documentation essays.
7. **No Meridian leakage into `cw/`** (`meridian spawn`, `$MERIDIAN_*`, old agent aliases, Mars-only frontmatter keys). Applies to **body prose too**, since bodies pass through lowering verbatim — phrase harness-specific instructions conditionally (see muse "After Work Settles" for the pattern).
8. **Version triple stays in sync.**
9. **Skills-only vs multi-agent:** Claude.ai uses `creative-writing-muse`; harnesses use `muse` + workers.
10. Skills are self-contained; agents declare their skills in frontmatter.

---

## 7. File touch map

| Goal | Edit first | Then |
|---|---|---|
| Craft methodology | `skills/*` | `--apply` if GENERATED |
| Agent role / tools / model | `agents/*.md` | `--apply` into `cw/agents` |
| Claude-only behavior | `cw/skills/*` (MANUAL list) | lint |
| Marketplace listing | `.claude-plugin/marketplace.json` | keep version synced |
| Plugin identity | `cw/.claude-plugin/plugin.json` | validate `cw` |
| Install UX docs | `README.md` | `docs/architecture.md` if system shape changed |
| End-user structure | bootstrap + project-setup skill | keep Meridian/Claude paths aligned |
| Packaging logic | `scripts/sync_cw_skills.py`, `create_skill_zips.py` | CI/hook |
| Release | `mars.toml` + `CHANGELOG.md` | tag `v*` |

---

## 8. End-user install paths (context for packaging decisions)

### Meridian

```bash
meridian mars add haowjy/creative-writing-skills
meridian mars sync
meridian bootstrap
meridian
```

### Claude Code / Cowork

```bash
/plugin marketplace add haowjy/creative-writing-skills
/plugin install creative-writing-skills@cw
claude --agent creative-writing-skills:muse
/creative-writing-skills:project-setup
```

### Claude.ai

- Marketplace add, or upload `.skill` zips
- Activate `creative-writing-muse`

---

## 9. Recommended extension order

1. Read:
   - `docs/architecture.md`
   - `AGENTS.md`
   - `skills/writing-staffing/SKILL.md`
   - `agents/muse.md`
   - `agents/writer.md`
2. Decide surface: craft content vs new agent vs Claude-only skill.
3. Edit Mars source.
4. Wire routing (`skills:` arrays, muse `subagents`, staffing skill).
5. Classify for `cw/` (GENERATED vs MANUAL) and run sync.
6. Validate package + plugin + lint (§5F).
7. Document (`README.md`, `CHANGELOG.md`).
8. Release only after validation is clean.

---

## 10. Known traps

- `story-planning/SKILL.md` (and similar hub skills) must stay accurate about real resource paths.
- Full `--apply` needs Mars + network deps; on machines without Meridian, GENERATED `cw/` edits must be hand-mirrored and confirmed with `--apply` elsewhere.
- CI/pre-commit lean on `--lint`; local `--apply` is the authoring path for GENERATED drift.
- Claude-lowered agents must still make sense after Mars strips Meridian tools/vocab — body prose passes through verbatim (rule 7).
- Marketplace add validates the plugin uses **`cw/.claude-plugin/plugin.json`**; marketplace schema validation alone is insufficient.

Resolved 2026-07-15 (branch `fix/chronicler-doc-drift`): stale `chronicler`
references in README/architecture/author-variant; Meridian-only `@kb-lead
--skills` instruction shipping in `cw/agents/muse.md`; `release.sh` now
carries a deprecation header.

---

## 11. Backlog (each item needs a scoped plan before execution)

1. **Add a new agent end-to-end** — needs a spec: which agent, role boundary vs existing workers, model policy. Then: Mars agent → muse routing → staffing → GENERATED/MANUAL → validate.
2. **Add or extend a craft skill** — needs a spec: which craft area, what resources. Then: resource files → skill wiring → `--apply` → lint.
3. **Behavioral validation** — the §5F matrix is structural only; define eval prompts or a smoke-test story project that exercises muse routing, writer modes, and knowledge capture in a live harness.
4. **Map live session dispatch** — muse routing + `kb/` memory updates session-by-session.

---

## References

- `README.md` — product surface and install docs
- `AGENTS.md` — repo development guidance
- `docs/architecture.md` — agent/skill/artifact architecture
- `mars.toml` — package metadata and targets
- `scripts/sync_cw_skills.py` — GENERATED/MANUAL classification and lint
- `scripts/create_skill_zips.py` — Claude.ai `.skill` packaging
- `.github/workflows/ci.yml` / `release.yml` — validation and release gates
- `.githooks/pre-commit` — local cw guards
