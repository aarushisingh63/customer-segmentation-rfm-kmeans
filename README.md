# Customer Segmentation using RFM Analysis and K-Means Clustering

Segmenting customers of a UK online gift retailer using RFM features and K-Means clustering, to identify which customers drive revenue and which need retention or win-back effort.

## Dataset
Online Retail II (UCI Machine Learning Repository): https://archive.ics.uci.edu/dataset/502/online+retail+ii
- 1,067,371 transactions, 1 Dec 2009 to 9 Dec 2011
- Columns: Invoice, StockCode, Description, Quantity, InvoiceDate, Price, Customer ID, Country
- The dataset is not included in this repository because of its size. Download it from the link above.

## Methodology

**1. Exploratory data analysis (raw data)**
Missing values, duplicates, cancelled invoices, negative quantity and price, country share, non-product StockCodes, monthly invoice trend, and Quantity and Price distributions. Each cleaning decision below is based on these checks.

**2. Cleaning** (1,067,371 to 794,163 rows)
- Removed rows with missing Customer ID (22.8%), since RFM is built per customer
- Removed exact duplicate rows (about 3%)
- Kept only product StockCodes (5-digit codes); removed POST, M, BANK CHARGES, ADJUST and similar
- Removed rows with Price <= 0

**3. RFM features** (reference date: 10 Dec 2011)
- Recency: days since last purchase (from purchases only)
- Frequency: number of unique invoices (from purchases only)
- Monetary: net spend, i.e. purchases plus returns (cancelled invoices are negative)
- Customers with net Monetary <= 0 were removed. Final: 5,832 customers

**4. Transformation and scaling**
- Skewness before: Recency 0.90, Frequency 12.02, Monetary 26.77
- Log transform applied to Frequency (skew 0.61) and Monetary (skew 0.23)
- Recency kept raw: its skew was mild, and log made it left-skewed (-0.61)
- Outliers were not removed, because the highest-value customers are the most important for the business. Log transform reduces their influence on the distance calculation instead
- StandardScaler applied to all three features

**5. Choosing K**
- Elbow and silhouette score were computed for K = 2 to 10. The elbow curve is smooth and silhouette is highest at K = 2 (0.425), followed by K = 3 (0.393)
- K = 3 and K = 4 profiles were compared. K = 4 was chosen for business usefulness: it separates Champions from Regular customers, and Recent one-time buyers from Lost one-time buyers
- Silhouette at K = 4: 0.356

## Results (K = 4)

| Segment | Customers | Median Recency (days) | Median Frequency | Median Monetary | Share of customers | Share of revenue |
|---|---|---|---|---|---|---|
| Champions | 1,087 | 19 | 14 | 5,447.5 | 18.6% | 73.7% |
| Regular customers | 1,927 | 60 | 5 | 1,347.0 | 33.0% | 18.7% |
| Recent one-time buyers | 1,261 | 75 | 1 | 329.4 | 21.6% | 2.9% |
| Lost one-time buyers | 1,557 | 466 | 1 | 321.8 | 26.7% | 4.7% |

Key insight: in this dataset, the 18.6% of customers in the Champions segment generate about 74% of revenue.

Suggested actions:
- Champions: loyalty rewards and retention
- Regular customers: nudges to increase purchase frequency
- Recent one-time buyers: follow-up to drive a second purchase
- Lost one-time buyers: low-cost win-back campaign

## Validation
- Stability: Adjusted Rand Index between the final model and models with different random seeds is 0.984 to 0.995
- Comparison with Hierarchical clustering (Ward, K = 4): silhouette 0.319 (K-Means 0.356), ARI between the two methods 0.518. Both methods agree strongly on Champions (about 99%) and Lost one-time buyers (about 98%); the middle segments (Regular and Recent one-time buyers) differ in where the boundary is drawn
- PCA (2 components) explains 95.2% of the variance and is used only for visualization

## Limitations
- Silhouette scores are in the weak-to-moderate range. RFM data is continuous, so the segments are a useful partition for business action, not naturally separated groups
- The dendrogram suggests 2 to 3 groups; K = 4 is a business-driven choice
- Middle segments depend on the algorithm and boundary used
- Seasonality (peak in Sep to Nov) was not modelled separately
- Data is from one UK retailer, and revenue figures are for the cleaned dataset only. Profit margin is not available

## Tools
Python, pandas, NumPy, SciPy, scikit-learn, matplotlib

## Files
- `rfm_customer_segmentation.ipynb`: full analysis
