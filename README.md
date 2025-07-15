### Impact of Measurement of HbA1c on Hospital Readmission: Diabetes Dataset - Revisiting 2014 Research

#### Overview
This repository aims to complement a diabetes study published in 2014, which highlighted the protective role of HbA1C measurements against early readmission in diabetes patients. Over a decade has passed since the original publication and significant advancements in AI gives us new tools and perspectives to reassess the data and conclusions drawn at the time. My interest in this data is for two reasons - [1] I've a personal interest in the implications of diabetes [2] The paper raised some questions that I wanted to evaluate, specifically the claim that the measurement of HbA1c was protective against readmission. It seemed to me that the measurement of HbA1c was a proxy for treatment or something else - but I wasn't sure what.

#### Purpose
The primary goal of this research is not to recreate the original paper but to augment it using AI/ML methods, specifically look at the data through a causal lens - i.e., not merely think of associations but find out if there is enough information in this data to get deeper insights. This analysis aims to assess the validity of the study's recommendations with today's advancements. There isn't enough data to recreate the old analyses and I neither have the desire nor the interest to do so. Instead, I use the paper from 2014 as a springboard to ask a different questions and take it forward from there.

#### Dataset
Description of the Dataset - Each row represents hospitalized patient records diagnosed with diabetes over ten years (1999-2008) of clinical care at 130 US hospitals and integrated delivery networks. It includes over 50 features representing patient and hospital outcomes. Information extracted from the database for encounters satisfied the following criteria - (1) It is an inpatient encounter. (2) It is a diabetic encounter - one during which any kind of diabetes was entered into the system as a diagnosis. (3) The length of stay was between 1 - 14 days. (4) Laboratory tests performed during the encounter. (5) Medications were administered during the encounter. 

The data contains attributes as patient number, race, gender, age, admission type, time in hospital, medical specialty of admitting physician, number of lab tests performed, HbA1c result, diagnosis, number of medications, diabetic medications, number of outpatient, inpatient, and emergency visits in the year before the hospitalization, etc. 

#### Analysis Approach
- **Re-evaluation**: Applying causal inference and ML techniques to the dataset after performing the basic EDAs.
- **Process**: This repository will focus on the thought process of analyzing this dataset and more importantly, doing a deep dive using the simplest of tools. In what follows, I try to articulate my thought process, assumptions, and conclusions drawn from the analysis. 

#### Why This Matters
Even without a background in the medical field, the importance of management plans for conditions like diabetes is obvious to me (and everyone else) but by revisiting this paper with new methods and a new perspective, my hope is to add another angle to the investigation and potentially validate or refine the strategies for managing diabetes, contributing to improved outcomes. I hope.

#### Contributing
Whether you are a data scientist, a medical professional, or just someone interested in diabetes management, your insights and contributions are always welcome. 

#### License
This project is made available under the [MIT License](LICENSE.md).

---
