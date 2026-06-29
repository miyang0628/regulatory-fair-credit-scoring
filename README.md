# RCFS: Regulatory-Constrained Fairness Framework for Credit Scoring

Reproducibility repository for the paper
**"A Regulatory-Constrained Fairness Framework for Credit Scoring: Aligning
Algorithmic Fairness with the Korean AI Basic Act and the EU AI Act."**

This repository contains the Jupyter notebooks, supporting code, and configuration
needed to reproduce the tables and figures in the paper. Notebooks are committed
**with their rendered outputs** so results can be inspected on GitHub without
re-running the full pipeline.

---

## Repository structure

```
rcfs-credit-fairness/
├── data/                       # Datasets (git-ignored; see notebook 01)
│   ├── raw/                    # Downloaded source files
│   └── processed/              # Cleaned / encoded data written by notebook 01
├── notebooks/                  # Numbered, run-in-order Jupyter notebooks
│   ├── 01_data_acquisition_and_eda.ipynb
│   ├── 02_regulatory_compliance_matrix.ipynb
│   ├── 03_baseline_models.ipynb
│   ├── 04_fairness_methods.ipynb
│   ├── 05_regulatory_constrained_model.ipynb
│   ├── 06_rcfs_evaluation.ipynb
│   └── 07_results_and_figures.ipynb
├── results/                    # All exported analysis outputs
│   ├── figures/                # Figures (PDF/PNG, 300 dpi)
│   ├── tables/                 # Tables (CSV/LaTeX)
│   └── models/                 # Serialized trained models
├── requirements.txt
├── .env                        # API keys / secrets (git-ignored; see .env.example)
└── README.md
```

**Design principle.** Each notebook is **self-contained**: helper functions
(loaders, metrics, plotting) are defined inline within the notebook that uses them,
so any notebook can be opened and run on its own. State is passed between notebooks
through shared files under `data/processed/` and `results/`.

---

## Datasets

| # | Dataset | Size | Region | Sensitive attributes | Source |
|---|---------|------|--------|----------------------|--------|
| 1 | German Credit (Statlog) | 1,000 | Europe | sex, age | UCI id=144 |
| 2 | Taiwan Credit Card Default | 30,000 | Asia | sex, education, age | UCI id=350 |
| 3 | Home Credit Default Risk | 307,511 | Emerging mkt | sex, age | Kaggle |
| 4 | DACON Credit Card Delinquency | ~26,000 | Korea | sex, age, education | DACON |
| 5 | Give Me Some Credit | 150,000 | UK | age | Kaggle |

Datasets are **not redistributed** here. Download instructions, links, and
credentials are documented in `notebooks/01_data_acquisition_and_eda.ipynb`.
Where a programmatic API exists (UCI via `ucimlrepo`, Kaggle via the Kaggle API),
notebook 01 downloads automatically; otherwise it expects manually downloaded files
under `data/raw/` at the paths the notebook documents.

---

## Setup

```bash
# 1. (Recommended) create a virtual environment
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. (Optional) configure secrets
cp .env.example .env               # then fill in any required keys
```

### Environment variables (`.env`)

`.env` is only needed if a notebook calls an external API (e.g. an LLM used in
notebook 02 to assist regulatory mapping, or the Kaggle API for downloads).
Copy `.env.example` to `.env` and fill in what you use:

```
# LLM (only if notebook 02 uses model-assisted analysis)
OPENAI_API_KEY=
ANTHROPIC_API_KEY=

# Kaggle (only if downloading Kaggle datasets via API)
KAGGLE_USERNAME=
KAGGLE_KEY=
```

Leave unused keys blank. `.env` is git-ignored and must never be committed.

---

## Reproducing the results

```bash
# 1. Download datasets (see notebook 01 for links/credentials),
#    placing manual downloads under data/raw/
# 2. Launch Jupyter and run notebooks in order, 01 → 07
jupyter lab
```

Each notebook reads and writes intermediate files under `data/processed/` and
`results/`, so a notebook can be re-run on its own once its upstream files exist.

---

## Reproducibility notes

- A single global seed (`RANDOM_SEED = 42`) is set in every notebook.
- Library versions are pinned in `requirements.txt`.
- Sensitive attributes are held out of training features and used **only** for
  fairness evaluation.
- Target encoding is unified across datasets: `1 = good / non-default`,
  `0 = bad / default`.

---

## License

- Code: MIT (see `LICENSE`).
- Datasets: governed by their respective licenses (UCI CC BY 4.0; Kaggle / DACON
  competition terms). Cite original sources as documented in the paper.

## Citation

```bibtex
@article{rcfs2026,
  title   = {A Regulatory-Constrained Fairness Framework for Credit Scoring},
  author  = {<authors>},
  journal = {<journal>},
  year    = {2026}
}
```
