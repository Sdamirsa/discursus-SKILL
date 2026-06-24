# Handshake — p01 (opening) — round 1 — 2026-06-23
Reviewers run (parallel): redundancy-backward, placement-forward, concision, logic-sort,
claim-evidence, spine-alignment, sciwriting-adherence

## claim-evidence (numbers vs results.md)
- F1 "around 0.86": minor — RF is 0.87 → ACCEPT "0.86–0.87".
- AUC 0.95 / GPT-4 0.43 / "largely failed": SUPPORTED (no change).
- "recall near zero": flagged BLOCKER, proposed "precision near zero".
  RESPONSE: **REJECT the proposed fix** — positive class is *survival*; degenerate LLMs
  predicted ~everyone "mortality", so recall(survival) ≈ 0.00–0.03 and precision ≈ 1.0. The
  reviewer misread the positive class; "precision near zero" would be WRONG. **ACCEPT the
  underlying ambiguity** → reword to the explicit failure mode ("predicting that almost every
  patient would die — almost never identifying survivors").
- Prior-work claim (sentence 1) uncited: major → ACCEPT (soften + defer citations to p05).

## spine-alignment (B7/B8/B3/A2)
- Closing states a flat verdict, not the contingency launch: BLOCKER → ACCEPT (new closing
  launches the thesis as "the first half… conditions the rest unpacks").
- Restore the dimensionality axis in the gap framing: major → PARTIAL — keep "high-dimensional"
  as the regime label; the two-part mechanism stays in p02 (reconciled with placement-forward).
- Numbers-before-thesis inversion: major → ACCEPT (answer-first reorder).
- External validation = minor scope creep here → ACCEPT (→p02; agrees with placement-forward).

## sciwriting-adherence (§C + B7/B8)
- B7 answer-first FAIL → ACCEPT (re-led with the result).
- C1 two jobs RISK → ACCEPT (critique subordinated to one clause).
- C6 unsupported field-critique FAIL → ACCEPT (soften + defer cites to p05).
- "stark" nit; B8 make gap-closure explicit RISK → ACCEPT (closing makes it explicit).
- C7 signal-to-noise PASS.

## redundancy-backward (A1/B9/B10)
- s1 restates the gap; s2 restates Methods; s4/s5 flat re-quote numbers: majors.
  RESPONSE: ACCEPT in part — cut the methods re-narration, fold numbers into an answer-first
  claim so they carry meaning. REJECT the call to add mechanism/clinical-meaning here (that is
  p02's job, per placement-forward) — the opening interprets only "the gap is real".

## placement-forward (B5 single-home)
- External validation → p02: BLOCKER → ACCEPT.
- Sample-size "reach its ceiling" mechanism → p02: major → ACCEPT.
- "at their best" hedge implies the low-dim arm: minor → ACCEPT (dropped).

## concision (C7) & logic-sort (C2/C3/C4)
- Delete "The contrast was stark." → ACCEPT.
- "head-to-head" redundant; tighten s1/s2 → ACCEPT.
- logic-sort: last sentence must ANSWER the opening question, not restate s5 (blocker) →
  ACCEPT (new closing does). LLM→CML reorder suggestion → NOT NEEDED after answer-first rewrite.

## Synthesis
- Applied: answer-first rewrite; deleted filler; moved external-validation + sample-size
  mechanism to p02; clarified the LLM failure mode (rejecting a wrong "precision" fix);
  0.86→0.86–0.87; closing now launches the contingency thesis; prior-work cites deferred to p05.
- Conflict adjudicated: redundancy-backward (more interpretation) vs placement-forward (defer
  mechanism) → resolved for placement-forward; the opening interprets only "the gap is real."
- False-positive caught: claim-evidence "recall→precision" — rejected against Table 3.
- NEEDs: none. Next: Dream-Quality gate on the revised paragraph.
- Decided by: claude synthesis (co-think; author may review).
