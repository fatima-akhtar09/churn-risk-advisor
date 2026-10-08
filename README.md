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
![batch error](batch_error.png)
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
