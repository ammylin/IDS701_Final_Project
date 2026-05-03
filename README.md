# Expanding Residential Choice: 2018 SAFMR Implementation Analysis

This repository evaluates the impact of the Small Area Fair Market Rent (SAFMR) policy mandate on the residential concentration of Housing Choice Voucher (HCV) holders. Using a quasi-experimental Difference-in-Differences (DiD) design, this study compares Dallas County (treatment) against Harris County (control) to determine whether transitioning from metro-wide rent ceilings to ZIP code-level payment standards disperses voucher households into higher-opportunity areas.

This repository contains the empirical implementation supporting the findings in the accompanying policy memo.

---

## Project Overview

The federal HCV program historically used a single Fair Market Rent (FMR) for entire metropolitan areas, often pricing voucher holders out of high-opportunity neighborhoods while over-subsidizing units in high-poverty areas.

In 2018, HUD mandated SAFMRs in 24 metropolitan areas. This reform replaced metro-wide rent ceilings with ZIP code-level payment standards, creating a natural policy experiment.

This analysis finds that the SAFMR mandate is associated with a reduction of approximately **11.2 fewer HCV households per tract in Dallas County** relative to the Harris County counterfactual.

---

## Policy Context

This project is based on a policy memo prepared for the U.S. Department of Housing and Urban Development (HUD), Office of Policy Development and Research.

The 2018 SAFMR mandate allows for causal inference because:
- Policy adoption was federally determined rather than locally chosen
- Treated and untreated metropolitan areas can be compared over time

The findings are intended to inform:
- Whether SAFMR should be expanded nationally  
- Whether pricing reforms should be paired with mobility support policies  

---

## Repository Layout

| Path | Description |
|------|-------------|
| `data/` | Raw HUD Excel files (`TRACT_MO_WY`), NHGIS census data, and cleaned panels (`clean_hcv_data.csv`, `clean_rc_data.csv`) |
| `outputs/` | Resulting CSVs and visualizations from the event study |
| `00_preprocessing.ipynb` | Constructs longitudinal panel from raw, tract-level HCV program data, and merges racial demographic data for desegregation analysis |
| `01_eda_pretrends.ipynb` | Baseline visualization and pre-trend validation |
| `02_DiD.ipynb` | Main DiD estimation with tract and year fixed effects |
| `03_DiD_graph.ipynb` | Comparative trend visualization |
| `04_Geospatial.ipynb` | Maps spatial distribution of voucher households |
| `05_event_study.ipynb` | Dynamic treatment effects and parallel trends testing |
| `06_geospatial_change_race.ipynb` | Desegregation analysis by tract composition |

---

## Data Description

The primary dataset (`data/clean_hcv_data.csv`) is a panel spanning **2014–2022** at the census tract level.

**Key variables:**
- `number_reported`: Count of active HCV households per tract (primary outcome)
- `treatment`: Binary indicator (1 = Dallas County; 0 = Harris County)
- `tpoverty`: Percentage of residents below the poverty line
- **Racial composition indicators:** High-minority tracts (≥70% non-White)

---

## Methodology

The analysis uses a Difference-in-Differences framework:

Y_it = β₀ + β₁(Treatment_i × Post_t) + α_i + γ_t + ε_it


Where:
- α_i = tract fixed effects  
- γ_t = year fixed effects  

An event study specification (see `05_event_study.ipynb`) interacts treatment status with year indicators, using **2017 as the reference year** to evaluate pre-policy trends and post-policy divergence.

---

## Key Findings

### Quantitative Impact
After 2018, Dallas County experienced a statistically significant reduction in voucher concentration relative to Harris County, decreasing the concentration gap by more than **25%**.

### Racial Desegregation
High-minority tracts in the treatment area lost an average of **15.7 voucher households per tract**, compared to **9.3** in lower-minority tracts.

### Geospatial Shift
The number of high-poverty tracts in Dallas with high voucher density declined from **59 in 2015 to 35 by 2022**, indicating spatial de-concentration.

### Dynamic Effects
The policy effect emerges gradually over time, consistent with housing market frictions such as lease cycles, search costs, and landlord participation.

---

## Limitations

- **Pre-trend differences:** Dallas and Harris County exhibit some divergence prior to 2018, introducing uncertainty into causal estimates  
- **Data suppression:** HUD censors tracts with fewer than 11 households, potentially understating movement into high-opportunity areas  
- **No origin-destination tracking:** The dataset captures outflows but not where households relocate  
- **Non-financial barriers:** Landlord discrimination, housing supply constraints, and search frictions are not directly modeled  

---

## Requirements and Usage

**Environment:**
- Python 3.10+

**Core Libraries:**
- pandas  
- numpy  
- matplotlib  
- seaborn  
- statsmodels  
- linearmodels  

**Workflow:**
1. Run `00_preprocessing.ipynb` to generate clean datasets from raw files  
2. Run `01`–`04` for exploratory analysis and baseline DiD results  
3. Run `05_event_study.ipynb` for dynamic treatment effects  
4. Run `06_geospatial_change_race.ipynb` for desegregation analysis  

---

## Contributors

- **Lingyue Hao**
- **He Jiang** 
- **Ammy Lin**
- **Myla Simmons**
- **Anvita Suresh**

---

## License

This project is licensed under the terms of the `LICENSE` file included in this repository.
