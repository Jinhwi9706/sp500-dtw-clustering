# S&P 500 Stock Pattern Analysis Using DTW-Based Clustering

## Overview
Stock tickers are traditionally grouped by sector (Technology, Financials, Energy, etc.).
But do stocks within the same sector actually move similarly?
And are there stocks across different sectors that share similar price patterns?

This project uses **Dynamic Time Warping (DTW)** to measure similarity between
S&P 500 stock price time series, then applies **hierarchical clustering** to discover
natural groupings based on actual price movement patterns — not just sector labels.

---

## Key Question
> *Do traditional sector classifications reflect actual stock price behavior?*

---

## Results Summary

| Metric | KMeans (Euclidean) | DTW Hierarchical |
|--------|-------------------|-----------------|
| Silhouette Score | 0.4004 | 0.0567 |
| ARI vs Sector | 0.0045 | 0.0033 |
| NMI vs Sector | 0.0059 | 0.0083 |
| Sector Purity | 15.0% | 16.1% |

**Key Findings:**
- Both methods show near-zero ARI against GICS sector labels
  → stock price patterns do NOT follow traditional sector boundaries
- DTW identified **1,327 cross-sector stock pairs** with similar price patterns
  that KMeans (Euclidean distance) failed to group together
- Macroeconomic events (rate hikes, AI boom, post-COVID recovery) appear to drive
  co-movement more than sector classification

---

## Project Structure
```
sp500-dtw-clustering/
│
├── Untitled.ipynb          # Main notebook (full analysis)
├── dtw_distance_matrix.npy # Precomputed DTW distance matrix (462×462)
├── dtw_ticker_list.txt     # Ticker list corresponding to the matrix
└── README.md
```

---

## Workflow
```
1. Data Collection       → S&P 500 tickers from Wikipedia + yfinance (5 years)
2. EDA                   → Sector trends, return distribution, volatility
3. Preprocessing         → Z-score normalization, NaN handling
4. KMeans Clustering     → Euclidean distance baseline (k=2)
5. DTW Distance Matrix   → 106,491 pairwise DTW calculations
6. Hierarchical Clustering → Average linkage on DTW distances
7. Comparison            → KMeans vs DTW (ARI, NMI, Silhouette, Sector Purity)
8. Conclusions           → Cross-sector insights
```

---

## Tech Stack

| Category | Tools |
|----------|-------|
| Data Collection | yfinance, requests, pandas |
| Distance Metric | fastDTW |
| Clustering | scipy (hierarchical), scikit-learn (KMeans) |
| Evaluation | scikit-learn (ARI, NMI, Silhouette) |
| Visualization | matplotlib, seaborn |
| Dimensionality Reduction | scikit-learn (PCA) |

---

## Notable Findings

**Cross-sector pairs identified by DTW (sample):**
- MMM (Industrials) ↔ ALL (Financials)
- FOX ↔ FOXA (both Communication Services — closest pair, distance: 34.39)
- EME ↔ PH (both Industrials — distance: 60.65)

**Most volatile stocks (5-year annualized):**
- CVNA: 111.5% | COIN: 86.1% | SMCI: 83.9%

**Sector with highest avg annual return:**
- Energy: +30.0% | Information Technology: +20.1%

---

## Limitations
- DTW approximated via fastDTW (not exact DTW)
- Closing price only — volume and fundamentals excluded
- 5-year window includes post-COVID recovery and rate hike cycles
- Optimal k=2 limits clustering granularity

## Future Work
- Apply progressive exponential weight penalties to DTW
- Test on different time windows
- Use DTW cluster membership as features for return prediction

---

## Course Info
**Course:** Data Mining / Machine Learning  
**Program:** M.S. Data Science, Pace University  
**Author:** Jinhwi Lee
