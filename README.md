# customer-segmentation-rfm-kmeans
RFM-based customer segmentation using K-Means clustering on Online Retail II dataset
# Customer Segmentation using RFM Analysis and K-Means Clustering

Segments customers of a UK-based online retailer into behaviourally meaningful groups using RFM (Recency, Frequency, Monetary) analysis and K-Means clustering, with automated optimal-cluster selection.

**Dataset:** [Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii) (UCI Machine Learning Repository) — 1M+ UK e-commerce transactions, 2009–2011.

## Project Workflow
1. Data cleaning (duplicates, missing Customer IDs, matched purchase-return pairs)
2. RFM feature engineering (Recency, Frequency, Monetary per customer)
3. IQR-based outlier removal
4. Skewness reduction — compared Log, Square Root, and Box-Cox transformations
5. Feature standardization (StandardScaler)
6. Automated optimal cluster count using WCSS + KneeLocator
7. K-Means clustering (k=4)
8. Evaluation via Silhouette Score
9. 2D (PCA) and 3D visualization of clusters
10. Cluster profiling and business-driven segment labeling

## Key Results
| Metric | Value |
|---|---|
| Customers segmented | 5,123 |
| Optimal clusters (k) | 4 |
| Best transformation | Box-Cox |
| Silhouette Score | 0.379 |

## Segments Identified
- **Champions** — recent, frequent, high-spending customers
- **At Risk** — previously valuable, now inactive
- **New/Occasional** — recent but low engagement
- **Low Engagement** — long inactive, low value

## Tools Used
Python · pandas · NumPy · scikit-learn · SciPy · Matplotlib · kneed (KneeLocator)

## Files
- `Customer_Segmentation_Notebook.ipynb` — full analysis code
- `Customer_Segmentation_Report.docx` — detailed methodology and findings report

## Methodology Reference
Workflow structured with reference to: Vo, B.G. et al., *"Customer Segmentation: Automatic K-Optimization and RFM-Based K-Means Clustering"*, ICIIT 2025. Implementation, cleaning logic, and business interpretation were carried out independently on this dataset, with results cross-validated against the paper's reported preprocessing statistics.
