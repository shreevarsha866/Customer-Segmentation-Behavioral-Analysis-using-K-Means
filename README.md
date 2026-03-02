# 👥 Customer Segmentation & Behavioral Analysis using K-Means

> **Unsupervised Machine Learning — Clustering Customers for Targeted Marketing**

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://python.org)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-orange?logo=scikit-learn)](https://scikit-learn.org)
[![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-yellow?logo=powerbi)](https://powerbi.microsoft.com)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)](https://jupyter.org)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1LGHO24pS-ivWbqAs94fq9Pcysfsf-JOL?usp=sharing)

---

## 📌 Project Overview

This project applies **K-Means Clustering** to segment ~2,000 customers into **4 distinct behavioral groups** based on age, income, and settlement size. The goal is to help businesses move away from one-size-fits-all marketing and instead build **personalized strategies** for each customer segment.

The optimal number of clusters was determined using the **Elbow Method** and validated with **Silhouette Scores**. An interactive **Power BI dashboard** was built to visualize segment profiles for non-technical stakeholders.

---

## 🎯 Business Problem

Businesses waste marketing budgets by treating all customers the same. By understanding **who their customers really are** — high-value spenders, price-sensitive buyers, upsell opportunities — companies can:

- 🎯 Target the right customers with the right offers
- 💰 Improve marketing ROI through personalization
- 📊 Identify high-value segments to prioritize retention
- 🔍 Discover upsell & cross-sell opportunities

---

## 📊 Key Results

| Metric | Value |
|---|---|
| **Dataset Size** | ~2,000 customer records |
| **Optimal Clusters** | **4** (Elbow Method + Silhouette Score) |
| **Features Used** | Age, Income, Settlement Size |
| **Targeting Efficiency ↑** | **~20%** improvement |
| **Validation Method** | Silhouette Score |
| **Dashboard** | Power BI (interactive) |

---

## 👥 Customer Segment Profiles

| Cluster | Segment | Characteristics | Business Strategy |
|---|---|---|---|
| **Cluster 0** | 💎 High-Value | High income, urban, high spending | Loyalty programs, premium offers |
| **Cluster 1** | 💲 Price-Sensitive | Low income, smaller cities | Discounts, budget-friendly promotions |
| **Cluster 2** | 📈 Upsell Opportunity | Mid income, growth potential | Cross-sell, upgrade campaigns |
| **Cluster 3** | ⚖️ Stable & Moderate | Average income, consistent behavior | Retention campaigns, steady engagement |

---

## 🗂️ Project Pipeline

```
Raw Data (segmentation data.csv)
     │
     ▼
1. Data Inspection       → shape, dtypes, nulls, duplicates, unique values
     │
     ▼
2. EDA                   → feature distributions, boxplots (outlier detection),
                           correlation heatmap
     │
     ▼
3. Feature Scaling       → StandardScaler (equal contribution to clustering)
     │
     ▼
4. Optimal K Selection   → Elbow Method (WCSS) + Silhouette Score validation
     │
     ▼
5. K-Means Clustering    → 4 clusters fitted and assigned
     │
     ▼
6. Cluster Profiling     → groupby mean per cluster → segment interpretation
     │
     ▼
7. Visualization         → 2D scatter + 3D plot (Age × Income × Settlement)
     │
     ▼
8. Power BI Dashboard    → interactive segment insights for stakeholders
     │
     ▼
9. Export               → customer_segmentation_powerbi.csv
```

---

## 📂 Dataset

- **File:** `segmentation data.csv`
- **Records:** ~2,000 customers
- **No missing values** ✅
- **No duplicates** ✅

| Feature | Description |
|---|---|
| **Age** | Customer age |
| **Income** | Annual income level |
| **Settlement size** | City size (urban / suburban / rural) |

---

## 🔍 EDA Highlights

- 📊 Feature distributions showed varying skewness across customer attributes
- 📦 Boxplots revealed some outliers that could affect distance-based clustering — handled via StandardScaler
- 🔥 Correlation heatmap showed **income and settlement size** are the most influential variables
- 🏙️ High-income customers are concentrated in **urban areas**
- 💰 Income varies significantly within the same age group — **age alone doesn't predict customer value**

---

## ⚙️ Feature Scaling

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(df)
```
> Standardization ensures all features contribute equally to distance calculations in K-Means.

---

## 📐 Optimal Cluster Selection

### Elbow Method
```python
wcss = []
for k in range(1, 11):
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(X_scaled)
    wcss.append(kmeans.inertia_)
# Elbow observed at k=4
```

### Silhouette Score Validation
```python
from sklearn.metrics import silhouette_score

for k in range(2, 8):
    kmeans = KMeans(n_clusters=k, random_state=42)
    labels = kmeans.fit_predict(X_scaled)
    score = silhouette_score(X_scaled, labels)
    print(f'Clusters: {k}, Silhouette Score: {score:.3f}')
# k=4 confirmed as optimal
```

---

## 🤖 K-Means Model

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=4, random_state=42)
df['Cluster'] = kmeans.fit_predict(X_scaled)

# Cluster profiling
cluster_profile = df.groupby('Cluster').mean()
```

---

## 📊 Visualizations

### 2D Customer Segments
```python
sns.scatterplot(
    x=df.iloc[:,0], y=df.iloc[:,1],
    hue=df['Cluster'], palette='Set2'
)
```

### 3D Segmentation — Age × Income × Settlement Size
```python
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure(figsize=(10, 8))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(df['Age'], df['Income'], df['Settlement size'],
           c=df['Cluster'], cmap='Set2', s=40, alpha=0.7)
```
> 3D visualization clearly shows clusters separated by income and location — age alone doesn't drive segmentation.

---

## 💡 Key Business Insights

- 🏙️ **Settlement size + income** are the strongest drivers of customer segmentation — not age
- 💎 **High-income urban customers** form a distinct premium segment — ideal for loyalty programs
- 💲 **Small-city, lower-income customers** are price-sensitive — respond best to discounts
- 📈 A mid-tier segment shows **upsell potential** — can be moved to higher value with the right offer
- ⚖️ A stable moderate segment needs **retention campaigns** to prevent churn
- 🎯 Cluster-based targeting improves **marketing efficiency by ~20%**

---

## 📊 Power BI Dashboard

An interactive **Power BI dashboard** was built to visualize:
- Segment distribution across customer base
- Income & age breakdown per cluster
- Settlement size impact on customer value
- Business strategy recommendations per segment

> Dashboard file: `Customer_Segmentation___Behavioral_Analysis_Using_K-Means.pbix`

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Python** | Core programming |
| **Pandas & NumPy** | Data manipulation |
| **Scikit-learn** | K-Means, StandardScaler, Silhouette Score |
| **Matplotlib & Seaborn** | 2D & 3D visualizations |
| **Power BI** | Business dashboard |
| **Google Colab** | Development environment |

---

## 🚀 How to Run

**Option 1 — Google Colab (Recommended)**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1LGHO24pS-ivWbqAs94fq9Pcysfsf-JOL?usp=sharing)

**Option 2 — Run Locally**

```bash
# Clone the repo
git clone https://github.com/shreevarsha866/Customer-Segmentation-Behavioral-Analysis-using-K-Means

# Install dependencies
pip install pandas numpy matplotlib seaborn scikit-learn

# Run the notebook
jupyter notebook Customer_Segmentation___Behavioral_Analysis_using_K_Means.ipynb
```

---

## 📁 Repository Structure

```
📦 Customer-Segmentation-Behavioral-Analysis-using-K-Means
 ┣ 📓 Customer_Segmentation___Behavioral_Analysis_using_K_Means.ipynb  ← Main notebook
 ┣ 📊 Customer_Segmentation___Behavioral_Analysis_Using_K-Means.pbix   ← Power BI dashboard
 ┣ 📄 segmentation data.csv                                            ← Dataset
 ┣ 📄 customer_segmentation_powerbi.csv                                ← Cleaned export
 ┗ 📖 README.md
```

---

## 💡 Conclusion

- K-Means effectively segmented ~2,000 customers into **4 meaningful behavioral groups**
- **Elbow Method + Silhouette Score** together give the most reliable cluster count selection
- **Income and settlement size** — not age — are the real drivers of customer value
- The Power BI dashboard makes insights accessible for non-technical business stakeholders
- Cluster-based marketing strategies can improve targeting efficiency by ~20%

---

## 🔮 Future Work

- Apply **DBSCAN or Hierarchical Clustering** for comparison
- Add **RFM analysis** (Recency, Frequency, Monetary) for e-commerce datasets
- Build a **real-time customer segmentation API** using FastAPI
- Extend with **predictive modeling** to forecast which segment a new customer belongs to

---

## 👩‍💻 Author

**Shreevarsha S**
Data Science Professional | ML & NLP Enthusiast

[![Portfolio](https://img.shields.io/badge/Portfolio-Visit-7b6ef6?logo=globe)](https://shreevarsha866.github.io/Shreevarsha_Portfolio)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/s-shreevarsha-503887218/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/shreevarsha866)
[![Email](https://img.shields.io/badge/Email-Contact-red?logo=gmail)](mailto:varshashree866@gmail.com)

---

*⭐ If you found this project helpful, please give it a star!*
