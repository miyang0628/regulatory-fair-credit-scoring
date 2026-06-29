# RCFS: Regulatory-Constrained Fairness Framework for Credit Scoring

Reproducibility repository for the paper
**"A Regulatory-Constrained Fairness Framework for Credit Scoring: Aligning Algorithmic Fairness with the Korean AI Basic Act and the EU AI Act."**

This repository contains all Jupyter notebooks, source code, and configuration
required to reproduce every table and figure in the paper. Notebooks are committed
**with their rendered outputs** so that results can be inspected on GitHub without
re-running the pipeline.

---

## Repository structure

```
rcfs-credit-fairness/
├── notebooks/                  # Numbered, run-in-order Jupyter notebooks
│   ├── 01_data_acquisition_and_eda.ipynb
│   ├── 02_regulatory_compliance_matrix.ipynb
│   ├── 03_baseline_models.ipynb
│   ├── 04_fairness_methods.ipynb
│   ├── 05_regulatory_constrained_model.ipynb
│   ├── 06_rcfs_evaluation.ipynb
│   └── 07_results_and_figures.ipynb
├── data/
│   ├── raw/                    # Downloaded datasets (git-ignored)
│   └── processed/              # Standardized objects written by notebook 01 (git-ignored)
├── results/
│   ├── figures/                # Exported figures (PDF/PNG, 300 dpi)
│   ├── tables/                 # Exported tables (CSV/LaTeX)
│   └── models/                 # Serialized trained models
├── configs/
│   └── datasets.yaml           # Per-dataset paths and sensitive-attribute config
├── requirements.txt
├── environment.yml
└── README.md
```

**Design principle.** Each notebook is **self-contained**: all helper functions
(loaders, metrics, plotting) are defined inline within the notebook that uses them,
so any notebook can be opened and run on its own. Notebooks read and write shared
artifacts under `data/processed/` and `results/`, which is how state is passed
between them.

---

## Datasets

| # | Dataset | Size | Region | Sensitive attributes | Source |
|---|---------|------|--------|----------------------|--------|
| 1 | German Credit (Statlog) | 1,000 | Europe | sex, age | UCI id=144 |
| 2 | Taiwan Credit Card Default | 30,000 | Asia | sex, education, age | UCI id=350 |
| 3 | Home Credit Default Risk | 307,511 | Emerging mkt | sex, age | Kaggle |
| 4 | DACON Credit Card Delinquency | ~26,000 | Korea | sex, age, education | DACON |
| 5 | Give Me Some Credit | 150,000 | UK | age | Kaggle |

Datasets are **not redistributed** in this repository. See
`notebooks/01_data_acquisition_and_eda.ipynb` for download instructions. All
loaders return a common schema so fairness metrics are comparable across datasets.

---

## Reproducing the results

```bash
# 1. Create environment
conda env create -f environment.yml
conda activate rcfs

# 2. Download datasets (see notebook 01 for credentials/links)
#    Place raw files under data/raw/ as documented in configs/datasets.yaml

# 3. Run notebooks in order, 01 → 07
jupyter lab
```

Each notebook is self-contained and reads/writes intermediate artifacts under
`data/processed/` and `results/`, so notebooks can be re-run independently once
their upstream artifacts exist.

---

## Reproducibility notes

- A single global seed (`RANDOM_SEED = 42`) is set in every notebook.
- Library versions are pinned in `requirements.txt` / `environment.yml`.
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
