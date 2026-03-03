# Enterprise-Power-BI-Finance-Analytics.
# Project: Global Export Resilience
**Enterprise BI Implementation: Financial Crisis and Policy Impact Analysis**

## 1. Project Overview
This project analyzes the relationship between international trade and macro-financial stability. It utilizes global industry-level trade data and banking crisis indicators to visualize how financial shocks affect sectors based on credit reliance and asset structures.

## 2. Problem Statement
Banking crises disrupt trade flows through liquidity constraints. Impact varies by **Financial Dependence (RZ)** and **Asset Tangibility (TANG)**. This project evaluates the efficacy of government interventions (liquidity support, debt relief) in stabilizing trade performance.

## 3. Business Questions
* **Crisis Impact:** Quantify the drop in `expgrowth` during banking crises.
* **Sectoral Vulnerability:** Compare losses between high `RZ` and high `TANG` industries.
* **Policy Effectiveness:** Identify which intervention (`liqsup`, `recaps`, `debtrelief`) yields the fastest recovery.
* **Systemic Interaction:** Assess how `pcrdbofgdp` influences export declines during recessions.
* **Global Sensitivity:** Compare `tradeshare` impact of foreign recessions on developed vs. developing nations.

## 4. Technical Architecture
* **ETL:** 10+ Power Query transformations (normalization, null handling, ISO standardization).
* **Modeling:** Star Schema optimization for DAX performance.
* **DAX:** 15+ measures (Time Intelligence, Recovery Velocity Indexes).
* **Visuals:** Decomposition Trees, Map Plots, and What-If parameters.

## 5. Data Model (Star Schema)

* **Fact Table:** `Fact_TradePerformance` (tradevalue, expgrowth, loss)
* **Dim_Geography:** exporter, developed, developing
* **Dim_Industry:** product, durables, RZ, TANG, herf
* **Dim_CrisisPolicy:** year, BANK, TWIN, recession, liqsup, recaps



## 7. Project Team
Angela Irungu
Martin kyaremateng
Queen Kibegi
Tumaini Muriithi
Leona Kamau 
Lesala Monaheng 
