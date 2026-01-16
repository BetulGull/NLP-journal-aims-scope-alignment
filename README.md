# Analyzing Thematic Alignment in Scientific Journals (Machine Learning, 2016–2025)

This repository contains a reproducible, auditable pipeline to assess whether a journal’s published articles remain semantically consistent with its publisher-authored **Aims & Scope** statement over time. The analysis uses **Sentence-Transformers (all-mpnet-base-v2)** for alignment scoring and **BERTopic** for long-term topic evolution, with exported artefacts (figures, tables, CSVs) for reporting and auditability.

---

## Contents
- `outputs_p7/` – Main pipeline outputs (datasets, embeddings, figures, checkpoints)
- `figures/` – Figures used in the report (Fig1–Fig12)
- `tables/` – Exported CSV artefacts (e.g., Table 9, Table 10)
- `machine_learning_aims_scope_snapshot.txt` – Saved snapshot of the journal’s Aims & Scope text
- Notebooks / scripts – Data curation, scoring, drift/outlier analysis, topic evolution

> Folder names may vary slightly depending on your local run; the pipeline is designed around checkpointed outputs and re-runs without refetching.

---

## Overview

### 1) Data curation (Crossref → Semantic Scholar)
1. Harvest a high-precision DOI universe for **Machine Learning (Springer)** for **2016–2025** using Crossref.
2. Enrich records with abstracts using the Semantic Scholar Graph API (checkpointed & resumable).
3. Filter to keep only records with valid year and non-empty abstracts.
4. Export curated/scored datasets and year-wise coverage evidence.

### 2) Alignment scoring (Sentence-Transformers)
- Encode scope sentences and abstracts with `sentence-transformers/all-mpnet-base-v2`.
- Compute cosine similarity between each abstract and each scope sentence.
- Derive:
  - **max similarity** (best-match scope sentence)
  - **top-k mean** (default `k=3`) as the primary alignment score

### 3) Report findings (distribution & drift)
- Global distribution, per-year distributions, bootstrap confidence intervals.
- Coverage-aware reporting due to abstract-availability variation across years.
- Drift sensitivity checks and exported evidence tables.

### 4) Outlier analysis
- Define global outliers via a percentile rule (e.g., bottom 5%).
- Compute year-wise outlier rates and tail-aware trends.
- Export qualitative validation sheets for lowest-scoring papers.

### 5) Long-term conceptual evolution (BERTopic)
- Fit/reload BERTopic on the same retained corpus.
- Track topic prevalence by year (topic share heatmap + time series).
- Export human-readable topic labels and pivot tables for auditability.

---

## Reproducibility principles
- **API-first** dataset construction (no hidden prebuilt dataset).
- **Checkpointing** to avoid refetching and to preserve stable outputs.
- **Exported artefacts** (CSVs/figures) so results are traceable to explicit pipeline steps.

---

## Requirements
- Python 3.10+ recommended
- Key libraries (typical):
  - `numpy`, `pandas`, `requests`
  - `sentence-transformers`
  - `bertopic`
  - `scikit-learn`
  - `matplotlib`

Install example:
```bash
pip install -r requirements.txt
