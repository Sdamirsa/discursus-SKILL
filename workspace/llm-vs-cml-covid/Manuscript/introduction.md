# Introduction (condensed from the published paper)
Source: Ghaffarzadeh-Esfahani et al., *Sci Rep* 2025;15:42712. DOI 10.1038/s41598-025-26705-7.
Retrieved via PubMed/PMC (PMC12663554).

LLMs excel at unstructured-text tasks in medicine, while classical machine learning (CML) —
feature-based models trained on structured data — is the established tool for predicting
patient outcomes. Most historical clinical data are structured, yet the usual pipeline
(convert text → structured features → train CML) loses information and complicates deployment.

## Knowledge gap (verbatim)
"While the efficacy of LLMs in handling unstructured text is well documented, their
performance in handling structured data and their comparative effectiveness against CML
models remain a critical area of investigation." Prior comparisons "focus on tasks with a
limited number of features (< 12), fail to represent real-world medical decisions, and train
instances for the models (< 1000), limiting the CMLs to reach their maximum performance."

## Aim (verbatim)
"Our study aims to address this knowledge gap by evaluating LLMs' predictive capabilities in
the context of COVID-19 mortality prediction via a high-dimensional dataset and simple
table-to-text transformation. By utilizing a sufficient number of training instances, we
provide the opportunity for CMLs to reach their maximum performance, enabling a more robust
comparison with LLMs."
