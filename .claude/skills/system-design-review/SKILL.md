---
name: system-design-review
description: >
  Audit the design quality of a Claude Code system — individual skills, subagents,
  slash commands, CLAUDE.md, and multi-agent orchestration. Use before shipping a
  .claude setup or a new SKILL.md/agent, when reviewing a multi-agent workflow, or
  whenever the user asks for a "quality check", "design review", "audit", or
  "sanity check" of a Claude system, skill, or agent design.
allowed-tools: Read, Grep, Glob
---

# Claude system-design quality check

Audit a Claude Code system (or a single component) against the design rubric in
`rubric.md`. Be a demanding but fair reviewer: every finding must be actionable.

## How to run

1. **Establish the target and intent.** Identify what you are reviewing (a skill, an
   agent, a slash command, or a whole `.claude/` system) and the job it is meant to do.
   If the intent is unclear, ask one focused question before reviewing — you cannot
   judge "fit" without knowing the goal.

2. **Gather the artifacts.** Read the relevant files: `SKILL.md` bodies and their
   frontmatter, `.claude/agents/*.md`, `.claude/commands/*.md`, `settings.json`,
   `CLAUDE.md`, and any orchestration/handshake design notes. Use Glob/Grep to map the
   system before judging it.

3. **Score each dimension.** Read `rubric.md` and evaluate every dimension that applies.
   For each, assign a severity to anything wrong:
   - **Blocker** — will not work, or is unsafe (e.g. destructive tool auto-approved).
   - **Major** — works but will misfire, waste context, or break under the chosen control mode.
   - **Minor** — friction or smell; fix when convenient.
   - **Nit** — style/polish.
   Also note what is genuinely good, so strengths are preserved through edits.

4. **Verify, do not assume.** Quote the exact frontmatter/line you are judging. If a claim
   depends on a frontmatter field whose behavior is version-dependent, say so and
   recommend verifying it against the live docs rather than asserting it is wrong.

5. **Report** in the format below. Lead with the verdict so a reader gets the headline first.

## Output format

```
## Verdict: <Ship / Ship with fixes / Needs rework>
<2–3 sentence summary: does the design do its one job well and safely?>

## Scorecard
| Dimension | Rating | Headline |
|---|---|---|
| Single responsibility | ✅ / ⚠️ / ❌ | … |
| … (one row per applicable rubric dimension) | | |

## Findings
### [Blocker] <title>
- **Where:** <file:line or component>
- **Issue:** <what is wrong>
- **Why it matters:** <concrete consequence>
- **Fix:** <specific change>

### [Major] … (repeat, ordered by severity)

## Strengths
- <what to keep>

## Open questions
- <anything that needs the author's intent to resolve>
```

## Principles to apply (full rubric in `rubric.md`)

- Each component does **one** focused job at the right altitude.
- Descriptions are concrete and trigger-rich; siblings don't have overlapping triggers.
- The **right primitive** is used (skill vs subagent vs command vs hook).
- Tools are **least-privilege**; nothing destructive is auto-approved.
- **Progressive disclosure**: lean bodies, depth in bundled files.
- Orchestration fans out only **independent** work; parallel agents don't share files;
  agents are **fully briefed** (they don't inherit parent context); results are summaries.
- The system **fits its human-control mode** (automated / on-the-loop / in-the-loop /
  co-thinker) with real checkpoints and inspectable artifacts.
- It is **testable**, **idempotent**, and never silently overwrites human work.

When reviewing this repo's own discussion-writing system, pay special attention to the
human-control-mode fit and to handshake/inspectability, since those are its core promises.
