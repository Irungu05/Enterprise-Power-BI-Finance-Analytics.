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
> - Recovery Velocity	Enables cross‑country comparisons of recovery speed, directly tied to policy evaluation.
> - YoY Export Growth (Error‑Handled)	Ensures dashboard stability and accuracy, especially in sparse datasets.
> - Crisis Impact	Provides a high‑level, actionable insight for executives and policymakers.
> - High RZ Flag	Makes industry‑level vulnerability analysis intuitive and dynamic.

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


### DAX Implementation Formula Screenshots

 `Avg Export Growth Normal` 
> <img width="447" height="118" alt="Screenshot 2026-04-01 003149" src="https://github.com/user-attachments/assets/405c68e2-f173-4d9b-b6e0-e0ed0f0c0b07" />

 `Avg Export Growth Crisis` 
> <img width="546" height="114" alt="Screenshot 2026-04-01 003123" src="https://github.com/user-attachments/assets/69a17bbe-2f63-4f1e-aadb-18c96229fa70" />

 `Crisis Impact`
>  <img width="654" height="43" alt="Screenshot 2026-04-01 003559" src="https://github.com/user-attachments/assets/3b5965b8-8e42-4a9b-9362-23816fe84f7c" />

 `Avg Loss High RZ` 
 > <img width="419" height="124" alt="Screenshot 2026-04-01 003454" src="https://github.com/user-attachments/assets/9267327f-3a91-43d3-96aa-c79ce1e3e116" />

 `Avg Loss Low RZ` 
 > <img width="425" height="121" alt="Screenshot 2026-04-01 003520" src="https://github.com/user-attachments/assets/7568c069-7589-412f-be48-20ebc6b5bce9" />

 `Recovery Velocity` 
 > <img width="783" height="363" alt="Screenshot 2026-04-01 003837" src="https://github.com/user-attachments/assets/f01f097e-65c7-46ae-a147-d1817b172a54" />

 `Avg Export Growth w Liquip` 
 > <img width="402" height="142" alt="Screenshot 2026-04-01 003327" src="https://github.com/user-attachments/assets/5013c3e8-5a2f-4215-8d1c-6865561d81b1" />

 `Avg Export Growth w DebtRelief`
  > <img width="419" height="141" alt="Screenshot 2026-04-01 003248" src="https://github.com/user-attachments/assets/e6b95fcb-9439-482a-a525-21ff4b5a1c0a" />

 `Avg Export Growth w Recaps`
 > <img width="386" height="144" alt="Screenshot 2026-04-01 003350" src="https://github.com/user-attachments/assets/dd11ec13-84c4-4043-bbe6-fcfd152a56bc" />

 `Export Decline High PcrDb` 
 > <img width="592" height="326" alt="Screenshot 2026-04-01 003706" src="https://github.com/user-attachments/assets/fab9daac-0239-4815-81d6-812bd5524420" />

 `Export Decline Low PcrDb` 
 > <img width="636" height="335" alt="Screenshot 2026-04-01 003739" src="https://github.com/user-attachments/assets/59ed5670-0404-44ef-b273-e045c3ce608d" />

 `TradeShare Change Dev Recession` 
 > <img width="473" height="251" alt="Screenshot 2026-04-01 004010" src="https://github.com/user-attachments/assets/8d858547-e3b4-461e-b74e-da985d06e6b6" />

 `TradeShare Change Devl Recession` 
 > <img width="492" height="260" alt="Screenshot 2026-04-01 004121" src="https://github.com/user-attachments/assets/08481a98-eaf3-4ea6-9e9d-aae9bf80236c" />

 `YoY Export Growth` 
 > <img width="556" height="209" alt="Screenshot 2026-04-01 004207" src="https://github.com/user-attachments/assets/438fd398-7926-44cd-a7b8-568a8d8f72d1" />

 `Avg Trimmed Export Growth`
 > <img width="547" height="43" alt="Screenshot 2026-04-01 003540" src="https://github.com/user-attachments/assets/6650c9be-844b-4c5a-a4b4-94b52ff4fd00" />

 `Total Trade Value` 
 > <img width="459" height="41" alt="Screenshot 2026-04-01 003934" src="https://github.com/user-attachments/assets/13fbed28-7cbc-4922-a4db-81b78131669f" />

 `Policy Intervention Count` 
 > <img width="424" height="213" alt="Screenshot 2026-04-01 003818" src="https://github.com/user-attachments/assets/237c4b98-df96-4fc9-a794-666c60a1d345" />

 `High RZ Flag`  
 > <img width="717" height="43" alt="Screenshot 2026-04-01 003800" src="https://github.com/user-attachments/assets/8b31acc0-422e-43ca-a937-74097200e291" />

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
