# Impact of Working Hours on Life Satisfaction

> **Data Science Fundamentals Module Project**

---

## Overview

This project investigates whether differences in average annual working hours across countries are associated with variations in **life satisfaction** and **mental health indicators** (anxiety and depressive disorders). The analysis spans a 10-year longitudinal period (2010–2019) covering 113 countries, and examines how the relationship between working hours and well-being shifts across different economic contexts.

The findings are intended to inform **policymakers, employers, and workers** seeking evidence-based guidance on labor policy and work-life balance.

---

## Research Question

> *Are differences in average working hours associated with variations in life satisfaction and mental health across countries — and does a country's economic status change this relationship?*

---

## Project Structure

```
├── SDAF_Team_4_Final.ipynb   # Main analysis notebook (Colab)
├── data/
│   ├── annual-working-hours-per-worker.csv   # DS1: Our World in Data
│   ├── oecd-working-hour.csv                 # DS2: OECD (explored, excluded)
│   ├── mental-illness.csv                    # DS3: Kaggle – Mental Health
│   └── world-happiness-record.csv            # DS4: Kaggle – World Happiness Report
└── README.md
```

---

## Datasets

| # | Dataset | Source | Coverage |
|---|---------|--------|----------|
| DS1 | Annual Working Hours per Worker | [Our World in Data](https://ourworldindata.org/grapher/annual-working-hours-per-worker) | 130 countries, 1870–2023 |
| DS2 | Avg. Annual Hours Worked per Worker | [OECD Data Explorer](https://data-explorer.oecd.org/) | 46 OECD countries *(excluded — subset of DS1)* |
| DS3 | Mental Illness Prevalence | [Kaggle](https://www.kaggle.com/datasets/imtkaggleteam/mental-health) | 200+ entities, 1990–2019 |
| DS4 | World Happiness Report | [Kaggle](https://www.kaggle.com/datasets/usamabuttar/world-happiness-report-2005-present) | 165 countries, 2005–2022 |

---

## Methodology

### 1. Ask
Defined the research question and identified key quality metrics — primarily the Spearman correlation coefficient between working hours and well-being indicators, supplemented by descriptive statistics and subgroup comparisons.

### 2. Prepare
Explored all four datasets for completeness, interpretability, and source credibility. DS2 was excluded due to redundant country coverage. DS1, DS3, and DS4 were selected for integration.

### 3. Process
The data preparation pipeline performs the following operations:

- **Country name standardisation** — used `thefuzz` (fuzzy string matching) to identify and resolve naming inconsistencies across datasets (e.g., `"Turkiye"` → `"Turkey"`, `"Congo (Brazzaville)"` → `"Congo"`)
- **Column selection and renaming** — retained relevant variables and unified column names to allow SQL-style joins
- **Dataset merging** — performed inner joins on `Country` and `Year` using `pandasql`, filtered to 2010–2019
- **Missing value imputation** — countries with fewer than 8 years of data were dropped; remaining gaps filled via linear interpolation, then forward/backward fill; global column means used as a last resort — resulting in **0% missing values**
- **Economic group classification** — countries classified as Developed (GDP ≥ $30,000), Developing ($5,000–$30,000), or Underdeveloped (<$5,000) based on their 10-year average real GDP per capita
- **Working hours categorisation** — countries grouped into Low / Medium / High tiers using 33rd and 66th percentile thresholds for balance

### 4. Analyse

Three analytical approaches were applied:

**Descriptive Analysis**
- Time-series line plots of happiness, freedom of choice, anxiety, and depressive disorder rates grouped by working hours category (2010–2019)
- Bar charts of country distributions by economic group and working hours tier

**Diagnostic Analysis — Correlation**
- Spearman rank correlation between annual working hours and each well-being indicator
- Analysis stratified by economic group (Underdeveloped / Developing / Developed)
- Regression scatter plots with annotated Spearman ρ per subgroup

**Hypothesis Testing**
- Chi-square test of independence between economic group and working hours category
- Result: statistically significant association (p < 0.05), confirming that economic development influences working hour patterns

---

## Key Findings

| Economic Group | Life Satisfaction | Freedom of Choice | Anxiety | Depression |
|---|---|---|---|---|
| **Developed** | ↓ Negative | ↓ Negative | ↓ Negative | ↓ Negative |
| **Developing** | ↓ Negative | ↑ Positive | ↑ Positive | ↑ Positive |
| **Underdeveloped** | ↑ Positive | ↑ Small positive | ≈ No effect | ↓ Negative |

Longer hours generally correlate with lower life satisfaction, but the direction and strength of relationships vary significantly by economic context — a "one-size-fits-all" labor policy is not supported by the evidence.

---

## Policy Implications

- **Developed**: Reduce working hours through flexible work and paid leave legislation to improve life satisfaction and mental health.
- **Developing**: Offer workplace mental health support (counselling, flexible hours) to counter burnout from long working hours.
- **Underdeveloped**: Prioritise job security and income stability; complement employment growth with accessible mental health programs.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 (Google Colab) | Analysis environment |
| `pandas`, `numpy` | Data wrangling and aggregation |
| `pandasql` | SQL-style multi-dataset joins |
| `thefuzz` | Fuzzy string matching for country name standardisation |
| `scipy.stats` | Spearman correlation, Chi-square test |
| `matplotlib`, `seaborn` | Visualisation |

---

## Installation & Usage

> The notebook was built for **Google Colab** with data files stored on Google Drive.

### Running locally

```bash
# Clone the repository
git clone https://github.com/pgphuc0408/Working-Hours-Life-Satisfaction.git
cd Working-Hours-Life-Satisfaction

# Install dependencies
pip install pandas numpy matplotlib seaborn scipy pandasql thefuzz
```

Then open `SDAF_Team_4_Final.ipynb` in Jupyter or VS Code. Update the file paths at the top of each data-loading cell to point to your local `data/` directory instead of Google Drive.

### Running on Colab

1. Upload the notebook to [Google Colab](https://colab.research.google.com/)
2. Mount your Google Drive and place all four CSV files in `MyDrive/data/`
3. Run all cells in order

---

## Data Ethics

All results are presented at the population level in non-stigmatising language to avoid labelling countries unfairly on mental health. No personally identifiable data was used; all datasets are aggregated country-year statistics.

---

## References

- Ezekekwu, E., Johnson, C., Karimi, S., Lorenz, D., & Antimisiaris, D. (2024). A longitudinal analysis of long working hours and the onset of psychological distress. *Journal of Occupational and Environmental Medicine, 67*(1), 11–18. https://doi.org/10.1097/jom.0000000000003231
- Ahn, S. (2017). Working hours and depressive symptoms over 7 years: evidence from a Korean panel study. *International Archives of Occupational and Environmental Health, 91*(3), 273–283. https://doi.org/10.1007/s00420-017-1278-z
- Cho, S., Ki, M., Kim, K., Ju, Y., Paek, D., & Lee, W. (2015). Working hours and self-rated health over 7 years: gender differences in a Korean longitudinal study. *BMC Public Health, 15*(1), 1287. https://doi.org/10.1186/s12889-015-2641-1

