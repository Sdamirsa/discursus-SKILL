# Methods (condensed)
- **Design:** retrospective; Tehran COVID-19 cohort; four tertiary centers; Mar 2020–May 2023
  (alpha/beta/delta/Omicron peaks).
- **Cohort:** 9,134 admitted COVID-19 patients (9,057 after exclusions; mortality 25.1%).
- **Features:** 81 on-admission features (demographics, symptoms, comorbidities, vitals, labs)
  → Lasso selection → top 40. Iterative/KNN imputation; standard-scaler normalization; SMOTE
  on the training set.
- **Validation:** internal = hospitals 1–3 (80/20 split; internal test n=2,470); external =
  Hospital-4 (n=2,248). Zero-shot subset n=590.
- **Models:** 7 CMLs (LR, SVM, DT, KNN, RF, MLP, XGBoost); 8 LLMs zero-shot (Mistral-7b,
  Mixtral-8×7b, Llama3-8b/70b, GPT-3.5T, GPT-4, GPT-4T, GPT-4o) + 2 LMs (BERT, ClinicalBERT)
  on table→text; Mistral-7b fine-tuned with QLoRA (4-bit). Training-size sweep (20→6118).
- **Explainability:** SHAP (global + granular); XGBoost as model-agnostic explainer.
