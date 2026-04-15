# 🏃‍♂️ Human Activity Recognition (HAR) – Data Analysis  
**Multivariate Analysis • PCA • Factor Analysis • ICA • Sensor Data Modeling**

This repository contains a full multivariate analysis of the Human Activity Recognition (HAR) dataset collected from smartphone sensors. The goal is to uncover latent structures, evaluate redundancy among features, and compare PCA, FA, and ICA in modeling human motion signals.

> “The primary objective of this report is to analyse the latent underlying structures of this dataset using multivariate methods… and interpret the resulting latent dimensions in the context of human physical activity.”

---

## 📌 Dataset Overview

The dataset is a simplified version of the UCI HAR dataset and includes:

- **30 volunteers** performing **6 activities**  
  - Walking  
  - Walking Upstairs  
  - Walking Downstairs  
  - Sitting  
  - Standing  
  - Laying  
- **33 features total**  
  - **20 time‑domain features**  
  - **13 frequency‑domain features** (via FFT)  
- **10,299 observations**  
- **No missing values**

All features are normalized to **[-1, 1]**, enabling fair comparison across sensors.

---

## 🔍 Data Inspection

### Outliers
- Outliers detected using **z-score > 5** (very conservative).
- Fewer than **50 outliers per feature** → **< 1%** of data.
- Outliers mainly appear in **3‑axial raw signals** (e.g., tBodyAcc-mean()-Z).
- Magnitude and frequency-domain features show **almost no outliers**.

Interpretation:
- Directional axes (X/Y/Z) capture abrupt motion → more variability.
- Magnitude and FFT features are smoother → more stable.

> “Retaining these outliers preserves the natural variability of human motion… important for meaningful latent structure analysis.”

---

## 📊 Exploratory Analysis

### Boxplots (Figure 1)
- All features normalized to [-1, 1], but **variance differs substantially**.
- Time-domain 3‑axial signals show **larger variability**.
- Magnitude and frequency-domain features are **more stable**.

Implications:
- Standardization is required before multivariate modeling.
- Variance differences suggest **non-normality** and **heterogeneous structure**.

---

## 🔥 Correlation Structure (Figure 2)

### Key patterns:
- **Strong correlation block** (r > 0.8):  
  - 13 frequency-domain features  
  - 5 magnitude time-domain features  
  → Suggests a **shared latent factor** (overall movement intensity)

- **Weak correlations** between:  
  - 3‑axial time-domain features  
  - frequency/magnitude features  
  → Suggests **independent signal sources**

### Methodological implications:
- PCA is appropriate for dimensionality reduction.  
- FA is suitable for modeling shared covariance among magnitude/FFT features.  
- ICA is promising because **independence** exists across feature groups.  
- CCA/MLR are **not suitable** due to lack of strong cross-block correlations.

---

## 🧪 Normality Assessment

### QQ Plots (Figure 3)
- Most features deviate from the normal line → **non-normal distributions**.

### Mardia’s Test
- Skewness = **623.41**, p = 0.0  
- Kurtosis = **3244.07**, p = 0.0  
→ Strong rejection of multivariate normality.

Implications:
- FA assumptions partially violated.  
- ICA is **well justified** (requires non-Gaussianity).  
- PCA remains valid but sensitive to heavy tails.

---

## 🧮 Principal Component Analysis (PCA)

### PCA Findings (Figure 4)
- **PC1 explains ~50%** of total variance.  
- **PC1–PC6 explain ~75%** of variance.  
- **PC1–PC7 explain ~80%** of variance.

### Component selection:
- Kaiser criterion → retain **6 PCs**  
- Eigenvalue ratios (>2) support similar range  
- Scree plot elbow at **PC1–PC2**

Conclusion:  
The 33-dimensional dataset can be reduced to **6–7 principal components** with minimal information loss.

---

## 🧱 Factor Analysis (FA)

### Suitability
- **KMO = 0.9291** → excellent factorability  
- Bartlett test unstable numerically, but KMO strongly supports FA

### Factor selection (Figure 5)
- Parallel analysis → **6 factors**  
- Likelihood ratio test → **6-factor model fits adequately (p > 0.05)**

### Interpretation
- FA captures shared structure among:
  - frequency-domain features  
  - magnitude features  
- 3‑axial time-domain features load weakly → partially independent

Conclusion:  
FA reveals **latent movement-intensity factors**, but does not fully capture independent directional signals.

---

## 🧬 Independent Component Analysis (ICA)

ICA is motivated by:
- strong non-normality  
- weak correlations across feature groups  
- multiple independent physical processes (gravity, body acceleration, jerk, gyro)

ICA extracts statistically independent components that likely correspond to:
- **gravity-related motion**
- **body acceleration**
- **jerk signals**
- **rotational motion (gyroscope)**

ICA complements PCA/FA by revealing **independent signal sources** rather than covariance-based structure.

---

## 🧠 Method Comparison

| Method | Captures | Strengths | Limitations |
|--------|----------|-----------|-------------|
| **PCA** | Max variance directions | Great for dimensionality reduction | Sensitive to heavy tails |
| **FA** | Shared covariance | Strong for magnitude/FFT block | Assumes MVN; weaker for 3‑axial signals |
| **ICA** | Independent sources | Ideal for non-Gaussian sensor data | Components may be unstable |

Conclusion:  
HAR data is best understood as a combination of **shared movement-intensity factors** and **independent directional/rotational components**.

---

## 📁 Repository Structure

```text
├── data/                 # HAR dataset (not included)
├── notebooks/            # EDA, PCA, FA, ICA analysis notebooks
├── src/                  # Modular Python scripts for analysis
├── figures/              # Exported plots (boxplots, heatmaps, PCA, FA, ICA)
└── README.md             # Project documentation
