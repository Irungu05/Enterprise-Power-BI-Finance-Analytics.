# 🌍 Global Export Resilience – DAX Measures Documentation

![Power BI](https://img.shields.io/badge/PowerBI-DAX-yellow?logo=powerbi)
![Data Analytics](https://img.shields.io/badge/Data-Analytics-blue)
![Status](https://img.shields.io/badge/Project-Active-success)
![License](https://img.shields.io/badge/License-MIT-green)

# DAX Development Documentation

## Overview

This document provides comprehensive documentation for all DAX measures developed in the **Global Export Resilience** project. Measures are designed to answer key business questions, support the executive dashboard, and ensure robust time intelligence. All measures are stored in a dedicated **Measures Table** and follow a consistent naming convention for clarity and maintainability.

---

## 📌 DAX Highlights

### “Recovery Velocity Index — measuring how fast trade bounces back”

**Purpose**  
Quantifies the speed of trade value recovery after a banking crisis, expressed as the percentage increase from the crisis trough to three years later.

**Implementation Rationale**  
Standard recovery metrics often stop at “time to pre‑crisis level,” missing acceleration. This measure normalizes recovery speed, enabling direct comparison across countries and crisis episodes. It answers: *Do policies like liquidity support lead to faster rebounds?*

**DAX Formula**
```ruby
Recovery Velocity = 
VAR CrisisStartYear = CALCULATE(MIN(Dim_Date[Year]), Dim_CrisisStatus[BANK] = 1)
VAR TroughYear = CrisisStartYear + 1   -- placeholder; replace with actual trough detection
VAR RecoveryYear = TroughYear + 3
VAR TradeAtTrough = 
    CALCULATE(
        SUM(Fact_TradePerformance[tradevalue]),
        Dim_Date[Year] = TroughYear
    )
VAR TradeAtRecovery = 
    CALCULATE(
        SUM(Fact_TradePerformance[tradevalue]),
        Dim_Date[Year] = RecoveryYear
    )
RETURN
    DIVIDE(TradeAtRecovery - TradeAtTrough, TradeAtTrough, 0)
```
> [!Caution]
>The TroughYear definition above uses a simplified assumption ```(crisis start year + 1)```. If you're using in a production environment, replace this with actual trough detection logic (e.g., identifying the year with minimum trade value within the crisis window) to ensure accurate recovery measurement.

### “YoY Export Growth — built to handle real‑world data quirks”

**Purpose**
Computes the annual percentage change in total trade value, with robust error handling to avoid misleading results when data is missing or previous‑year values are zero.

**Implementation Rationale**
Raw growth formulas break when denominators are zero or blank. This version returns a blank instead of an error or infinity, and ensures the dashboard remains clean and reliable.

**DAX Formula**

```ruby
YoY Export Growth = 
VAR CurrentYear = MAX(Dim_Date[DateKey])
VAR CurrentYearValue = 
    CALCULATE(
        SUM(Fact_TradePerformance[tradevalue]),
        Dim_Date[DateKey] = CurrentYear
    )
VAR PrevYearValue = 
    CALCULATE(
        SUM(Fact_TradePerformance[tradevalue]),
        Dim_Date[DateKey] = CurrentYear - 1
    )
VAR Result = 
    IF(
        OR(ISBLANK(PrevYearValue), PrevYearValue = 0),
        BLANK(),
        DIVIDE(CurrentYearValue - PrevYearValue, PrevYearValue, 0)
    )
RETURN Result
```
> [!Note]
> This measure assumes ```Dim_Date[DateKey]``` is an integer column representing the year. If your date table uses a proper date column, adapt the measure using ```SAMEPERIODLASTYEAR``` instead.

> [!Tip]
> Use this measure as a base for trend lines in reports. To avoid visual gaps, you can wrap the result in ```COALESCE``` with a default value, but be cautious—blanks are often preferable for indicating missing data.

### “Crisis Impact on Export Growth — one number that tells the story”

**Purpose**
Captures the absolute drop in average export growth during systemic banking crises compared to normal periods.

**Implementation Rationale**
This single measure answers the core business question: “How much does export growth typically drop during a banking crisis?” It serves as the foundation for deeper segmentation—by industry, country group, or policy intervention.

**DAX Formula**

```ruby
Avg Export Growth Normal = 
CALCULATE(
    AVERAGE(Fact_TradePerformance[expgrowth]),
    Dim_CrisisStatus[BANK] = 0
)

Avg Export Growth Crisis = 
CALCULATE(
    AVERAGE(Fact_TradePerformance[expgrowth]),
    Dim_CrisisStatus[BANK] = 1
)
```
```ruby 
Crisis Impact = [Avg Export Growth Crisis] - [Avg Export Growth Normal]
```
> [!Important]
> The Crisis Impact measure is subtraction measure that ensure that ```Avg Export Growth Normal``` and ```Avg Export Growth Crisis``` are computed on the same filtered context (e.g., same set of countries, industries). Otherwise, the difference may be misleading.

> [!Tip]
> For deeper analysis, create versions of these measures that accept slicers for policy intervention types or country development status.

### “High Financial Dependence Flag — making vulnerability visible”

**Purpose**
A dynamic indicator that marks industries with financial dependence (RZ) above the global median, used in scatter plots and conditional formatting.

**Implementation Rationale**
Without this flag, users would have to manually set thresholds. By embedding the median comparison directly into DAX, we enable instant visual identification of high‑risk sectors.

**DAX Formula**

```ruby
High RZ Flag = IF(SELECTEDVALUE(Fact_TradePerformance[RZ]) > MEDIAN(Fact_TradePerformance[RZ]), 1, 0)

```
> [!Warning]
> This measure uses SELECTEDVALUE, which returns a value only if a single industry is selected. If multiple industries are selected, it will return blank. Use this measure in visuals where the industry granularity is at the appropriate level (e.g., scatter plots with one point per industry).

📁 Measures Documentation in Repository
All DAX measures are stored in the project repository under /DAX_Measures/ with a markdown file containing:

Measure name and description

Full DAX formula

Dependencies (tables/columns used)

Business question addressed

This ensures transparency, version control, and easy handover.

Note
The repository also includes a Power BI template (.pbit) that pre‑populates the measures table, so you can start building visuals immediately.

📊 KPI Definitions Explained
KPI	Definition	Formula Reference
Export Growth (Normal)	Avg annual export growth during years with no banking crisis	Avg Export Growth Normal
Export Growth (Crisis)	Avg annual export growth during years with a banking crisis	Avg Export Growth Crisis
Crisis Impact	Absolute difference in export growth between crisis and normal	Crisis Impact
Recovery Velocity	% increase in trade value from crisis trough to 3 years later	Recovery Velocity
YoY Export Growth	Year‑over‑year change in total trade value	YoY Export Growth
Policy Effectiveness Score	Avg export growth during crisis when a specific policy was active	Avg Export Growth w [Policy]
Important
Policy effectiveness scores should be interpreted cautiously—correlation does not imply causation. Other confounding factors (e.g., crisis severity, global economic conditions) may influence results.

🧠 Justification for Advanced Measures
Measure	Justification
Recovery Velocity	Enables cross‑country comparisons of recovery speed, directly tied to policy evaluation.
YoY Export Growth (Error‑Handled)	Ensures dashboard stability and accuracy, especially in sparse datasets.
Crisis Impact	Provides a high‑level, actionable insight for executives and policymakers.
High RZ Flag	Makes industry‑level vulnerability analysis intuitive and dynamic.
📋 Complete List of Measures
Below is the full list of measures created for this project. Detailed formulas are maintained in the repository.

#	Measure Name	Description
1	Avg Export Growth Normal	Average export growth in non‑crisis years
2	Avg Export Growth Crisis	Average export growth in crisis years
3	Crisis Impact	Difference between crisis and normal growth
4	Avg Loss High RZ	Average output loss for industries with RZ > median
5	Avg Loss Low RZ	Average output loss for industries with RZ ≤ median
6	Recovery Velocity	Trade value recovery speed after crisis
7	Avg Export Growth w Liquip	Export growth during crises with liquidity support
8	Avg Export Growth w DebtRelief	Export growth during crises with debt relief
9	Avg Export Growth w Recaps	Export growth during crises with recapitalization
10	Export Decline High PcrDb	Export growth drop in high‑banking‑depth countries
11	Export Decline Low PcrDb	Export growth drop in low‑banking‑depth countries
12	TradeShare Change Dev Recession	Trade share change for developed countries during foreign recession
13	TradeShare Change Devl Recession	Trade share change for developing countries during foreign recession
14	YoY Export Growth	Year‑over‑year change in total trade value
15	Avg Trimmed Export Growth	Average of outlier‑adjusted export growth
16	Total Trade Value	Sum of trade value
17	Policy Intervention Count	Count of years with any active policy response
18	High RZ Flag	Indicator for industries with high financial dependence

> [!Note]
> Measures 4–5 (Avg Loss High/Low RZ) rely on the loss column, which may contain estimated values. For robustness, consider using loss2 as an alternative where available.

> [!Warning]
Measures using MEDIAN (e.g., Avg Loss High RZ, Export Decline High PcrDb) compute the median across all rows in the current filter context. If you want a fixed global median, consider storing the median value in a separate table or using CALCULATE with ALL to ignore filters.

> [!Notes]
>Tables: Fact_TradePerformance (fact), Dim_Date, Dim_Country, Dim_CrisisStatus (dimensions).
>
> Relationships: Active relationships exist between fact and dimension tables as per the star schema.
>
> Measures Table: All measures are stored in a dedicated table named Measures to keep the model clean.

> [!Important]
> Always verify that relationships are correctly configured before using these measures. A broken relationship can cause measures to return unexpected results (e.g., blanks or incorrect aggregations).

> [!Caution]
> When adding new columns to the fact table (e.g., additional policy flags), ensure they are properly integrated into existing measures or create new measures as needed. Avoid breaking changes that affect already published dashboards.
