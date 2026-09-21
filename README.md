
# Polymarket Behavioral Playbooks

Unsupervised behavioral clustering and risk-adjusted performance analysis of **604,578 Polymarket trader profiles**.

## Research question

Do successful Polymarket traders converge on one dominant trading playbook, or are multiple behavioral schemes associated with an edge?

Rather than ranking traders by total profit and loss alone, this project studies *how* traders participate: their activity level, market breadth, transaction sizing, execution behavior, and topic specialization.

## Key finding

The analysis identifies four distinct behavioral playbooks:

| Playbook | Core behavioral pattern | Typical outcome |
| --- | --- | --- |
| **Low-Tempo Focused** | Slow, focused participation with relatively small trades | Most polarized: highest sharp-label share, but also the highest awful-label share |
| **High-Tempo Generalists** | Frequent activity across the broadest markets and topics | Broad participation without the strongest normalized performance |
| **Concentrated Conviction** | Larger positions concentrated in a narrow set of topics | Near break-even median efficiency at the greatest median scale |
| **Selective Specialists** | Very low activity, very small trades, and near-complete topic specialization | Strongest typical result: the only positive median profit per dollar traded |

The main takeaway is that there is **no single universal playbook**. The data distinguish between extreme upside and consistent efficiency. Low-Tempo Focused traders are high-variance, while Selective Specialists produced the strongest median profit per dollar traded in this sample.

## Dataset

Each observation represents one unique trader and includes:

- Total P&L and trading volume
- Transaction count, transactions per day, and markets per day
- Mean transaction value and execution-related measures
- Allocation across 17 market topics
- A supplied trader label: `awful`, `bad`, `good`, or `sharp`

The dataset is not included in this repository. Obtain it through the original assignment or data source before running the notebook.

## Methodology

1. **Data audit and feature engineering**
   - Verified 604,578 unique traders and no exact duplicate rows.
   - Constructed topic concentration, entropy, effective-topic-count, and dominant-topic measures.
   - Converted skewed clustering inputs to percentile ranks and standardized them.

2. **Direct behavioral analysis**
   - Compared topic specialization, activity, and execution-depth groups using median profit per dollar traded and sharp-label share.
   - Estimated a logistic regression for the probability of receiving a sharp label while controlling for scale and activity.

3. **Outcome-blind clustering**
   - Applied K-means clustering to five behavioral dimensions: tempo, market breadth, conviction sizing, price/execution intensity, and topic diversification.
   - Excluded P&L, profit per dollar traded, profitability, and trader labels from all clustering inputs.
   - Evaluated two through seven clusters using inertia, silhouette scores, interpretability, and cluster balance; selected four clusters.

4. **Performance evaluation**
   - Compared playbooks using median profit per dollar traded (PPV), profitability, label composition, percentile risk distributions, bootstrap confidence intervals, Kruskal-Wallis testing, and chi-square testing.

## Results at a glance

| Playbook | Traders | Median PPV | Profitable traders | Sharp rate | Good or sharp |
| --- | ---: | ---: | ---: | ---: | ---: |
| Low-Tempo Focused | 140,142 | -154.11 bps | 37.52% | 29.22% | 38.07% |
| High-Tempo Generalists | 202,071 | -14.06 bps | 40.42% | 14.33% | 41.28% |
| Concentrated Conviction | 133,432 | -3.50 bps | 44.74% | 12.02% | 46.08% |
| Selective Specialists | 128,933 | 6.05 bps | 54.51% | 10.73% | 63.55% |

`PPV = trader P&L / trader volume × 10,000`, expressed in basis points.

## Statistical evidence

- PPV distributions differ across the four playbooks: Kruskal-Wallis `H(3) = 12,425.54`, `p < 0.001`; epsilon squared = `0.021`.
- Trader-label composition differs across playbooks: chi-square `χ²(9) = 112,022.95`, `p < 0.001`; Cramér's V = `0.249`.
- The small PPV effect size means that playbook is informative but not destiny: behavior describes a trader's style, while execution still determines outcomes.

## Repository contents

- `Polymarket_Trader_Analysis.ipynb` — complete analysis notebook
- `README.md` — project overview and reproducibility notes

## Running the analysis

The notebook was developed in Google Colab. Open `Polymarket_Trader_Analysis.ipynb`, upload or mount the source dataset, update the file path in the data-loading cell, and run cells in order.

Core Python packages:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
scipy
statsmodels
```

## Limitations

This is an exploratory trader-level analysis, not a causal study or a trading strategy backtest. The dataset does not contain complete transaction histories, market-resolution outcomes, or a documented definition of the supplied labels. The results identify associations in this sample and should not be interpreted as evidence that copying a playbook will cause profitability.

## References

1. Diquigiovanni, J., & Scarpa, B. (2018). *Analysis of Association Football Playing Styles: An Innovative Method to Cluster Networks.* arXiv. https://arxiv.org/abs/1805.10933
2. Wolfers, J., & Zitzewitz, E. (2004). *Prediction Markets.* Journal of Economic Perspectives, 18(2), 107-126. https://www.aeaweb.org/articles?id=10.1257/0895330041371321

