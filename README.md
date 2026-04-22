# IDS701 Final Project — Housing Choice Vouchers & SAFMR Policy

This repository analyzes Housing Choice Voucher (HCV) tract-level data for **Dallas County (treatment)** versus **Harris County / Houston (control)** around the **2018 Small Area Fair Market Rent (SAFMR)** policy context. The work combines preprocessing, exploratory analysis, difference-in-differences (DiD), visualization, geospatial exploration, and an **event study** of dynamic treatment effects relative to 2018.

---

## Repository layout

| Path | Description |
|------|-------------|
| `data/clean_hcv_data.csv` | Panel of tract-year observations used in regression and plots |
| `00_preprocessing.ipynb` | Data cleaning and construction of analysis-ready fields |
| `01_eda_pretrends.ipynb` | Exploratory analysis and pre-trend checks |
| `02_DiD.ipynb` | Main DiD specification (e.g., tract and time fixed effects via `linearmodels`) |
| `03_DiD_graph.ipynb` | Descriptive figures comparing treatment vs control over time |
| `04_Geospatial.ipynb` | Map-based / spatial views of the data |
| `05_event_study.ipynb` | Event study: `treatment × year` interactions, plots, parallel-trends check |
| `outputs/` | Generated tables and figures from the event study notebook |
| `LICENSE` | Project license |

Figures committed at the repo root (e.g. `02_pretrend_number_reported.png`, `03_pretrend_tpoverty.png`, `04_balance_check.png`) come from earlier notebooks in the pipeline.

---

## Data (`data/clean_hcv_data.csv`)

The cleaned panel includes tract identifiers, year, a **treatment** indicator (Dallas vs Houston), voucher counts, rent and income controls, and related covariates. Key columns used in DiD and the event study include:

- `code` — census tract identifier  
- `year` — calendar year (panel)  
- `treatment` — 1 = Dallas (treatment), 0 = Houston (control)  
- `number_reported` — outcome used in DiD / event study in this workflow  
- `post` — indicator for periods after the policy window (as defined in the notebooks)  
- Additional controls: `rent_per_month`, `hh_income`, `tpoverty`, `poverty_indicator`, etc.

---

## Event study (`05_event_study.ipynb`)

The event study notebook:

1. Estimates **dynamic effects** by interacting `treatment` with year indicators (with **2018** as the reference period for interpretation).  
2. Plots coefficients **over event time** (`year − 2018`) and **over calendar year**.  
3. Assesses **parallel trends** by testing whether pre-2018 effects (relative to 2018) are jointly zero.

Outputs (after you run the notebook):

- `outputs/event_study_coefficients.csv` — year, event time, coefficient, standard error, and confidence bands  
- `outputs/event_study_coefficients.png` — plot vs event time  
- `outputs/event_study_coefficients_by_year.png` — plot vs calendar year  

The first code cell ensures `statsmodels` is available in the active Jupyter kernel (installs it if missing).

---

## How to run

**Requirements:** Python 3.10+ recommended; Jupyter. Typical packages: `pandas`, `numpy`, `matplotlib`, `seaborn` (where used), `statsmodels`, and `linearmodels` for panel DiD in `02_DiD.ipynb`.

```bash
cd IDS701_Final_Project
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install pandas numpy matplotlib seaborn statsmodels linearmodels jupyter
jupyter lab   # or: jupyter notebook
```

Open notebooks in numeric order (`00` → `05`) or jump to `05_event_study.ipynb` if you only need the event study, after `clean_hcv_data.csv` is present under `data/`.

---

## License

See [`LICENSE`](LICENSE) for terms.
