# Supplier Risk Assessment & Supply Chain Disruption Cost Analysis

## 📋 Project Overview

This project analyzes supplier performance data to identify high-risk suppliers, quantify the financial impact of supply chain disruptions, and recommend a phased risk mitigation strategy. Using a data-driven approach, the analysis combines risk scoring models with financial impact modeling to provide actionable insights for procurement optimization.

**Core Question:** Which suppliers pose the highest disruption risk, and how should we optimize sourcing decisions to reduce financial impact through a phased, data-driven approach?

---

## 🎯 Key Findings

| Metric | Value |
|--------|-------|
| **Total Annual Risk Exposure** | ₹6,11,641 |
| **High-Risk Suppliers** | 2 (Beta_Supplies, Delta_Logistics) |
| **Phase 1 Annual Savings** | ₹1,46,000 |
| **Phase 1 Investment Cost** | ₹2,00,000 |
| **Phase 1 Payback Period** | 1.4 years |
| **Combined Impact (Phase 1+3)** | ₹1,86-196K/year |

**Critical Insight:** Risk is primarily driven by supplier performance variability (on-time delivery, quality, consistency) rather than supplier dependency concentration.

---

## 📊 Phased Strategy

### Phase 1: Immediate Action (Months 0-3)
**Target:** High-risk suppliers (Beta_Supplies, Delta_Logistics)
- Replace Beta_Supplies with better performer
- Diversify Delta_Logistics (shift 40% volume to Epsilon_Group)
- **Investment:** ₹2,00,000 | **Savings:** ₹1,46,000/year | **Payback:** 1.4 years

### Phase 2: Validation & Monitoring (Months 3-6)
**Target:** Proof of concept validation
- Track KPIs: On-time delivery %, defect rate, lead-time variability
- Measure actual savings vs projected
- **Decision Gate:** IF Phase 1 achieves 80%+ of projected savings → PROCEED to Phase 3

### Phase 3: Scale to Medium Risk (Months 6-12)
**Target:** Medium-risk supplier (Alpha_Inc) optimization
- Apply proven strategy to Alpha_Inc (59% on-time delivery)
- Performance improvement plan + selective diversification
- **Investment:** ₹60-80K | **Savings:** ₹40-50K/year
- **Condition:** Only proceed if Phase 1 validates business case

---

## 🗂️ Project Structure

```
supplier-risk-analysis/
├── README.md                           # This file
├── CASE_STUDY.md                       # Full business case study 
│
├── notebooks/
│   ├── Supply Chain Analytics.ipnyb
│   
├── data/
│   ├── raw/
│   │   └── Procurement_KPI_Analysis_Dataset.csv    # Source data (777 records)
│   └── processed/
│       ├── Final_KPI_Procurement_Dataset.xls       # Processed Dataset
│
├── dashboards/
│   └── supplier_risk_dashboard.pbix               # Power BI dashboard
│
└── outputs/
    ├── Supplier_Risk_Case_Study_Final.pdf         # Executive case study
    └── visualizations/                            # Charts & plots
```

---

## 📈 Methodology

### Phase 1-2: Performance Metrics Calculation
**Evaluated suppliers on 3 dimensions:**
- **On-time Delivery %** (40% weight): Reliability indicator
- **Defect Rate %** (30% weight): Quality indicator
- **Lead Time Variability** (30% weight): Consistency indicator

All metrics were normalized (Min-Max scaling) to ensure comparability.

### Phase 3: Concentration Risk Assessment
Analyzed supplier share per product category:
- Single-source items (100% dependency) = Critical risk
- High concentration (>70% from one supplier) = High risk
- Medium concentration (50-70%) = Medium risk
- Diversified (<50%) = Low risk

**Finding:** All suppliers are <50% per category (healthy diversification). Risk is performance-driven, not dependency-driven.

### Phase 4: Risk Scoring Model
Applied weighted risk formula:
```
Risk Score = 0.4 × (1 - On-Time%) + 0.3 × Defect% + 0.3 × Lead_Time_Variability
```

**Scoring Scale:**
- Green (Low Risk): 0.0 - 0.3
- Yellow (Medium Risk): 0.3 - 0.7
- Red (High Risk): 0.7 - 1.0

### Phase 5: Financial Quantification
Converted risk scores to financial impact:
```
Expected Loss = Risk Score × Daily Production Loss × Average Delay Days
```

**Assumptions:**
- Daily production loss during disruption: ₹50,000
- Average supplier delay when failure occurs: 5 days

---

## 📊 Key Results by Supplier

| Supplier | Risk Score | Risk Level | Expected Loss | Status |
|----------|-----------|-----------|---------------|--------|
| **Beta_Supplies** | 0.73 | HIGH | ₹1,83,372 | Action Required (Phase 1) |
| **Delta_Logistics** | 0.73 | HIGH | ₹1,81,569 | Action Required (Phase 1) |
| **Alpha_Inc** | 0.59 | MEDIUM | ₹1,47,806 | Monitor & Plan (Phase 3) |
| **Epsilon_Group** | 0.30 | LOW | ₹75,978 | Maintain |
| **Gamma_Co** | 0.09 | LOW | ₹22,914 | Maintain |

---

## 💰 Financial Impact

### Phase 1 Impact (High-Risk Suppliers)
```
Current Annual Risk Exposure:    ₹6,11,641
After Phase 1 (Beta + Delta):    ₹3,13,835
────────────────────────────────────────
Risk Reduction:                  ₹2,97,806 (-49%)
Annual Savings:                  ₹1,46,000
Implementation Cost:             ₹2,00,000
Payback Period:                  1.4 years
```

### Phase 1 + Phase 3 Impact (Including Medium-Risk)
```
Combined Annual Savings:         ₹1,86-196K/year
Combined Investment:             ₹2,60-280K
Combined Payback:                1.5-1.6 years
3-Year Cumulative Benefit:       ₹3,80-420K
```

---

## 🔧 How to Run This Analysis

### Prerequisites
```bash
Python 3.8+
Jupyter Notebook
pandas, numpy, matplotlib, seaborn
scikit-learn (for Min-Max normalization)
```

### Installation
```bash
# Clone repository
git clone https://github.com/yourusername/supplier-risk-analysis.git
cd supplier-risk-analysis

# Install dependencies
pip install -r requirements.txt
```

### Running the Analysis
```bash
# 1. Start Jupyter
jupyter notebook

# 2. Run notebooks in order:
# - 01_data_exploration.ipynb
# - 02_feature_engineering.ipynb
# - 03_concentration_risk.ipynb
# - 04_risk_scoring.ipynb
# - 05_expected_loss.ipynb
# - 06_recommendations.ipynb
```

### Data Requirements
The analysis requires the Procurement KPI Analysis Dataset with the following columns:
- `PO_ID`: Purchase order identifier
- `Supplier`: Supplier name
- `Order_Date`: Date order placed
- `Delivery_Date`: Actual delivery date
- `Item_Category`: Product category
- `Order_Status`: Delivery status
- `Quantity`: Order quantity
- `Unit_Price`: Unit cost
- `Negotiated_Price`: Final negotiated price
- `Defective_Units`: Number of defective units (if any)
- `Compliance`: Compliance certification

---

## 📊 Power BI Dashboard

The project includes an interactive Power BI dashboard (`supplier_risk_dashboard.pbix`) with:

**KPI Cards:**
- Total Expected Loss (₹6,11,641)
- Potential Savings (₹1,46,000)
- High-Risk Supplier Count (2)
- Total Suppliers (5)

**Visualizations:**
1. **Risk Matrix (Scatter Plot):** On-time Delivery % vs Defect Rate %, bubble size = spend, color = risk level
2. **Expected Loss (Bar Chart):** Annual disruption cost by supplier (sorted descending)
3. **Current vs Proposed (Clustered Bar):** Financial impact of risk mitigation
4. **Supplier Risk Details (Table):** Risk scores, categories, expected losses, concentration risk
5. **Key Insights (Cards):** Summary of high-risk suppliers and savings opportunity

**Interactive Filters:**
- Supplier
- Risk Category (Green/Yellow/Red)
- Item Category

---

## 🎯 Recommendations Summary

### Priority 1: Immediate Action (Months 0-3)
1. **Replace Beta_Supplies**
   - Current: 40% on-time, 9.9% defect
   - Target: 85%+ on-time, <2% defect
   - Cost: ₹1,00,000 | Savings: ₹73,372/year

2. **Diversify Delta_Logistics**
   - Current: 46% on-time, 14.5% defect
   - Strategy: Shift 40% volume to Epsilon_Group
   - Cost: ₹1,00,000 | Savings: ₹72,569/year

### Priority 2: Validation (Months 3-6)
- Monitor Phase 1 results
- Track KPIs monthly
- Validate ₹1,46,000 savings achievement
- **Decision Point:** Proceed to Phase 3 only if 80%+ of projected savings achieved

### Priority 3: Optimization (Months 6-12)
- **Apply to Alpha_Inc** (if Phase 1 successful)
- Negotiate SLAs, implement quality improvement
- Expected additional savings: ₹40-50K/year
- Cost: ₹60-80K

---

## 📄 Key Assumptions

1. **Production Downtime Cost:** ₹50,000/day based on manufacturing operational impact estimates
2. **Improvement Rate:** 40% risk reduction (conservative; actual range: 30-50%)
3. **Concentration Risk:** Simulated from spend distribution (actual product-supplier mappings not in dataset)
4. **Lead Time Variability:** Calculated from ~30 orders per supplier (may not capture seasonality)
5. **Payback Calculation:** Assumes ₹2,00,000 implementation cost and consistent annual savings

---

## 🎓 Project Insights

### What Makes This Analysis Strong

✅ **Holistic Risk Framework**
- Not just on-time delivery, but quality + consistency
- Normalized metrics ensure fair comparison

✅ **Financially Grounded**
- Quantified impact (₹6,11,641 annual risk exposure)
- Clear ROI (1.4-year payback)
- Actionable cost-benefit analysis

✅ **Phased Approach**
- De-risks implementation (Phase 1 first)
- Proves business case (Phase 2 validation)
- Scalable (Phase 3 conditional on success)

✅ **Data-Driven Decision Making**
- Risk scores based on objective metrics
- Clear thresholds for action
- Decision gates prevent overcommitment

✅ **Practical Recommendations**
- Specific suppliers identified
- Concrete actions (replace, diversify, improve)
- Clear targets (85%+ on-time, <2% defect)

---

## 🔍 Project Limitations

1. **Dataset Size:** 777 records across 5 suppliers (small sample)
2. **Concentration Risk:** Simulated from spend distribution, not actual dependencies
3. **Defect Data:** Limited availability in dataset
4. **Seasonality:** Lead time variability may not capture seasonal patterns
5. **External Factors:** Model doesn't account for geopolitical/economic disruptions

---

## 📈 Future Enhancements

- [ ] Incorporate supplier financial health data (credit ratings, bankruptcy risk)
- [ ] Add geopolitical risk factors
- [ ] Develop predictive model for disruption probability
- [ ] Include supplier capacity utilization
- [ ] Benchmark against industry standards
- [ ] Extend to multi-tier supply chain (Tier 2, Tier 3 suppliers)
- [ ] Integrate with real-time supply chain visibility platform

---

## 👤 Author

**Rajeev Dhami**

Data Analyst | Supply Chain Enthusiast | Python | SQL | Power BI

- **LinkedIn:**  https://www.linkedin.com/in/rajeev-dhami-994552107/
- **GitHub:** https://github.com/RajeevDhami/supply-chain-risk-analysis

---

## 📞 Contact & Questions

For questions, suggestions, or to discuss this analysis:
- Email: rajeev.dhami@email.com
- Open an issue on GitHub
- LinkedIn message

---

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🙏 Acknowledgments

- **Data Source:** Procurement KPI Analysis Dataset (Kaggle)
- **Tools Used:** Python (Pandas, NumPy, Scikit-learn), Jupyter Notebook, Power BI
- **Methodology:** Risk Score Model + Financial Impact Modeling

---

## 📞 How to Use This Repository

1. **For Data Analysts:** Review notebooks (01-06) to understand the analysis pipeline
2. **For Procurement Teams:** Review the Power BI dashboard for visual insights
3. **For Executives:** Read `CASE_STUDY.md` for strategic recommendations
4. **For Developers:** Fork this repo to extend or customize the analysis

---

**Last Updated:** May 2026  
**Status:** Complete & Ready for Review

---

## Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/supplier-risk-analysis.git

# 2. Install requirements
pip install -r requirements.txt

# 3. Open Jupyter and run notebooks 01-06 in order
jupyter notebook

# 4. Open Power BI dashboard
# File: dashboards/supplier_risk_dashboard.pbix

# 5. Read the case study
# File: CASE_STUDY.md
```

---

**This model enables proactive, data-driven supplier risk management with measurable financial impact.**
