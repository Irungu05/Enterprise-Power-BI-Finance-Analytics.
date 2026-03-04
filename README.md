# Enterprise-Power-BI-Finance-Analytics.
# Banking Crisis and Global Export Resilience
**Enterprise BI Project | World Bank Data Analysis**

## 1. Group Members 
* **Angela Irungu** 
* **Martin Kyaremateng**
* **Queen Kibegi** 
* **Tumaini Micheni** 
* **Leona Kamau**
* **Lesala Monaheng**


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

The project utilizes a Star Schema to ensure analytical precision, improve DAX performance, and avoid Many-to-Many relationship conflicts.

**Fact_TradePerformance:** Centralized trade performance metrics and macroeconomic indicators at the exporter–year level of granularity.

**Dim_Date:** Time dimension containing calendar attributes used to support trend analysis and time intelligence calculations.

**Dim_Country:** Country-specific economic indicators and development classification used for segmentation and cross-country comparison.

**Dim_CrisisStatus:** Crisis classification dimension capturing systemic banking crises, twin crises, recession indicators, and foreign recession conditions for structured disruption analysis.

The model follows strict Star Schema principles with one central fact table connected to three dimension tables through one-to-many, single-direction relationships.

