# Handshake — outline — round 1 — 2026-06-23
Reviewers run: spine-alignment, logic-sort, placement-forward (parallel)

## logic-sort (B4 order, C2/C3)
- ISSUE: p03 literature arm interleaved between p02 and p04 → zig-zag in the study's own chain.
  SEVERITY: major. RESPONSE: ACCEPT → reorder to p01,p02,p04,p05,p03,p06,p07 (renumbered so
  study-internal results p01–p04 are contiguous; literature arm becomes p05).
- ISSUE: p07 "choose by data regime" loses its antecedent mid-chain. major.
  RESPONSE: ACCEPT (resolved by the reorder; p05 now precedes limitations/closing).
- ISSUE: p01 lacks a context-setting first sentence (C2). major. RESPONSE: ACCEPT → p01 opens
  on the confounded-prior-literature context, then the result.
- ISSUE: p05→(lit) transition needs a register-shift topic sentence. minor. RESPONSE: ACCEPT
  → recorded in the literature paragraph's keep note.

## spine-alignment (B1/B3/B15 conditions)
- ISSUE: p01 "clearly outperforms" understates severity (most zero-shot LLMs near-degenerate,
  F1 0.01). major. RESPONSE: ACCEPT → "largely fail as classifiers."
- ISSUE: p02 implies dimensionality was experimentally SWEPT; only sample size was. major.
  RESPONSE: ACCEPT → frame dimensionality as the fixed design condition; data availability is
  the swept factor; the dimensionality arm is argued by design + the p05 literature contrast.
- ISSUE: p05 "the two literatures agree" elevates the cross-study arm to joint confirmation.
  major. RESPONSE: ACCEPT → "consistent with, but not tested by, this study."
- ISSUE: p07 "value grows with mixed structured+unstructured data" = untested scope creep.
  major. RESPONSE: ACCEPT → cut; replace with within-design outlook.
- ISSUE: intro's "simple table-to-text transformation" methodological promise is unclosed;
  don't quantify transformation effect (no ablation). minor. RESPONSE: ACCEPT → note in p06.
- p03 "approach, not match" and p04 SHAP-as-supporting: confirmed correct (nit).

## placement-forward (B5 single-home; read published-discussion.md to find drafting traps)
- BLOCKER: sample-size sentence (5k→9k, AUC 0.82→0.94) must live in p02, not p01 (else p02
  circular). RESPONSE: ACCEPT → explicit defer note on p01 + keep note on p02.
- BLOCKER: "8–15 features / <500 rare-class" prior-work characterization → p05 only, not p02.
  RESPONSE: ACCEPT → quarantine note on p02.
- BLOCKER: SHAP sentence → p04 exclusively; p03 closes on numeric gain. RESPONSE: ACCEPT.
- MAJOR: low-dim contrast must not appear in p01 even in passing; SHAP must not echo in p01.
  RESPONSE: ACCEPT → exclusions added to p01.
- MAJOR: p07 must not re-quantify fine-tuning ("comparable" barred) nor re-argue the mechanism;
  one-clause callback only. RESPONSE: ACCEPT → callback rule on p07.

## Synthesis
- Applied: REORDERED to p01,p02,p03(fine-tune),p04(SHAP),p05(literature),p06,p07; sharpened
  every takeaway; folded all sentence-level quarantines into keep/defer notes.
- B6 breadth check: PASS (see outline.md).
- Deferred to Stage 3 (drafting): glossing "zero-shot"/"fine-tuning"; the register-shift topic
  sentence opening p05; closing the transformation-as-design-choice loop in p06.
- NEEDs raised: none.
- Decided by: claude synthesis; author gate pending on the revised outline.
