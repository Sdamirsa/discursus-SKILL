# discursus — workspace & the ground log

The workspace is the system's memory. It is plain markdown so a human can read it and any
other system (or a fresh agent after a context limit) can resume from it.

## Layout

```
<manuscript_dir>/discursus/        # or repo-local workspace/<slug>/ in a cloud session
├── STATE.md                       # current snapshot — read this FIRST to resume
├── claim.md                       # central claim + alternatives + rationale
├── outline.md                     # reverse-outline: per-paragraph takeaway + keep/defer note
├── inputs/                        # provided source: intro/methods/results excerpts, cited papers, notes
├── paragraphs/
│   └── p01-<slug>.md              # one file per paragraph (header = takeaway + status)
└── log/
    ├── decisions.md               # APPEND-ONLY history — the durable ground log
    └── handshake/
        └── p01-round1.md          # per paragraph, per round: issue → solution → response
```

**Roles:** `STATE.md` = where we are now (overwritten). `log/decisions.md` = why we got
here (append-only, never edited). `log/handshake/*` = the review detail. A resume needs
only STATE.md + decisions.md.

---

## Templates (copy these verbatim, then fill)

### STATE.md
```markdown
# discursus — STATE
> Resume: read this file → tail `log/decisions.md` → the current stage's artifact →
> continue from "Next action". Do not redo accepted work or overwrite human edits.

- Manuscript: <manuscript_dir>
- Mode: <co-thinker | human-on-the-loop | human-in-the-loop | fully-automated>
- Stage: <claim | outline | paragraph:p03 | whole-section | done>
- Updated: <ISO-8601 timestamp>

## Central claim
<current one-sentence claim, or TBD>

## Paragraphs
| id  | one-line takeaway                 | status                          |
|-----|-----------------------------------|---------------------------------|
| p01 | ...                               | planned/drafted/reviewed/accepted |

## Open NEEDs
- <unresolved requests for sections or full-text sources, or "none">

## Next action
<the single next step>
```

### decisions.md (append one block per gate)
```markdown
## [<ISO timestamp>] <stage> — <gate name>
- Decision: <what was decided>
- Decided by: <human | claude | both (co-think)>
- Options considered: <a; b; c>
- Why: <one- or two-line rationale>
- Artifacts: <files changed; handshake ref>
```

### handshake/p<NN>-round<n>.md
```markdown
# Handshake — p<NN> — round <n> — <timestamp>
Reviewers run: <comma-separated agent names>

## <agent-name> — <one-line focus>
- ISSUE: <what & where; quote the text>
- SOLUTION: <specific fix / rewrite>
- SEVERITY: <blocker | major | minor | nit>
- RESPONSE: <ACCEPT (how) | REJECT (why) | DEFER (note added to outline)>

<...one block per agent...>

## Synthesis
- Applied: <fixes applied this round>
- Deferred: <items + where they now live>
- NEEDs raised: <agent NEED: requests, or none>
- Decided by: <human | claude | both>
```

### claim.md
```markdown
# Central claim
**Claim (one sentence):** <...>

## Alternatives considered
1. <conservative scope> — <why not / when better>
2. <bolder scope> — <why not / when better>

## Rationale
- Passes one-sentence test (B1): <yes/note>
- Matches intro gap & results (B3): <note>
- Conceptual advance not increment (A3): <note>
- Defensible for study design (B15): <note>
```

### outline.md
```markdown
# Reverse-outline
> Read the takeaway column alone, top to bottom — it must tell the whole story (B2).

| id  | takeaway (one line) | what this paragraph does | keep here / defer note |
|-----|---------------------|--------------------------|------------------------|
| p01 (opening) | ... | answers the intro's gap | ... |
| ...  | ... | ... | ... |
| pNN (closing) | ... | states what changes | ... |
```

---

## Idempotency & safety
- Re-running is safe: STATE.md is the source of truth; accepted units are not redrafted.
- Human edits are sacred — propose changes as diffs and record them; never overwrite silently.
- One writer for shared files (the orchestrator). Parallel agents only return findings.
