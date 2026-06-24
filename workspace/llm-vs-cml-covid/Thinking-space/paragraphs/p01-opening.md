# p01 — opening
takeaway: on a high-dimensional, well-powered task, zero-shot LLMs largely fail while classical
ML reaches its ceiling — answering whether the prior LLM-vs-CML gap is real. status: ACCEPTED (Dream-Quality PASS, round 1)

## Design (before prose)
- **One point (C1):** the high-dimensional, well-powered head-to-head shows classical ML
  succeeds where zero-shot LLMs fail — the headline that answers the intro's gap.
- **First sentence (C2 / B7 answer-first):** lead with the result, subordinating the prior-work
  critique to one clause.
- **Last sentence (C3):** launches the contingency thesis (this holds in the data-rich,
  high-dimensional regime; the rest of the Discussion unpacks the conditions).
- **Middle old→new (C4):** answer → supporting numbers (CML vs zero-shot) → regime-conditioned
  takeaway.
- **Quarantines:** sample-size mechanism → p02; external validation → p02; low-dim/LLM-may-win
  arm + prior-work citations → p05; SHAP → p04; fine-tuning → p03.

## Draft — round 1 (after 7-reviewer handshake)
On a high-dimensional, well-powered clinical task — predicting in-hospital COVID-19 mortality
from routinely collected admission data — classical machine learning decisively outperformed
zero-shot large language models, resolving a comparison that earlier, smaller studies had left
ambiguous. Random forest and XGBoost reached F1 scores of 0.86–0.87 (AUC up to 0.95).
Zero-shot LLMs, by contrast, largely failed as classifiers: the best, GPT-4, managed only an F1
of 0.43, while most collapsed to predicting that almost every patient would die — almost never
identifying survivors. That a general-purpose model reading the same data as text cannot
compete in this data-rich, high-dimensional regime is the first half of our central message,
one whose conditions the rest of this Discussion unpacks.

## What changed from round 0 (and why)
- Re-led **answer-first** (B7 fail → fix; sciwriting/spine): result first, prior-work critique
  cut to one subordinate clause (C1 two-jobs → one).
- **Deleted "The contrast was stark."** (concision + logic-sort + sciwriting: filler).
- **Moved external validation → p02** (placement-forward blocker) and the **sample-size
  "reach its ceiling" mechanism → p02** (placement-forward major); kept "high-dimensional" to
  name the regime axis without explaining the mechanism.
- **Closing now launches the contingency thesis** (spine-alignment blocker) — "first half of
  our central message… conditions the rest unpacks" — without importing the low-dim arm (→p05).
- **"recall near zero" clarified** to the actual failure mode ("predicting that almost every
  patient would die — almost never identifying survivors"): REJECTED claim-evidence's
  "precision near zero" fix (factually wrong — positive class is survival; precision was ~1.0),
  but fixed the ambiguity it surfaced.
- **F1 "around 0.86" → "0.86–0.87"** (claim-evidence: RF 0.87, XGBoost 0.86).
- Prior-work claim softened + citations **deferred to p05** (where the cited characterization
  lives), resolving the C6/claim-evidence "uncited assertion" flag for the opening.

## Dream-Quality gate — round 1: PASS
- §C all PASS; B7/B8 PASS; launches claim B without poaching later paragraphs.
- Residual (non-blocking): (1) "reading the same data as text" lightly gestures at mechanism —
  acceptable; keep p02 from re-stating it (faint B5 echo to watch). (2) Cross-paragraph AUC:
  "up to 0.95" here = XGBoost internal; p02's "0.94" = RF — reconciled, not a drift.
- p01 ACCEPTED.

