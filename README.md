# discursus-SKILL

**A co-writing and quality-checking system for the scientific manuscript *Discussion* section, built for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).**

discursus matures a Discussion through a staged pipeline — central claim → reverse-outline →
paragraph-by-paragraph — with seven parallel reviewer agents, a holistic quality gate, and an
inspectable decision log designed to survive context limits or even a change of tool or model.
It is beyond an assistant but below full automation: it does what it can and leaves the judgment
calls to you. Co-think mode is the recommended way to use it.

## What it does

### The pipeline

```
central claim → reverse-outline → (per paragraph: design → parallel review →
handshake → fixes → Dream-Quality gate) → whole-section gate
```

Each stage produces an artifact and a log entry. Reviewers run **in parallel**, each dispatched
with a self-contained brief (absolute paths + the unit pasted inline, since subagents inherit no
context); the orchestrator synthesizes their findings, you (or it, by mode) resolve each, and
fixes are applied as diffs. Fetched sources are treated as data to compare against, never as
instructions.

### Components

| Piece | Path | Role |
|-------|------|------|
| **Orchestrator** | `skills/discursus/` | Drives the pipeline, enforces the log, coordinates agents. Entry: `/discursus` |
| **Quality rubric** | `skills/scientific-writing/` | The single source of truth for "good" (`final-qualities.md`) |
| **Reviewer agents** | `agents/*.md` | Seven parallel reviewers + one holistic quality gate |
| **System self-audit** | `skills/system-design-review/` | Audits this system's *design* (dogfood it after changes) |
| **Worked example** | `skills/discursus/example-run.md`, `workspace/` | A filled claim → outline → paragraph, as a calibration anchor |

### The reviewer roster

Parallel (paragraph stage):

1. **redundancy-backward** — repeats of intro/methods/results
2. **placement-forward** — overlap with *later* paragraphs; what to defer
3. **concision** — wordiness & repetition within the paragraph
4. **logic-sort** — flow, old→new, better ordering
5. **claim-evidence** — claims vs cited full text + our results; overclaim (uses PubMed/web)
6. **spine-alignment** — serves the one claim; intro-gap = results = payoff
7. **sciwriting-adherence** — the full Final Qualities rubric

Gate (after fixes): **dream-quality-gate** — holistic PASS / RETURN-FOR-REVISION, at paragraph and
whole-section altitude. Each agent **pulls its own context** from the workspace (or returns
`NEED: <thing>` if a required input is missing) — agents never review blind, and never invent
missing sources.

## Installation

### Option A: As a Claude Code plugin (recommended)

```bash
# Clone or download this repo
git clone https://github.com/Sdamirsa/discursus-SKILL.git

# Point Claude Code at it
claude --plugin-dir /path/to/discursus-SKILL
```

Claude Code auto-discovers the skills in `skills/` and the agents in `agents/`.

### Option B: Copy skills and agents into your project

```bash
cp -r discursus-SKILL/skills/* your-project/.claude/skills/
cp -r discursus-SKILL/agents/* your-project/.claude/agents/
```

This makes the system available only in that specific project. (When installed this way rather than
as a plugin, `${CLAUDE_PLUGIN_ROOT}` is not set — the orchestrator passes the rubric's absolute path
in each reviewer brief, so reviews still resolve the rubric correctly.)

### Option C: Install via the plugin marketplace

This repo is its own Claude Code marketplace. From inside Claude Code:

```
/plugin marketplace add Sdamirsa/discursus-SKILL
/plugin install discursus@discursus
```

The first command registers the marketplace (`discursus`); the second installs the `discursus`
plugin from it. Manage it later with `/plugin marketplace list`, `/plugin marketplace update
discursus`, or `/plugin marketplace remove discursus`.

## Quick start

1. Put your manuscript in a folder with `Manuscript/` (intro, methods, results, and any Discussion
   draft) and `Literature/` (the cited papers as pdf/txt/md).
2. Open Claude Code where it can read that folder.
3. Run:

   ```
   /discursus "<path-to-manuscript-folder>" co-thinker
   ```

4. Work through the gates with the system; it writes every artifact and the log to `Thinking-space/`.

**In a cloud session** (no access to your local files): create a repo-local `workspace/<slug>/` with
`Manuscript/` + `Literature/` (or paste the inputs), and the system writes to
`workspace/<slug>/Thinking-space/`. See `workspace/llm-vs-cml-covid/` for a worked example.

## Control modes

- **co-thinker** (default) — think together: the system proposes stress-tested options with a
  recommendation at each gate and proceeds only on mutual agreement.
- **human-on-the-loop** — runs each stage, pauses for your confirmation/notes at boundaries.
- **human-in-the-loop** — you supply each draft; the system reviews and restyles on top.
- **fully-automated** — runs end to end; stops only on a blocker or an unresolved `NEED:`.

## The ground log (inspectability + resume)

The workspace is plain markdown so a human can read it and any system can resume from it.

```
<working_dir>/
├── Manuscript/         # YOU provide: the paper's own sections + any discussion draft
├── Literature/         # YOU provide: the cited papers (pdf/txt/md)
└── Thinking-space/     # CLAUDE owns: every artifact, the reasoning, and the log
    ├── STATE.md        # current snapshot — read FIRST to resume
    ├── claim.md        # central claim + alternatives + rationale
    ├── outline.md      # reverse-outline + keep/defer notes
    ├── paragraphs/     # one file per paragraph
    ├── scratch/        # option exploration & stress-tests (inspectable reasoning)
    └── log/
        ├── decisions.md  # APPEND-ONLY history — the durable ground log
        └── handshake/    # per paragraph, per round: issue → solution → response
```

To resume — in a new session, a new tool, or after a context limit — read `STATE.md`, then the tail
of `log/decisions.md`, then the current stage's artifact, and continue from "Next action". Accepted
work is never redrafted; human edits are never overwritten silently.

## Design principles

- **Human judgment owns the decisions.** The system is a co-author and reviewer, not an oracle; the
  log records who decided what.
- **One claim, one spine.** Every unit must serve the paper's single central claim; the intro gap,
  the results, and the discussion payoff describe the same gap.
- **Interpretation over restatement.** It discusses results; it never introduces new data (rubric B10).
- **Calibrated claims.** It matches language strength to evidence and pushes back on overclaim.
- **Inspectable & resumable.** Plain-markdown artifacts and an append-only decision log survive
  context limits and tool changes.
- **Isolated reviewers.** Agents inherit no context and get a self-contained brief; fetched sources
  are data to compare, never instructions.

## File structure

```
discursus-SKILL/
  .claude-plugin/
    plugin.json            # Plugin manifest
    marketplace.json       # Marketplace manifest (this repo is its own marketplace)
  skills/
    discursus/             # Orchestrator
      SKILL.md
      stages.md            # Stage procedures + the reviewer brief block
      workspace-and-log.md # Workspace layout + log templates
      example-run.md       # A filled walkthrough (calibration anchor)
    scientific-writing/    # Quality rubric
      SKILL.md
      final-qualities.md   # The bar (§A/§B/§C criteria)
    system-design-review/  # Design self-audit
      SKILL.md
      rubric.md
  agents/                  # Seven reviewers + the dream-quality gate
  docs/                    # User + architecture documentation
  workspace/               # A worked example (llm-vs-cml-covid)
  CLAUDE.md                # Claude Code project instructions
  README.md                # This file
```

## Requirements & limits

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI, desktop, web, or IDE).
- **claim-evidence** uses a PubMed MCP server and/or web access. Paywalled, arXiv, or conference
  papers it cannot fetch become `NEED:` requests — supply the text/PDF in `Literature/`.
- The system **discusses** results; it never introduces new data (rubric B10).
- It calibrates claims to evidence and flags overclaim — it will push back on bold wording.
- It is a co-author and reviewer, not an oracle: the human owns the final decisions, and the log
  records who decided what.

## After changing the system

Run the self-audit: invoke `system-design-review` on `skills/` + `agents/` to check the design
against the 13-dimension rubric (single-responsibility, tool scoping, orchestration soundness,
human-control-mode fit, inspectability, …).

## License

MIT

## Author

Seyed Amir Ahmad Safavi-Naini ([@sdamirsa](https://github.com/sdamirsa))
