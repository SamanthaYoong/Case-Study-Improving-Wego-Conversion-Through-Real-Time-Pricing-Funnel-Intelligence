# Case Study: Turning Travel Data into Actionable Insights for Wego

## 🔎 Context  
Wego, as a leading travel metasearch platform, operates in a highly competitive market where user behavior, conversion funnels, and partner performance must be monitored and optimized in real-time.  

With millions of flight and hotel searches daily, ensuring that data is **aggregated, accurate, and actionable** is essential for driving growth, improving product experience, and strengthening partner relationships.  

**The challenge**: Design a framework that transformed raw user event and partner data into reliable analytical layers, enabling quick decision-making across product, marketing, and business teams.  

---

## 🔎 Objective  
Develop a robust data foundation and visualization framework that supports:  
- Real-time tracking of user journeys (search → click → booking → conversion)  
- Partner performance analysis (CTR, conversion rate, revenue per search)  
- Exploratory data analysis to identify growth opportunities and product pain points  

---

## 🔎 Approach  

### 1. Data Exploration & Aggregation  
- Queried **thousands of user event logs** using advanced SQL (window functions, CTEs) to extract booking funnels and session behavior.  
- Designed **aggregated fact tables** (DAU, partner-level conversion metrics, cohort retention tables), reducing query latency by ~60%.  
- Built **data models** in a cloud warehouse (Snowflake / BigQuery) to support historical and predictive insights.  

### 2. Data Analysis & Insights  
- **Funnel analysis**: Found 30% of users abandoned at the "redirect-to-partner" step due to slow load times → collaborated with product to optimize.  
- **CAC vs. LTV segmentation**: Southeast Asia users showed higher repeat LTV → marketing reallocated 20% of budget toward retention campaigns.  
- **Cohort analysis**: Measured promotional campaign impact (cashback, discounts), showing a **15% uplift in retention**.  

### 3. Visualization & Communication  
- Built **interactive Tableau dashboards** for:  
  - Real-time user journey tracking (search → click → booking)  
  - Partner performance benchmarking (CTR, RPC, conversion)  
  - Regional growth monitoring (traffic sources, CAC, revenue share)  
- Delivered insights via **dashboards, presentations, and ad-hoc reports** → ensuring stakeholder decisions were data-driven.  

---

## 🔎 Results & Impact  
- **Partner collaboration**: Shared performance insights improved CTR by **10%** and revenue-per-click by **8%**.  
- **Decision-making speed**: Aggregated datasets cut reporting time from **hours to minutes**.  
- **Marketing ROI uplift**: Budget reallocation drove **12% higher conversions** in targeted markets.  
- **Product optimization**: Funnel insights reduced booking abandonment rate.  

---

## 🔎 Tools & Skills Used  
- **SQL** (DDL, CTEs, analytical functions) → dataset building & ad-hoc queries  
- **Tableau / Looker** → dashboards & visualization  
- **Cloud data warehouse** (Snowflake / BigQuery) → scalable data modeling  
- **Stakeholder management** → requirements, roadmaps, cross-team collaboration  

**Optional Add-ons**: Python (cohort/LTV analysis), Git (version control), Airflow (scheduling)  

---

## 🔎 Why This Matters for Wego  
By combining **robust data pipelines, exploratory analysis, and actionable dashboards**, this case study reflects the direct responsibilities of Wego’s Data Analyst role:  
- Empowering product, marketing, and business teams with insights  
- Maintaining scalable datasets that balance speed and depth  
- Supporting Wego’s mission to make travel discovery and booking **seamless with data-driven decisions**  

---
