# Building Claude Code Systems: The `.claude` Folder, Skills, Subagents & Orchestration

> A practical reference for designing multi-component Claude Code systems in this repo.
> Compiled June 2026 from the official docs (see [Sources](#sources)) and cross-checked
> against stable, observed behavior.
>
> **Honesty note on field accuracy.** `name` and `description` are the stable core of
> every skill/agent and behave as documented. The richer frontmatter fields below
> (e.g. `context: fork`, `effort`, `paths`, `memory`, `arguments`) are real in the
> mid-2026 docs but evolve quickly and differ across versions. Treat the tables as a
> menu to verify against the live docs linked at the bottom — and when you *ship* a
> skill, prefer the smallest set of fields that does the job, so it keeps working as
> the platform changes.

---

## 1. How to use this document

Claude Code lets you steer it with four composable primitives. Pick the lightest one
that fits:

| Primitive | What it is | Lives in | Reach for it when |
|---|---|---|---|
| **CLAUDE.md / rules** | Always-on instructions | `CLAUDE.md`, `.claude/rules/` | A standing convention should apply without being asked |
| **Skill** | An on-demand prompt + bundled files | `.claude/skills/<name>/SKILL.md` | A repeatable task/workflow needs a procedure and (optionally) helper files |
| **Subagent** | A specialized Claude in an isolated context | `.claude/agents/<name>.md` | Work is verbose, parallelizable, or needs restricted tools |
| **Hook** | A deterministic script at a lifecycle event | `.claude/settings.json` + scripts | Something must happen *every time*, enforced by the harness (not by Claude's judgment) |

The rest of this doc details each, then covers orchestration patterns and a decision guide.

---

## 2. The `.claude` folder map

```
your-project/
├── CLAUDE.md                      # Project instructions (committed)
├── CLAUDE.local.md                # Personal project instructions (gitignored)
├── .gitignore
└── .claude/
    ├── settings.json              # Project settings + hooks (committed)
    ├── settings.local.json        # Personal overrides (gitignored)
    ├── skills/
    │   └── <skill-name>/
    │       ├── SKILL.md            # Required: frontmatter + instructions
    │       ├── reference.md        # Optional: loaded on demand (progressive disclosure)
    │       ├── templates/…         # Optional: files the skill fills in
    │       └── scripts/…           # Optional: helper scripts the skill runs
    ├── agents/
    │   └── <agent-name>.md         # Subagent definition (frontmatter + system prompt)
    ├── commands/
    │   └── <name>.md               # Legacy slash command (now equivalent to a skill)
    ├── rules/
    │   └── <topic>.md              # Path-scoped standing instructions
    └── hooks/
        └── *.sh                    # Scripts referenced by settings.json hooks

~/.claude/                          # USER level — applies to all projects, never committed
├── settings.json
├── skills/  agents/  rules/        # Your personal, cross-project versions
```

**Committed vs ignored**

| Path | Committed? | Why |
|---|---|---|
| `CLAUDE.md`, `.claude/settings.json`, `.claude/skills/`, `.claude/agents/`, `.claude/rules/` | **Yes** | Team-shared behavior travels with the repo |
| `.claude/settings.local.json`, `CLAUDE.local.md`, `*-local/` | **No** | Personal/machine-specific; add to `.gitignore` |
| `~/.claude/**` | **Never** (different machine) | Your personal config across all projects |

**Precedence** (later wins): managed/enterprise → user (`~/.claude`) → project (`.claude`) → local (`.claude/*.local`) → CLI flags.

---

## 3. Skills

A skill is a folder with a required `SKILL.md`. The body is a prompt; bundled files are
loaded only when needed.

### Minimal, robust SKILL.md

```markdown
---
name: my-skill
description: >
  One or two sentences naming the concrete action and the trigger phrases a user
  would say. This is what Claude matches on to decide whether to run the skill.
---

# What this skill does
Concise, imperative instructions. State *what*, not *why*. Every line costs tokens.

For long material, point to a bundled file and let Claude read it on demand:
see `reference.md`.
```

### Frontmatter fields (verify advanced ones against live docs)

| Field | Stability | Purpose |
|---|---|---|
| `name` | **core** | Identifier; also the `/command` name. kebab-case. |
| `description` | **core** | When to use it. Be concrete; include trigger phrases. (~1–1.5k char budget shared with `when_to_use`.) |
| `allowed-tools` | stable | Pre-approve specific tools, e.g. `Read, Grep, Bash(git status)`. |
| `disable-model-invocation` | stable | `true` ⇒ only the user can invoke it (good for side-effecting actions). |
| `user-invocable` | stable | `false` ⇒ hide from the `/` menu (Claude-only). |
| `argument-hint` / `arguments` | stable | Autocomplete help; named args map to `$name` substitutions. |
| `model` | stable | `sonnet` / `opus` / `haiku` / `inherit`. |
| `disallowed-tools` | evolving | Remove tools for the skill's duration. |
| `context: fork` + `agent:` | evolving | Run the skill in an isolated/forked subagent context. |
| `effort` | evolving | `low…max` reasoning level. |
| `paths` | evolving | Only surface the skill when working on matching files. |
| `hooks` | evolving | Lifecycle hooks scoped to this skill. |

### Progressive disclosure (the key mental model)

1. **At session start** Claude sees only each skill's `name` + `description`.
2. **On invocation** the full `SKILL.md` body loads into context and stays.
3. **Bundled files** (`reference.md`, `templates/`, `scripts/`) load **only** when Claude
   reads or runs them.

Consequence: keep `SKILL.md` lean (rule of thumb < ~500 lines) and push depth into
reference files. Lean skills survive context compaction better.

### Invocation

- `/my-skill [args]` — explicit.
- Automatic — Claude matches the `description` to the situation.
- `@my-skill` — force it.

### Skill quality checklist

- One skill = one focused job (if the description needs "and … and …", split it).
- Description states a concrete action + the words a user would actually say.
- Tools scoped to least privilege; side-effecting skills set `disable-model-invocation: true`.
- Heavy detail lives in bundled files, not the body.

---

## 4. Subagents

A subagent is a specialized Claude that runs in its **own context window**. Its verbose
work (logs, searches, fetched pages) stays isolated; only its final message returns to
the caller. Multiple subagents can run in **parallel**.

### Definition (`.claude/agents/<name>.md`)

```markdown
---
name: claim-checker
description: >
  Verify that factual/citation claims in a manuscript paragraph are supported by the
  cited sources. Use when checking a discussion paragraph's claims against full text.
tools: Read, Grep, Glob, WebFetch
model: sonnet
---

You are a meticulous scientific fact-checker. For each claim in the provided text:
1. Identify the supporting citation (or note it is unsupported).
2. Compare the claim to the source.
3. Report: claim → verdict (supported / overstated / unsupported) → evidence → fix.
Return a concise table. Do not introduce new claims.
```

### Frontmatter fields

| Field | Required | Purpose |
|---|---|---|
| `name` | **yes** | Identifier; kebab-case. |
| `description` | **yes** | When the orchestrator should delegate here. |
| `tools` | no | Whitelist; inherits all if omitted. Supports MCP tools (`mcp__server__*`). |
| `model` | no | `sonnet` / `opus` / `haiku` / `inherit`. |
| `disallowedTools`, `permissionMode`, `maxTurns`, `skills`, `memory`, `isolation: worktree`, `effort`, `color`, `background` | no | Advanced controls — verify against live docs before relying on them. |

### What a subagent sees / doesn't see

- **Sees:** its own system prompt (the body), the delegation task, CLAUDE.md/memory,
  any skills named in `skills:`.
- **Does NOT see:** the parent's conversation history, files the parent already read, or
  earlier tool outputs. ⇒ The orchestrator must pass everything the subagent needs *in the
  task prompt*.

This is the single most common multi-agent bug: assuming a subagent inherits context it
does not. Brief it fully.

---

## 5. Slash commands

Custom commands have effectively merged into skills. A file at `.claude/commands/foo.md`
behaves like `.claude/skills/foo/SKILL.md` and supports the same frontmatter and
`$ARGUMENTS` / `$1` / `$name` placeholders. **Prefer skills for new work.**

---

## 6. Orchestration patterns

| Pattern | Shape | Use when |
|---|---|---|
| **Parallel fan-out** | Orchestrator → N independent subagents → synthesize | Tasks are independent (e.g. several reviewers auditing the same paragraph on different axes) |
| **Sequential pipeline** | A → B → C, each output feeds the next | There is real data flow (review → fix → verify) |
| **Forked skill** | Skill runs in an isolated/forked context | A workflow needs isolation but should inherit the conversation |
| **Skill as orchestrator** | A skill's body spawns & coordinates subagents | You want a repeatable, named multi-agent workflow |
| **Nested** | Subagent spawns its own subagents | Hierarchical decomposition (keep depth ≤ 2–3; max 5) |

### Making fan-out safe

- **Independence:** parallel agents must not depend on each other's output, or write to
  the same file. If they must share, write to *separate* files and let the orchestrator merge.
- **Full briefing:** each agent gets everything it needs in its prompt (subagents don't
  share parent context).
- **Summaries, not dumps:** instruct each agent to return a compact, structured result;
  the orchestrator synthesizes. This is what saves context.
- **Handshake artifacts:** for auditable workflows, have each agent's findings written to a
  versioned file (`issue → fix → status`), then have the orchestrator record accept/reject
  decisions. This makes a multi-agent review inspectable by a human afterward.

---

## 7. Hooks & settings

Hooks run deterministic logic at lifecycle events — enforced by the harness, not left to
Claude's judgment. Configure them in `settings.json`.

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": ["Bash(npm run test)", "Skill(system-design-review)"],
    "deny": ["Bash(curl *)", "Read(./.env)"]
  },
  "hooks": {
    "SessionStart": [
      { "hooks": [{ "type": "command", "command": ".claude/hooks/setup.sh" }] }
    ],
    "PostToolUse": [
      { "matcher": "Write|Edit",
        "hooks": [{ "type": "command", "command": "npm run lint" }] }
    ]
  }
}
```

Common events: `SessionStart`, `UserPromptSubmit`, `PreToolUse` (can block, exit 2),
`PostToolUse`, `Stop`, `SubagentStop`. A `command` hook receives JSON on stdin; exit `2`
blocks on blocking events, `0` continues.

---

## 8. Decision guide: which primitive?

```
Need it to apply ALWAYS, without being asked?          → CLAUDE.md / rules
Need a repeatable procedure the user/Claude invokes?   → Skill
  …with side effects (deploy, push, overwrite)?        → Skill + disable-model-invocation
Output is verbose, or you want parallelism/isolation?  → Subagent
Several independent analyses of the same input?        → Orchestrator + parallel subagents
Must happen deterministically every time, enforced?    → Hook
```

---

## 9. Best-practices checklist

**Skills** — focused; concrete trigger-rich description; least-privilege tools; depth in
bundled files; body lean.
**Subagents** — narrow specialists; vague "helper" agents delegate poorly; restrict tools
(read-only reviewers need no `Write`); brief them fully (no inherited context).
**Orchestration** — parallelize only independent work; separate files to avoid write races;
summaries not dumps; inspectable handshake artifacts for human review.
**CLAUDE.md** — under ~200 lines; specific over vague; no contradictory rules.
**Safety** — never auto-approve destructive tools; treat external/fetched content as
untrusted; require human confirmation for outward/irreversible actions.

---

## Sources

Official documentation (code.claude.com/docs, June 2026):

- How Claude Code works — https://code.claude.com/docs/en/how-claude-code-works
- Skills — https://code.claude.com/docs/en/skills
- Subagents — https://code.claude.com/docs/en/sub-agents
- The `.claude` directory — https://code.claude.com/docs/en/claude-directory
- Memory & CLAUDE.md — https://code.claude.com/docs/en/memory
- Hooks reference — https://code.claude.com/docs/en/hooks
- Settings — https://code.claude.com/docs/en/settings
- Commands — https://code.claude.com/docs/en/commands
- Agent teams — https://code.claude.com/docs/en/agent-teams

Community guidance (2026): Anthropic blog "Steering Claude Code"
(https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more);
practical subagent/skill guides.

> Re-verify advanced frontmatter fields against the live docs before depending on them.
