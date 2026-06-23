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
- Decision: PENDING author agreement. Revised the proposed claim to **C3** after spine-alignment
  raised 6 issues (all accepted): calibrated "narrows but does not close"; dropped the scope-
  creep qualifier "adequate training samples"; named dimensionality as the mechanism to close
  the B3 gap-loop; DEMOTED the qualitative SHAP realignment from the claim to a body paragraph;
  kept C over B (recovered the conceptual advance via the dimensionality mechanism); fixed B1
  (single arc).
- Decided by: claude proposed; author agreement pending (co-think gate).
- Artifacts: `claim.md` (Round 1 revision); `log/handshake/claim-round1.md`.
