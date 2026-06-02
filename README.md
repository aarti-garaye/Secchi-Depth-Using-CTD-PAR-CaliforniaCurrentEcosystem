# Predictive Modeling of Water Clarity (Secchi Depth) from CTD-PAR Data in the California Current Ecosystem

Estimating water clarity from oceanographic sensor data using machine learning and statistical modeling.

## Overview

Water clarity is a fundamental ecological indicator that influences photosynthesis, primary productivity, and ecosystem structure throughout the water column. Traditionally, water clarity is measured using **Secchi depth**, a visual observation that is dependent on weather conditions, observer judgment, and available ship time.

This project investigates whether **CTD-PAR (Conductivity, Temperature, Depth – Photosynthetically Active Radiation)** measurements collected by CalCOFI can be used to estimate Secchi depth through predictive modeling.

Using CTD sensor profiles collected throughout the California Current Ecosystem (CCE), we develop and evaluate multiple statistical and machine learning approaches to estimate water clarity and expand Secchi depth coverage at stations where direct measurements are unavailable.

---

## Authors

- Aarti Garaye — University of California, Santa Barbara
- Lucas Childs — University of California, Santa Barbara
- Anna Liang — University of California, Santa Barbara

### Advisors

- Dr. Tony Coburn — University of California, Santa Barbara
- Dr. Rasmus Swalethorp — Scripps Institution of Oceanography / CalCOFI

---

## Research Goals

The objectives of this project are:

- Develop predictive models for estimating Secchi depth using CTD-PAR measurements.
- Identify the most informative optical parameters associated with water clarity.
- Compare statistical and machine learning methods including:
  - Polynomial Regression
  - LASSO Regression
  - Elastic Net Regression
  - Generalized Additive Models (GAMs)
  - Random Forests
- Evaluate model performance using:
  - R²
  - RMSE
  - MAE
- Generate Secchi depth estimates for stations lacking direct observations.

---

## Data Sources

### CalCOFI CTD Downcast Data

Collected from CTD-Rosette deployments throughout the California Current Ecosystem (1993–present).

Variables of interest include:

- Photosynthetically Active Radiation (PAR)
- Estimated Chlorophyll
- Phaeopigments
- Beam Attenuation Coefficient
- Latitude and Longitude
- Additional CTD-derived environmental variables

Measurements are recorded approximately every meter throughout the water column.

### CalCOFI Bottle Database

Contains historical oceanographic observations collected at CalCOFI stations (1949–2021).

This project uses:

- Observed Secchi depth measurements
- Cast metadata used to align bottle observations with CTD profiles

---

## Methodology

### Defining the Photic Zone

To characterize light attenuation, a photic depth was calculated using PAR measurements.

Key decisions:

- Baseline PAR value established at **2 meters depth**
- Casts beginning shallower than 2 m were truncated
- Final dataset contained **1,971 observations**
- A **4.7% light threshold** was selected via 10-fold cross-validation

The photic depth is defined as the depth where PAR reaches 4.7% of the baseline PAR value at 2 m.

---

### Feature Engineering

Several robust optical parameters were constructed based on literature review and exploratory analysis.

#### ESTCHL_SUMMED

Cumulative estimated chlorophyll from baseline depth to:

```text
min(photic_depth, median_secchi_depth)
```

#### K_PAR

Diffuse attenuation coefficient calculated using the Beer-Lambert relationship.

#### K_PAR_SLOPE

Slope of the log-linear regression of PAR versus depth.

#### XMISS_SUMMED

Cumulative transmissometer measurements integrated over the photic zone.

---

### Feature Selection

Feature importance was assessed using:

- Random Forest Mean Decrease in Impurity (MDI)
- GAM diagnostics
- Domain knowledge from optical oceanography literature

The three most informative variables were:

1. ESTCHL_SUMMED
2. K_PAR_SLOPE
3. XMISS_SUMMED

These variables were used in the final predictive models.

---

## Models Evaluated

### Random Forest

A nonparametric ensemble model providing the strongest predictive performance.

### Polynomial Regression (Degree 3)

Provides an explicit analytical equation for Secchi depth estimation while capturing nonlinear relationships.

### Polynomial Regression (Degree 2)

A simpler and more interpretable alternative with fewer interaction terms.

---

## Model Performance

All models were trained using:

- Top 3 engineered features
- 80/20 train-test split
- 5-fold cross-validation

Response variable:

```text
log(Secchi Depth)
```

| Model | R² | RMSE | MAE |
|---------|---------|---------|---------|
| Random Forest | 0.7690 | 0.2296 | 0.1781 |
| Polynomial Regression (Degree 3) | 0.7018 | 0.2738 | 0.2130 |
| Polynomial Regression (Degree 2) | 0.6102 | 0.3130 | 0.2516 |

---

## Key Findings

- Random Forest achieved the highest predictive accuracy.
- Polynomial models provide interpretable equations but sacrifice performance.
- ESTCHL_SUMMED consistently emerged as the most influential predictor.
- CTD-PAR measurements serve as a reliable proxy for estimating water clarity.
- Model performance remained stable across nearshore and offshore regions.

---

## Application

After training on the full dataset, the Random Forest model was used to estimate Secchi depth at CalCOFI stations lacking observations.

This increased available Secchi depth coverage by approximately:

**27%**

The resulting expanded dataset can improve:

- Long-term ecosystem monitoring
- Water clarity trend analysis
- Climate anomaly studies
- California Current ecosystem assessments

---

## Comparison with Existing Work

We compared our approach with existing satellite-based Secchi depth estimation methods developed for the California Current Ecosystem.

Our model achieved comparable predictive performance while offering several advantages:

- Less sensitivity to cloud cover
- Less sensitivity to wind conditions
- Less sensitivity to rainfall events
- Direct use of in situ CTD measurements

---

## Future Work

Potential extensions include:

- Incorporating post-2021 CTD observations
- Evaluating additional optical predictors
- Testing deep learning approaches
- Expanding the methodology to other marine ecosystems
- Building operational tools for automated Secchi depth estimation

---

## Repository Structure

TO BE UPDATED

```text
.
├── data/
│   ├── raw/
│   ├── processed/
│   └── external/
│
├── notebooks/
│   ├── exploratory_analysis.ipynb
│   ├── feature_engineering.ipynb
│   └── model_evaluation.ipynb
│
├── src/
│   ├── preprocessing/
│   ├── feature_engineering/
│   ├── modeling/
│   └── visualization/
│
├── results/
│   ├── figures/
│   ├── models/
│   └── predictions/
│
├── README.md
└── requirements.txt
```

---


## Citation

If you use this work, please cite:

```bibtex
@misc{garaye_childs_liang_2026,
  title={Predictive Modeling of Water Clarity (Secchi Depth) from CTD-PAR Data in the California Current Ecosystem},
  author={Garaye, Aarti and Childs, Lucas and Liang, Anna},
  year={2026},
  note={University of California, Santa Barbara Statistics and Applied Probability Capstone Project}
}
```

---

## Acknowledgements

We thank:

- UC Santa Barbara Department of Statistics and Applied Probability
- Scripps Institution of Oceanography
- California Cooperative Oceanic Fisheries Investigations (CalCOFI)
- Dr. Tony Coburn
- Dr. Rasmus Swalethorp

for their guidance, mentorship, and support throughout this project.
