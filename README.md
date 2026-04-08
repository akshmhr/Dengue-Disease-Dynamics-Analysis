# Dengue Disease Dynamics Analysis — India

Analysis of dengue trends across India using multiple data sources, covering exploratory data analysis, data integration, and forecasting models at both national and state level.

---

## Project Structure

```
├── main.ipynb                    # Main analysis notebook
├── DENGUE_filtered_data.csv      # OpenDengue national case data
├── dengue_ncvbdc_2021_2025.csv   # NCVBDC state-level official data
├── Global_Mobility_Report.csv    # Google Community Mobility data
├── ind_ppp_2020.tif              # WorldPop India population raster
└── README.md
```

---

## Data Sources

| Dataset | Source | Description |
|---------|--------|-------------|
| `DENGUE_filtered_data.csv` | OpenDengue | National-level annual dengue case counts for India, 1991–2024 |
| `dengue_ncvbdc_2021_2025.csv` | NCVBDC / NVBDCP | State and UT-wise reported dengue cases and deaths, 2021–2025 |
| `Global_Mobility_Report.csv` | Google COVID-19 Community Mobility Reports | Daily % change in visits to retail, transit, workplace etc. vs baseline, 2020–2022 |
| `ind_ppp_2020.tif` | WorldPop | India gridded population estimates (PPP, 2020), used for per-capita calculations |

---

## What the Notebook Does

1. **Data Loading & Preprocessing**
   - Loads all four datasets
   - Reshapes the NCVBDC table from wide format to tidy (state × year × cases × deaths)
   - Filters Google Mobility to India state-level rows and aggregates to annual means
   - Joins dengue + population + mobility into a single panel dataset

2. **EDA**
   - National dengue trend 1991–2024
   - Top states by case count (heatmap + bar chart)
   - Annual deaths and case fatality rate
   - Correlation between mobility indicators and dengue incidence
   - Per-capita incidence by state

3. **Modelling**
   - **Linear Regression** on national annual data (train 1991–2018, test 2019–2024, forecast to 2027)
   - **Random Forest** on state-level panel (train 2021–2023, test 2024)

4. **Results & Limitations**

---

## How to Run

### Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
```

Install with:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

Or if using conda:
```bash
conda install pandas numpy matplotlib seaborn scikit-learn
```

### Running the Notebook

1. Make sure all CSV/TIF files are in the same folder as `main.ipynb`
2. Open Jupyter:
```bash
jupyter notebook main.ipynb
```
3. Run all cells top to bottom (Kernel → Restart & Run All)

> **Note:** The `ind_ppp_2020.tif` file is a 1.7 GB raster. The notebook does not read it directly — state-level population estimates derived from Census 2011 projections are hardcoded instead, so the notebook runs fine without the raster loaded into memory.

---

## Key Findings

- India's dengue cases have grown at ~11.6% per year since 1991
- 2023 saw the highest recorded cases: **289,235**
- The 2020 dip is likely explained by COVID-19 lockdowns reducing outdoor exposure and possibly surveillance gaps
- **Karnataka, Tamil Nadu, and Kerala** had the highest burden in 2024
- Previous year's case count is the single strongest predictor in the Random Forest model (58% feature importance)
- Transit and workplace mobility show a negative correlation with incidence — consistent with reduced vector exposure during mobility-restricted periods
