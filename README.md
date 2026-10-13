# Customers Like You

## Success targets

Targets recorded before fitting: factors at least 90% smaller than dense float64 storage; selected-model fit plus product-page refresh under 60 seconds on this machine; hit rate at 10 above unseen best sellers; and a readable map of 3-6 named groups. All four targets were met in the recorded run.

Individual scope: DE1-DE5, ML1-ML6 and J1-J2. No DE6 standalone matrix-building script, ML7 sensitivity experiment, or slides. I have implemented both (James Cottrell). Brief decisions and reasons are maintained in one file: [decisions.txt](decisions.txt).

## Results

Recorded run: 13 October 2026; Python 3.12.14; arm64; one numerical-library thread. Timing and sampled memory vary by run.

| Measure | Result |
| --- | --- |
| Source lines, both sheets | 1,067,371 |
| Clean purchase lines | 802,651 |
| Training matrix | 5,482 customers x 3,690 products |
| Main model | Truncated SVD, k = 50 |
| Dense / CSR / factor arrays | 161.83 / 5.00 / 3.67 MB |
| Factor reduction versus dense / CSR | 97.73% / 26.57% |
| Relative Frobenius reconstruction error | 0.7895 |
| Test window | 2011-10-10 through 2011-12-09 |
| Eligible test customers | 1,910 |
| SVD hit rate at 10 | 21.26% (406/1,910) |
| Unseen best-seller baseline | 17.38% (332/1,910) |
| PCA hit rate at 10 | 21.52% |
| SVD improvement over baseline | +3.87 percentage points |
| SVD fit + product-page refresh | 1.037 seconds |
| SVD complete model refresh, including customer scores | 2.177 seconds |
| Customer segments | 3 |

PCA matched five additional customers in this cohort. Fit time and sampled peak RSS are recorded in the comparison table and [both-run observations](reports/repeated_fit_observations.csv); both varied across fresh runs, and the peak-memory ordering reversed. These measurements do not establish a universal performance advantage. SVD remains the main method chosen before fitting. The rank was selected from training reconstruction error before final test hit rates were examined.

The evaluation covers 71.45% of all new test purchase pairs. Cold-start customers and unavailable products are disclosed in [split_summary.json](reports/split_summary.json). The primary hit rate applies to returning customers with at least one new product in the retained training catalogue. It does not assess repeat buying, cold-start performance or causal sales uplift.

## Reproduce

Use Python 3.12. Create virtual environment and install pinned dependency closure:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Place the original workbook at `data/raw/online_retail_II.xlsx`, then rebuild the matrix, analysis reports, figures and one-page memo with one command:

```bash
python -m nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=-1 analysis.ipynb
```

The kernel must use the active virtual environment. In an IDE, select `.venv/bin/python`. The notebook uses fixed seeds, float64 arrays and one numerical thread. It reads both raw sheets and regenerates the local train/test files, matrix and mappings rather than relying on a cached interactive session. A fresh-kernel rerun reproduced the selected factors, recommendations, test targets and cluster membership exactly; see [reproduction_check.json](reports/reproduction_check.json).

## Data and privacy

Chen, D. (2012). Online Retail II [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5CG6D. Licence: CC BY 4.0. Download: https://archive.ics.uci.edu/dataset/502/online+retail+ii.

Both sheets contain 1,067,371 rows in total: 525,461 and 541,910. Raw SHA-256: `bcbe73b35f5b7babf197fb0cb983a11f5d9ff929078d4aa53d171b1f2df2e980`.

`data/` is ignored by Git. Raw transactions, train/test records, factors with customer coordinates and customer-index mappings remain local. Notebook outputs, figures and reports contain aggregate information or product details. The factor-array comparison excludes historical purchase masks and serving caches; including those arrays gives approximately 9.33 MB before labels and file overhead. Compressed NPZ file size is reported separately from in-memory CSR size.

## Files and required evidence

| File | Requirement |
| --- | --- |
| `analysis.ipynb` | Executed DE1-DE5 and ML1-ML6 analysis, recommendation functions, reproducible matrix rebuild, memo generation |
| `decisions.txt` | Single brief decision and justification log, including pre-fit ML1 |
| `reports/data_profile.csv`, `reports/sheet_profile.csv` | DE1 schema, counts and missingness |
| `reports/cleaning_log.csv` | DE2 sequential removals |
| `reports/split_summary.json` | DE3 dates, new-purchase targets, exclusions and coverage |
| `reports/matrix_report.md` | DE4-DE5 matrix definition and memory |
| `reports/svd_rank_results.csv`, `figures/svd_reconstruction_error.png` | ML2 rank curve and selection |
| `reports/evaluation.csv`, `reports/timings.csv` | ML4 held-out hit rate, baseline and nightly job timing |
| `reports/pca_vs_svd.csv`, `reports/pca_vs_svd.md` | All eight ML5 comparison rows |
| `figures/customer_map.png`, `reports/segments.csv`, `reports/segment_products.csv` | ML6 named map, products and countries |
| `reports/findings.md` | Readable interpretation and limitations |
| `memo.pdf` | J2 one-page stakeholder memo |
| `requirements.txt`, `reports/environment.json` | R6 pinned packages and runtime details |
| `reports/verification.json`, `reports/reproduction_check.json` | Leakage, exclusion, consistency and fresh-run checks |
| `reflection.md`, `contribution_log.csv` | Individual reflection draft and honest work evidence |

Generated local data include `train.csv.gz`, `test_period.csv.gz`, `test_new_pairs.csv.gz`, `customer_product.npz`, `customer_index.csv`, `product_index.csv`, model-factor NPZ files and recommendation caches under `data/processed/`. They are regenerated from the source and excluded from the submission archive and Git.

## References

- [TruncatedSVD](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html): sparse uncentred decomposition.
- [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html): centring and sparse-compatible solvers.
- Local assignment brief: `Mini-Project#01.pdf`, eight pages. It is retained locally as a reference, outside the submission archive.
