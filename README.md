# 🏥 Hospital Readmission Prediction
### Can we predict which diabetic patients will return 
### to hospital within 30 days?

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=flat&logo=python)
![PowerBI](https://img.shields.io/badge/PowerBI-Dashboard-yellow?style=flat&logo=powerbi)
![XGBoost](https://img.shields.io/badge/ML-XGBoost-green?style=flat)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat)

---

## 👋 What is this project about?

Every year thousands of diabetic patients get discharged 
from hospital and then come back within 30 days. 
This is expensive for hospitals, stressful for patients, 
and often preventable.

I wanted to answer one question:

> **"Can we predict BEFORE a patient leaves hospital 
> whether they will come back within 30 days?"**

To answer this I analyzed real clinical data from 
130 US hospitals covering 10 years (1999–2008) and 
built machine learning models to predict readmission risk.

This project is inspired by and extends the original 
research paper by Strack et al. (2014) which studied 
the same dataset but used only basic statistical methods. 
I went further by applying modern ML algorithms and 
engineering new features that the original paper never used.

---

## 🔍 What did I actually find?

Here are the most interesting things I discovered 
from the data:

**Who is most at risk?**
- Elderly patients (80-90 years old) are readmitted 
  at nearly double the rate of younger patients
- Patients admitted for injury or trauma have the 
  highest readmission rate because diabetes makes 
  healing much harder
- Patients NOT discharged home (sent to facilities) 
  have nearly double the readmission rate of patients 
  going home

**What predicts readmission best?**
- Where you go AFTER hospital matters more than 
  anything else (discharge destination)
- How many lab tests were done during your stay
- How many medications you take per day 
  (I created this feature myself — it was not 
  in the original data!)
- Whether you have been hospitalized before

**Which ML model worked best?**
- I tested 3 models: Logistic Regression, 
  Random Forest, and XGBoost
- XGBoost won with AUC-ROC of 0.6448
- This is better than the basic logistic regression 
  used in the original 2014 paper

---

## 📊 Results at a glance

| What I measured | Result |
|----------------|--------|
| Total patients analyzed | 69,973 |
| Overall readmission rate | 8.97% |
| Highest risk age group | 80-90 years (10.7%) |
| Highest risk diagnosis | Injury (10.7%) |
| Lowest risk diagnosis | Respiratory (7.3%) |
| Best ML model | XGBoost |
| Best AUC-ROC score | 0.6448 |

---

## 🤖 How did the models perform?

| Model | Accuracy | AUC-ROC |
|-------|----------|---------|
| Logistic Regression | 63.65% | 0.6258 |
| Random Forest | 73.75% | 0.6319 |
| **XGBoost** 🏆 | **66.92%** | **0.6448** |

I used AUC-ROC as the main metric rather than accuracy 
because only 9% of patients were readmitted. A model 
that just predicts "not readmitted" for everyone would 
get 91% accuracy but be completely useless AUC-ROC 
tells the real story.

---

## 💡 How is this different from the original paper?

The original Strack et al. (2014) paper was groundbreaking 
for its time but had limitations I tried to address:

| What original paper did | What I did differently |
|------------------------|----------------------|
| Only logistic regression | Added Random Forest + XGBoost |
| Only primary diagnosis | Used all 3 diagnosis fields |
| Raw features only | Engineered 8 new clinical features |
| No feature importance | Identified top predictors |
| No dashboard | Built interactive Power BI dashboard |

---

## 🧹 How did I clean the data?

Raw data is messy — here is what I had to fix:
101,766 records to start with
Problem 1 → Same patient appeared multiple times
Solution: Keep only first visit per patient
Problem 2 → Missing values written as "?" not empty
Solution: Replace all ? with proper NaN
Problem 3 → 3 columns were 40-97% empty
Solution: Drop weight, payer_code,
medical_specialty
Problem 4 → Dead patients cant be readmitted
Solution: Remove deceased and hospice patients
Problem 5 → Age stored as text "[60-70)" not numbers
Solution: Convert to numeric midpoints
Problem 6 → 848 different diagnosis codes
Solution: Group into 9 clinical categories
Final result → 69,973 clean records ready for analysis
---

## ⚙️ New features I created

I created 8 new features based on clinical knowledge 
of what makes patients likely to be readmitted:

med_per_day        → How many medications per day of stay
(became 3rd most important feature!)
total_prior_visits → Total times patient visited hospital
before this admission
high_medication    → Flag for patients on 15+ medications
long_stay          → Flag for stays longer than 7 days
high_complexity    → Flag for patients with 7+ diagnoses
on_insulin         → Is this patient on insulin?
on_diabetes_med    → Is this patient on diabetes medication?
med_changed        → Did medication change during this stay?

These features improved the AUC-ROC score by 0.086 
compared to using raw features alone.

---

## 🏆 Most important features for prediction

After running Random Forest I found these are the 
features that matter most:

| Rank | Feature | Why it matters |
|------|---------|---------------|
| 1 | Discharge destination | Where you go after hospital |
| 2 | Lab procedures | How many tests were done |
| 3 | Medications per day ⭐ | My engineered feature! |
| 4 | Total medications | How complex your treatment is |
| 5 | Prior inpatient visits | Have you been here before? |
| 6 | Age | Older = higher risk |
| 7 | Time in hospital | How long you stayed |
| 8 | Prior total visits ⭐ | My engineered feature! |
| 9 | Primary diagnosis | Why you came in |
| 10 | Number of diagnoses | How many conditions you have |

⭐ = Features I created that were not in original data

---

## 📈 Charts and visualizations

I created 9 charts throughout this project:
Chart 1 → Readmission distribution
Shows how imbalanced the data is
Chart 2 → Readmission rate by age group
Clear upward trend with age
Chart 3 → Readmission by diagnosis group
Injury highest, Respiratory lowest
Chart 4 → Medications vs readmission
Readmitted patients had more medications
Chart 5 → Hospital stay vs readmission
Readmitted patients stayed longer
Chart 6 → Correlation heatmap
Which features relate to each other
Chart 7 → Feature importance
What the Random Forest model learned
Chart 8 → Model comparison + ROC curves
XGBoost clearly wins on AUC-ROC
Chart 9 → All 3 diagnosis groups side by side
Primary, secondary and tertiary diagnoses

---

## 📊 Power BI Dashboard

I also built an interactive 3-page dashboard in Power BI 
so hospital managers can explore the data themselves:

Page 1 — Executive Overview
The big picture numbers at a glance
Total patients, readmission rate,
average stay, age breakdown
Page 2 — Clinical Analysis
Which diagnoses and admission types
have highest readmission rates
Gender and race breakdown
Page 3 — ML Model Results
How the 3 models compare
Which features matter most
XGBoost as recommended model

---

## 🛠️ Tools I used

Python          → Main programming language
Pandas          → Data cleaning and manipulation
NumPy           → Numerical operations
Scikit-learn    → Machine learning models
XGBoost         → Best performing model
Seaborn         → Statistical visualizations
Matplotlib      → Custom charts
Power BI        → Interactive dashboard
Jupyter         → Development environment
Git + GitHub    → Version control

---

## 📥 How to run this project yourself

**Step 1 — Download the dataset:**

The dataset is not included here due to file size.
Download it free from Kaggle:
🔗 https://www.kaggle.com/brandao/diabetes

**Step 2 — Clone this repository:**
```bash
git clone https://github.com/the-bist12/Hospital-readmission-analysis-.git
cd Hospital-readmission-analysis-
```

**Step 3 — Install required libraries:**
```bash
pip install pandas numpy scikit-learn xgboost 
pip install seaborn matplotlib imbalanced-learn
```

**Step 4 — Run the notebook:**

---

## 📖 Original Research Paper

This project is based on and extends:

> Strack, B., DeShazo, J.P., Gennings, C., Olmo, J.L.,
> Ventura, S., Cios, K.J. and Clore, J.N. (2014).
> *Impact of HbA1c Measurement on Hospital Readmission 
> Rates: Analysis of 70,000 Clinical Database Patient 
> Records.*
> BioMed Research International, Article ID 781670.

The original paper is included in this repository 
as `description.pdf`

---

## 🌱 What I learned from this project

This was my first end-to-end data science project 
and it taught me:

- How to work with real messy clinical data
- Why class imbalance matters so much in ML
- How feature engineering can improve model performance
- How to read and extend academic research
- How to communicate findings through dashboards
- The importance of AUC-ROC over accuracy for 
  imbalanced datasets

---

## 🔮 What could be improved next

If I had more time I would:

- Try deep learning models (Neural Networks)
- Add SHAP values for better explainability
- Build a web app where you input patient data 
  and get readmission risk score
- Test on more recent hospital data
- Publish as a formal research paper

---

## 👤 About me

Hi! I am Dinesh from Kathmandu, Nepal.
I am learning data analytics and this is one of 
my portfolio projects combining Python, Machine 
Learning, and Power BI.

I built this project to demonstrate end-to-end 
data science skills using real clinical data.

🔗 GitHub: https://github.com/the-bist12

---

## 📄 License

This project is open source under MIT License.
Dataset is under Creative Commons Attribution 4.0
(Source: UCI ML Repository / Kaggle)

---

*If you found this project useful or interesting, 
please consider giving it a ⭐ on GitHub!*
