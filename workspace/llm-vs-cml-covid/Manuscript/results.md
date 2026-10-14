# Results (key numbers, from Table 3 + text)

## Classical ML (CML)
- Best: **RF and XGBoost.** Internal: RF F1 0.87 (acc 0.86, AUC 0.94); XGBoost F1 0.86
  (acc 0.87, AUC 0.95). External: RF F1 0.83 (AUC 0.91); XGBoost F1 0.82 (AUC 0.92).
- Only a **2–5% AUC drop** internal→external (good generalization). SVM/KNN/DT consistent
  across both sets.

## Zero-shot LLMs
- Best: **GPT-4** — acc 0.62, F1 0.43, recall 0.28. Most LLMs had very low recall (predicted
  almost everything as "mortality"); gpt-4o F1 0.01; BERT/ClinicalBERT F1 0.01 (degenerate,
  predicted one class).

## Fine-tuned Mistral-7b (QLoRA)
- F1 **0.03 → 0.74** internal, **0.69** external; recall **1% → 79%**; internal ≈ external
  (generalizes).

## Sample size & explainability
- All CMLs improve with training size; XGBoost best at every size; CMLs beat zero-shot LLMs
  with **< 100** training samples; small-sample fine-tuning showed "negative transfer."
- SHAP: CML feature impacts coherent (age, O2 saturation top); LLM impacts noisier;
  fine-tuning realigned Mistral's top-10 features toward XGBoost/clinician logic.
