# ML-Driven Metrics for Research Quality Evaluation

**Authors:** Benjamin Collins & Mo'tasim Mauthoor  
**Supervisors:** Thomas Brunner, PhD (McGill University) · Soud Al Kharusi, PhD (Stanford University)  
**Course:** PHYS 489 — McGill University Department of Physics

---

## Overview

This project explores whether machine learning-based text embeddings can produce meaningful metrics for evaluating academic research quality beyond traditional bibliometrics like the H-index. By embedding paper abstracts into vector space and computing pairwise cosine similarity, we develop two complementary analysis pipelines:

- **Author Self-Similarity** — measures the thematic consistency of an individual researcher's body of work
- **Field-Level Paper Comparison** — identifies outliers within a scientific field using K-nearest neighbor similarity

---

## Motivation

The exponential growth of scientific literature, driven in part by the "publish or perish" culture, has made it increasingly difficult to assess research quality using conventional metrics. The H-index, while widely used, favors high-volume fields, disadvantages early-career scientists, and fails to capture the conceptual depth or originality of a body of work. This project investigates whether semantic similarity derived from large language models can complement these metrics.

---

## Methods

### Data Collection
All data was retrieved from the [OpenAlex API](https://openalex.org/), an open-access bibliographic database. Two datasets were used:
- **1,195** McGill University Physics & Astronomy researchers (author-level analysis)
- **9,417** Nuclear and High Energy Physics papers (field-level analysis)

A small auxiliary dataset of **1,602** Astronomy & Astrophysics papers was used for model benchmarking.

### Embedding Models
Four candidate embedding models were evaluated on a benchmark set of 11 sentence pairs spanning identical, synonymous, antonymous, homonymous, and unrelated relationships:

| Model | Notes |
|---|---|
| `all-mpnet-base-v2` | Used for Self-Similarity analysis |
| `E5-Base-V2` | Used for field-level NHE analysis |
| `SPECTER` | Pre-trained on scientific citation graphs |
| `Embedding Gemma` | Accessed via Ollama |

### Pipelines

**Self-Similarity:** For each author with at least 3 valid abstracts, all unique abstract pairs are embedded and their cosine similarity is averaged to produce a single Self-Similarity score and standard deviation.

**Comparison by Field:** The K-nearest neighbor algorithm is applied to the NHE dataset using a configurable author-overlap threshold τ, which controls how much authorship overlap is tolerated when identifying nearest neighbors.

---

## Key Findings

- The distribution of author Self-Similarity across 1,195 McGill Physics researchers is well described by a normal distribution (µ = 0.418, σ = 0.181, p = 0.72).
- A **weak negative correlation** (r = −0.15) exists between average Self-Similarity and H-index, suggesting Self-Similarity captures a dimension of research output that conventional metrics do not.
- Self-Similarity is largely independent of citation count and publication count, supporting its use as a **complementary** rather than replacement metric.
- The field-level similarity decay curve shows promise as a **lightweight data quality screening tool**, capable of flagging potential plagiarism at one extreme and interdisciplinary or misclassified papers at the other.
- Data availability in OpenAlex introduces a confounding bias: authors with more abstracts indexed show a weaker Self-Similarity–H-index correlation, suggesting the observed correlations would diminish further with a more complete dataset.

---

## Interpreting Self-Similarity

Self-Similarity is inherently ambiguous as a standalone metric:

| High Self-Similarity | Low Self-Similarity |
|---|---|
| Deep domain expertise | Meaningful interdisciplinary breadth |
| Repetitive publication strategies | Early-career exploration |

For this reason, it is best used to **complement** existing metrics rather than replace them.

---

## Limitations

- Analysis restricted to **abstracts only** — full-text embeddings would provide richer signal
- MathML-encoded equations in abstracts were excluded, as current models cannot process this format
- OpenAlex does not guarantee complete or unique abstract coverage for all authors
- Results are specific to McGill Physics researchers and Nuclear/HEP papers — generalizability requires further study
- Models used are moderate-performance; higher-resourced alternatives like `SPECTER2` or `E5-large-v2` would improve embedding quality

---

## Future Work

- Embed full paper texts rather than abstracts only
- Extend analysis to multiple institutions and disciplines
- Adopt domain-specific models such as [SPECTER2](https://huggingface.co/allenai/specter2)
- Convert MathML expressions into model-interpretable formats prior to embedding

---

## Repository Structure

```
├── data/               # Collected author and paper datasets (CSV)
├── embeddings/         # Embedding generation scripts
├── analysis/           # Self-Similarity and field-level KNN pipelines
├── figures/            # Output plots and visualizations
└── README.md
```

---

## References

Key references for this project include the original H-index paper (Hirsch, 2005), the OpenAlex database (Priem et al., 2022), and the sentence-transformer framework underlying `all-mpnet-base-v2` (Reimers & Gurevych, 2019). See the full paper for a complete reference list.

---

## Citation

If you use this work, please cite:

```
Collins, B. & Mauthoor, M. (2026). Investigating Machine Learning Driven Metrics 
to Evaluate Research Quality. McGill Physical Journal, McGill University.
```
