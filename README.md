# Enterprise-Power-BI-Finance-Analytics.
# Banking Crisis and Global Export Resilience
**Enterprise BI Project | World Bank Data Analysis**

## 1. Group Members 
* **Angela Irungu** 
* **Martin Kyaremateng**
* **Queen Kibegi** 
* **Tumaini Muriithi** 
* **Leona Kamau**
* **Lesala Monaheng**
---

## 2. Project Overview
This project investigates the impact of systemic banking and currency crises on global export performance. By analyzing industry-level trade data, we identify which sectors are most vulnerable based on their financial structures and evaluate the effectiveness of government policy interventions.

## 3. Problem Statement
Financial crises restrict credit, but the impact is not uniform across all industries. This project analyzes how **External Financial Dependence (RZ)** and **Asset Tangibility (TANG)** determine an industry's resilience or failure during a banking crisis.

## 4. Business Questions
* **Crisis Impact:** What is the average drop in export growth (`expgrowth`) during a systemic banking crisis?
* **Structural Vulnerability:** Do high-RZ industries suffer more significant losses than high-TANG industries?
* **Policy Efficacy:** Which intervention (Liquidity Support vs. Recapitalization) correlates with faster trade recovery?
* **Systemic Interaction:** How does a country's private credit levels (`pcrdbofgdp`) influence export declines?
* **Global Contagion:** How do recessions in major partner countries (`RecessionAbroad`) impact local trade shares?

## 5. Data Source
* **Official Source:** [World Bank Data Catalog: Banking Crisis and Exports](https://datacatalog.worldbank.org/search/dataset/0041188/banking-crisis-and-exports-dataset)
* **Dataset Size:** 8,000+ observations covering global trade and crisis indicators.

## 6. Technical Architecture
* **ETL:** Data cleaning, normalization, and ISO standardization using Power Query.
* **Modeling:** Star Schema optimization (1 Fact Table, 5 Dimension Tables).
* **DAX:** Implementation of Time Intelligence and Crisis Window metrics.
* **Visualization:** Power BI Interactive Dashboards.

## 7. The Data Model (Star Schema)
The project utilizes a **Star Schema** to ensure analytical precision and avoid Many-to-Many relationship conflicts:
* **Fact_TradePerformance:** Centralized trade metrics and growth rates.
* **Dim_Geography:** Country-specific economic indicators and development status.
* **Dim_Industry:** Sector-specific financial ratios (RZ, TANG, HERF).
* **Dim_CrisisPolicy:** Yearly crisis markers and government response data.
* **Dim_ForeignConditions:** External global economic factors and partner recessions.
* **Dim_CrisisWindow:** Specialized country-year crisis event tracking using a composite key.
