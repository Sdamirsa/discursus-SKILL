# Installation Guide

## Prerequisites

- **Claude Code** (Anthropic's CLI for Claude) installed and configured.
- For the `claim-evidence` reviewer: a **PubMed MCP server** and/or web access (used to verify
  citations against the cited full text). Optional but recommended — without it, citation checks
  that need a source fall back to `NEED:` requests.

## Install as a Claude Code Plugin (Recommended)

### 1. Clone the repository

```bash
git clone https://github.com/Sdamirsa/discursus-SKILL.git
cd discursus-SKILL
```

### 2. Point Claude Code at the plugin

```bash
claude --plugin-dir /path/to/discursus-SKILL
```

Claude Code auto-discovers the skills in `skills/` and the agents in `agents/`. They appear in
Claude Code's skill/agent lists and trigger automatically based on your prompts.

### 3. Verify installation

Open Claude Code and say: "What discursus skills are available?" Claude should list the
`discursus`, `scientific-writing`, and `system-design-review` skills, plus the eight reviewer
agents.

## Install via the Plugin Marketplace

This repo is its own Claude Code marketplace. From inside Claude Code:

```
/plugin marketplace add Sdamirsa/discursus-SKILL
/plugin install discursus@discursus
```

Manage it later with `/plugin marketplace list`, `/plugin marketplace update discursus`, or
`/plugin marketplace remove discursus`.

## Install by Copying Skills and Agents

If you prefer to embed the system directly in one project:

```bash
cp -r discursus-SKILL/skills/* /path/to/your-project/.claude/skills/
cp -r discursus-SKILL/agents/* /path/to/your-project/.claude/agents/
```

This makes the system available only in that project. Note: installed this way,
`${CLAUDE_PLUGIN_ROOT}` is not set, so the orchestrator passes the scientific-writing rubric's
absolute path in each reviewer brief (the reviewers read the bar from the brief, not from a fixed
plugin path).

## PubMed / web access (for citation verification)

The `claim-evidence` agent verifies each cited claim against the source. It uses:

- a **PubMed MCP server** (preferred for biomedical literature), and/or
- **web access** to fetch open-access full text.

Configure a PubMed MCP server in your Claude Code MCP settings if you work with biomedical papers.
Paywalled, arXiv, or conference papers the agent cannot fetch are reported as `NEED:` requests —
supply the text/PDF in your `Literature/` folder so the reviewer can read it.

## Verify the Full Pipeline

1. Create a manuscript folder with `Manuscript/` (intro, methods, results, any draft) and
   `Literature/` (the cited papers).
2. Open Claude Code where it can read that folder.
3. Run: `/discursus "<path-to-manuscript-folder>" co-thinker`
4. The orchestrator should initialize `Thinking-space/` and start at the central-claim stage,
   pausing at the first gate.

See `workspace/llm-vs-cml-covid/` for a worked example layout.

## Troubleshooting

**Skills/agents not loading:** Verify each skill directory contains a `SKILL.md` with valid YAML
frontmatter (`name` and `description`), and each agent file in `agents/` has `name` + `description`
frontmatter.

**Plugin not recognized:** Ensure `.claude-plugin/plugin.json` exists and has a valid `name` field.

**Reviewer can't find the rubric:** When run as a plugin, the rubric resolves via
`${CLAUDE_PLUGIN_ROOT}/skills/scientific-writing/final-qualities.md`; when run from copied files,
the orchestrator must pass the rubric's absolute path in the reviewer brief.

**Citation checks return `NEED:`:** The source could not be fetched. Add the paper's text/PDF to
`Literature/`, or configure a PubMed MCP server / web access.
