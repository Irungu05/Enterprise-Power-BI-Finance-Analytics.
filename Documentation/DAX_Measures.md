# 🌍 Global Export Resilience – DAX Measures Documentation

![Power BI](https://img.shields.io/badge/PowerBI-DAX-yellow?logo=powerbi)
![Data Analytics](https://img.shields.io/badge/Data-Analytics-blue)
![Status](https://img.shields.io/badge/Project-Active-success)
![License](https://img.shields.io/badge/License-MIT-green)

# DAX Development Documentation

## Overview

This document provides comprehensive documentation for all DAX measures developed in the **Global Export Resilience** project. Measures are designed to answer key business questions, support the executive dashboard, and ensure robust time intelligence. All measures are stored in a dedicated **Measures Table** and follow a consistent naming convention for clarity and maintainability.

---

## DAX Highlights

### “Recovery Velocity Index — measuring how fast trade bounces back”

**Purpose**  
Quantifies the speed of trade value recovery after a banking crisis, expressed as the percentage increase from the crisis trough to three years later.

**Implementation Rationale**  
> Standard recovery metrics often stop at “time to pre‑crisis level,” missing acceleration. This measure normalizes recovery speed, enabling direct comparison across countries and crisis episodes. It answers: *Do policies like liquidity support lead to faster rebounds?*

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
>The TroughYear definition above uses a simplified assumption ```(crisis start year + 1)```. If you're using in a production environment, please replace this with actual trough detection logic (e.g., identifying the year with minimum trade value within the crisis window) to ensure accurate recovery measurement.

### “YoY Export Growth — built to handle real‑world data quirks”

**Purpose**
Computes the annual percentage change in total trade value, with robust error handling to avoid misleading results when data is missing or previous‑year values are zero.

**Implementation Rationale**
> Raw growth formulas break when denominators are zero or blank. This version returns a blank instead of an error or infinity, and ensures the dashboard remains clean and reliable.

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
> This measure assumes ```Dim_Date[DateKey]``` is an integer column representing the year. If your date table uses a proper date column, please adapt the measure using ```SAMEPERIODLASTYEAR``` instead.

> [!Tip]
> Use this measure as a base for trend lines in reports. To avoid visual gaps, we recommend you wrap the result in ```COALESCE``` with a default value, but be cautious—blanks are often preferable for indicating missing data.

### “Crisis Impact on Export Growth — one number that tells the story”

**Purpose**
Captures the absolute drop in average export growth during systemic banking crises compared to normal periods.

**Implementation Rationale**
> This single measure answers the core business question: “How much does export growth typically drop during a banking crisis?” It serves as the foundation for deeper segmentation—by industry, country group, or policy intervention.

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
> For deeper analysis, please ensure you create versions of these measures that accept slicers for policy intervention types or country development status.

### “High Financial Dependence Flag — making vulnerability visible”

**Purpose**
A dynamic indicator that marks industries with financial dependence (RZ) above the global median, used in scatter plots and conditional formatting.

**Implementation Rationale**
> Without this flag, users would have to manually set thresholds. By embedding the median comparison directly into DAX, we enable instant visual identification of high‑risk sectors.

**DAX Formula**

```ruby
High RZ Flag = IF(SELECTEDVALUE(Fact_TradePerformance[RZ]) > MEDIAN(Fact_TradePerformance[RZ]), 1, 0)

```
> [!Warning]
> This measure uses ```SELECTEDVALUE```, which returns a value only if a single industry is selected. If multiple industries are selected, it will return blank. Use this measure in visuals where your industry granularity is at the appropriate level (e.g., scatter plots with one point per industry).

### 📊 KPIs
KPI	Definition	Formula Reference
- Export Growth (Normal):	Avg annual export growth during years with no banking crisis	```Avg Export Growth Normal```
- Export Growth (Crisis):	Avg annual export growth during years with a banking crisis	```Avg Export Growth Crisis```
- Crisis Impact:	Absolute difference in export growth between crisis and normal	```Crisis Impact```
- Recovery Velocity:	% increase in trade value from crisis trough to 3 years later	```Recovery Velocity```
- YoY Export Growth:	Year‑over‑year change in total trade value	```YoY Export Growth```
- Policy Effectiveness Score:	Avg export growth during crisis when a specific policy was active	```Avg Export Growth w [Policy]```

> [!Important]
> Policy effectiveness scores should be interpreted cautiously—correlation does not imply causation. It is appropriate to note that other confounding factors (e.g., crisis severity, global economic conditions) may influence results.

## Justification for Advanced Measures
### Measure	Justification
- Recovery Velocity	Enables cross‑country comparisons of recovery speed, directly tied to policy evaluation.
- YoY Export Growth (Error‑Handled)	Ensures dashboard stability and accuracy, especially in sparse datasets.
- Crisis Impact	Provides a high‑level, actionable insight for executives and policymakers.
- High RZ Flag	Makes industry‑level vulnerability analysis intuitive and dynamic.

### Complete List of Measures
Below is the full list of measures created for this project with screenshots of formulas.

#	Measure Name	Description
| Measure | Description | 
|---------|-------------|
| `Avg Export Growth Normal` | Average annual export growth during years with no banking crisis | 
| `Avg Export Growth Crisis` | Average annual export growth during years with a systemic banking crisis | 
| `Crisis Impact` | Absolute difference in export growth between crisis and normal periods | 
| `Avg Loss High RZ` | Average output loss for industries with financial dependence (RZ) above median | 
| `Avg Loss Low RZ` | Average output loss for industries with financial dependence (RZ) ≤ median | 
| `Recovery Velocity` | Percentage increase in trade value from crisis trough to three years later | 
| `Avg Export Growth w Liquip` | Average export growth during crises where liquidity support was provided | 
| `Avg Export Growth w DebtRelief` | Average export growth during crises where debt relief was implemented | 
| `Avg Export Growth w Recaps` | Average export growth during crises where bank recapitalization occurred | 
| `Export Decline High PcrDb` | Export growth drop during crises for countries with private credit/GDP above median |
| `Export Decline Low PcrDb` | Export growth drop during crises for countries with private credit/GDP ≤ median | 
| `TradeShare Change Dev Recession` | Change in export share for developed countries when major trading partners are in recession | 
| `TradeShare Change Devl Recession` | Change in export share for developing countries when major trading partners are in recession | 
| `YoY Export Growth` | Year‑over‑year change in total trade value (with error handling) | 
| `Avg Trimmed Export Growth` | Average of the outlier‑adjusted export growth variable | 
| `Total Trade Value` | Sum of trade value (base measure) | 
| `Policy Intervention Count` | Count of years where any policy response (liquidity support, debt relief, or recapitalization) was active |
| `High RZ Flag` | Dynamic flag: 1 if industry’s financial dependence (RZ) is above median, else 0 |

> [!Note]
> Measures 4–5 (Avg Loss High/Low RZ) rely on the loss column, which may contain estimated values. For more sophisticated analysis, you can consider using loss2 as an alternative where available.

> [!Warning]
When considering measures using MEDIAN (e.g., Avg Loss High RZ, Export Decline High PcrDb) We recommend you compute the median across all rows in the current filter context. If you want a fixed global median, consider storing the median value in a separate table or using CALCULATE with ALL to ignore filters.

> [!Note]
> Tables: Fact_TradePerformance (fact), Dim_Date, Dim_Country, Dim_CrisisStatus (dimensions).
>
> Relationships: Active relationships exist between fact and dimension tables as per the star schema.
>
> Measures Table: All measures are stored in a dedicated table named Measures to keep the model clean.

> [!Important]
> Always verify that relationships are correctly configured before using these measures. A broken relationship can cause measures to return unexpected results (e.g., blanks or incorrect aggregations).

> [!Caution]
> When adding new columns to the fact table (e.g., additional policy flags), ensure they are properly integrated into existing measures or create new measures as needed. Avoid breaking changes that affect already published dashboards.
