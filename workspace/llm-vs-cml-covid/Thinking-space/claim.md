# Central claim — STAGE 1 (PROPOSED = C3, awaiting agreement)

Reconstructed from the intro gap + results only (the published Discussion was held aside).

## Candidates (initial)

**A — conservative / results-anchored**
> For COVID-19 mortality prediction from high-dimensional tabular data, classical ML
> (XGBoost/RF) outperforms zero-shot LLMs, while resource-efficient fine-tuning of a small
> LLM (Mistral-7b, QLoRA) closes most of the gap.

**B — bold / contingency thesis (matches the paper's published Conclusion)**
> Whether LLMs or classical ML win on clinical tabular data is not absolute but contingent on
> data dimensionality and training-data availability...

**C — calibrated middle (initial pick)**
> On high-dimensional clinical tabular data with adequate training samples, classical ML
> remains the stronger, more reliable choice than zero-shot LLMs, but parameter-efficient
> fine-tuning substantially narrows the gap and realigns a small LLM's feature reasoning
> toward clinically meaningful predictors.

## Stress-test (initial)
| Test | A | B | C |
|---|---|---|---|
| B1 one sentence | ✓ | ✓ (long) | ✓ |
| B3 gap = results = payoff | ✓ | ✓ strongest | ✓ |
| A3 conceptual advance | weak | **strong** | good |
| B15 strength matches design | ✓ | **RISK** (low-dim arm untested here) | ✓ |

## Round 1 revision (after `spine-alignment`; see log/handshake/claim-round1.md)
The reviewer flagged 6 issues on C: "narrows the gap" overstates near-closure; "adequate
training samples" is scope creep with no payoff; dimensionality is used as a label not a
mechanism (B3 loop open); the SHAP realignment is only qualitative in the paper, so it cannot
be a load-bearing claim clause; C otherwise beats B on calibration but must recover the
mechanism; and B1 — C was three payoffs in one sentence.

**C3 — recommended (clean single arc; SHAP demoted to the body):**
> On high-dimensional clinical tabular data, classical ML decisively outperforms zero-shot
> LLMs for COVID-19 mortality prediction — because high dimensionality and ample training data
> play to CML's strengths — while parameter-efficient fine-tuning of a small LLM narrows, but
> does not close, that gap.

**C2 — alternative (keeps a softened SHAP mention in the spine):**
> …that gap, and begins to realign the model's feature reasoning toward clinically meaningful
> predictors.

## Proposed: **C3**
- B1: one defensible sentence, single arc. B3: gap (high-dim, <12-feature lacuna) ↔ results
  (CML > zero-shot; fine-tuning 0.03→0.69 ext) ↔ payoff (dimensionality drives CML's edge;
  fine-tuning is the partial bridge) — loop closed. A3: dimensionality-as-driver framing is a
  conceptual advance, without B's untested low-dim arm. B15: every clause backed by this
  study's data. SHAP realignment → a supporting Discussion paragraph, not the spine.
