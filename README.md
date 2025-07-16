# HbA1c & Hospital Readmission  
*Revisiting a 2014 diabetes study with a causal‑AI lens*

---

## 📌 Overview
In 2014 researchers reported that **measuring HbA1c appeared “protective” against early readmission** among hospitalised diabetes patients (1999 – 2008 data, 130 US hospitals).  
More than a decade—and major AI advances—later, I’m revisiting the same public dataset for two reasons:

1. **Personal curiosity** about diabetes management.  
2. **Methodological doubts**: Is HbA1c measurement itself protective, or merely a proxy for better care?

Rather than replicate the original statistics, I’ll probe the data with modern **causal‑inference & ML** techniques to test that claim.

---

## 🎯 Purpose
* **Not** a replication effort.  
* **Yes** to asking new, counterfactual questions:  
  *Would the same patient be less likely to return if an HbA1c was measured vs not measured, holding everything else equal?*

---

## 🗄 Dataset snapshot  
* Source: *Diabetes 130‑US Hospitals* (1999‑2008)  
* Encounters: inpatient stays where diabetes appears in any diagnosis field  
* Inclusion filters:  
  1. Length of stay 1 – 14 days  
  2. ≥ 1 lab test during stay  
  3. Medication administration record available  

| Feature groups | Examples |
|----------------|----------|
| **Demographics** | Patient ID, race, gender, age |
| **Utilisation** | Admission type, time in hospital, prior outpatient / ED visits |
| **Labs** | HbA1c result, number of labs |
| **Treatments** | 24 diabetes medications (metformin, insulin…) |
| **Outcomes** | 30‑day readmission flag |

---

## 🔍 Analysis roadmap

1. **Basic EDA** – distributions, missingness, crude readmission rates.  
2. **Causal framing** – DAG sketch, confounder list, treatment = *HbA1c measured?*  
3. **Estimators**  
   * Propensity score weighting / doubly robust estimators  
   * Sensitivity checks (unobserved confounding, positivity)  
4. **ML side‑cars** – XGBoost / causal forest for heterogeneous effects.  
5. **Interpretation** – does HbA1c measurement itself matter, or is it care quality signal?  

_All code and thinking will be documented step‑by‑step in Jupyter notebooks._

---

## 🌱 Why this matters
Better diabetes care saves limbs, lives, and dollars.  
If re‑analysis shows HbA1c measurement is **only a proxy**, policy should focus on the underlying care processes—not the lab draw itself.

---

## 🤝 Contributing
Data scientists, clinicians, causal‑inference buffs—PRs and discussions welcome.

---

## 📄 License
MIT  

