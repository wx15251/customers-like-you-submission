# Findings

Measured snapshot of the verified 13 October 2026 run. Rebuilding the notebook updates numerical tables, figures and the memo; timings vary by run.

## Recommendation quality

SVD hit rate at 10 is 21.26% (406/1910); best sellers reach 17.38% (332/1910). The gain is 3.87 percentage points. A paired customer bootstrap gives a descriptive 95% interval of 1.47 to 6.23 percentage points, conditional on this fixed eligible cohort. It is not a causal trial or evidence about customers excluded from the cohort.

PCA reaches 21.52%, five additional hits. The same matrix, rank, product-vector normalisation, customer weighting, catalogue, exclusions and test customers are used for both. No recommendation settings were changed after viewing test rates.

## Compression and performance

k=50 is the maximum-gap elbow under the recorded rule. The k=50 and k=100 gaps are close (0.1546 and 0.1533); this is a pragmatic compression trade-off, not proof of a uniquely optimal rank. Relative reconstruction error is 0.7895. The retained projection preserves 37.67% of the original squared Frobenius norm; this is different from explained variance and purchase hit rate.

Factors use 3.669 MB versus 161.829 MB dense and 4.997 MB CSR. SVD's factor reduction is 97.73% relative to dense storage and 26.57% relative to CSR. History, factors and top-10 caches total 9.328 MB before mappings, labels and file overhead. Factor-only compression should not be described as the total serving-system footprint.

SVD fitting took 0.2926 seconds; PCA took 0.3111. Sampled peak fit RSS was 150.31 MB for SVD and 166.45 MB for PCA. RSS is sampled at 2 ms intervals in isolated processes, and imports/matrix loading determine baseline process memory. Very brief peaks can be missed. The memory ordering reversed between the two complete notebook runs, so no consistent memory advantage is claimed. Subsecond fit timings also varied; the local measurements establish that the target was met, not a general speed ranking. Total model refresh includes fitting, factor transformation, all product neighbours and all customer scores; it excludes process start, raw Excel reading and artifact serialization. See timings.csv for the stage breakdown and environment.json for the machine and packages.

## Customer map

Three groups are exploratory summaries of two training components. K-means uses standardised coordinates; the figure displays the original coordinates. Product prevalence lift is relative to all retained training customers, with a minimum support requirement. Country is the most frequent training country for each customer. These choices are recorded in decisions.txt.

- **Focused kitchen and gifts**: 4,554 customers (83.1%), median 32 distinct retained products. Characteristic products: REX CASH+CARRY JUMBO SHOPPER; BAKING SET 9 PIECE RETROSPOT; REGENCY CAKESTAND 3 TIER; RETROSPOT TEA SET CERAMIC 11 PC; WHITE HANGING HEART T-LIGHT HOLDER. Countries: United Kingdom 92.3%; France 1.3%; Germany 1.3%.
- **Heart and home decor**: 537 customers (9.8%), median 182 distinct retained products. Characteristic products: T-LIGHT HOLDER SWEETHEART HANGING; HOUSE WRECKING METAL SIGN; FRENCH GARDEN SIGN BLUE METAL; FILIGRIS HEART WITH BUTTERFLY; FRENCH KITCHEN SIGN BLUE METAL. Countries: United Kingdom 97.8%; EIRE 0.6%; Cyprus 0.4%.
- **Children's lunchware and gifts**: 391 customers (7.1%), median 171 distinct retained products. Characteristic products: SPACEBOY BEAKER; SPACEBOY MINI BACKPACK; DOLLY GIRL BEAKER; CHARLOTTE BAG APPLES DESIGN; WRAP CIRCUS PARADE. Countries: United Kingdom 71.1%; Germany 9.5%; France 5.9%.

The first component strongly reflects purchasing intensity. The largest group contains customers with narrower purchase histories, so the grouping partly separates buying breadth rather than only product taste. The United Kingdom dominates every group. Names are product-based summaries, not demographic labels or validated campaign categories; a two-component projection omits substantial information.

## Coverage and limitations

Of 62,625 new test customer-product pairs, 13,279 have customers without usable training profiles and 4,602 have known customers but unavailable products. The remaining 44,744 pairs (71.45%) form targets for 1,910 customers. Customers with only repeat test purchases are outside this discovery task.

The model learns positive purchases; absence is not an explicit dislike. Returns and cancellations are removed rather than netted against earlier sales. Exact repeated invoice lines are retained because the data lack a line-level key. Rare-product filtering, wholesale volume, popularity and the seasonal October-December hold-out can affect generalisation. No live availability or stock-on-hand information is supplied, so the retained historical catalogue is the recommendation universe.

SVD is a reasonable candidate for a controlled trial after product review and later-period validation. Keep a best-seller fallback for cold starts, verify stock availability before deployment, and compare future outcomes on newly untouched data. Full decisions remain in the single text log; this report interprets the observed results.
