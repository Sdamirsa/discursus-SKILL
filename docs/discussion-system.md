# Discursus: the scientific-discussion writing system

Discursus co-writes and quality-checks a manuscript **Discussion** section through a staged
pipeline, with parallel reviewer agents and an inspectable decision log designed to survive
context limits and even a change of tool or model.

## Components

| Piece | Path | Role |
|---|---|---|
| **Orchestrator** | `.claude/skills/discursus/` | Drives the pipeline, enforces the log, coordinates agents. Entry: `/discursus` |
| **Quality rubric** | `.claude/skills/scientific-writing/` | The single source of truth for "good" (`final-qualities.md`) |
| **Reviewer agents** | `.claude/agents/*.md` | Seven parallel reviewers + one final quality gate |
| **System self-audit** | `.claude/skills/system-design-review/` | Audits this system's *design* (dogfood it after changes) |

## The pipeline

```
central claim → reverse-outline → (per paragraph: design → parallel review →
handshake → fixes → Dream-Quality gate) → whole-section gate
```

Each stage produces an artifact and a log entry. Reviewers run **in parallel**; the
orchestrator synthesizes their findings, you (or it, by mode) resolve each, and fixes are
applied as diffs.

## The reviewer roster

Parallel (paragraph stage):

1. **redundancy-backward** — repeats of intro/methods/results
2. **placement-forward** — overlap with *later* paragraphs; what to defer
3. **concision** — wordiness & repetition within the paragraph
4. **logic-sort** — flow, old→new, better ordering
5. **claim-evidence** — claims vs cited full text + our results; overclaim (uses PubMed/web)
6. **spine-alignment** — serves the one claim; intro-gap = results = payoff
7. **sciwriting-adherence** — the full Final Qualities rubric

Gate (after fixes): **dream-quality-gate** — holistic PASS / RETURN-FOR-REVISION, at
paragraph and whole-section altitude.

Each agent **pulls its own context** from the workspace (or returns `NEED: <thing>` if a
required input is missing) — agents never review blind, and never invent missing sources.

## Control modes

- **co-thinker** (default) — think together: the system proposes stress-tested options with
  a recommendation at each gate and proceeds only on mutual agreement.
- **human-on-the-loop** — runs each stage, pauses for your confirmation/notes at boundaries.
- **human-in-the-loop** — you supply each draft; the system reviews and restyles on top.
- **fully-automated** — runs end to end; stops only on a blocker or an unresolved `NEED:`.

## The ground log (inspectability + resume)

The workspace is plain markdown so a human can read it and any system can resume from it.

```
<manuscript_dir>/discursus/
├── STATE.md            # current snapshot — read FIRST to resume
├── claim.md            # central claim + alternatives + rationale
├── outline.md          # reverse-outline + keep/defer notes
├── inputs/             # intro/methods/results excerpts, cited papers, notes
├── paragraphs/         # one file per paragraph
└── log/
    ├── decisions.md    # APPEND-ONLY history — the durable ground log
    └── handshake/      # per paragraph, per round: issue → solution → response
```

- **STATE.md** answers "where are we now?" (overwritten each step).
- **log/decisions.md** answers "how did we get here, and who decided?" (append-only).
- **log/handshake/** holds the per-round reviewer detail.

To resume — in a new session, a new tool, or after a context limit — read `STATE.md`, then
the tail of `log/decisions.md`, then the current stage's artifact, and continue from
"Next action". Accepted work is never redrafted; human edits are never overwritten silently.

## Running it

**Locally (recommended for real manuscripts).** Run Claude Code where it can read your
manuscript folder, then invoke:

```
/discursus "<path-to-manuscript-folder>" co-thinker
```

The system creates `<folder>/discursus/` and works there, writing handshake and log files
beside your draft.

**In a cloud session** (no access to your local files): put inputs in a repo-local
`workspace/<slug>/inputs/` (or paste them), and the system writes the workspace there.

## Requirements & limits

- **claim-evidence** uses a PubMed MCP and/or web access. Paywalled, arXiv, or conference
  papers it cannot fetch become `NEED:` requests — supply the text/PDF in `inputs/`.
- The system **discusses** results; it never introduces new data (rubric B10).
- It calibrates claims to evidence and flags overclaim — it will push back on bold wording.
- It is a co-author and reviewer, not an oracle: the human owns the final decisions, and the
  log records who decided what.

## After changing the system

Run the self-audit: invoke `system-design-review` on `.claude/` to check the design against
the 13-dimension rubric (single-responsibility, tool scoping, orchestration soundness,
human-control-mode fit, inspectability, …).
