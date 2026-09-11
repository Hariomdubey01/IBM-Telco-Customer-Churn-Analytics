# IBM Telco Customer Churn Analytics

> End-to-end telecom customer churn analysis using Python, Excel, and Power BI, covering EDA, statistical analysis, customer segmentation, revenue analysis, data modeling, and interactive dashboards.

## 📊 Dashboard Preview

![Dashboard Preview](https://github.com/Hariomdubey01/IBM-Telco-Customer-Churn-Analytics/blob/main/screenshots/executive-summary.png)

## Table of Contents

- [Overview](#overview)
- [Project Objectives](#project-objectives)
- [Project Workflow](#project-workflow)
- [Dataset](#dataset)
- [Tech Stack](#tech-stack)
- [Data Preparation](#data-preparation)
- [Python Analysis](#python-analysis)
- [Dashboard Highlights](#dashboard-highlights)
- [Key Business Insights](#key-business-insights)
- [Business Recommendations](#business-recommendations)
- [Business Value](#business-value)
- [Skills Demonstrated](#skills-demonstrated)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Future Enhancements](#future-enhancements)
- [About Me](#about-me)

---

## Overview

This project presents an end-to-end Data Analytics case study on customer attrition within the telecommunications sector using the IBM Telco dataset. Customer churn poses a substantial risk to recurring subscription revenue, acquisition cost efficiency, and customer lifetime value.

To evaluate this business challenge, the project executes a data quality audit of customer records, conducts exploratory and inferential statistical analysis in Python, establishes an analytical Star Schema in Microsoft Excel, and delivers an interactive, five-page executive dashboard in Microsoft Power BI. The deliverable translates observed customer patterns into quantifiable revenue exposure metrics and actionable retention strategies without assuming causality.

---

## Project Objectives

1. Audit, clean, and validate raw customer data to establish data integrity and analytical consistency.
2. Conduct exploratory data analysis (EDA) to identify customer segments and attributes exhibiting elevated churn patterns.
3. Apply inferential statistics (Chi-Square Test of Independence) to test whether observed associations between categorical features and churn are statistically significant.
4. Engineer analytical segments evaluating customer tenure, billing magnitude, risk tier, and customer value.
5. Quantify revenue exposure across key operational dimensions, contract types, and payment channels.
6. Design a dimensional Star Schema with fact and dimension tables to support scalable business intelligence reporting.
7. Develop a 5-page interactive Power BI dashboard providing operational and executive visibility into churn metrics.
8. Deliver non-causal, evidence-based business recommendations to support targeted retention initiatives.

---

## Project Workflow

```
Raw Data
   ↓
Data Quality Assessment & Audit
   ↓
Data Preparation & Feature Engineering
   ↓
Exploratory Data Analysis (EDA)
   ↓
Inferential Statistical Testing (Chi-Square)
   ↓
Data Modeling (Star Schema Design)
   ↓
Interactive Power BI Dashboard Development
   ↓
Business Insights & Strategic Recommendations
```

---

## Dataset

The analysis uses the standardized **IBM Telco Customer Churn Analytics** dataset, which captures customer demographics, subscribed services, account profiles, billing details, and historical churn status.

| Attribute | Specification |
|---|---|
| **Dataset Name** | `Telco-Customer-Churn-Dataset.csv` |
| **Record Count** | 7,043 rows |
| **Feature Count** | 21 columns |
| **Target Variable** | `Churn` (`Yes` / `No`) |
| **Demographic Features** | `gender`, `SeniorCitizen`, `Partner`, `Dependents` |
| **Service Subscriptions** | `PhoneService`, `MultipleLines`, `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies` |
| **Account & Billing** | `tenure`, `Contract`, `PaperlessBilling`, `PaymentMethod`, `MonthlyCharges`, `TotalCharges` |
| **Identifier** | `customerID` |

---

## Tech Stack

**Programming & Statistical Analysis**
- Python
- Pandas (Data wrangling, aggregation, manipulation)
- NumPy (Vectorized operations and numerical handling)
- Matplotlib & Seaborn (Exploratory distributions and visualizations)
- SciPy (`scipy.stats` for inferential Chi-Square testing)

**Data Modeling**
- Microsoft Excel (Dimensional data modeling, schema mapping, validation)
- Star Schema Architecture (Fact table, dimension tables, primary/foreign keys)

**Business Intelligence & Reporting**
- Microsoft Power BI Desktop
- DAX (Data Analysis Expressions for calculated measures, ratios, and risk indicators)

---

## Data Preparation

The raw dataset underwent a structured data quality assessment to resolve anomalies, invalid data types, and missing values before analytical modeling:

1. **Identifier Uniqueness:** Confirmed `customerID` contains 7,043 unique values with zero duplicate records across the dataset.
2. **Data Type Correction (`TotalCharges`):**
   - The `TotalCharges` field was stored as an `object` (string) due to whitespace entries representing blank strings (`" "`).
   - The 11 records with blank values in `TotalCharges` all correspond to customers with `tenure = 0`. These missing values were treated as 0 for cumulative-charge analysis while retaining the customer records.
   - Converted `TotalCharges` to numeric (`float64`) to preserve analytical consistency.
3. **Categorical Recoding & Harmonization:**
   - Standardized binary indicator features (`SeniorCitizen`) for uniform analytical display.
   - Evaluated service-related values (`No internet service`, `No phone service`) across auxiliary lines to ensure consistency during segmentation and grouping.
4. **Analytical Feature Engineering:**
   - **Tenure Cohorts:** Grouped raw `tenure` (0–72 months) into categorical cohorts (`0-12 Months`, `13-24 Months`, `25-48 Months`, `49-72 Months`) to evaluate milestone-based attrition patterns.
   - **Monthly Charge Tiers:** Segmented `MonthlyCharges` into analytical expenditure bands (`Low`, `Medium`, `High`) to assess charge sensitivity.
   - **Risk & Value Segments:** Cross-referenced contractual terms against revenue contribution to identify customer accounts with higher relative churn exposure.

---

## Python Analysis

The exploratory and statistical analysis was conducted within `python/Telco_Customer_Churn_EDA.ipynb`:

### 1. Data Quality & Distribution Profiling
- Audited shape, summary statistics, variance, and distributions across all numerical and categorical features.
- Evaluated the global churn distribution: 1,869 customers churned out of 7,043 total records, establishing an overall baseline churn rate of approximately **26.5%**.

### 2. Exploratory Data Analysis (EDA)
- **Contract Type:** Evaluated churn rates across contract structures. Month-to-month contracts showed substantially higher observed churn compared to one-year and two-year commitments.
- **Tenure Relationship:** Customers within their first 12 months accounted for the highest concentration of churn events, with observed attrition tapering down significantly as tenure increased.
- **Service Stack:** Identified higher churn rates among customers subscribed to Fiber Optic internet service compared to DSL or No Internet customers. Conversely, subscribers with auxiliary support services (`TechSupport`, `OnlineSecurity`) exhibited lower churn frequencies.
- **Payment Method:** Electronic check payments showed a notably higher churn rate relative to automated bank transfers, credit cards, or mailed checks.

### 3. Inferential Statistical Analysis
To evaluate whether observed differences in churn across categorical variables were statistically significant, Chi-Square Tests of Independence ($\chi^2$) were performed using `scipy.stats.chi2_contingency`:
- Tested relationships between `Churn` and categorical features: `Contract`, `InternetService`, `PaymentMethod`, `TechSupport`, `OnlineSecurity`, `PaperlessBilling`, and demographic attributes.
- Results confirmed statistically significant associations ($p < 0.001$) between customer churn and key operational dimensions: `Contract`, `PaymentMethod`, `InternetService`, and support service attachments.
- Demographic variables such as `gender` did not show statistically significant associations with churn ($p > 0.05$).

### 4. Customer Segmentation & Revenue Analysis
- Evaluated total monthly recurring revenue (MRR) exposure across high-churn categories.
- Highlighted that month-to-month subscribers with high monthly charges represent the largest share of unrealized recurring revenue associated with early cancellation.

---

## Dashboard Highlights

The Microsoft Power BI dashboard (`powerbi/Customer_Churn_Analytics_Dashboard.pbix`) is structured into five dedicated analytical views:

### 1. Executive Summary

**Purpose:**  
Provides senior leadership with a consolidated overview of organizational churn, customer volume, and associated monthly recurring revenue impact.

**Key Elements:**
- High-level KPI cards: Total Customers, Total Churn, Churn Rate (%), Total Monthly Charges, Total Revenue Lost.
- Trend visuals and distribution charts comparing churn across contract categories.
- Top-level slicers for immediate demographic and account filtering.

**Business Use:**  
Enables executives to monitor core retention benchmarks and evaluate overall financial exposure at a glance.

---

### 2. Customer Demographics & Profile Analysis

**Purpose:**  
Examines how customer characteristics, household structures, and seniority intersect with service cancellation patterns.

**Key Elements:**
- Demographic comparative charts: Senior Citizen status, Partner status, and Dependents distribution.
- Churn breakdown across customer demographic segments.
- Slicers for gender, partner status, and dependent presence.

**Business Use:**  
Identifies whether retention risks correlate with specific customer lifecycle profiles, helping marketing teams tailor segment-specific retention messaging.

---

### 3. Service Subscriptions & Product Mix

**Purpose:**  
Evaluates churn rates across telecommunication service offerings, connection types, and supplementary add-ons.

**Key Elements:**
- Churn rate comparisons across `InternetService` types (Fiber Optic, DSL, None).
- Service adoption matrices: `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, and `StreamingMovies`.
- Interactive slicers to isolate specific service combinations.

**Business Use:**  
Highlights which products or service configurations are associated with higher cancellation rates, supporting service reviews and cross-sell/bundle strategies.

---

### 4. Contract, Billing & Payment Analysis

**Purpose:**  
Investigates account characteristics, including billing channels, payment modes, and contract terms.

**Key Elements:**
- Attrition breakdown across Contract structures (Month-to-Month, One Year, Two Year).
- Churn rates categorized by Payment Method (Electronic Check, Mailed Check, Bank Transfer, Credit Card).
- Visual distribution of `MonthlyCharges` and paperless billing adoption versus churn status.

**Business Use:**  
Guides billing operations and finance teams on the observed stability of customer payment channels and contract conversion priorities.

---

### 5. Risk & Value Segmentation (Retention Prioritization)

**Purpose:**  
Segments the customer base by attrition likelihood indicators and revenue contribution to prioritize outreach.

**Key Elements:**
- Matrix/Scatter visualization positioning customers across tenure bands and monthly charge tiers.
- High-Risk / High-Value segment summary tables detailing customer counts and aggregate monthly billing.
- Drill-through filters for account-level retention intervention lists.

**Business Use:**  
Allows retention teams to direct proactive customer service capacity toward accounts that represent substantial recurring revenue exposure.

---

## Key Business Insights

### 1. Contract Structure & Early Account Attrition

**Finding:**  
Customers on month-to-month contracts exhibited a substantially higher churn rate (over 40%) compared to customers on one-year (~11%) and two-year (~3%) agreements. Furthermore, attrition is heavily concentrated within the first 12 months of tenure.

**Business Implication:**  
The business experiences its highest churn concentration during early onboarding. While flexible month-to-month terms lower the barrier to sign up, they are associated with early churn before customer acquisition costs may be fully recovered.

---

### 2. Support Service Attachments & Retention

**Finding:**  
Customers lacking auxiliary services—specifically `TechSupport` and `OnlineSecurity`—showed churn rates exceeding 40%, whereas customers with these features active churned at rates below 16%. Inferential testing confirmed these associations are statistically significant ($p < 0.001$).

**Business Implication:**  
Subscribers utilizing standalone connectivity without integrated support or security features exhibit higher attrition rates. Bundled support services are associated with increased customer stickiness and longer tenure.

---

### 3. Fiber Optic Internet Churn Rates

**Finding:**  
Subscribers with Fiber Optic service exhibited a higher churn rate (~42%) compared to DSL subscribers (~19%) and customers without internet service (~7%), despite Fiber Optic carrying higher average monthly fees.

**Business Implication:**  
Fiber Optic subscribers represent premium monthly billing, but also the highest observed rate of turnover. This pattern may indicate customer sensitivity around pricing, installation expectations, or competitive alternatives, warranting further operational investigation.

---

### 4. Payment Method & Billing Friction

**Finding:**  
Subscribers paying via Electronic Check demonstrated churn rates approaching 45%, compared to rates below 20% for automated methods (Bank Transfer, Credit Card) and Mailed Checks.

**Business Implication:**  
Manual electronic payment channels are associated with higher cancellation rates, whereas automated billing methods show a statistically significant relationship with account continuity. This pattern may indicate transactional friction or varying customer commitment levels across payment options.

---

## Business Recommendations

| Priority | Finding | Recommended Action | Expected Business Benefit |
|:---:|---|---|---|
| **High** | Month-to-month contracts show the highest churn, particularly in months 0–12. | Introduce targeted incentives (e.g., promotional onboarding rates, value-added service bundles) for transitioning month-to-month users to 1-year commitments after month 3. | Expected to improve early-tenure stability and directionally reduce baseline contract churn. |
| **High** | Electronic Check payment is associated with churn rates above 40%. | Implement automated payment incentive campaigns (e.g., a one-time account credit for enrolling in recurring ACH or credit card autopay). | May help lower transactional billing friction and reduce payment-related cancellation rates. |
| **Medium** | Absence of `TechSupport` and `OnlineSecurity` correlates with elevated churn. | Package security and support features into standard entry-level tiers or offer introductory 90-day trials during onboarding. | Potential increase in product stickiness and customer lifetime value. |
| **Medium** | Fiber Optic service accounts exhibit elevated churn despite premium billings. | Conduct service quality and customer satisfaction reviews on Fiber Optic accounts, assessing onboarding support and pricing competitiveness. | May help protect high-ARPU (Average Revenue Per User) accounts from competitive migration. |
| **Low** | High-value, long-tenure customers account for substantial cumulative margin. | Deploy proactive VIP loyalty programs and prioritized customer care routing for accounts exceeding $80/month. | Directionally safeguards core baseline revenue against service cancellations. |

---

## Business Value

- **Strategic Retention Targeting:** Replaces broad-brush retention discounts with targeted segment interventions, directing resources toward accounts with higher churn exposure.
- **Operational Prioritization:** Translates raw customer records into actionable cohorts, enabling customer success teams to focus attention on vulnerable tenure windows (months 1–12).
- **Revenue Protection:** Quantifies monthly recurring revenue exposure across contract categories and payment channels, providing finance leaders with visibility into predictable cash flow risks.
- **Self-Service Analytical Reporting:** Delivers an intuitive 5-page Power BI dashboard that allows department leads to filter metrics across contract, demographic, payment, service, and product dimensions independently.

---

## Skills Demonstrated

- **Data Analytics & EDA:** Data profiling, distribution analysis, bivariate/multivariate analysis, correlation checks.
- **Data Cleaning & Preprocessing:** Data type casting, missing value handling, duplicate verification, cohort grouping.
- **Statistical Analysis:** Hypothesis testing, contingency tables, Chi-Square Test of Independence ($\chi^2$).
- **Customer Segmentation & Analytics:** Cohort grouping, churn rate modeling, risk and value matrix design.
- **Data Modeling:** Star Schema design, fact table normalization, dimensional lookup structuring in Excel.
- **Business Intelligence & Dashboards:** Microsoft Power BI report design, interactive UI layout, cross-filtering, DAX measures.
- **Business Storytelling:** Formulating evidence-based, non-causal insights, and translating metrics into strategic business recommendations.

---

## Repository Structure

```
├── excel/
│   └── Telco_Customer_Churn_Star_Schema.xlsx
├── powerbi/
│   └── Customer_Churn_Analytics_Dashboard.pbix
├── python/
│   └── Telco_Customer_Churn_EDA.ipynb
├── Telco-Customer-Churn-Dataset.csv
└── README.md
```

---

## Getting Started

### 1. Python Environment & EDA
1. Ensure Python 3.8+ is installed with the required libraries:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy jupyter
   ```
2. Open `python/Telco_Customer_Churn_EDA.ipynb` in Jupyter Notebook or JupyterLab.
3. Ensure `Telco-Customer-Churn-Dataset.csv` is located in the root repository directory (or update the file path accordingly in the read step).
4. Run all cells to execute the data audit, statistical Chi-Square calculations, and exploratory visualizations.

### 2. Excel Data Model
- Open `excel/Telco_Customer_Churn_Star_Schema.xlsx` in Microsoft Excel to review the dimensional architecture, fact tables, and lookup tables.

### 3. Power BI Dashboard
- Open `powerbi/Customer_Churn_Analytics_Dashboard.pbix` in Microsoft Power BI Desktop to interact with the slicers, measures, and all 5 dashboard reporting pages.

---

## Future Enhancements

- **Predictive Churn Modeling:** Train and evaluate supervised classification models (e.g., Logistic Regression, Random Forest, XGBoost) to estimate individual account churn probabilities.
- **Customer Lifetime Value (CLV) Forecasting:** Integrate survival analysis techniques to estimate expected lifetime duration and lifetime revenue per customer cohort.
- **Automated Data Refresh:** Configure automated data pipelines to ingest new billing cycles and update Power BI visuals via scheduled gateway refresh.

---

## 👨‍💻 About Me

**Hariom Dubey**

Aspiring **Data Analyst** passionate about transforming data into meaningful business insights.

### Areas of Interest

- Data Analytics
- Business Intelligence
- Data Visualization
- SQL
- Python
- Power BI
- Machine Learning

---

## 📬 Contact

| Platform | Link |
|----------|------|
| 📧 Email | <mailto:hariomkumard8@gmail.com> |
| 💼 LinkedIn | [linkedin.com/in/itzhariomdubey](https://www.linkedin.com/in/itzhariomdubey) |
| 💻 GitHub | [github.com/Hariomdubey01](https://github.com/Hariomdubey01) |
---
