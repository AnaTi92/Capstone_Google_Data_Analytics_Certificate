# 🧩 Capstone Project — Social Inclusion, Physical Health, and Wellbeing in Older Europeans

**Author:** Ana Tivold  
**Program:** Google Data Analytics Professional Certificate — Capstone Project  
**Date:** October 2025  
**Repository:** Capstone_Google_Data_Analytics_Certificate  
**License:** MIT  

---

## 📘 Project Overview
This project investigates how **social inclusion**, **physical health**, and **partnership status** relate to **quality of life** and **mental health** among adults aged 50 + across Europe, using harmonized microdata from the *Survey of Health, Ageing and Retirement in Europe* (SHARE) – *easySHARE Release 9.0.0*.

**Key Research Questions**
1. Does providing or receiving help (social support) predict higher quality of life (CASP-12)?
2. Does living with a partner predict better quality of life?
3. How are physical health indicators (grip strength, chronic diseases) associated with mental health (EURO-D)?
4. Do these relationships differ by gender or European region (North, South, East, West)?

---

## 🧠 Data Source
**Dataset:** easySHARE Release 9.0.0 (June 2024)  
**Provider:** SHARE-ERIC (Survey of Health, Ageing and Retirement in Europe)  
**DOI:** [10.6103/SHARE.easy.900](https://doi.org/10.6103/SHARE.easy.900)

The dataset includes individuals aged 50 + from 28 European countries and Israel.  
It integrates demographic, social, health, and economic indicators harmonized across **Waves 1–9 (2004–2021)**.  
All analyses in this project use **Wave 9 (2021)** respondents aged 50 + .

> ⚠️ For copyright and data-protection reasons, **raw SHARE data are not uploaded** to this repository.  
> Access requires academic registration at [https://share-eric.eu/data/data-access](https://share-eric.eu/data/data-access).

---

## 📊 Variables of Interest
| Concept | Variable(s) | Description |
|----------|--------------|-------------|
| Quality of life | `casp` | CASP-12 index (12–48; higher = better) |
| Mental health | `eurod` | EURO-D depression scale (0–12; higher = worse) |
| Social support | `sp002_mod`, `sp008_` | Received / provided help outside household |
| Partner status | `partnerinhh` | 1 = with partner, 3 = without partner |
| Physical health | `maxgrip`, `chronic_mod`, `adla`, `iadla` | Grip strength, chronic conditions, functional limitations |
| Controls | `age`, `female`, `isced1997_r`, `co007_`, `country_mod` | Demographics, SES, region |

Variable definitions follow the *easySHARE 9.0.0 Release Guide* (SHARE-ERIC 2024) and  
the *Scales Manual 9.0.0* (Gruber et al., 2024).

---

## 🧩 Repository Structure
The repository is organized as follows:

```text
Capstone_Google_Data_Analytics_Certificate/
│
├── report/
│   └── Capstone_Report.pdf                     # Final Capstone report (APA 7 structure, Appendices A & B)
│
├── scripts/
│   └── Capstone_analysis.Rmd                   # R Markdown workflow (data cleaning, regression, diagnostics, visualization)
│
├── visuals/
│   ├── euro_d_grip_strength.png                # Relationship between grip strength and depressive symptoms (EURO-D)
│   ├── quality_of_life_providing_help.png      # Quality of life (CASP-12) by providing help
│   ├── quality_of_life_receiving_help.png      # Quality of life (CASP-12) by receiving help
│   └── region_x_partner.png                    # CASP-12 by region and partner status
│
├── data/
│   └── codebook_capstone_easyshare_recoded.xlsx # Variable definitions and coding notes (no raw data)
│
├── LICENSE                                     # MIT License
├── .gitignore                                  # Ignored temporary or confidential files
└── README.md                                   # Project documentation (you are here)

---

## 🧮 Methodology

All analyses were conducted in **RStudio 2024.09** using **R v4.4.1** and the following key packages:

- `tidyverse` – data wrangling and visualization  
- `broom`, `dplyr`, `lmtest`, `sandwich`, `car` – regression and diagnostics  
- `ggplot2`, `ggeffects` – plots and marginal effects  

**Modeling approach**
- Multiple linear regressions with **HC3 heteroskedasticity-robust SEs**
- Nonlinear checks (quadratic chronic term)
- Interaction terms for gender and region (moderation)
- Post-estimation diagnostics: Ramsey RESET, Breusch–Pagan, VIF, normality tests

---

## 📈 Key Findings

| Research Question | Main Result | Interpretation |
|--------------------|-------------|----------------|
| **RQ1 – Social support → Quality of life (CASP-12)** | Providing help ↑ CASP (+0.12 SD), Receiving help ↓ CASP (−0.14 SD) | Active support provision enhances wellbeing; dependence reduces it. |
| **RQ2 – Partner → Quality of life** | Partner presence β = +0.15, *p* < .001 | Partnership confers clear wellbeing benefits. |
| **RQ3 – Physical health → Mental health (EURO-D)** | Grip strength β = −0.16; Chronic burden β = +0.13 | Better physical vitality and fewer chronic illnesses lower depression. |
| **RQ4 – Moderation by gender & region** | Gender ns; Region (East vs West): stronger positive effect of help & partner in East | Social ties matter more where structural supports are weaker. |

---

## 🧾 Reproducibility
To reproduce results locally:
```r
# 1. Load packages
library(tidyverse); library(lmtest); library(sandwich)

# 2. Load prepared dataset (not provided here)
load("data/easyshare_wave9_cleaned.Rda")

# 3. Run regression
model <- lm(casp_z ~ received_help_bin + provided_help_bin + age_z +
             female + isced1997_r + income_difficulty_bin +
             maxgrip_z + chronic_mod_z + I(chronic_mod_z^2) +
             adla_z + iadla_z + region4, data = df)
coeftest(model, vcov = vcovHC(model, type = "HC3"))

---

📚 References

SHARE-ERIC (2024). easySHARE Release 9.0.0 [Dataset]. SHARE ERIC. https://doi.org/10.6103/SHARE.easy.900

Gruber, S., Wagner, M., & Batta, F. (2024). Scales and Multi-Item Indicators in SHARE (Version 9.0.0). SHARE Berlin Institute. https://doi.org/10.6103/scmn.900

Hyde, M., Wiggins, R. D., Higgs, P., & Blane, D. B. (2003). A measure of quality of life in early old age: The CASP-19 scale. Aging & Mental Health, 7(3), 186–194.

Prince, M. J., et al. (1999). Development of the EURO-D scale: A European Union initiative to compare depression symptoms. The British Journal of Psychiatry, 174(4), 330–338.

Andersen-Ranberg, K., Petersen, I., Frederiksen, H., Mackenbach, J. P., & Christensen, K. (2009). Cross-national differences in grip strength among Europeans 50 +. European Journal of Ageing, 6(3), 227–236.

---

⚖️ Citation and Use

If you use this repository or derived materials, please cite as:

Tivold, A. (2025). Social Inclusion, Physical Health, and Wellbeing in Older Europeans: Insights from easySHARE Release 9.0.0. GitHub repository: https://github.com/AnaTi92/Capstone_Google_Data_Analytics_Certificate

---

💡 Acknowledgements

This project uses data from the Survey of Health, Ageing and Retirement in Europe (SHARE), coordinated by SHARE-ERIC.
The author acknowledges the SHARE-ERIC consortium for providing harmonized data and documentation under the DOI 10.6103/SHARE.easy.900.
