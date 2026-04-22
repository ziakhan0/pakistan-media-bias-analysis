# 📰 Pakistan Media Dataset — Analysis Project

**Data Cleaning · EDA · Bias Analysis**

| | |
|---|---|
| **Student** | Zia ur Rehman — `23F-0547` |
| **Dataset** | `pakistan-media-dataset-synthetic.csv` |
| **Notebook** | `pakistan-media-bias-analysis.ipynb` |

---

## 📋 Project Overview

This project analyzes a **synthetic dataset** of Pakistan's media outlets. The work focuses on three core areas:

1. **Data Cleaning** — Transforming raw data into a consistent, usable format
2. **Exploratory Data Analysis (EDA)** — Understanding patterns, distributions, and relationships
3. **Bias Analysis** — Exploring the relationship between political affiliation and revenue

---

## 🗂️ Dataset Columns

| Column | Description |
|---|---|
| `Channel` | Name of the TV/media channel |
| `Journalist` | Name of the reporter or journalist |
| `Newspaper` | Name of the newspaper |
| `Region` / `City` | Geographic area of coverage |
| `Topic` | News topic category |
| `Headline` | News headline text |
| `Language` | Language of the broadcast (Urdu, English, etc.) |
| `PoliticalAffiliation` | Political leaning of the channel |
| `Date` | Publication or broadcast date |
| `Airtime` | Airtime duration in minutes |
| `Ratings` | Channel ratings (0–100 scale) |
| `SentimentScore` | Sentiment score of the content |
| `BiasScore` | Measured bias level |
| `Viewership` | Total viewership count |
| `Shares` | Number of social media shares |
| `SocialMediaInteractions` | Total social media interactions |
| `Revenue` | Revenue earned (in PKR) |
| `AdSpend` | Advertisement spend (in PKR) |
| `ControversyFlag` | Whether the content was controversial (0/1) |
| `MissingDataFlag` | Indicator for missing data entries |

---

## 🔧 Project Workflow

### 1. Data Loading & Initial Inspection
- Loaded the dataset using `pd.read_csv()`
- Inspected structure with `df.info()` and `df.shape`
- Calculated null value percentages — some columns had up to ~50% missing data

---

### 2. Data Cleaning

**String Standardization**
- All text columns (Channel, Journalist, Region, etc.) were standardized using `.strip().str.title()`
- Extra whitespace was removed using regex normalization

**Money Columns (Revenue & AdSpend)**
- Data contained mixed formats: `"5 million"`, `"50 lakh"`, `"1 crore"`, `"4,452,987"`
- A custom `parse_money_to_pkr()` function was built to convert all formats into numeric PKR values

**Numeric Columns**
- Applied `pd.to_numeric(errors='coerce')` across all numeric fields
- `Ratings` values were clipped to the valid range of 0–100
- Negative values in `Airtime` were replaced with `NaN`

**Date Parsing**
- All dates were parsed using `pd.to_datetime(errors='coerce')`

**Missing Value Treatment**

| Column | Strategy | Reason |
|---|---|---|
| `SocialMediaInteractions` | Fill with `0` | Missing implies no engagement |
| `Shares` | Fill with `0` | Same rationale |
| `BiasScore` | Fill with `2` (neutral) | Represents a neutral point on the scale |
| `SentimentScore` | Fill with `0` | Represents neutral sentiment |
| `Airtime` | Median imputation | ~50% missing; robust to outliers |
| `Ratings` | Median imputation | ~49% missing |
| `Viewership` | Median imputation | ~33% missing |
| `PoliticalAffiliation` | Fill with `'unknown'` | Categorical — no meaningful default |
| `Language` | Fill with `'unknown'` | Categorical — no meaningful default |
| `ControversyFlag` | Map Yes/No → 1/0, fill `0` | Binary flag standardization |

---

### 3. Outlier Visualization
- Generated **boxplots** for `Viewership`, `Revenue`, `AdSpend`, `Airtime`, and `Shares`
- Identified extreme outliers in revenue and viewership distributions

---

### 4. Exploratory Data Analysis (EDA)

- **Distributions** — Histograms with KDE curves for `Ratings`, `BiasScore`, and `SentimentScore`
- **Correlation Heatmap** — Full correlation matrix across all numeric columns
- **Top Topics** — Bar chart showing the 15 most frequently covered news topics

---

### 5. Bias Analysis

**Political Affiliation Groups:**
- `pro-govt` — Pro-government media outlets
- `opposition` — Opposition-leaning outlets
- `neutral` — Neutral outlets
- `other` — All remaining outlets

**Analysis Performed:**
- **Boxplot** of Revenue vs Political Affiliation (symlog scale for readability)
- **Mann-Whitney U Test** — Statistical comparison of revenue between pro-govt and all other channels
  - `p-value < 0.05` → Statistically significant difference in revenue
  - `p-value ≥ 0.05` → No statistically significant difference

---

### 6. Final Output
- The cleaned dataset was saved as `pakistan-media-dataset-cleaned.csv`

---

## 🛠️ Libraries Used

```
pandas       — Data manipulation and analysis
numpy        — Numerical computations
matplotlib   — Base plotting
seaborn      — Statistical data visualization
scipy.stats  — Mann-Whitney U statistical test
re           — Regular expressions for text cleaning
datetime     — Date and time parsing
```

---

## ▶️ How to Run

1. Install the required libraries:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy
   ```

2. Place `pakistan-media-dataset-synthetic.csv` in the same directory as the notebook

3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook 23F-0547.ipynb
   ```

4. Run all cells via `Kernel > Restart & Run All`

---

## 📁 Project Structure

```
📦 Pakistan-Media-Analysis/
├── pakistan-media-bias-analysis.ipynb     ← Main notebook
├── pakistan-media-dataset-synthetic.csv   ← Input dataset
├── pakistan-media-dataset-cleaned.csv     ← Cleaned output (generated on run)
└── README.md                              ← This file
```

---

*Developed by: Zia ur Rehman — 23F-0547*
