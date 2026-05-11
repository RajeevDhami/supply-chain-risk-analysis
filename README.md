# Supplier Risk Assessment & Supply Chain Disruption Cost Analysis

## 📋 Project Overview

This project analyzes supplier performance data to identify high-risk suppliers, quantify the financial impact of supply chain disruptions, and recommend a phased risk mitigation strategy. Using a data-driven approach, the analysis combines risk scoring models with financial impact modeling to provide actionable insights for procurement optimization.

**Core Question:** Which suppliers pose the highest disruption risk, and how should sourcing decisions be optimized to reduce financial impact?

**Objective:** To identify high-risk suppliers based on delivery reliability, quality, and lead time variability; assess supplier dependency through concentration risk; and quantify the financial impact of potential disruptions to recommend strategies that minimize supply chain risk and cost.

---

# 🎯 Key Findings

| Metric                          | Value                              |
| ------------------------------- | ---------------------------------- |
| **Total Annual Risk Exposure**  | ₹6,11,641                          |
| **High-Risk Suppliers**         | 2 (Beta_Supplies, Delta_Logistics) |
| **Phase 1 Annual Savings**      | ₹1,46,000                          |
| **Phase 1 Investment Cost**     | ₹2,00,000                          |
| **Phase 1 Payback Period**      | 1.4 years                          |
| **Combined Impact (Phase 1+3)** | ₹1,86-196K/year                    |

**Critical Insight:** Risk is primarily driven by supplier performance variability (on-time delivery, quality, consistency) rather than supplier dependency concentration.

---

# 🖼️ Dashboard Preview

## Supplier Risk Dashboard


<img width="1517" height="852" alt="Supplier_Risk_Overview" src="https://github.com/user-attachments/assets/5a2956fe-9441-404a-88ad-bf5d4eecbc4e" />



The Power BI dashboard provides:

* Supplier risk segmentation
* Financial impact analysis
* Expected disruption cost estimation
* Risk mitigation comparison
* KPI tracking and supplier performance insights

---

# 🗂️ Project Structure

```text
Supply_Chain_Project/
├── README.md
├── Business Case Study/
│   └── Supplier_Risk_Case_Study_Final.pdf
├── Dashboard/
│   └── Supplier_Risk_Dashboard.pbix
├── Dataset/
│   ├── raw_supplier_dataset.csv
│   └── processed_supplier_risk_dataset.xlsx
├── Notebook/
│   └── Supply_Chain_Analytics.ipynb
└── Screenshot/
    └── Supplier_Risk_Overview.png
```

---

# 📊 Analysis Pipeline

The analysis is divided into 5 integrated phases:

## Phase 1: Data Loading & Exploration

* Load procurement KPI dataset (777 records, 11 columns)
* Inspect data structure, types, and quality
* Summary statistics

## Phase 2: Feature Engineering & Metrics Calculation

### Metrics Calculated

* On-Time Delivery %
* Defect Rate %
* Lead Time Variability
* Annual Spend

### Normalization

Min-Max scaling applied to standardize metrics on a 0–1 scale.

---

## Phase 3: Concentration Risk Assessment (Simulated)

### Risk Classification

* Critical: 100% dependency
* High: >70% dependency
* Medium: 50–70% dependency
* Low: <50% dependency

---

## Phase 4: Risk Scoring Model

### Risk Score Formula

```text
Risk Score =
0.4 × (1 - Normalized_On-Time%)
+ 0.3 × Normalized_Defect_Rate
+ 0.3 × Normalized_Lead_Time_Variability
```

### Risk Categories

* Green → Low Risk
* Yellow → Medium Risk
* Red → High Risk

---

## Phase 5: Expected Loss & Financial Impact

### Expected Loss Formula

```text
Expected Annual Loss =
Risk Score × Daily Production Loss × Average Delay Days
```

### Business Assumptions

* Daily production loss: ₹50,000
* Average disruption delay: 5 days

### Current State

* Total Annual Risk Exposure: ₹6,11,641
* High-Risk Suppliers: 2

### Proposed Scenario

* Simulated 40% risk reduction for high-risk suppliers
* Total Risk After Improvement: ₹3,13,835

### Financial Metrics

* Risk Reduction: ₹2,97,806
* Annual Savings: ₹1,46,000
* Investment Cost: ₹2,00,000
* Payback Period: 1.4 years

---

# 📊 Key Results by Supplier

| Supplier            | Risk Score | Risk Level | Expected Loss | Status        |
| ------------------- | ---------- | ---------- | ------------- | ------------- |
| **Beta_Supplies**   | 0.73       | HIGH       | ₹1,83,372     | High Priority |
| **Delta_Logistics** | 0.73       | HIGH       | ₹1,81,569     | High Priority |
| **Alpha_Inc**       | 0.59       | MEDIUM     | ₹1,47,806     | Monitor       |
| **Epsilon_Group**   | 0.30       | LOW        | ₹75,978       | Stable        |
| **Gamma_Co**        | 0.09       | LOW        | ₹22,914       | Stable        |

---

# 💰 Phased Financial Impact

## Phase 1: Immediate Action (High-Risk Suppliers)

```text
Current Annual Risk:           ₹6,11,641
After Phase 1 Improvements:    ₹3,13,835
────────────────────────────────────────
Risk Reduction:                ₹2,97,806 (-49%)
Annual Savings:                ₹1,46,000
Investment Cost:               ₹2,00,000
Payback Period:                1.4 years
```

### Recommended Actions

1. Reduce dependency on Beta_Supplies through supplier diversification and supplier performance improvement initiatives.
2. Evaluate partial sourcing redistribution toward lower-risk suppliers such as Epsilon_Group.

---

## Phase 2: Validation & Monitoring (Months 3–6)

* Track operational KPI improvements
* Validate projected savings and operational improvements before scaling mitigation efforts
* Monitor delivery reliability, quality performance, and lead time consistency

---

## Phase 3: Scale to Medium-Risk Suppliers (Optional)

```text
Additional Investment:         ₹60–80K
Additional Annual Savings:     ₹40–50K
Combined Total Savings:        ₹1,86–196K/year
Combined Payback:              1.5–1.6 years
```

### Recommended Action

* Apply targeted supplier performance optimization strategies for medium-risk suppliers.

---

# 📈 Power BI Dashboard

**Dashboard Features**

* KPI cards for financial exposure and savings
* Supplier risk matrix
* Expected loss analysis
* Risk mitigation comparison
* Supplier risk detail table
* Interactive slicers and filters

### Visualizations Included

1. Risk Matrix (Scatter Plot)
2. Expected Loss by Supplier
3. Current vs Proposed Risk Comparison
4. Supplier Risk Details Table
5. Concentration Risk Profile

---

# 🎯 Recommendations

## Priority 1: Immediate Mitigation

* Diversify sourcing for high-risk suppliers
* Improve supplier quality monitoring
* Reduce operational variability
* Implement continuous supplier KPI tracking

## Priority 2: Monitoring & Validation

* Validate projected savings through KPI monitoring
* Assess operational improvements before scaling mitigation strategies

## Priority 3: Long-Term Optimization

* Extend successful mitigation strategies to medium-risk suppliers
* Enhance supplier performance governance

---

# 📝 Key Assumptions

1. Production downtime loss estimated at ₹50,000/day
2. 40% risk reduction assumed for scenario analysis
3. Concentration risk simulated using spend distribution
4. Historical delivery data used for lead time analysis
5. Savings assume stable operating conditions

---

# ⚠️ Limitations

1. Dataset size limited to 777 records
2. Concentration risk is simulated rather than operationally mapped
3. Limited defect inspection data
4. Seasonal effects not modeled
5. External geopolitical/economic risks excluded

---

# 📈 Future Enhancements

* Add predictive disruption modeling
* Include supplier financial health indicators
* Integrate external risk data sources
* Add real-time monitoring
* Expand to multi-tier supplier analysis

---

# 👤 Author

**Rajeev Dhami**

Aspiring Data Analyst | Supply Chain Analytics | Python | SQL | Power BI

* LinkedIn: https://www.linkedin.com/in/rajeev-dhami-994552107/
* GitHub: https://github.com/RajeevDhami/supply-chain-risk-analysis

---

# 📜 License

This project is licensed under the MIT License.

---

# 🙏 Acknowledgments

* Procurement KPI Analysis Dataset (Kaggle)
* Python (Pandas, NumPy)
* Jupyter Notebook
* Microsoft Power BI

---

**This project demonstrates a data-driven approach to supplier risk assessment and supply chain disruption cost analysis using business intelligence and analytics techniques.**
