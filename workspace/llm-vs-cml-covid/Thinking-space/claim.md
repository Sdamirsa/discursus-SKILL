# Central claim — STAGE 1 (LOCKED = B)

## LOCKED claim (author decision, co-think)
> Whether LLMs or classical ML win on clinical tabular data is contingent on data
> dimensionality and training-data availability: with high dimensionality and ample data,
> classical ML beats zero-shot LLMs, while resource-efficient fine-tuning lets a small LLM
> approach (not match) classical-ML performance.

### Conditions carried into the outline (B15 mitigation from spine-alignment round 1)
1. The **low-dimensional arm** ("LLMs may win when features/samples are few") is **attributed
   to prior literature** (TabLLM, MediTab, EHR-CoAgent, XAI4LLM) — it is *not* tested by this
   study. It gets its own paragraph, clearly marked as cross-study.
2. **SHAP realignment** is **qualitative** in this paper → a supporting body paragraph, never
   a load-bearing clause in the opening/claim.
3. Fine-tuning is phrased "**approach, not match**" (residual F1 gap ~0.13–0.14).

## Candidates considered (for the record)
- **A** conservative/results-anchored — weak on A3.
- **B** contingency thesis — chosen; strongest A3 + matches the publication; B15 risk mitigated
  structurally (condition 1).
- **C / C3** calibrated middle — high-dim-only spine; not chosen, but its discipline
  (conditions 2–3) is folded into B.

See `log/handshake/claim-round1.md` for the spine-alignment round that produced these
conditions.
