# SEER Cancer Dataset — Exploratory Data Analysis

## Project Overview

This project performs Exploratory Data Analysis (EDA) on the **SEER (Surveillance, Epidemiology, and End Results)** dataset, a publicly available cancer registry maintained by the National Cancer Institute (NCI), USA. The dataset contains patient-level records covering demographics, tumour characteristics, treatment stages, and survival outcomes across multiple cancer types.

The goal of this project is to clean, process, and visualise the data to uncover patterns related to cancer distribution, survival rates, and the relationship between clinical features.

---

## Dataset

- **Source:** SEER (Surveillance, Epidemiology, and End Results) Program — National Cancer Institute
- **File:** `export.csv` (exported from the SEER*Stat software)
- **Key Columns Used:**

| Original Column Name | Renamed To | Description |
|---|---|---|
| Age recode with <1 year olds and 90+ | `Age` | Patient age group |
| Sex | `Sex` | Patient sex |
| Year of diagnosis | `Year_Diagnosis` | Year cancer was diagnosed |
| Site recode ICD-O-3/WHO 2008 | `Site_Recode` | Standardised cancer site code |
| Grade Clinical (2018+) | `Grade` | Tumour grade |
| Histology recode - broad groupings | `Histology` | Histological classification |
| Primary Site - labeled | `Primary_Site` | Organ/site where cancer originated |
| Combined Summary Stage (2004+) | `Stage` | Cancer stage at diagnosis |
| Tumor Size Summary (2016+) | `Tumor_Size` | Size of tumour (mm) |
| Vital status recode | `Vital_Status` | Patient alive or deceased |
| Survival months | `Survival_Months` | Months survived post-diagnosis |
| COD to site recode (1999+) | `Cause_of_Death` | Cause of death (if applicable) |

---

## Project Pipeline

### 1. Data Loading & Inspection
- Loaded the dataset using `pandas`
- Inspected shape, column names, data types, and first few rows

### 2. Column Renaming
- All columns were renamed from verbose SEER labels to short, clean identifiers for easier handling

### 3. Missing Value Handling
- Identified SEER-specific unknown placeholders: `'Blank(s)'`, `'Unknown'`, `'Not applicable'`, `'99'`, `'999'`, `'NA'`
- Replaced all such values with `NaN`
- Computed missing count and missing percentage per column

### 4. Data Cleaning
- Dropped rows with missing values in critical columns: `Grade`, `Tumor_Size`, `Stage`, `Survival_Months`
- Reset the DataFrame index after dropping rows
- Converted `Grade`, `Tumor_Size`, and `Survival_Months` to numeric (integer) types

---

## Analysis & Visualisations

### 1. Cancer Case Distribution by Age and Sex *(Bar Chart)*
- Grouped cases by age group and sex
- Visualised using a grouped bar chart (pink = Female, blue = Male)
- **Insight:** Shows which age groups are most affected and whether there is a sex-based difference in cancer incidence

### 2. Age Distribution by Vital Status *(Box Plot)*
- Compared the age distribution of patients who are **Alive** vs **Deceased**
- Age groups were numerically extracted for plotting
- **Insight:** Reveals whether older patients have higher mortality rates

### 3. Alive vs. Deceased Patient Split *(Pie Chart)*
- Visualised the overall proportion of patients who survived vs. those who died
- **Insight:** Provides a high-level view of mortality in the dataset

### 4. Top 10 Most Frequent Cancer Primary Sites *(Horizontal Bar Chart)*
- Counted the most common cancer primary sites across all patients
- Filtered to the top 10 and plotted using `sns.countplot`
- **Insight:** Identifies which organs/sites account for the majority of cancer cases

### 5. Distribution of Survival Months *(Histogram with KDE)*
- Plotted the distribution of `Survival_Months` across all patients
- Overlaid with a KDE (Kernel Density Estimate) curve
- **Insight:** Shows the spread and skew of survival time — whether most patients survive for a short or long period

### 6. Correlation & Covariance Analysis *(Heatmap)*
- Computed the correlation and covariance matrices for `Age_Numeric`, `Tumor_Size`, and `Survival_Months`
- Plotted a heatmap using `sns.heatmap` with `coolwarm` palette
- **Insight:** Examines whether larger tumours, older age, or longer survival are statistically related

### 7. Survival Rate by Cancer Stage *(Stacked Horizontal Bar Chart)*
- Used a cross-tabulation of `Stage` vs `Vital_Status` (normalised by row)
- Plotted as a 100% stacked bar chart (green = Alive, red = Deceased)
- **Insight:** Compares survival outcomes across stages:
  - *In Situ* — cancer confined to origin, not spread
  - *Localised* — confined to the organ of origin
  - *Regional by Direct Extension* — spread to nearby tissue
  - *Regional to Lymph Nodes* — spread to nearby lymph nodes

---

## Libraries Used

| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, manipulation |
| `numpy` | Numerical operations, NaN handling |
| `matplotlib` | Base plotting framework |
| `seaborn` | Statistical visualisations |

---

## How to Run

1. Clone or download the repository
2. Ensure Python 3.x is installed with the required libraries:
   ```
   pip install pandas numpy matplotlib seaborn
   ```
3. Place your SEER export file as `export.csv` in the working directory (or update the path in Cell 1)
4. Open `SEER.ipynb` in Jupyter Notebook or JupyterLab and run all cells sequentially

---

## Conclusion

This analysis of the SEER cancer dataset reveals several key patterns across demographics, tumour characteristics, and survival outcomes:

- **Age & Sex Distribution:** Cancer incidence peaks in the middle-to-older age groups (50+), with the distribution varying between males and females across age brackets. Younger age groups show comparatively fewer cases, consistent with known epidemiological trends.

- **Vital Status & Age:** The box plot analysis indicates that deceased patients tend to skew slightly older than surviving patients, suggesting age is a contributing factor in cancer mortality — though overlap between the two groups is significant.

- **Survival Split:** A large proportion of patients in the dataset are recorded as deceased, reflecting the severity of cancer diagnoses captured in long-term registry data. The alive cohort represents patients either in remission or diagnosed more recently.

- **Primary Cancer Sites:** The top 10 most frequent cancer sites account for a disproportionately large share of all cases, with a small number of organs (such as breast, lung, and colon) dominating the dataset — which aligns with global cancer incidence statistics.

- **Survival Months Distribution:** The distribution of survival months is right-skewed, with a large number of patients surviving for a short period post-diagnosis. However, a notable tail exists, representing long-term survivors — indicating that outcomes vary significantly depending on cancer type and stage.

- **Correlation Analysis:** The correlation heatmap shows weak-to-moderate relationships between `Age`, `Tumor_Size`, and `Survival_Months`. Notably, larger tumour size shows a slight negative correlation with survival months, suggesting that more advanced tumours are associated with shorter survival — though no single variable is a strong standalone predictor.

- **Stage vs. Survival:** The stacked bar chart clearly demonstrates that cancer stage at diagnosis is a strong determinant of patient outcome. *In Situ* and *Localised* stage patients have a significantly higher survival rate compared to those diagnosed at *Regional* stages, reinforcing the critical importance of early detection in improving cancer outcomes.

**Overall**, the analysis underscores that early-stage diagnosis, younger age at detection, and smaller tumour size are associated with better survival outcomes. These findings are consistent with established oncological research and highlight the value of population-level cancer registries like SEER in informing public health decisions.

---

## Notes

- The dataset is exported from **SEER\*Stat** software and may require an approved data access request from NCI
- Unknown/missing SEER codes (e.g., `99`, `999`, `Blank(s)`) are handled during preprocessing
- All visualisations are generated inline within the Jupyter Notebook
