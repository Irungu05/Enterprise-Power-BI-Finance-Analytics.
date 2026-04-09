# Global Export Resilience: Dashboard Architecture and Design 

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

1. **Geospatial Bubble Map**: Displays Private Credit by Country. Larger bubbles (e.g., USA at 5.8K and JPN at 3.2K) highlight nations with high credit depth, which acts as a buffer or a risk amplifier during crises.

2. **KPI Overview**: Displays the "Total Countries in Recession" (23) and "Average GDP Growth" (0.67) to show the immediate health of the global economy.

**User Interactivity**: A Development Status Slicer allows users to filter the entire map between Developed and Developing nations to compare resilience.

**Research Question Alignment**: Provides the data foundation for Q1 (Growth drops) and Q5 (Developed vs. Developing disparity).

## **Page 2: Sectoral Impact & Financial Dependence**
**Objective**: To investigate the "RZ Index" (External Financial Dependence) and its effect on industry growth.

**Core Visuals**:

1. **Scatter Plot (Financial Dependence vs. Growth)**: Plots industry sectors by their Average RZ.
   * It identifies "Vulnerability Clusters"—sectors that rely heavily on credit and thus suffer more during banking freezes.

2. **Recovery Velocity Gauge**: Shows a real-time recovery score of 0.27.
   * This visualizes the lag between actual performance and the stable-period expectation.

**User Interactivity**: A Trade Category Sidebar enables users to isolate specific product groups (e.g., Durable vs. Non-Durable) to see varying levels of credit sensitivity.

**Research Question Alignment**: Directly addresses Q2 (Financial dependence impact).
