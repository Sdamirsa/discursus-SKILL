# discursus — decision log (append-only)

## [2026-06-23] init — live validation on a published paper
- Decision: validate the discursus spine (Stage 1 claim + Stage 2 outline) on Ghaffarzadeh-
  Esfahani et al., Sci Rep 2025;15:42712 (DOI 10.1038/s41598-025-26705-7) — the author's own
  open-access paper, so the reconstructed spine can be checked against the real Discussion.
- Decided by: both (co-think)
- Inputs: intro/methods/results condensed into `Manuscript/`; full text via PubMed/PMC
  (PMC12663554). Published Discussion held aside in `Manuscript/published-discussion.md`.

## [2026-06-23] stage 1 — central claim (candidates)
- Decision: three candidates drafted (A conservative, B published contingency thesis, C
  calibrated middle). Initial pick C.
- Options/why: A weak on A3; B has a B15 risk (low-dim arm untested here); C is a conceptual
  point backed by this study's data.
- Artifacts: `claim.md`.

## [2026-06-23] stage 1 — review round 1 (spine-alignment) → revised claim
- Decision: revised the proposed claim to C3 after spine-alignment raised 6 issues (all
  accepted): calibrated "narrows but does not close"; dropped scope-creep qualifier; named
  dimensionality as the mechanism; DEMOTED qualitative SHAP from the claim; kept C over B;
  fixed B1.
- Decided by: claude proposed; author agreement pending.
- Artifacts: `claim.md`; `log/handshake/claim-round1.md`.

## [2026-06-23] stage 1 — claim LOCKED = B (author decision)
- Decision: author locked **Candidate B** (contingency thesis), matching their publication.
- Conditions carried into the outline: (1) low-dim arm **attributed to prior literature**
  (B15 mitigation); (2) SHAP realignment stays a **qualitative supporting** paragraph; (3)
  fine-tuning phrased **"approach, not match."**
- Decided by: both (co-think).
- Artifacts: `claim.md` (LOCKED); `outline.md`.

## [2026-06-23] stage 2 — reverse-outline drafted; reviewers dispatched
- Decision: 7-paragraph reverse-outline on B; promotes the contingency thesis to p01–p03 (the
  published paper buried it in the conclusion). Dispatched spine-alignment + logic-sort +
  placement-forward.
- Decided by: claude (pending review synthesis + author gate).
- Artifacts: `outline.md`; handshake to follow.

## [2026-06-23] stage 2 — review round 1 synthesis (3 reviewers) → revised outline
- Decision: REORDERED the outline to keep study-internal results contiguous (p01,p02,
  fine-tuning,SHAP, then the literature arm), and sharpened every takeaway. PENDING author gate.
- Options/why: logic-sort found a zig-zag (literature arm splitting the study's own chain) →
  accepted its reorder; spine-alignment caught a dimensionality-was-swept overclaim and an
  untested mixed-modality scope creep in the closing → both fixed; placement-forward's
  sentence-level quarantines (sample-size→p02, low-dim chars→p05, SHAP→p04) folded into
  keep/defer notes. B6 breadth check PASS.
- Validation finding: vs the held-aside published Discussion, the reconstructed spine reaches
  the SAME thesis but is better sequenced (thesis promoted, not buried in the conclusion) and
  better calibrated (low-dim arm quarantined to literature; "approach, not match" instead of
  "comparable"; SHAP given a demoted home).
- Decided by: claude synthesis; author agreement pending.
- Artifacts: `outline.md` (revised); `log/handshake/outline-round1.md`.
