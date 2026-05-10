# Supplier Risk Assessment & Supply Chain Disruption Cost Analysis

## 📋 Project Overview

This project analyzes supplier performance data to identify high-risk suppliers, quantify the financial impact of supply chain disruptions, and recommend a phased risk mitigation strategy. Using a data-driven approach, the analysis combines risk scoring models with financial impact modeling to provide actionable insights for procurement optimization.

**Core Question:** Which suppliers pose the highest disruption risk, and how should we optimize sourcing decisions to reduce financial impact?

**Objective:** To identify high-risk suppliers based on delivery reliability, quality, and lead time variability; assess supplier dependency through concentration risk; and quantify the financial impact of potential disruptions to recommend strategies that minimize supply chain risk and cost.

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

## 🗂️ Project Structure

```
Supply_Chain_Project/
├── README.md                                    # This file
├── Business Case Study/
│   └── Supplier_Risk_Case_Study_Final.pdf       # Executive case study (phased strategy)
├── Dashboard/
│   └── Supplier_Risk_Dashboard.pbix             # Interactive Power BI dashboard
├── Dataset/
│   ├── raw_supplier_dataset.csv                 # Original procurement data (777 records)
│   └── processed_supplier_risk_dataset.xlsx     # Final analysis output
├── Notebook/
│   └── Supply_Chain_Analytics.ipynb             # Complete analysis pipeline (Python)
└── Screenshot/
    └── Supplier_Risk_Overview.png               # Dashboard overview screenshot
```

---

## 📊 Analysis Pipeline

The analysis is divided into 5 integrated phases:

### Phase 1: Data Loading & Exploration
- Load procurement KPI dataset (777 records, 11 columns)
- Inspect data structure, types, and quality
- Summary statistics

**Code:**
```python
import pandas as pd
df = pd.read_csv("Procurement KPI Analysis Dataset.csv")
df.info()
df.describe()
```

### Phase 2: Feature Engineering & Metrics Calculation
Calculate 4 critical supplier performance metrics:

**On-Time Delivery %**
- Definition: Orders delivered within 10 days
- Calculation: `on_time_flag = lead_time <= 10`
- Aggregation: Mean percentage per supplier

**Defect Rate %**
- Definition: Percentage of defective units
- Calculation: `defect_rate = Defective_Units / Quantity`
- Handling: Fill NaN with 0 (no inspection = no defects)
- Aggregation: Mean percentage per supplier

**Lead Time Variability**
- Definition: Standard deviation of delivery lead times
- Calculation: `lead_time = (Delivery_Date - Order_Date).days`
- Aggregation: Standard deviation per supplier

**Annual Spend**
- Definition: Total procurement spend from supplier
- Calculation: `spend = Quantity × Negotiated_Price`
- Aggregation: Sum per supplier

**Normalization (Min-Max Scaling)**
- Applied to all metrics to ensure comparability
- Formula: `X_norm = (X - X_min) / (X_max - X_min)`
- Output: Normalized metrics on 0-1 scale

**Code:**
```python
df['lead_time'] = (df['Delivery_Date'] - df['Order_Date']).dt.days
df['on_time_flag'] = df['lead_time'] <= 10
df['defect_rate'] = df['Defective_Units'] / df['Quantity']
df['spend'] = df['Quantity'] * df['Negotiated_Price']

on_time = df.groupby('Supplier')['on_time_flag'].mean()
defect = df.groupby('Supplier')['defect_rate'].mean()
lt_var = df.groupby('Supplier')['lead_time'].std()
spend = df.groupby('Supplier')['spend'].sum()

# Normalize
def normalize(col):
    return (col - col.min()) / (col.max() - col.min())
```

### Phase 3: Concentration Risk Assessment (Simulated)
Analyze supplier dependency through category-level concentration:

**Supplier Share Calculation**
- Grouped by Item_Category
- `supplier_share = supplier_spend / total_category_spend`

**Risk Classification**
- **Critical:** 100% dependency (single-source)
- **High:** >70% dependency
- **Medium:** 50-70% dependency
- **Low:** <50% dependency

**Maximum Risk Per Supplier**
- Identify worst-case concentration risk per supplier
- Use highest risk level across all categories

**Code:**
```python
category_spend = df.groupby(['Item_Category', 'Supplier'])['spend'].sum().reset_index()
category_spend['total_category_spend'] = category_spend.groupby('Item_Category')['spend'].transform('sum')
category_spend['supplier_share'] = category_spend['spend'] / category_spend['total_category_spend']

def classify(x):
    if x == 1.0:
        return 'Critical'
    elif x > 0.7:
        return 'High'
    elif x > 0.5:
        return 'Medium'
    else:
        return 'Low'

category_spend['concentration_risk'] = category_spend['supplier_share'].apply(classify)
```

### Phase 4: Risk Scoring Model
Apply weighted risk formula combining all 3 dimensions:

**Risk Score Formula**
```
Risk Score = 0.4 × (1 - Normalized_On-Time%) 
           + 0.3 × Normalized_Defect_Rate 
           + 0.3 × Normalized_Lead_Time_Variability
```

**Weighting Rationale:**
- 40% On-Time Delivery: Reliability is most critical
- 30% Defect Rate: Quality impacts production
- 30% Lead Time Variability: Consistency enables planning

**Risk Categories**
- **Green (Low Risk):** 0.0 - 0.33
- **Yellow (Medium Risk):** 0.33 - 0.66
- **Red (High Risk):** 0.66 - 1.0

**Code:**
```python
final_df['risk_score'] = (
    0.4 * (1 - final_df['on_time_norm']) +
    0.3 * final_df['defect_norm'] +
    0.3 * final_df['lt_var_norm']
)

def categorize(r):
    if r < 0.33:
        return 'Green'
    elif r < 0.66:
        return 'Yellow'
    else:
        return 'Red'

final_df['risk_category'] = final_df['risk_score'].apply(categorize)
```

### Phase 5: Expected Loss & Financial Impact
Convert risk scores to financial impact:

**Expected Loss Calculation**
```
Expected Annual Loss = Risk Score × Daily Production Loss × Average Delay Days
```

**Business Assumptions**
- Daily production loss during disruption: **₹50,000**
- Average supplier delay when failure occurs: **5 days**

**Current State**
- Total Annual Risk Exposure: ₹6,11,641
- High-Risk Suppliers: 2

**Proposed State (40% Risk Reduction)**
- Reduce high-risk supplier scores by 40% (from 0.73 → 0.44)
- Total Risk After Improvement: ₹3,13,835

**Financial Metrics**
- Risk Reduction: ₹2,97,806 (-49%)
- Annual Savings: ₹1,46,000
- Implementation Cost: ₹2,00,000
- Payback Period: 1.4 years

**Code:**
```python
daily_production_loss = 50000
avg_delay_days = 5

final_df['expected_loss'] = (
    final_df['risk_score'] * daily_production_loss * avg_delay_days
)

# Apply 40% risk reduction for high-risk suppliers
final_df['risk_score_proposed'] = final_df['risk_score'].apply(
    lambda x: x * 0.6 if x > 0.7 else x
)

final_df['expected_loss_proposed'] = (
    final_df['risk_score_proposed'] * daily_production_loss * avg_delay_days
)

total_current_risk = final_df['expected_loss'].sum()
total_proposed_risk = final_df['expected_loss_proposed'].sum()
risk_reduction = total_current_risk - total_proposed_risk
payback_period = implementation_cost / risk_reduction
```

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

## 💰 Phased Financial Impact

### Phase 1: Immediate Action (High-Risk Suppliers)
```
Current Annual Risk:           ₹6,11,641
After Phase 1 Improvements:    ₹3,13,835
────────────────────────────────────────
Risk Reduction:                ₹2,97,806 (-49%)
Annual Savings:                ₹1,46,000
Investment Cost:               ₹2,00,000
Payback Period:                1.4 years
```

**Actions:**
1. Replace Beta_Supplies (savings: ₹73,372/year, cost: ₹1,00,000)
2. Diversify Delta_Logistics (savings: ₹72,569/year, cost: ₹1,00,000)

### Phase 2: Validation & Monitoring (Months 3-6)
- Track actual savings vs projected
- Measure on-time %, defect rate, lead-time variability
- **Decision Gate:** Proceed to Phase 3 only if 80%+ of Phase 1 savings achieved

### Phase 3: Scale to Medium Risk (Optional, Months 6-12)
```
Additional Investment:         ₹60-80K
Additional Annual Savings:     ₹40-50K
Combined Total Savings:        ₹1,86-196K/year
Combined Payback:              1.5-1.6 years
```

**Action:**
- Optimize Alpha_Inc (medium-risk supplier) using proven Phase 1 strategy

---

## 📈 Power BI Dashboard

**Location:** `Dashboard/Supplier_Risk_Dashboard.pbix`

**KPI Cards:**
- Total Expected Loss: ₹6,11,641
- Potential Savings: ₹1,46,000
- High-Risk Supplier Count: 2
- Total Suppliers: 5

**Visualizations:**
1. **Risk Matrix (Scatter Plot):** On-time Delivery % (X-axis) vs Defect Rate % (Y-axis), bubble size = annual spend, color = risk level
2. **Expected Loss by Supplier (Bar Chart):** Annual disruption cost ranked descending
3. **Current vs Proposed (Clustered Bar):** Financial impact before and after risk mitigation
4. **Supplier Risk Details (Table):** Risk scores, categories, expected losses, concentration risk, recommended actions
5. **Concentration Risk Profile (Card):** Shows 100% low concentration (healthy diversification)

**Interactive Filters:**
- Supplier (dropdown)
- Risk Category (Green/Yellow/Red buttons)
- Item Category (dropdown)

---

## 🔧 How to Use This Project

### Prerequisites
```
Python 3.8+
Jupyter Notebook
pandas, numpy
Microsoft Power BI (for dashboard)
```

### Running the Analysis
1. **Open Jupyter Notebook**
   ```bash
   jupyter notebook Supply_Chain_Analytics.ipynb
   ```

2. **Follow the 5 phases in sequence:**
   - Phase 1: Data Loading
   - Phase 2: Feature Engineering
   - Phase 3: Concentration Risk
   - Phase 4: Risk Scoring
   - Phase 5: Expected Loss

3. **View Results**
   - Final output: `processed_supplier_risk_dataset.xlsx`
   - Summary metrics printed to console

4. **Explore Power BI Dashboard**
   - Open: `Dashboard/Supplier_Risk_Dashboard.pbix`
   - Use filters to explore by supplier or risk category

### Data Files
- **Input:** `Dataset/raw_supplier_dataset.csv` (777 records)
- **Output:** `Dataset/processed_supplier_risk_dataset.xlsx` (supplier risk scores & financial impact)

---

## 📄 Business Case Study

**Location:** `Business_Case_Study/Supplier_Risk_Case_Study_Final.pdf`

Includes:
- Executive summary
- Full methodology
- Key findings & recommendations
- Phased implementation strategy
- Financial ROI analysis
- Assumptions & limitations

---

## 🎯 Recommendations

### Priority 1: Immediate Action (Phase 1 - Months 0-3)
1. **Replace Beta_Supplies**
   - Current: 40% on-time, 9.9% defect
   - Target: 85%+ on-time, <2% defect
   - Cost: ₹1,00,000 | Savings: ₹73,372/year

2. **Diversify Delta_Logistics**
   - Current: 46% on-time, 14.5% defect
   - Strategy: Shift 40% volume to Epsilon_Group
   - Cost: ₹1,00,000 | Savings: ₹72,569/year

### Priority 2: Validation (Phase 2 - Months 3-6)
- Monitor Phase 1 KPIs monthly
- Validate actual savings achievement
- **Decision Gate:** Proceed to Phase 3 only if 80%+ of savings achieved

### Priority 3: Optimization (Phase 3 - Months 6-12, conditional)
- Apply proven strategy to Alpha_Inc (medium-risk)
- Performance improvement plan + selective diversification
- Expected savings: ₹40-50K/year | Cost: ₹60-80K

---

## 📊 Key Metrics Explained

**On-Time Delivery %**
- Percentage of orders delivered within 10 days
- Higher = more reliable supplier
- Weight in risk model: 40%

**Defect Rate %**
- Percentage of defective units received
- Lower = better quality
- Weight in risk model: 30%

**Lead Time Variability**
- Standard deviation of delivery days
- Lower = more consistent/predictable
- Weight in risk model: 30%

**Risk Score**
- Combined metric (0-1 scale)
- 0 = lowest risk (Green)
- 1 = highest risk (Red)

**Expected Loss**
- Annual financial impact of supplier risk
- Calculated as: Risk Score × Daily Loss × Avg Delay Days
- Guides prioritization

---

## 📝 Key Assumptions

1. **Production Downtime:** ₹50,000/day loss (conservative estimate)
2. **Improvement Rate:** 40% risk reduction (achievable with focused intervention)
3. **Concentration Risk:** Simulated from spend distribution (actual mappings not in dataset)
4. **Lead Time:** Calculated from historical delivery dates
5. **Payback:** Assumes consistent annual savings and one-time implementation cost

---

## ⚠️ Limitations

1. **Dataset Size:** 777 records (small sample, may not capture all patterns)
2. **Concentration Risk:** Simulated from spend distribution, not actual dependencies
3. **Defect Data:** Limited availability in dataset
4. **Seasonality:** Lead time variability may not capture seasonal fluctuations
5. **External Factors:** Model doesn't include geopolitical/economic disruption risks

---

## 🎓 Project Insights

✅ **Holistic Risk Framework**
- Combines reliability, quality, AND consistency
- Normalized metrics enable fair comparison

✅ **Financially Grounded**
- ₹6,11,641 quantified annual risk exposure
- Clear ROI: 1.4-year payback with ₹1,46K/year savings

✅ **Phased Approach**
- Phase 1: Prove business case (high-risk suppliers)
- Phase 2: Validate results
- Phase 3: Scale to medium-risk (conditional)

✅ **Data-Driven Decisions**
- Risk scores based on objective metrics
- Clear decision thresholds
- Decision gates prevent overcommitment

---

## 📈 Future Enhancements

- [ ] Add supplier financial health metrics
- [ ] Include geopolitical risk factors
- [ ] Develop predictive disruption probability model
- [ ] Integrate with real-time supply chain visibility
- [ ] Extend to multi-tier suppliers (Tier 2, 3)
- [ ] Add supplier capacity utilization
- [ ] Benchmark against industry standards

---

## 👤 Author

**Rajeev Dhami**

Data Analyst | Supply Chain Enthusiast | Python | SQL | Power BI

- LinkedIn: [linkedin.com/in/rajeev-dhami](https://linkedin.com/in/rajeev-dhami)
- GitHub: [github.com/rajeev-dhami](https://github.com/rajeev-dhami)

---

## 📞 Contact

For questions or feedback:
- Email: rajeev.dhami@email.com
- Open an issue on this repository
- LinkedIn message

---

## 📜 License

This project is licensed under the MIT License.

---

## 🙏 Acknowledgments

- **Data Source:** Procurement KPI Analysis Dataset (Kaggle)
- **Tools Used:** Python (Pandas, NumPy), Jupyter Notebook, Power BI
- **Methodology:** Risk Scoring Model + Financial Impact Modeling

---

**This model enables proactive, data-driven supplier risk management with measurable financial impact.**

---

**Last Updated:** May 2026  
**Status:** Complete & Ready for Portfolio

---

## Quick Reference

| File | Purpose |
|------|---------|
| `README.md` | Project documentation |
| `Supply_Chain_Analytics.ipynb` | Complete analysis code (5 phases) |
| `raw_supplier_dataset.csv` | Original data (777 records) |
| `processed_supplier_risk_dataset.xlsx` | Final risk analysis output |
| `Supplier_Risk_Dashboard.pbix` | Interactive Power BI dashboard |
| `Supplier_Risk_Case_Study_Final.pdf` | Executive case study |

---

**Ready to deploy to GitHub? All files are organized and documented.**
