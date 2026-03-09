# credit_card_segmentation_w_revenue_prediction

# Credit Card Behavioral Segmentation
## Early Lifecycle Customer Intelligence & Risk-Adjusted Revenue Forecasting

**Author:** Jing Cheng  
**Background:** 20+ years in consumer credit analytics across major financial institutions  
**Note:** This project replicates a real-world analysis using synthetically generated data. No proprietary data was used. AI-assisted implementation; all analytical design, methodology, and business interpretation are the author's own.

---

## Business Problem

A major financial institution's credit card revenue model (XGBoost regression) predicted poorly across the newly booked portfolio — R² < 0.15. Pre-campaign customer attributes alone had limited power to forecast 24-month interest income.

**Root cause:** The portfolio contains fundamentally different customer types. A single model applied to a mixed population cannot capture the distinct behavioral logic of revolvers, transactors, gamers, and high-risk accounts simultaneously.

**Proposed solution:** Segment first, then predict within segment.

| Model | R² | Spearman |
|---|---|---|
| Baseline — XGBoost, no segmentation | < 0.15 | — |
| Segmentation + within-segment prediction | between 0.3 ~ 0.50 | ~ 0.55 |

> Segmentation establishes the structure of the portfolio and improves revenue forecasting significantly.

---

## Key Findings

**8 distinct behavioral segments identified with portfolio share and financial contribution:**

| Segment | % Portfolio | Avg Risk-Adj Revenue | Profile |
|---|---|---|---|
| Mainstream Transvolver | ~50% | $50 | Largest segment — pays little to no interest |
| Gamer | ~10% | -$150 | Exploits BT promo, exits at month 13–15 |
| High Transactor | ~10% | $420 | High spend, pays in full — interchange revenue |
| BT Revolver | ~8% | $1,400 | Carried BT balance past promo — high interest |
| Credit Builder | ~7% | $280 | Low credit line, moderate interest and fees |
| High Revolver | ~6% | $3,000 | Highest revenue — consistent purchase revolvers |
| Early Churn | ~5% | $25 | Uses card occasionally then churns within 12 months |
| High Risk | ~2% | -$2,000 | Net negative after charge-off losses |
| Dormant | ~10% of booked | — | Excluded pre-modeling — high bureau score, did not activate |

**Findings for Immediate Action:**

| Segment | Finding | Action |
|---|---|---|
| Gamer | 10% of active portfolio — zero revenue, exit around month 13–15 | Redesign BT program with minimum spending requirement |
| High Risk | 2% of portfolio — net -$2,000 per account after charge-off losses | Tight monitoring and early collection strategy |
| Dormant | ~10% of booked accounts — high bureau score, card not activated | Credit line re-evaluation and activation incentive |

---

## Methodology

**Phase 1 — Behavioral Segmentation**
- Observe newly booked accounts over 24-month post-booking window
- Features: purchase, balance, utilization, payment, balance transfer count and amount, months on book, active usage percentage, delinquency counts
- Dormant accounts excluded before clustering — no behavioral signal
- Algorithm comparison: K-Means vs GMM vs DBSCAN
- K-Means selected — highest silhouette score (0.28), most interpretable and stable segments
- Validation: t-SNE visualization, grouped radar plots, month-over-month trajectory analysis

**Phase 2 — Segment Prediction & Revenue Forecasting**
- Predict segment membership using pre-campaign attributes only (bureau + bank relationship)
- No income (internal restriction), no BT offer or credit limit (unknown pre-campaign — leakage)
- Predict risk-adjusted revenue within each segment separately
- Feature engineering + K-Means clustering + XGBoost classifier + XGBoost regression

---

## Project Structure

```
cc_behavioral_segmentation.ipynb
│
├── Part 1 — Simulate dataset
│           8 segments calibrated to real portfolio composition
│
├── Part 2 — EDA
│           Revenue concentration, portfolio share
│           Standardized feature heatmap by segment
│
├── Part 3 — Clustering comparison
│           K-Means vs GMM vs DBSCAN
│           Elbow method + silhouette scores
│
├── Part 4 — Segment validation
│           t-SNE visualization
│           Grouped radar plots (3 behavioral groups)
│           Month-over-month transaction patterns per segment
│
├── Part 5 — Phase 2: Pre-campaign segment prediction
│           Random Forest classifier on bureau + relationship features
│           Feature importance analysis
│
└── Part 6 — Revenue forecasting
            Baseline vs within-segment R² comparison
            Rank ordering validation table (predicted decile vs actual revenue)
            Spearman correlation
```

---

## Technical Stack

| Tool | Usage |
|---|---|
| Python | Core implementation |
| pandas / numpy | Data manipulation and simulation |
| scikit-learn | K-Means, GMM, DBSCAN, Random Forest, GBM |
| scipy | Spearman rank correlation |
| matplotlib / seaborn | Visualization |

---

## Related Work

[credit card segmentation & revenue prediction](https://github.com/jingcheng-ai/credit_card_segmentation_w_revenue_prediction)

---

## About

Senior data scientist with 20+ years in financial services — JPMorgan, Citi, PNC.  
Specializing in AML modeling, fraud detection, credit risk, and customer analytics.  
Caltech AI/ML certificate (2025).

[LinkedIn](https://linkedin.com/in/jing-cheng-analytics)
