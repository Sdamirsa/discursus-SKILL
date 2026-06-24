# CLAUDE.md -- discursus Plugin

discursus is a Claude Code plugin that co-writes and quality-checks a scientific manuscript
**Discussion** section end to end, with parallel reviewer agents and an inspectable decision
log that survives context limits or even a change of tool or model.

## What this repo is

A **Claude Code plugin** containing 3 skills and 8 agents.

### Skills
| Skill | Role |
|-------|------|
| `discursus` | Orchestrator. Drives the pipeline (claim -> outline -> paragraphs -> gates), enforces the ground log, dispatches reviewers. Entry: `/discursus` |
| `scientific-writing` | The quality rubric -- single source of truth for "good" (`final-qualities.md`) |
| `system-design-review` | Audits the *design* of this (or any) Claude system; dogfood it after changes |

### Agents (seven reviewers + one gate)
| Agent | Checks |
|-------|--------|
| `redundancy-backward` | Repeats of intro/methods/results |
| `placement-forward` | Overlap with *later* paragraphs; what to defer |
| `concision` | Wordiness & repetition within the paragraph |
| `logic-sort` | Flow, old->new, better ordering |
| `claim-evidence` | Claims vs cited full text + our results; overclaim (PubMed/web) |
| `spine-alignment` | Serves the one claim; intro-gap = results = payoff |
| `sciwriting-adherence` | The full Final Qualities rubric |
| `dream-quality-gate` | Holistic PASS / RETURN-FOR-REVISION, at paragraph and section altitude |

## How to install

```bash
# Option A: as a plugin (recommended)
claude --plugin-dir /path/to/discursus-SKILL

# Option B: copy into your project's .claude/ folder
cp -r discursus-SKILL/skills/* your-project/.claude/skills/
cp -r discursus-SKILL/agents/* your-project/.claude/agents/
```

Or install from the marketplace -- see `README.md`.

## How to use

```
/discursus "<path-to-manuscript-folder>" co-thinker
```

The folder holds `Manuscript/` (the paper's own sections + any draft) and `Literature/` (the
cited papers). The system reads those and writes every artifact and the log to `Thinking-space/`.
To resume in a fresh session, read `Thinking-space/STATE.md`, then the tail of
`Thinking-space/log/decisions.md`, then the current stage's artifact.

Modes: `co-thinker` (default) | `human-on-the-loop` | `human-in-the-loop` | `fully-automated`.

## Conventions

- The orchestrator coordinates; **agents** analyze in isolated contexts with no shared working
  directory, so every reviewer is dispatched with a self-contained brief (absolute paths + the
  unit pasted inline). See `skills/discursus/stages.md` for the brief block.
- References to bundled files use `${CLAUDE_PLUGIN_ROOT}` so they resolve wherever the plugin is
  installed (e.g. the rubric at `${CLAUDE_PLUGIN_ROOT}/skills/scientific-writing/final-qualities.md`).
- The `scientific-writing` rubric (`final-qualities.md`) is the single bar for quality.
- The system **discusses** results; it never introduces new data, never edits `Manuscript/` or
  `Literature/`, and never invents sources -- missing material surfaces as a `NEED:` request.
- The ground log under `Thinking-space/` is plain markdown so any human or system can resume from it.
- After changing the system, run `system-design-review` on `skills/` + `agents/` to re-audit the design.

## Requirements

- The `claim-evidence` reviewer uses a **PubMed MCP server** and/or web access to verify citations
  against the cited full text. Paywalled, arXiv, or conference papers it cannot fetch become `NEED:`
  requests -- supply the text/PDF in `Literature/`.

## File structure

```
discursus-SKILL/
  .claude-plugin/
    plugin.json            # Plugin manifest
    marketplace.json       # Marketplace manifest (this repo is its own marketplace)
  skills/                  # 3 skills (auto-discovered by Claude Code)
    discursus/             # Orchestrator (+ stages.md, workspace-and-log.md, example-run.md)
    scientific-writing/    # Quality rubric (final-qualities.md)
    system-design-review/  # Design self-audit (rubric.md)
  agents/                  # 8 reviewer/gate agents (auto-discovered)
  docs/                    # User + architecture documentation
  workspace/               # A worked example manuscript and Thinking-space
  CLAUDE.md                # This file
  README.md                # GitHub-facing documentation
```
