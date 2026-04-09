# Global Export Resilience: Dashboard Architecture and Design 

Power BI Dashboard Link: https://app.fabric.microsoft.com/view?r=eyJrIjoiNmFjODFlOWItYmQyNS00MGFkLWFjMGEtZTQxMjZjZGEyODJkIiwidCI6IjE2ZDgzZWU2LTI1NGEtNDY5ZC1hNmNjLTU0ZTJjYTIzMTNlNyIsImMiOjh9

## Introduction
* This section provides an analytical walkthrough of the four-page Power BI suite, detailing how each interface addresses the core research questions regarding banking crises and global export resilience.

## Objective
* To addresses the core research questions regarding banking crises and global export resilience.

## Key Sections:
* **Page 1: Global Crisis Tracker**
* **Page 2: Sectoral Impact & Financial Dependence**
* **Page 3: Risk Exposure & Resilience Forecast**
* **Page 4: Analytics & Data Explorer**

## **Page 1: Global Crisis Tracker**
**Objective**: To establish the global economic context and identify systemic credit risks.

**Core Visuals**:

1. **Geospatial Bubble Map**: Displays Private Credit by Country.
   * Larger bubbles (e.g., USA at 5.8K and JPN at 3.2K) highlight nations with high credit depth, which acts as a buffer or a risk amplifier during crises.

2. **KPI Overview**: Displays the "Total Countries in Recession" (23) and "Average GDP Growth" (0.67) to show the immediate health of the global economy.

**User Interactivity**: A Development Status Slicer allows users to filter the entire map between Developed and Developing nations to compare resilience.

<img width="880" height="496" alt="Page 1_ Global Crisis Tracker" src="https://github.com/user-attachments/assets/58c36345-4d11-4ae8-8a21-a7a9a0bd7056" />

## **Page 2: Sectoral Impact & Financial Dependence**
**Objective**: To investigate the "RZ Index" (External Financial Dependence) and its effect on industry growth.

**Core Visuals**:

1. **Scatter Plot (Financial Dependence vs. Growth)**: Plots industry sectors by their Average RZ.
   * It identifies "Vulnerability Clusters" sectors that rely heavily on credit and thus suffer more during banking freezes.

2. **Recovery Velocity Gauge**: Shows a real-time recovery score of 0.27.
   * This visualizes the lag between actual performance and the stable-period expectation.

**User Interactivity**: A Trade Category Sidebar enables users to isolate specific product groups (e.g., Durable vs. Non-Durable) to see varying levels of credit sensitivity.

<img width="702" height="373" alt="Page 2_Sectoral Impact" src="https://github.com/user-attachments/assets/edbdf37d-7b47-490f-91ba-4efd00933308" />

## **Page 3: Risk Exposure & Resilience Forecast**
**Objective**: To rank country-level risk and identify high-value commodities at stake.

**Core Visuals**:

1. **Risk by Country & Status (Ranked Bar Chart)**: Visualizes the Aggregated Risk Score across major economies.
   * JPN, USA, and SWE are ranked based on their credit-to-GDP ratios and exposure.

2. **Top Commodities by Trade Value (Funnel)**: Highlights product codes 3843 ($5.43bn) and 3832 ($5.33bn) as the largest drivers of trade value, indicating where a crisis would cause the most financial damage.

**User Interactivity**: Uses a Systematic Resilience metric (0.27) to provide a comparative benchmark against Expected Growth (3.41).

<img width="702" height="373" alt="Page 3_Risk Exposure" src="https://github.com/user-attachments/assets/1db9a9bf-a323-4203-86ef-79f0ff1e19c8" />

## **Page 4: Analytics & Data Explorer**
**Objective**: To provide a granular audit trail and "Stress-Testing" capabilities.

**Core Visuals**:

1. **Historical Trend Chart**: A line graph comparing Adjusted Growth to find historical correlations.

2. **Audit Table**: A row-by-row view of the 39,500+ observations, including crisis flags (BANK, TWIN) and policy responses (liqsup).

**User Interactivity**: This page hosts the What-if Parameter (Growth Stress Rate). Moving the slider updates the Stress-Adjusted Risk measure, allowing for real-time scenario modeling of economic shocks.

<img width="702" height="496" alt="Page 4_Analytics and Data Explorer" src="https://github.com/user-attachments/assets/bc3c67c1-57a1-4fbd-a046-040d4ebb21b6" />
