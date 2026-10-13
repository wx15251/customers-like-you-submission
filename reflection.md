# Individual reflection

I approached the project using Truncated SVD because the customer-product table is sparse and its leading singular values offer a compact description of shared purchase patterns.

The clearest lesson from the results is that compression and recommendation quality require separate evidence. With 50 components, factor storage fell to 3.67 MB, but the relative reconstruction error was 0.790. The recommendation test was more useful for the business question where SVD reached 21.26% hit rate at 10 against 17.38% for best sellers. PCA was slightly higher at 21.52%, so choosing SVD does not mean it wins every comparison.

The temporal split and customer-specific exclusions are as important as the decomposition. Previously purchased products had to be excluded from both model and baseline rankings, and cold-start customers and unavailable products had to be counted rather than just discarded. The customer map also shows purchase breadth, which limits how confidently its groups can be treated as marketing segments.

With more time, I would use an earlier validation period to examine ranking and popularity effects, then evaluate on another untouched period. I would also check product availability and test campaign responses before treating the exploratory groups as business categories.
