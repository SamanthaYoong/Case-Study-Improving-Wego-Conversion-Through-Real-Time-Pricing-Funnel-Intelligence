#  Case Study: Improving Conversion Through Real-Time Pricing & Funnel Intelligence 

_Focus: Identifying root causes & to help with conversion rate dropping QoQ issues through real-time funnel data._

---

## 🗒️Overview

This case study simulates a real-world project for **Wego**, a leading travel platform, focused on solving key business intelligence challenges in travel data:  
- **Price volatility** and **data lag** from multiple providers  
- **Conversion drop-offs** QoQ in the search-to-booking funnel  

The project demonstrates how real-time data pipelines and actionable dashboards can help diagnosing root causes and improving conversion over time.

---

## 📍 1. Context & Problem Statement

Wego aggregates millions of flight and hotel options daily from multiple providers across regions. Internal analysis revealed two major challenges:

1. **Price Volatility & Data Lag**  
   Flight and hotel prices often changed faster than dashboards updated, causing mismatches between displayed and actual booking costs.

2. **Conversion Drop QoQ After Search**  
   Despite high search activity, the conversion rate from *search → click → booking redirect* was declining by ~8% QoQ.  
   Product managers suspected **pricing inconsistencies** and **irrelevant results** as main culprits.

---

## ✅ 2. Objective

Identify whether the conversion drop after the *search stage* was primarily caused by:

- Inaccurate or delayed pricing data  
- Poor search relevance or mismatched results

To increase **conversion rate** by improving data freshness, funnel visiblity,and user trust through:

- Real-time **price accuracy tracking**  
- **Funnel performance visualization**
- **Partner reliability scoring**

---

##  3. ? Data Sources & Architecture

| Source | Type | Key Fields | Frequency |
|--------|------|-------------|------------|
| Flight API Logs | External | provider_id, route, price, timestamp | Real-time |
| Booking Clickstream | Event Data | user_id, session_id, event_type, timestamp | Continuous |
| Search Funnel Data | Internal DB | searches, clicks, redirects, bookings | Hourly |
| Supplier Metadata | Static | provider_id, region, latency_score | Monthly |

**Cloud:** BigQuery (GCP)  
**BI Tools:** Looker Studio, Tableau Public  
**ETL Simulation:** Airflow (Simulated)

---

## 🔍 4. Analytical Approach

### A. Data Exploration & Cleaning
- Used **SQL (CTEs + Window Functions)** to unify logs and filter invalid price entries (>3 hours stale).
- Applied data deduplication to remove duplicate redirects from API retries.
- Built aggregated tables(flight_api_log, fx_conversion_table etc) in **BigQuery** for each flight route combining search, click, and booking data.
- Query the latest flight API log from each provider.
- Example:
```sql
WITH flight_api_log AS (
  SELECT
    ROW_NUMBER() OVER (PARTITION BY provider_id, route ORDER BY timestamp DESC) AS number
    provider_id,
    route,
    fare,
    currency,
    timestamp,
    response_status,
    latency_ms,
  FROM raw_logs
  WHERE fare IS NOT NULL
)
SELECT * FROM flight_api_log WHERE rn = 1;
```

---

### **B. Main Funnel Overview**

**Purpose:** Quantify *where* conversion drop occurs and who it affects most.

- Constructed user funnel: **Search → Click → Detail → Booking**
- Segmented by **region**, **route**, and **provider**
- Measured conversion per stage and trend over time

**🧩 Finding:**  
CTR declined **15%** across SEA, indicating trust or relevance issues post-search. Unpredicted **high demand spike** happening on Q3.

### **C. Price Accuracy & Volatility Monitor**

**Purpose:** Validate if price inconsistencies directly correlate with conversion drop among SEA users.

| Metric | Formula | Description |
|--------|----------|-------------|
| **FX Drift %** | `(price_api - price_fx_normalized) / price_fx_normalized * 100` | Currency normalization deviation |
| **Volatility Index** | `STDDEV(price)/AVG(price)*100` | Price fluctuation severity |
| **Stale Price Rate** | `SUM(flag_stale)/COUNT(*)*100` | % of outdated flight results |
| **Price–Click Correlation** | `CORR(price_dev_pct, CTR)` | Sensitivity of CTR to price deviation |

**Visualization Ideas:**
- Heatmap of **FX Drift % by Provider & Region**
- Scatter plot of **Volatility vs Conversion Rate**
- Tooltip overlay showing CTR trend and price deviation

**🧩 Finding:**  
Providers with **>10% FX drift** and **>20% stale rates** saw **CTR drop 18%**, confirming pricing latency as a conversion risk among SEA users.

## **Pricing Inconsistencies Root Cause Deep Dive**

**Purpose:** Connect **business KPIs** with **system metrics** for root-cause attribution.

**Layers of Analysis:**

| Layer                 | Insight                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------- |
| **FX System**         | FX Drift spiked 8–10% in Thailand due to outdated currency cache.                       |
| **Cache / Latency**   | Data refresh delayed by 6–8 minutes; stale price rate rose 22%.                         |
| **Provider Behavior** | Provider A’s reliability score dropped from 0.94 → 0.82, aligning with 17% CTR decline. |

**Visualization:**

* Multi-axis correlation plot (FX Drift %, Cache/Latency, CTR)
* Drill-down: Provider → Route → Currency
* Event annotations for cache and latency spikes

🧩 **Finding:**
System-level **FX cache staleness** and **latency bottlenecks** amplified price discrepancies, eroding user trust and click-through intent.

### **D. Search Relevance & Conversion Quality***
**Purpose:** Rule out irrelevant search results as alternative cause.

| Metric | Formula |  
|--------|----------|
| **Relevance Score (RS)** | `clicked results / total results shown * normalized CTR` | 
| **Click Relevance Index (CRI)** | `average rank of clicked results/total results` | 
| **Conversion Relevance Rate(CRR)** | `conversions from top 5 results/total conversions` | 
| **Relevance Weighted Conversion Rate (RWCR)** | `∑(conversion Rate x relevance score)` | 
| **Bounce Rate** | `searches with no click/total searches` | 


**Visualization Ideas:**
- Conversion tree (**Search → Click → Detail → Booking**)  
- Region & route filters  
- Tooltip with price deviation and provider reliability

**🧩 Finding:**  
Relevance scores remained stable across most routes, confirming **pricing accuracy**, not content mismatch, as the main driver of the drop.

### **E. Root Cause Deep Dive**

**Purpose:** Connect **business KPIs** with **system metrics** for root-cause attribution.

**Layers of Analysis:**

| Layer                 | Insight                                                                                 |
| --------------------- | --------------------------------------------------------------------------------------- |
| **FX System**         | FX Drift spiked 8–10% in Thailand due to outdated currency cache.                       |
| **Cache / Latency**   | Data refresh delayed by 6–8 minutes; stale price rate rose 22%.                         |
| **Provider Behavior** | Provider A’s reliability score dropped from 0.94 → 0.82, aligning with 17% CTR decline. |

**Visualization:**

* Multi-axis correlation plot (FX Drift %, Cache/Latency, CTR)
* Drill-down: Provider → Route → Currency
* Event annotations for cache and latency spikes

🧩 **Finding:**
System-level **FX cache staleness** and **latency bottlenecks** amplified price discrepancies, eroding user trust and click-through intent.

---

## F.Implementation of ARIMA-Prophet Forecasting Model**
---

## ✅ **5.Insights Summary**

| Insight | Impact | Recommendation |
|----------|---------|----------------|
| Price updates lagging 3–6 mins for 4 key providers | 15% lower CTR | Prioritize API caching refresh or exclude outdated listings |
?Provider| Mobile app users dropped off at redirect step | −10% funnel completion | Optimize redirect UX and speed |
| High seasonality spikes not reflected in forecasts | Inaccurate demand predictions | Integrate ARIMA/Prophet models for seasonal adjustment |

---

?## ✅ **6. Outcome Simulation**

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



