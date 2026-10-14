# Published Discussion + Conclusion (reference target — what the authors actually wrote)
> Kept aside so we can compare discursus's reconstructed spine against the real one.
> Verbatim from the open-access article (figure call-outs removed).

Our study reveals a notable performance gap between CML models and LLMs in predicting patient
mortality via tabular data. RF and XGBoost emerged as the top CML performers, achieving over
80% accuracy and an F1 score of 0.86. In contrast, the best-performing LLM, GPT-4, achieved
62% accuracy and an F1 score of 0.43 in zero-shot classification. This disparity highlights
the challenges LLMs face when dealing with purely tabular data. Notably, increasing our sample
size from 5,000 patients in our previous study to 9,000 patients in this study significantly
improved the performance of CML models. The AUC of RF improved from 0.82 to 0.94, underscoring
the importance of large and diverse datasets in realizing the full potential of CMLs.

LLM performance heavily relies on the knowledge embedded within model weights, the complexity
of input data, and the table-to-text transformation technique. Our approach... achieved
results comparable to those of similar studies (F1 0.50–0.60). However, in line with many
previous studies, we found that CMLs can outperform this zero-shot performance with even fewer
than 100 training samples.

Given the performance gap, researchers have explored two approaches for improving LLMs:
pipeline improvements and fine-tuning. Previous studies show LLMs can close the gap via prompt
engineering, few-shot, multiple runs, a tree-based explainer, or novel text-to-table
transformation. However, many of their tasks may not resonate with real-world use (8–15
features, < 500 rare-class instances) that restrict CMLs from reaching maximum performance.

The alternative, fine-tuning, aims to modify model weights to teach a new task. We validated
this in our high-dimensional task, where fine-tuning Mistral increased the F1 from 0.03 to
0.69 even with a resource-efficient QLoRA method. Our SHAP analysis provides initial evidence
of an improved rationale after fine-tuning, as the top 10 features more closely align with
XGBoost and clinician decision-making.

Despite these advancements, LLMs still face limitations: vulnerability to hallucination, token
limits, data-privacy concerns (proprietary/cloud), and API cost that can disproportionately
impact low- and middle-income communities. Small pretrained models and rule-based systems are
resource-efficient alternatives, but our brief test of BERT/ClinicalBERT revealed their
limitations.

**Limitations.** The resource-efficient fine-tuning may not be the most effective; fine-tuning
for conversational rather than classification responses may reduce reliability (but mirrors
how clinicians interact with AI); the fine-tuned model was the smallest/lowest-performing LLM;
the transformation/prompts were simple; and the retrospective, single-resource-context design
(Iranian tertiary centers) necessitates prospective, multi-setting validation.

**Conclusion.** The efficacy of LLMs versus CML approaches in medical tasks appears to be
contingent upon data dimensionality and data availability. In low-dimensional scenarios with
limited samples, LLM-based methodologies may offer superior performance; however, as
dimensionality increases and diverse sample sizes become available, CML techniques tend to
outperform the zero-shot capabilities of LLMs. Notably, fine-tuning LLMs can substantially
enhance their pattern recognition and logical processing, potentially achieving performance
levels comparable to those of CMLs. The potential of LLMs to process both structured and
unstructured data may outweigh marginally lower performance metrics. Ultimately, the choice
should be guided by task complexity, data characteristics, and clinical context.
