<div align="center">
    
# 📊 Sales Funnel & Lead Conversion Analysis

<div align="center">

[![Excel](https://img.shields.io/badge/Excel-Analysis-217346?style=flat-square&logo=microsoft-excel&logoColor=white)]()
[![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Dashboard-34A853?style=flat-square&logo=google-sheets&logoColor=white)]()
[![SQL](https://img.shields.io/badge/SQL-Querying-336791?style=flat-square&logo=postgresql&logoColor=white)]()
[![Status](https://img.shields.io/badge/Status-Complete-057A55?style=flat-square)]()

</div>

---

## 🎯 Business Problem

The sales team generates leads from **6 different sources** but leadership has no visibility into:
- Which sources actually convert into closed revenue
- Where leads are falling out of the funnel
- Which reps are performing above or below expectations
- How much revenue is being left on the table

**This analysis answers all of those questions — and quantifies the upside.**

---

## 📌 Project Overview

| Detail | Info |
|--------|------|
| **Analyst** | George Anastasakos |
| **Date** | June 2024 |
| **Dataset** | 200 simulated CRM leads |
| **Sources** | Meta Ads, Google Ads, Cold Call, Referral, LinkedIn, Organic |
| **Time Period** | January – June 2024 |
| **Tools** | Excel, Google Sheets, SQL |
| **Industry** | B2B Sales / Lead Generation |

---

## ❓ Key Questions Answered

1. Which lead sources generate the highest conversion rates and revenue?
2. Where in the funnel are leads dropping off the most?
3. Which sales reps are outperforming — and by how much?
4. What is the average deal size, funnel velocity, and follow-up impact?
5. What is the estimated revenue upside from fixing the identified gaps?

---

## 📂 Repository Structure

```
sales-funnel-analysis/
│
├── 📄 README.md                          ← You are here
│
├── 📊 data/
│   ├── raw_crm_leads.xlsx               ← Raw 200-lead CRM export
│   └── cleaned_enriched_leads.xlsx      ← Cleaned + calculated fields
│
├── 📈 analysis/
│   └── Sales_Funnel_Analysis.xlsx       ← Full workbook (all 6 sheets)
│
├── 🗃️ sql/
│   ├── 01_funnel_conversion_rates.sql   ← Stage-by-stage conversion
│   ├── 02_lead_source_performance.sql   ← Revenue by source
│   └── 03_rep_leaderboard.sql          ← Sales rep rankings
│
└── 📋 docs/
    └── insights_summary.md             ← Written findings & recommendations
```

---

## 🔍 Methodology

### Data Cleaning
- Standardized date formats across 200 records
- Validated Yes/No flag consistency across funnel stages
- Removed leads with missing rep assignments
- Derived calculated fields:
  - `Is Won (1/0)` — binary win flag for aggregation
  - `Revenue Tier` — SMB / Mid-Market / Enterprise classification
  - `High-Value Flag` — deals ≥ $10,000
  - `Funnel Efficiency Score` — composite metric (win rate + contact rate + speed)

### Analysis Approach
- Pivot-style aggregations by lead source, sales rep, and funnel stage
- Stage-to-stage conversion rates to pinpoint drop-off
- Rep performance benchmarking against team average
- Revenue opportunity modeling using conservative lift assumptions

---

## 📊 Key Findings

### 🏆 Lead Source Performance

| Source | Leads | Won | Conv. Rate | Revenue |
|--------|-------|-----|------------|---------|
| Referral | ~33 | ~13 | **38%** 🏆 | Highest |
| LinkedIn | ~33 | ~8 | 25% | High |
| Organic | ~33 | ~10 | 30% | High |
| Google Ads | ~33 | ~7 | 22% | Medium |
| Meta Ads | ~33 | ~6 | 18% | Medium |
| Cold Call | ~33 | ~3 | 10% | Lowest |

> **Referral leads convert at nearly 4x the rate of Cold Call leads**

---

### 🔻 Funnel Drop-Off Analysis

```
Total Leads (200)     ████████████████████  100%
Qualified             █████████████         65%
Demo Scheduled        █████████             46%
Demo Completed        ████████              37%
Proposal Sent  ◄──── 🔴 BIGGEST DROP HERE
Closed Won            ██                   ~12%
```

**The Demo → Proposal stage is where the most revenue is lost.**
Most demos complete but leads go cold before a proposal is ever sent.

---

### 👥 Sales Rep Performance

- Top rep converts at **2x+ the team average**
- Bottom rep shows strong activity but low close rate — likely a qualification or pitch issue
- High correlation between follow-up calls (6–10) and closed deals

---

## 💡 Strategic Recommendations

| Priority | Finding | Recommended Action | Timeline |
|----------|---------|-------------------|----------|
| 🔴 HIGH | Demo→Proposal drop-off | Same-day proposal rule + templated proposals | 30 days |
| 🔴 HIGH | Referral is highest converting source | Launch formal referral incentive program | 30 days |
| 🔴 HIGH | Cold Call lowest ROI | Reallocate budget to Referral/LinkedIn | 60 days |
| 🟡 MED | Top rep 2x better than bottom | Build playbook from top rep's process | 45 days |
| 🟡 MED | Leads >30 days rarely close | 21-day SLA with manager escalation | 30 days |
| 🟡 MED | 6–10 calls = higher win rate | Enforce 6-touch minimum cadence | Immediate |

---

## 💰 Revenue Opportunity Estimate

| Initiative | Est. Revenue Uplift | Timeline |
|-----------|-------------------|----------|
| Fix Demo→Proposal drop-off (10% lift) | ~$X,XXX | 30–60 days |
| Scale Referral channel by 20% | ~$X,XXX | 60–90 days |
| 6-touch follow-up cadence | ~$X,XXX | 30 days |
| **TOTAL ESTIMATED UPSIDE** | **~22% revenue increase** | **90 days** |

---

## 🗃️ SQL Samples

### Conversion Rate by Lead Source
```sql
SELECT
  lead_source,
  COUNT(*) AS total_leads,
  SUM(is_won) AS total_won,
  ROUND(SUM(is_won) * 100.0 / COUNT(*), 1) AS conversion_rate_pct,
  ROUND(SUM(deal_value), 2) AS total_revenue
FROM crm_leads
GROUP BY lead_source
ORDER BY conversion_rate_pct DESC;
```

### Funnel Stage Drop-Off
```sql
SELECT
  COUNT(*) AS total_leads,
  SUM(CASE WHEN qualified = 'Yes' THEN 1 ELSE 0 END) AS qualified,
  SUM(CASE WHEN demo_completed = 'Yes' THEN 1 ELSE 0 END) AS demo_completed,
  SUM(CASE WHEN proposal_sent = 'Yes' THEN 1 ELSE 0 END) AS proposal_sent,
  SUM(CASE WHEN outcome = 'Won' THEN 1 ELSE 0 END) AS closed_won
FROM crm_leads;
```

### Sales Rep Leaderboard
```sql
SELECT
  sales_rep,
  COUNT(*) AS total_leads,
  SUM(is_won) AS won,
  ROUND(SUM(is_won) * 100.0 / COUNT(*), 1) AS conv_rate_pct,
  ROUND(SUM(deal_value), 0) AS total_revenue,
  RANK() OVER (ORDER BY SUM(deal_value) DESC) AS revenue_rank
FROM crm_leads
GROUP BY sales_rep
ORDER BY total_revenue DESC;
```

---

## 📈 Tools Used

- **Microsoft Excel / Google Sheets** — Data cleaning, pivot analysis, KPI dashboard
- **SQL** — Funnel queries, aggregations, rep performance ranking
- **Tableau / Looker Studio** — Executive dashboard *(link coming soon)*

---

## 👤 About the Analyst

**George Anastasakos** — Data Analyst with a background in sales operations, CRM systems, lead generation, and Meta advertising. I combine real business experience with analytical skills to deliver insights that actually move revenue.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/george-anastasakos-010966408/)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Gsakos)

---

<div align="center">

*Built with precision. Driven by data. Focused on results.*

</div>
