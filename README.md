# London House Price Prediction — Data Science Coursework

**Module:** CMP-N205-0 Data Science | University of Roehampton, Year 2  
**Student:** Yuhang Wang (WAN23614422)

---

## Project Overview

A two-part academic data science project that builds a machine learning model to predict residential property prices in London. The project covers the full data science pipeline — from raw data acquisition and cleaning through to model deployment and business analysis.

**Business Context (UrbanNest Analytics):**
1. Automate property valuations to replace manual appraisals
2. Identify the key attributes that drive price
3. Detect luxury properties for market segmentation

---

## Repository Structure

```
Data_Science/
├── CW1/                                         # Coursework 1 — EDA & Preprocessing
│   ├── Yuhang_Wang.ipynb                        # Jupyter notebook (analysis)
│   ├── CW1_Yuhang_Wang.docx                     # Written submission
│   ├── Raw_London_House_Prices.xlsx             # Original dataset (3,480 records)
│   ├── London_House_Prices_Cleaned_JN.xlsx      # Cleaned dataset (3,465 records)
│   ├── London_House_Prices_Cleaned_Visualise.xlsx
│   ├── Raw.png                                  # Before cleaning screenshot
│   └── clean.png                                # After cleaning screenshot
│
└── CW2/                                         # Coursework 2 — Model & Evaluation
    ├── CW2_Yuhang_Wang.ipynb                    # Jupyter notebook (ML model)
    ├── CW2_Yuhang_Wang.docx                     # Written submission
    ├── CW2_Yuhang_Wang_Report.pdf               # Final report
    ├── Assessment Brief CMP-N205-0 Coursework 2.pdf
    └── London_House_Prices_Cleaned.xlsx         # Dataset used for modelling
```

---

## Coursework 1 — Data Exploration & Preprocessing

**Dataset:** Kaggle — Housing Prices in London  
**Original size:** 3,480 records × 11 features  
**Features:** Property Name, Price, House Type, Area (sq ft), Bedrooms, Bathrooms, Receptions, Location, City/County, Postal Code

### Cleaning Steps

| Step | Action | Records Affected |
|------|--------|-----------------|
| 1 | Removed auto-generated index column | — |
| 2 | Filled 962 missing Location values with `'Unknown'` | 962 |
| 3 | Removed records with 0 bedrooms or bathrooms (invalid) | 10 |
| 4 | Removed extreme low-price outlier (< £200,000) | 1 |
| 5 | Flagged luxury outliers (> £10,000,000) as separate segment | 38 |
| 6 | Removed duplicate records | 4 |

**Final clean dataset:** 3,465 records (29.2% of data required cleaning)

### Visualisations
- Price distribution histogram
- House type breakdown bar chart
- Correlation heatmap of numerical features

### Proposed ML Solution
Multiple Linear Regression — continuous target (price), interpretable coefficients, efficient computation.

---

## Coursework 2 — Model Implementation & Evaluation

### Feature Engineering

| Step | Detail |
|------|--------|
| Outlier removal | Dropped 38 luxury properties (>£10M) — separate market |
| Area Code extraction | `'SW15 1PL'` → `'SW15'` |
| One-Hot Encoding | Top 20 postal area codes |
| Label Encoding | House Type (Bungalow=0 … Studio=7) |
| Feature creation | `Total_Rooms = Bedrooms + Bathrooms + Receptions` (resolved multicollinearity) |

**Final feature matrix:** 3,427 properties × 23 features

### Model

| Setting | Value |
|---------|-------|
| Algorithm | Multiple Linear Regression (scikit-learn + statsmodels OLS) |
| Train / Test split | 80% / 20% (random state = 42) |
| Training records | 2,741 |
| Testing records | 686 |

### Results

| Metric | Value |
|--------|-------|
| R² (test set) | **0.5277** |
| R² (full OLS) | 0.561 |
| RMSE | £927,582 |
| MAE | £563,560 |
| F-statistic | 188.88 (p < 0.0001) |

Adding postal area codes improved R² from 0.4272 → 0.5277.

### Statistically Significant Features (p < 0.05)

| Feature | Coefficient | Significant? |
|---------|-------------|-------------|
| Area in sq ft | +£812 per sq ft | Yes (p < 0.001) |
| House Type | +£124,192 per category | Yes (p < 0.001) |
| Total Rooms | −£24 | No (p = 0.997) |

### Top Location Premiums

| Postal Area | Premium vs Baseline |
|-------------|-------------------|
| SW3 (Knightsbridge/Chelsea) | +£2,373,000 |
| W8 (Kensington) | +£1,980,000 |
| NW8 (St John's Wood) | +£1,367,000 |
| NW3 (Hampstead) | +£857,500 |

---

## Key Findings

1. **Location is the dominant price driver.** SW3 and W8 command premiums of £2–2.4M above baseline.
2. **Size premium:** Each additional 100 sq ft adds approximately £81,200 to value.
3. **Property type matters:** House type is statistically significant with a ~£124K effect per category.
4. **Room count paradox:** Total rooms is negatively correlated with price (not statistically significant), likely because developers in prime areas maximise revenue through smaller premium units.
5. **Luxury segment is distinct:** 38 properties (1.1% of records) inflate the portfolio average by £167,000.
6. **Model limitation:** Linear regression explains ~53% of price variance. Unmodelled factors include proximity to the tube, school catchment areas, and property condition.

---

## Example Prediction

> 1,000 sq ft Flat in SW15 (Putney): **£599,944**

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3 | Core language |
| pandas, numpy | Data manipulation |
| matplotlib, seaborn | Visualisation |
| scikit-learn | Linear Regression, train/test split |
| statsmodels | OLS analysis, VIF, p-values |
| Jupyter Notebook | Interactive analysis |
| Excel (.xlsx) | Data storage |
| Git | Version control |

---

## How to Run

1. Clone the repository
2. Install dependencies: `pip install pandas numpy matplotlib seaborn scikit-learn statsmodels openpyxl`
3. Open `CW1/Yuhang_Wang.ipynb` for data exploration and preprocessing
4. Open `CW2/CW2_Yuhang_Wang.ipynb` for model training and evaluation

---

## Limitations & Future Work

- Linear regression captures only 52.8% of price variance — non-linear models (Random Forest, XGBoost) may improve performance
- Dataset sourced from Kaggle; real-world accuracy would require live market data
- Additional features (tube station distance, school ratings, energy rating, property age) would likely improve predictions significantly
- Luxury properties (>£10M) require a separate dedicated model
