# Unsupervised-Learning

*A growing collection of hands-on, research-driven projects exploring clustering, segmentation, and pattern discovery.*

This repository documents my journey into **Unsupervised Machine Learning** — a domain focused on discovering hidden structure in unlabeled data.
The goal is to learn by doing: experimenting with multiple clustering algorithms, evaluating their performance, comparing behaviors, and understanding where each method succeeds or fails.

The repo will expand over time with multiple practice projects, each inside its own sub-directory.
Currently, it includes work on:

* **Customer segmentation using multiple clustering methods**
* **Online retail segmentation using RFM + KMeans**

Each project is structured in a way similar to an applied ML research notebook —
**clear EDA → thoughtful preprocessing → algorithm exploration → evaluation → interpretation.**

---

# 📁 Repository Structure

```
Unsupervised-Learning/
│
├── Online retail cluster analysis with Kmeans.ipynb
│     └── Full RFM-based customer segmentation using KMeans  
│
├── Customer Cluster Analysis.ipynb
│     └── Multi-algorithm clustering exploration (KMeans, DBSCAN, Agglomerative, Divisive)
│
└── (future folders)
       ├── market-segmentation/
       ├── anomaly-detection/
       ├── dimensionality-reduction/
       └── kaggle-practice-projects/
```

---

#  1. Online Retail Customer Segmentation (RFM + KMeans)

**Notebook:** `Online retail cluster analysis with Kmeans.ipynb`

##  Dataset

* Kaggle “Online Retail II”
* > 525k transactions
* Includes: invoice, product, quantities, prices, customer IDs, timestamp, country

##  Data Cleaning & Important Decisions

* Removed missing `Customer ID` records (as segmentation requires identifiable users)
* Identified and removed **returns** by excluding invoices starting with *“C”*
* Cleaned all negative quantities and prices
* Engineered total monetary spend per row:
  `Total = Quantity × Price`

These cleaning steps prevent misleading RFM metrics.

##  RFM Feature Engineering

Calculated per customer:

| Feature       | Meaning                   |
| ------------- | ------------------------- |
| **Recency**   | Days since last purchase  |
| **Frequency** | Number of unique invoices |
| **Monetary**  | Total spending            |

Applied:

* **Log transformation** (to reduce skew)
* **Standardization** (for KMeans distance stability)

##  KMeans Clustering

* Used **k-means++ initialization**
* Tested k = 2–10 using the *Elbow Method*
* **Optimal k ≈ 4**

###  Evaluation

* **Silhouette Score:** ~0.33
* **Davies–Bouldin:** ~1.009
  (Healthy but imperfect — typical for retail behavior data.)

##  Cluster Insights

| Cluster | Size | Customer Behavior                                 |
| ------- | ---- | ------------------------------------------------- |
| **0**   | 779  | VIP segment — high monetary, frequent shoppers    |
| **1**   | 1403 | High-recency (inactive), low frequency, low spend |
| **2**   | 1185 | Mid-spend, moderate recency, regular customers    |
| **3**   | 947  | Low frequency, moderate recency                   |

##  Visualizations

* Cluster heatmap
* Recency–Monetary scatter
* Cluster size distribution
* Pie charts of proportions

##  Model Saved

`kmeans_model.joblib`

---

#  2. Multi-Algorithm Cluster Analysis (Customer Attributes Dataset)

**Notebook:** `Customer Cluster Analysis.ipynb`

##  Dataset

Kaggle customer segmentation data with:

* Sex
* Marital status
* Age
* Education
* Income
* Occupation
* Settlement size

2000 rows, all numeric categories.

##  EDA & Distributions

Explored:

* Skewness and kurtosis of **Age** and **Income**
* Category count distributions
* Correlation between numeric & ordinal features
* Pairplots for deeper intuition

##  Preprocessing

* Removed `ID` column
* Separated numeric vs categorical features
* No missing values
* Standardization performed inside each clustering pipeline

---

#  Clustering Algorithms Implemented

##  **1. KMeans**

* Pipeline: StandardScaler → KMeans
* Baseline k=3
* Silhouette ≈ **0.017**

  * (Low: dataset has overlapping clusters)

##  **2. DBSCAN**

* Tuned using **k-distance graph**
* Silhouette ≈ **–0.687**

  * High density variance → DBSCAN struggled

##  **3. Agglomerative Clustering**

* Linkage: Ward
* Performs **best** among tested algorithms
* Silhouette ≈ **0.113**

##  **4. Divisive Clustering (Custom Implementation)**

* Wrote a custom hierarchical divisive clustering class
* Repeatedly splits largest cluster using KMeans
* Silhouette ≈ **0.024**

---

#  Model Comparisons

### Silhouette Score (↑ better):

KMeans = 0.017
DBSCAN = –0.687
Agglomerative = **0.113**
Divisive = 0.024

### Davies–Bouldin Index (↓ better):

Agglomerative = **1.924**
Divisive = 1.805
KMeans = 2.056
DBSCAN = 202.9

###  **Best overall: Agglomerative Clustering**

---

#  Goals of the Repository

###  Build practical intuition

Learn how different clustering algorithms behave on real-world messy datasets.

###  Research-oriented experimentation

Evaluate models using **Silhouette**, **Davies–Bouldin**, inertia, and interpretability.

###  Develop a reusable structure

Future practice notebooks & Kaggle projects will follow the same pattern:

1. EDA
2. Preprocessing
3. Feature engineering
4. Multiple clustering methods
5. Evaluation & comparison
6. Visual interpretations

###  Keep expanding

This will evolve into a mini learning hub for:

* Market segmentation
* Anomaly detection
* Density estimation
* Understanding manifold structure
* Using PCA/UMAP for visualization
* Advanced clustering models (GMM, Spectral, HDBSCAN)

---

# Future Additions

I will extend this repository with subdirectories like:

```
future-projects/
│
├── Retail RFM Analysis/
├── Customer Loyalty Patterns/
├── Anomaly Detection in Transactions/
├── Product Affinity Clustering/
└── dimensionality-reduction-experiments/
```

Each with structured README + documentation.

---

# Contributing to Myself (or Others)

This repo is primarily a **self-learning research space**, but structured so others can follow the journey.

You’re welcome to:

* suggest new clustering techniques
* propose datasets
* help reorganize project folders
* or fork and experiment

---

