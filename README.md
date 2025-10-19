#  Case Study:  Dignosing Rootcause to Improve WEGO Conversion Through Real-Time Pricing & Funnel Intelligence

---

## 🗒️Overview

This case study simulates a real-world project for **Wego**, a leading travel platform, focused on solving key business intelligence challenges in travel data:  
- **Price volatility** and **data lag** from multiple providers  
- **Conversion drop-offs** in the search-to-booking funnel  

The project demonstrates how real-time data pipelines and actionable dashboards can help diagnosing root causes and increase conversion and user trust across millions of daily travelers.

---

## 📍 1. Context & Problem Statement

Wego aggregates millions of flight and hotel options daily from multiple providers across regions. Internal analysis revealed two major challenges:

1. **Price Volatility & Data Lag**  
   Flight and hotel prices often changed faster than dashboards updated, causing mismatches between displayed and actual booking costs.

2. **Conversion Drop After Search**  
   Despite high search activity, the conversion rate from *search → click → booking redirect* was declining by 8% QoQ.  
   Product managers suspected **pricing inconsistencies** and **irrelevant results** as main culprits.

---

## ✅ 2. Business Goal

Diagnosing business problems:

- Validate the hypothesis
- Uncover root causes
- Quantify the business impact using **funnel**, **pricing**, and **system intelligence** 

Increase **conversion rate** and **user trust** by improving data freshness, accuracy, and funnel visibility through:

- Real-time **price accuracy tracking**  
- **Funnel performance visualization**  
- **Partner (supplier) reliability scoring**

---

##  3. Data Sources & Architecture

| Source | Type | Key Fields | Frequency |
|--------|------|-------------|------------|
| Flight API Logs | External | provider_id, route, price, timestamp | Real-time |
| Booking Clickstream | Event Data | user_id, session_id, event_type, timestamp | Continuous |
| Search Funnel Data | Internal DB | searches, clicks, redirects, bookings | Hourly |
| Supplier Metadata | Static | provider_id, region, latency_score | Monthly |

**Cloud:** BigQuery (GCP)  
**BI Tools:** Looker Studio, Tableau Public  
**ETL Simulation:** Airflow

---

## 🔍 4. Analytical Approach

### A. Data Exploration & Cleaning
- Used **SQL (CTEs + Window Functions)** to unify logs and filter invalid price entries (>3 hours stale).  
- Built aggregated tables in **BigQuery** for each flight route combining search, click, and booking data.  
- Applied data deduplication to remove duplicate redirects from API retries.

```sql
WITH clean_data AS (
  SELECT
    route,
    provider_id,
    price,
    event_type,
    user_id,
    timestamp,
    ROW_NUMBER() OVER (PARTITION BY user_id, route ORDER BY timestamp DESC) AS rn
  FROM raw_logs
  WHERE price IS NOT NULL
)
SELECT * FROM clean_data WHERE rn = 1;
```

---

### **B. Funnel & Price Accuracy Analysis**

- Constructed a funnel from **Search → Result → Redirect → Booking**  
- Introduced a new KPI:  
Price Accuracy Rate = Accurate Prices / Total Searches
- Analyzed correlation between **price accuracy** and **conversion rate** per provider.  

✅ **Finding:**  
Providers with **<85% price accuracy** had **30% lower conversion** than average.

---

## **C. Visualization & Stakeholder Dashboards**

Created two interactive dashboards in **Tableau**:

### **1. Price Accuracy Monitor**
- Real-time % of outdated prices by provider & route  
- Alerts for latency spikes > 5 minutes  

### **2. Funnel Conversion Insights**
- Visualized flow: **Search → Click → Redirect → Booking**  
- Segmentation by region, device type, and provider  

These dashboards were designed for **Product** and **Business Ops** teams to quickly identify performance bottlenecks and data freshness issues.

---

## ✅ **5. Business Insights**

| Insight | Impact | Recommendation |
|----------|---------|----------------|
| Price updates lagging 3–6 mins for 4 key providers | 15% lower CTR | Prioritize API caching refresh or exclude outdated listings |
| Mobile app users dropped off at redirect step | −10% funnel completion | Optimize redirect UX and speed |
| High seasonality spikes not reflected in forecasts | Inaccurate demand predictions | Integrate ARIMA/Prophet models for seasonal adjustment |

---

## ✅ **6. Outcome Simulation**

After implementing **provider reliability scoring** and **dynamic dashboard refresh**:

| Metric | Before | After | Improvement |
|--------|--------|--------|-------------|
| **Price Accuracy Rate** | 82% | 96% | +17% |
| **Conversion Rate** | 3.8% | 5.2% | +37% |
| **Data Latency (Avg)** | 6 mins | 1.5 mins | −75% |

---

##  **7. 📌 Tools & Skills Demonstrated**

| Category | Tools / Skills |
|-----------|----------------|
| **Data Management** | SQL (CTEs, aggregation, DDL), BigQuery |
| **Visualization** | Tableau, Looker Studio |
| **Exploration** | Funnel & Cohort Analysis |
| **Collaboration** | Requirements documentation with Product Managers |
| **Pipeline Simulation** | Airflow scheduling concepts |
| **Predictive Outlook** | ARIMA / Prophet for seasonality forecasting |

---

##  **8. Strategic Takeaway**

This project illustrates how **data freshness** and **funnel transparency** directly influence **user trust and revenue** in travel platforms.  
By maintaining **accurate real-time datasets** and delivering **data-driven dashboards**, analysts empower Wego to create the **seamless travel discovery experience** it promises — turning millions of searches into confident bookings.

---

##  **Keywords**
`Travel Analytics` · `Funnel Optimization` · `Data Quality` · `Real-Time Dashboards` · `SQL` · `Tableau` · `BigQuery` · `User Behavior Analysis` · `Conversion Insights` · `Stakeholder Reporting`

---

## 🧩 **Author**
**Samantha Yoong**  
📍 Kuala Lumpur, Malaysia  
🔗 [LinkedIn](https://www.linkedin.com/in/samantha-yoong-8551b4226/) | [Tableau Portfolio](https://public.tableau.com/app/profile/samantha.yoong/vizzes) | [GitHub Projects](https://samanthayoong.github.io/my-portfolio/)



