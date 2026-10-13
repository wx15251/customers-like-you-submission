# PCA versus Truncated SVD

| Measure | PCA | Truncated SVD |
| --- | --- | --- |
| k used | 50 | 50 |
| Fit time (seconds) | 0.3111 | 0.2926 |
| Peak memory during fit (MB; sampled total RSS) | 166.445 | 150.307 |
| Size of stored factors (MB) | 3.699 | 3.669 |
| Variance explained or relative reconstruction error | Explained variance: 34.92% | Relative Frobenius error: 0.7895 |
| Hit rate at 10 on held-out purchases | 21.52% | 21.26% |
| Best-seller baseline hit rate at 10 | 17.38% | 17.38% |
| What component 1 represents | Centred purchase-volume variation; correlation with log-weighted purchase volume +0.968. See product loadings. | Uncentred purchase intensity; correlation with log-weighted purchase volume +0.968. See product loadings. |

Both use the identical training matrix, k, cosine scoring, candidate catalogue and eligible test cohort. MB is decimal. Peak fit memory is total process RSS sampled every 2 ms in a fresh process; very short peaks may be missed. Baseline RSS and incremental peaks are in svd_rank_results.csv and pca_fit_metrics.json. Stored factors include customer scores, product components, singular values and PCA means; these are in-memory float64 sizes, excluding file headers, historical purchase masks and serving caches. Explained variance and reconstruction error have different definitions.
