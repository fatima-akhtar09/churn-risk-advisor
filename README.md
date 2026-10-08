# Churn Risk Advisor

**Live App:** https://churn-risk-advisor-5p9ao2d7dkfhdeklwmovnj.streamlit.app

## Lab 4 - Task 3.3 Deployment
- Main file: app.py
- Python: 3.11
- Public URL works in Incognito: YES
- First deployment succeeded, no ModuleNotFoundError

## Task 2.3 - Test it like a user would
#### One Customer Test
Default inputs: Tenure 4, Month-to-month, Fiber optic -> Result 89% HIGH, Contact now

<img width="1896" height="873" alt="image" src="https://github.com/user-attachments/assets/93dccf7d-0aa1-4318-b91f-dc259680e103" />


#### Batch Scoring Test - Success
<img width="1919" height="1013" alt="image" src="https://github.com/user-attachments/assets/f82ee815-47b1-4f8d-b729-1e3ac7f20b25" />

Uploaded sample file -> 31 of 50 customers above threshold, table shown

#### Batch Scoring Test - Broken CSV
<img width="1905" height="857" alt="image" src="https://github.com/user-attachments/assets/4d66b33f-bb34-4609-a8a5-c52146f3a79e" />

Uploaded missing_col.csv -> Shows "Missing columns: ['tenure']" without crashing

### All Tests from Slide 9
1. Default -> HIGH band PASSED
2. Internet=No -> Add-on disappears PASSED
3. Tenure 60 + 2-year -> LOW band PASSED
4. Threshold slider -> Band changes, probability same PASSED
5. Batch upload 50 customers -> Download works PASSED
6. Broken CSV -> Error, no crash PASSED

### Write it - Reflection
**What surprised me?**
When I set Contract to Two-year, the risk dropped from 89% to 61.5% (difference -0.275). The model is very sensitive to contract length. I did not expect one field to change risk that much.

**What still handles badly for a real user?**
If a real user enters Monthly charges = 0 or TotalCharges that does not match tenure (e.g., Tenure 10 but TotalCharges 5), the app still predicts. Also if user uploads a CSV with text in numeric column, error is technical "ValueError" not friendly. It should validate and say "Please check MonthlyCharges should be > 0".

## Model card: Churn Risk Advisor v1.0

**Intended use:** Rank telecom customers by churn risk for retention team prioritization. Decision support, not automated action.

**Not for:** Credit decisions, pricing, or denying service.

**Data:** IBM Telco Customer Churn, 7,043 customers (from model_meta.json v1.0)

**Model:** XGBClassifier (sklearn 1.6.1) - churn_model.joblib
30 features: SeniorCitizen, tenure, MonthlyCharges, TotalCharges, gender_Male, Partner_Yes, Dependents_Yes, PhoneService_Yes, MultipleLines_No phone service, MultipleLines_Yes, InternetService_Fiber optic, InternetService_No, OnlineSecurity_No internet service, OnlineSecurity_Yes, OnlineBackup_No internet service, OnlineBackup_Yes, DeviceProtection_No internet service, DeviceProtection_Yes, TechSupport_No internet service, TechSupport_Yes, StreamingTV_No internet service, StreamingTV_Yes, StreamingMovies_No internet service, StreamingMovies_Yes, Contract_One year, Contract_Two year, PaperlessBilling_Yes, PaymentMethod_Credit card (automatic), PaymentMethod_Electronic check, PaymentMethod_Mailed check

**Performance:**
- CV AUC: 0.8504 +/- 0.0125
- Test AUC: 0.8478 (20% holdout, used once)

**Threshold:** 0.30 (from model_meta.json). LOW <0.30, MEDIUM 0.30-0.65, HIGH >0.65. Chosen from cost: FN = customer loss PKR 6,000, FP = retention offer PKR 1,000.

**Limitations:** US-only data from one period; no Pakistan data; correlation not causation; may drift with new plans; no service notes.

**Fairness check:** Check recall by gender & SeniorCitizen. No large gap expected but monitor seniors.

**Owner:** Fatima Akhtar, v1.0, 2026-05-13

**Live App:** https://churn-risk-advisor-5p9ao2d7dkfhdeklwmovnj.streamlit.app
- First deployment succeeded, no ModuleNotFoundError
- Tested in Incognito: YES

### How to run locally
```bash
pip install -r requirements.txt
streamlit run app.py
```
## Note for Sir - Week 4 Update in Week 3 Notebook

Sir, this Week 3 notebook is updated for Week 4 Lab.

Dataset used is same as Week 3: Telco Customer Churn (WA_Fn-UseC_Telco-Customer-Churn.csv)

- link: https://www.kaggle.com/code/fatimaakhtar123/week-3-model-optimization
What I edited for Week 4 in this notebook:
- Added model_meta.json with real values (0.8504 CV AUC, 0.8478 Test AUC, Threshold 0.30)
- Saved model as churn_model.joblib
 
Student: Fatima Akhtar
Updated: 2026-10-9 (v1.0)
