# Handshake — claim — round 1 — 2026-06-23
Reviewers run: spine-alignment (Stage 1 is light; one reviewer)

## spine-alignment — does Candidate C work as the single spine? (B1/B3/A2)
1. ISSUE: "substantially narrows the gap" implies near-closure; residual F1 gap is ~0.13–0.14
   (RF 0.83 ext vs fine-tuned Mistral 0.69 ext). SEVERITY: major.
   RESPONSE: ACCEPT → "narrows, but does not close, that gap."
2. ISSUE: "adequate training samples" is a stated condition with no payoff — scope creep.
   SEVERITY: major. RESPONSE: ACCEPT (option a) → drop the qualifier; scope the claim to the
   study's fixed high-dimensional, well-powered design.
3. ISSUE: intro gap names "< 12 features"; claim uses "high-dimensional" only as a setting
   label, never as the mechanism — the gap-payoff loop doesn't close (B3). SEVERITY: major.
   RESPONSE: ACCEPT → add "because high dimensionality and ample data play to CML's strengths."
4. ISSUE: SHAP realignment is qualitative in the paper (no rank-correlation/overlap metric),
   yet the claim makes it a co-equal payoff. SEVERITY: blocker (conditional).
   RESPONSE: ACCEPT → confirmed qualitative in the full text; DEMOTE SHAP from the central
   claim to a supporting observation in the Discussion body.
5. ISSUE: prefer C over B, but C as written surrenders B's conceptual advance. SEVERITY: major.
   RESPONSE: ACCEPT → the mechanistic bridge in (3) recovers the advance without B's untested
   low-dimensional arm.
6. ISSUE: B1 — C is rhetorically three payoffs in one sentence. SEVERITY: major.
   RESPONSE: ACCEPT → with SHAP demoted (4), the claim becomes a single arc.

## Synthesis
- Applied: revised the proposed claim to **C3** (below); demoted SHAP to a body observation;
  named dimensionality as the mechanism; calibrated "narrows but does not close."
- Revised proposed claim **C3 (recommended):**
  "On high-dimensional clinical tabular data, classical ML decisively outperforms zero-shot
  LLMs for COVID-19 mortality prediction — because high dimensionality and ample training data
  play to CML's strengths — while parameter-efficient fine-tuning of a small LLM narrows, but
  does not close, that gap."
- Alternative **C2** (keeps a softened SHAP mention in the spine): C3 + ", and begins to
  realign the model's feature reasoning toward clinically meaningful predictors."
- Deferred: SHAP realignment → Discussion body paragraph (a supporting contribution).
- NEEDs raised: none.
- Decided by: pending author agreement (co-think gate).
