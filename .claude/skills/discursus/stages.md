# discursus — stage procedures

Each stage: do the work → run the stage's reviewers in parallel → synthesize a handshake →
resolve (accept/reject/defer) → apply fixes as diffs → append a decision + update STATE.
Honor the mode's gate (see SKILL.md). Think and stress-test before offering options.

---

## Stage 1 — Central claim  → `claim.md`
Goal: the paper's single central claim, as one sentence, with the narrative built around it.

1. Gather the spine inputs: the intro's gap statement, the main results, the aim. (Read
   `inputs/`; if absent, `NEED:` them.)
2. Draft 2–3 candidate one-sentence claims at different scopes (conservative → bold).
3. Stress-test each: Does it pass the one-sentence test (B1)? Does it match the
   intro's gap and the results (B3)? Is it a conceptual advance, not an increment (A3)?
   Is it defensible given the study design (B15)?
4. Reviewers (parallel): `spine-alignment`. (Claim stage is small; keep it light.)
5. Record the chosen claim, the alternatives, and *why* in `claim.md` + `decisions.md`.

**Gate:** the claim must pass B1 + B3 before the outline stage.

---

## Stage 2 — Reverse-outline  → `outline.md`
Goal: expand the claim into an ordered set of one-line section/paragraph takeaways that, read
alone, tell the whole story (B2).

1. Propose the paragraph sequence; write each as a single takeaway line + a 1-clause "what
   this paragraph does" note. Plan the opening (answers the gap, B7/B8) and the closing
   (what changes, B19).
2. For each paragraph, add a **"keep here / defer"** note so content has exactly one home
   (B5) — this seeds `placement-forward` later.
3. Reviewers (parallel): `spine-alignment`, `logic-sort`, `placement-forward`,
   plus a breadth check (B6) — read the takeaways alone; do they tell the story with no
   gaps or zig-zags?
4. Stress-test a better ordering (B4). Present the outline + any re-sort options.

**Gate:** the reverse-outline must pass B2 (story stands alone) + B4 (logical order) before
paragraph drafting.

---

## Stage 3 — Paragraph by paragraph  → `paragraphs/p<NN>-<slug>.md`
For each paragraph, in outline order:

1. **Design first.** Before prose, write the paragraph's plan: its one point (C1), its
   context-setting first sentence (C2), its takeaway last sentence (C3), and the old→new
   spine of the middle (C4). Invest in flow, smooth transitions, and plain, learnable
   English — the reader should glide through a neat background narrative.
2. **Draft** (or, in human-in-the-loop, take the user's draft).
3. **Parallel review** — run all seven: `redundancy-backward`, `placement-forward`,
   `concision`, `logic-sort`, `claim-evidence`, `spine-alignment`, `sciwriting-adherence`.
   Brief each with: the paragraph, the claim, the outline takeaway, and where inputs live.
4. **Handshake.** Collect findings into `log/handshake/p<NN>-round<n>.md`. Synthesize and
   de-duplicate. For each: ACCEPT (how) / REJECT (why) / DEFER (add the note to
   `outline.md` and say where it now lives).
5. **Fix** as diffs (never silent overwrite). If material changed, re-run the affected
   reviewers (new round) until clean.
6. **Dream-Quality gate.** Run `dream-quality-gate` on the revised paragraph. If it returns
   for revision, loop to step 4. On pass, mark the paragraph accepted in STATE.

`placement-forward` keeps the outline's "keep here / defer" notes current so later
paragraphs do not repeat earlier ones.

---

## Stage 4 — Whole-section gate  → updates STATE to `done`
After all paragraphs pass:

1. Assemble the full Discussion. Run `dream-quality-gate` at **section altitude**: does the
   opening answer the gap (B7/B8); does the closing answer "what changes" (B19); does it
   read as one narrative with no zig-zag (B5); is the contribution stated in one quotable
   sentence (B11); are limitations functional (B17)?
2. Run `redundancy-backward` once on the whole section vs intro/methods/results.
3. Record the final verdict and any residual `NEED:`s in `decisions.md`; set STATE to
   `done` (or list what remains).
