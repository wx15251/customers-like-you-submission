# Training matrix report

Purchase value: log(1 + total training units) per customer-product pair.

Shape: 5,482 customers x 3,690 products. Nonzeros: 414,566. Sparsity: 97.95%.

Float64 dense equivalent: 161.829 MB. CSR arrays: 4.997 MB. Compressed NPZ on disk: 1.057 MB. MB means 1,000,000 bytes throughout.

Training ends before 2011-10-10; test is [2011-10-10, 2011-12-10). Catalogue threshold: 10 distinct training customers.

Evaluable cohort: 1,910 customers and 44,744 new purchase pairs. This covers 71.45% of all new test pairs and 83.19% of customers with new test purchases. See split_summary.json for all exclusions.

No held-out pair appears in its customer's full pre-filter training history. Matrix, catalogue, descriptions and profiles use training only. Customer maps and train/test records are stored locally under ignored data/.
