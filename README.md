# 🌍 Global Export Resilience

![Power BI](https://img.shields.io/badge/PowerBI-DAX-yellow?logo=powerbi)
![Data Analytics](https://img.shields.io/badge/Data-Analytics-blue)
![Status](https://img.shields.io/badge/Project-Active-success)
![License](https://img.shields.io/badge/License-MIT-green)

# Enterprise-Power-BI-Finance-Analytics.
# Banking Crisis and Global Export Resilience
**Enterprise BI Project | World Bank Data Analysis**

**Power BI Dashboard Link**: https://app.fabric.microsoft.com/view?r=eyJrIjoiNmFjODFlOWItYmQyNS00MGFkLWFjMGEtZTQxMjZjZGEyODJkIiwidCI6IjE2ZDgzZWU2LTI1NGEtNDY5ZC1hNmNjLTU0ZTJjYTIzMTNlNyIsImMiOjh9


## 1. Group Members 
> * **Angela Irungu** : *Created repository, Project Proposal & Business Report*
> * **Martin Kyaremateng**: *Created 18 DAX measures*
> * **Queen Kibegi** : *Data Cleaning*
> * **Tumaini Micheni**: *Data Modeling*
> * **Leona Kamau**: *Dashboard Architecture & Design*
> * **Lesala Monaheng**

> [!Note]
> Each group member’s contributions are tracked in the repository’s commit history with respective deliverables in files in the documentation.

## **1. Project Overview**
> This project investigates the intersection of international trade and macro-financial stability. By leveraging global industry-level trade data and banking crisis indicators, the project visualizes how financial shocks disrupt trade flows. The primary objective is to build a complete end-to-end BI solution that identifies vulnerable sectors during financial crises and evaluates whether government policy responses effectively stabilize the situation.

> [!Important]
> The analysis covers the period from 1980 to 2010. Results may not fully reflect post‑2010 financial architectures or modern policy frameworks (e.g., Basel III, pandemic response mechanisms).

## **2. Problem Statement**
> Banking crises generate severe liquidity constraints that disrupt trade flows and weaken export performance. However, the impact of these shocks is uneven across industries, largely depending on their level of financial dependence (RZ) and asset tangibility (TANG). While governments implement recovery measures such as liquidity support (liqsup) and debt relief (debtrelief), there is limited empirical visibility into which sectors are most vulnerable and whether these interventions effectively mitigate the damage.

> [!Caution]  
> The dataset does not capture informal trade or shadow banking activity. Consequently, the measured export declines may underestimate the full economic disruption during crises.

## **3. Business Research Questions**
*The analysis is structured to answer the following five critical research questions from the project proposal:*
> * **Q1:** How much does export growth typically drop during a banking crisis compared to stable periods?
> * **Q2:** Are industries that rely heavily on external finance (RZ) more affected than those with tangible assets (TANG)?
> * **Q3:** Which measure (liquidity support, recapitalization, or debt relief) is associated with the quickest trade recovery?
> * **Q4:** Does the depth of a country's banking system (Private Credit/GDP) influence the severity of export declines?
> * **Q5:** How does a recession in major trading partners affect developed vs. developing countries?

> [!Tip]  
> Use the dashboard’s **What‑If Parameter** for credit market fluctuations to simulate answers to Q4 under different banking depth scenarios.

## **4. Data Dictionary**
```ruby
The dataset comprises 44 columns and over 39,500 observations sourced from the World Bank.
* Performance Metrics: `tradevalue` (Export Value), `expgrowth` (Growth Rate), `tradeshare` (Export Share), `GDPgr` (Domestic GDP Growth).
* Crisis Indicators: `BANK` (Banking Crisis), `TWIN` (Twin Crisis), `recession` (Domestic Recession), `Recession Abroad`.
* Industry Features: `RZ` (Financial Dependence), `TANG` (Asset Tangibility), `durables` (Durable Goods), `CCC` (Cash Conversion Cycle).
* Policy Variables: `liqsup` (Liquidity Support), `recaps` (Recapitalization), `debtrelief` (Debt Relief), `policytot` (Total Policy Index).
 ``` 
> [!Warning*] 
> Approximately 5% of financial ratio records (RZ, TANG) contain null values. These were imputed using industry‑level means. Users should exercise caution when aggregating at very granular sectoral levels (e.g., 4‑digit ISIC).

## **5. Technical Methodology**
> * **Data Transformation (ETL)**: Extensive cleaning was performed in Power Query, including null handling for RZ/TANG indices, data type standardization, and ISO-3 country code mapping.
> * **Data Modeling:** Implementation of a **Star Schema** to optimize performance, featuring a central `Fact_TradePerformance` table and dimensions for `Dim_Date`, `Dim_Country`, `Dim_CrisisStatus`, and `Dim_Sector`.
> * **Outlier Management:** Utilized the `expgrowthTRIM` variable to ensure average growth calculations were not skewed by extreme fluctuations (growth rates > 500%).

> [!Note]  
> The `expgrowthTRIM` variable is a pre‑processed version of export growth with the top and bottom 1% of observations removed. For all trend analyses, this trimmed measure is preferred over raw `expgrowth`.

## **6. DAX Calculations (Technical Documentation)**
The following measures provide the analytical logic for the dashboard and are documented within the repository:
> * **Total Trade Value:** `SUM(Fact_TradePerformance[tradevalue])`
> * **Average GDP Growth:** `AVERAGE(Fact_TradePerformance[GDPgr])`
> * **Average Private Credit:** `AVERAGE(Dim_Country[pcrdbofgdp])`
> * **YoY Export Growth:** `DIVIDE([Total Trade Value] - CALCULATE([Total Trade Value], SAMEPERIODLASTYEAR('Dim_Date'[Date])), CALCULATE([Total Trade Value], SAMEPERIODLASTYEAR('Dim_Date'[Date])))`
> * **Recovery Velocity:** `DIVIDE([YoY Export Growth], [Expected Growth])`
> * **Systematic Resilience:** `CALCULATE([YoY Export Growth], Dim_CrisisStatus[Recession Abroad] = 1)`
> * **Aggregated Risk Score:** `([Systemic Risk Index] * 0.6) + ([Specific Risk Index] * 0.4)`
> * **Risk Index:** `RANKX(ALL(Dim_Country), [Aggregated Risk Score], , DESC)`
> * **Expected Growth:** `3.41` (Static benchmark based on stable period growth averages).
> * **High RZ Exposure Flag:** `IF([Average RZ] > 0.5, 1, 0)`
> * **Total Countries in Recession:** `DISTINCTCOUNT(Fact_TradePerformance[exporter])` (Filtered where `recession = 1`).
> * **Sectoral Impact Growth:** `AVERAGE(Fact_TradePerformance[expgrowthTRIM])` (Grouped by RZ Category).

> [!Caution]
> The `SAMEPERIODLASTYEAR` function requires a continuous date column. If your `Dim_Date` table uses an integer year column, replace it with a proper date hierarchy to avoid unexpected blanks.

> [!Important]
> The static benchmark `Expected Growth = 3.41` is derived from non‑crisis periods in the dataset (1980‑2010). Do not use this value for comparisons with data outside the original time frame without recalibration.

## 7. Dashboard Architecture & Design
* The solution is designed as a professional-grade analytical suite with a focus on executive-level:*
```ruby
 (a) Page 1: Executive Summary (Global Crisis Tracker)
    Function: Geopolitical mapping of financial stability.
    Key Visuals: Interactive Map highlighting recession hotspots and KPI cards for Global Credit Depth.

 (b) Page 2: Analytical Deep Dive (Sectoral Impact)
    Function: Correlation between financial dependence (RZ) and export performance.
    Key Visuals: Scatter plot clusters analyzing high-RZ sectors against growth volatility.
 
 (c) Page 3: Risk & Resilience Analysis (Risk Forecast)
    Function: Identification of systemic vulnerabilities in trade commodities.
    Key Visuals: Sectoral Treemaps and custom Risk Index rankings.
    
 (d) Page 4: Analytics & Data Explorer (Country-Specific)
    Function: Granular historical auditing and raw data transparency.
    Key Visuals: Time-series charts comparing Private Credit vs. Growth trends and a detailed Data Table.
```
> [!Tip]  
> Use the **Drill‑through** functionality on the Data Explorer page to inspect individual exporter‑product pairs. This is particularly useful for validating outliers in the `expgrowth` variable.

## **8. Key Insights & Findings**
> * **Global Status:** 23 countries were identified in a recessionary state during the study period.
> * **Growth Disparity:** While the **Expected Growth** is **3.41**, the actual **Recovery Velocity** for crisis-impacted sectors is significantly lower at **0.27**.
> * **Credit Depth vs. Risk:** The **USA** maintains the highest private credit depth (**5.8K**), supporting high trade volumes but resulting in a total **Risk Index of 1.82**.
> * **Commodity Contribution:** Product codes **3843** ($5.43bn) and **3832** ($5.33bn) are the primary drivers of trade value.
> * **Vulnerability:** Scatter plot analysis confirms that high-RZ sectors (financially dependent) cluster in low-growth zones during financial disruptions.

> [!Warning] 
> The **Risk Index** is a relative rank within the dataset. A high index (e.g., 1.82 for the USA) does not indicate absolute risk but rather a comparative position. Always interpret alongside absolute measures like trade value or credit depth
> **Note**  
> The product codes (3843, 3832) correspond to ISIC Rev. 2 categories. For cross‑reference with modern classification systems (e.g., HS or NAICS), refer to the mapping table in the `/docs` folder.

## **9. Strategic Recommendations**
> 1.  **Targeted Policy:** Governments should prioritize **Liquidity Support (liqsup)** for high-RZ sectors, as they demonstrate the most immediate decline during credit crunches.
> 2.  **Credit Depth Buffers:** Developing nations should focus on increasing banking depth (**Private Credit/GDP**) to improve their **Systematic Resilience** score, which currently lags at **0.27**.
3.  **Export Diversification:**
> Nations relying on top commodities (e.g., 3843) should diversify their export portfolios to mitigate the "Aggregated Risk" of sector-specific shocks.
> [!Important]  
> Recommendations are derived from historical data (1980‑2010). Before implementation, validate them with contemporary datasets, as global supply chains and financial regulations have evolved significantly.
## **Data Source**
> Official Source: World Bank Data Catalog: Banking Crisis and Exports Dataset

> Dataset Content: This dataset contains 39,588 observations and 44 columns identifying global industry-level trade and systemic banking crisis indicators from 1980 to 2010.

> [!Caution]  
> The raw data file exceeds GitHub’s file size limit (100 MB). It is stored externally; instructions for downloading it are in the repository’s `README.md`. Do not commit the raw CSV to version control.
