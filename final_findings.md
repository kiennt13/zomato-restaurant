# Zomato Restaurant EDA — Findings

## Project focus

This exploratory analysis describes restaurant coverage, ratings, votes, price range, service availability, geography, and cuisine. The goal is to identify useful patterns and associations in the dataset, not to claim that any feature causes higher ratings.

## Dataset and data quality

- The dataset contains **9,551 restaurant records** and **21 columns**. It has **9 missing cuisine entries**, no fully duplicated rows, and 9,551 unique restaurant IDs.
- **2,148 restaurants (22.5%) have an Aggregate rating of 0 and are labeled “Not rated.”** Treat these as unrated, not as genuine zero-star reviews. Exclude them from rating comparisons; the rated subset contains 7,403 restaurants.
- The `Switch to order menu` field is `No` for all 9,551 rows, so it has no variation and cannot explain differences.
- There are **499 rows with zero latitude and longitude**. These coordinates are placeholders or invalid locations; exclude them from map-based analysis unless verified.
- Average cost for two is recorded in multiple currencies and countries. Do not compare raw costs across countries as if they shared a unit; analyze within currency/country or convert with a documented method.
- The 9 missing cuisine values are a small share of records and can be retained for analyses that do not require cuisine, or left as missing when grouping by cuisine.

## Key distributions

### Ratings

Among the 9,551 rows, the rating labels are: **Average 3,737**, **Not rated 2,148**, **Good 2,100**, **Very Good 1,079**, **Excellent 301**, and **Poor 186**. These categories sum to the full dataset. The full-column median is **3.2**, but this summary includes zero-coded unrated records; the full-column mean of 2.67 is likewise depressed by those records. Use the 7,403 rated restaurants for rating summaries; the rated-only overall mean and median were not included in the shared results.

### Votes

Votes are strongly right-skewed: the mean is **156.9**, while the median is **31**; the 75th percentile is **131**, the 95th is **699**, and the maximum is **10,934**. **1,094 restaurants (11.45%) have zero votes.** A small number of restaurants account for very large vote counts.

### Price range

Price range 1 is most common (**4,444 restaurants**), followed by range 2 (**3,113**), range 3 (**1,408**), and range 4 (**586**). Among rated restaurants, mean ratings by price range rise from **3.20** (range 1) and **3.31** (range 2) to **3.68** (range 3) and **3.73** (range 4). This is a descriptive pattern and may reflect differences in location, cuisine, or restaurant type.

## Relationships

The strongest reported Spearman relationships with Aggregate rating were:
- Votes and Aggregate rating with a 0.6821 coefficient, suggesting strong positive monotonic association => restaurants with higher ratings tend to have more votes.
- Price range and Aggregate rating with 0.4017 coefficient, suggeting a moderate positive monotonic association => higher price range tends to accompany higher ratings.

Both p-values were reported as `0.0` by the notebook, making them both statistically significant. Votes may also reflect restaurant exposure, and higher ratings may attract more votes.

## Geographic findings

The records are heavily concentrated in India: **8,652 (90.59%)** use Indian Rupees. New Delhi has **5,473 restaurants**, Gurgaon **1,118**, Noida **1,080**, Faridabad **251**, and Ghaziabad **25**. These counts are highly unbalanced, so overall averages are dominated by the largest cities.

For the five largest listed cities, mean ratings are close: New Delhi **3.30** (4,048 rated), Gurgaon **3.33** (890), Noida **3.16** (696), Faridabad **3.10** (151), and Ghaziabad **3.10** (23). Some smaller cities in the table have higher means (for example, Lucknow **4.20**, n=21), but the small samples make those estimates less stable. Compare cities with rated counts and uncertainty shown; do not rank tiny samples as definitive winners.

## Cuisine findings

Cuisine labels are multi-valued, so cuisine counts overlap and percentages do not sum to 100%. North Indian appears in **3,960 records (41.46%)**, Chinese in **2,735 (28.64%)**, and Fast Food in **1,986 (20.79%)**. These are the most common cuisine labels in the analysis.

Among cuisine groups with larger rated samples, reported mean ratings include Seafood **3.93 (n=171)**, European **3.96 (n=146)**, Mediterranean **4.02 (n=110)**, and Italian **3.75 (n=726)**; North Indian is **3.30 (n=3,017)** and Chinese **3.28 (n=2,184)**. Treat smaller-group means as exploratory. In the two-cuisine comparison, Italian exceeds North Indian by **0.35 rating points** (Italian n=125, North Indian n=803; reported 95% CI **0.253–0.447**). However, cuisine and price are not independent: in the adjusted OLS comparison, the North Indian coefficient was **−0.242** relative to the model’s reference cuisine, while price-range 3 and 4 coefficients were **+0.416** and **+0.477** relative to price range 1. The model used 3,059 observations and had **R²=0.212**; these estimates are conditional associations, not causal effects.

## Limitations and interpretation

- The data are observational and appear to be a snapshot; there is no experimental design or time sequence here. Associations cannot show that price, cuisine, location, or service availability causes ratings to change.
- Restaurant coverage is uneven by city and country. Cuisine and city comparisons can be confounded by price range, location, restaurant type, exposure, and unequal sample sizes.
- Ratings and vote counts summarize platform activity, not necessarily representative customer opinions. Zero votes and zero ratings have distinct meanings.
- Cuisine assignments overlap, and the analysis compared only selected cuisines. Group means can hide within-group variation.
- The Spearman results are pairwise correlations. They do not control for other variables; the OLS result is limited to its included variables and subset.