# Reverse-outline — spine = Candidate B (contingency thesis) — REVISED (Stage-2 round 1)
> Read the takeaway column alone (B2). Order revised per logic-sort (B4): study-internal
> results stay contiguous (p01–p04); the literature arm (p05) is a broadening beat before
> limitations. Keep/defer notes carry placement-forward's sentence-level quarantines.

| id | takeaway (one line) | what it does | keep / defer (sentence-level) |
|----|---------------------|--------------|-------------------------------|
| p01 (opening) | Prior LLM-vs-CML comparisons confounded data regimes; on a high-dimensional, well-powered task, zero-shot LLMs largely fail as classifiers while classical ML reaches its ceiling. | C2 context (confounded prior work) → answers the intro gap (B7/B8) with the headline result | KEEP headline result only; name the near-degenerate zero-shot recall (not just "worse"). DEFER: sample-size numbers (5k→9k, AUC 0.82→0.94)→p02; low-dim/LLM-may-win contrast→p05; any SHAP/feature-reasoning mention→p04. |
| p02 | The gap is set by data regime: high dimensionality is the study's (fixed) design condition, and the sample-size sweep shows CML's edge grows with data — both favor CML over zero-shot LLMs. | the contingency thesis's TESTED arm | KEEP the sample-size sweep (5k→9k, AUC 0.82→0.94; CML beats zero-shot <100 samples) as the concrete evidence. Frame dimensionality as a DESIGN condition, not an internal sweep. QUARANTINE the "8–15 features / <500 rare-class" prior-work characterization → p05 only. |
| p03 | Resource-efficient QLoRA fine-tuning moves a small LLM from near-useless to within striking distance of CML — without matching it (F1 0.03→0.69 ext; recall 1%→79%; residual ~0.14). | the "approach, not match" half of the claim | KEEP numeric F1/recall gain only. EXCLUDE the SHAP sentence → p04. |
| p04 (supporting) | Fine-tuning also begins to realign the model's feature reasoning toward clinically meaningful predictors — a supporting, qualitative observation. | the demoted SHAP contribution; sets register for the broadening beat | KEEP SHAP here only; label "supporting observation," not a primary finding; no echo in p01/p03. |
| p05 (literature arm) | Prior work in low-dimensional, small-sample regimes shows LLMs can match or beat CML — consistent with, but not tested by, this study; the two literatures cohere through the data-regime lens. | completes the contingency thesis from the LITERATURE (B12; B15 mitigation) | KEEP ALL cross-study comparison + the "8–15 features / <500 rare-class" characterization here. Opening sentence shifts register internal→external. Verb preserves asymmetry ("consistent with, not tested by"). |
| p06 (limitations) | The bridge is partial and bounded. | functional limitations (B17) | KEEP: smallest/lowest LLM fine-tuned; conversational-not-classification objective; simple (un-ablated) transformation; retrospective single-resource-context → prospective multi-site needed. Do NOT quantify the transformation's effect (no ablation). |
| p07 (closing) | For high-dimensional structured prediction today, use CML; treat fine-tuned small LLMs as a fast-improving bridge whose value grows as fine-tuning/efficiency improve — choose by data regime, not hype. | answers "what changes" (B19) + one quotable sentence (B11) | CALLBACK rule: echo the contingency in ONE clause; do NOT re-argue the mechanism (p02/p05) or re-quantify fine-tuning ("comparable" is barred — claim says "approach, not match"). CUT "mixed structured+unstructured data" (untested scope creep) → "value grows as fine-tuning/efficiency improve." |

## B6 breadth check (orchestrator)
Takeaways read alone give a non-specialist a clean arc: problem+result → why (data regime) →
fine-tuning narrows it → reasoning realigns → prior literature agrees → limits → what changes.
**PASS.** (Prose should lightly gloss "zero-shot" and "fine-tuning.")

## Validation vs the published Discussion (held aside in Manuscript/)
- Published **opens on the raw performance gap and buries the contingency thesis in the
  Conclusion**; this outline promotes the thesis to p01–p02 + p05 → tighter spine (B7/B11).
- Published **pairs the high-dim result with the low-dim caveat in the Conclusion** (the B15
  risk); this outline quarantines the low-dim arm to p05, marked "not tested here."
- Published says fine-tuning may reach **"comparable"** performance; this outline holds
  **"approach, not match"** (residual ~0.14 F1).
- Published **embeds SHAP in the fine-tuning paragraph**; this outline gives it a demoted home (p04).
