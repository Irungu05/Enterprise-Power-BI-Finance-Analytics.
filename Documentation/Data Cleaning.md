**DATA CLEANING**

**Data Cleaning and Transformation**

The dataset underwent a series of preprocessing steps in Power Query to ensure data quality, consistency, and suitability for business intelligence analysis. The following steps were performed.



<img width="1586" height="855" alt="Screenshot 2026-03-10 232234" src="https://github.com/user-attachments/assets/fb428c17-7c9c-41c2-8a34-d99f5d815dda" />


**Step 1: Data Type Correction**

  
Data types were standardized to ensure consistency across numerical and categorical variables.

Columns were converted to appropriate data types to ensure correct calculations and analysis.

Examples:

year → Whole Number
exporter → Text
economic indicators such as tradevalue, expgrowth, and GDPcap → Decimal Number.

Correct data types prevent calculation errors and improve performance during data modeling and DAX computations.

**Before Cleaning**

<img width="975" height="601" alt="image" src="https://github.com/user-attachments/assets/c6db5f2a-587f-4c87-a334-561bb1ca0763" />

**After Cleaning**

<img width="825" height="532" alt="image" src="https://github.com/user-attachments/assets/279d59b5-9399-47b8-900d-2f639114f4fd" />


**Step 2: Handling Missing Values**

Several economic indicators contained null values, which represent missing or unavailable data.
Instead of replacing them with zeros, null values were retained because replacing them could misrepresent actual economic performance. In financial and macroeconomic datasets, null values often indicate that data was not reported rather than zero activity.

<img width="631" height="936" alt="image" src="https://github.com/user-attachments/assets/e0464581-6e06-4131-a93e-115b7d5aa7f6" />

**Step 3: Duplicate Removal**

Duplicate values were removed when creating dimension tables such as Dim_Country and Dim_CrisisStatus to ensure that each dimension contains unique records.
This step is critical in implementing a Star Schema, where dimension tables must have unique keys.

<img width="975" height="652" alt="image" src="https://github.com/user-attachments/assets/8818940c-5eff-4cce-80d1-c187b8cf1fec" />

**Step 4: Merge Queries**

Merge operations were performed to integrate dimension tables with the fact table and generate foreign keys for relational modeling

<img width="975" height="884" alt="image" src="https://github.com/user-attachments/assets/2cb6c746-1885-40fe-831f-1da713a531a6" />

<img width="975" height="887" alt="image" src="https://github.com/user-attachments/assets/b2c77f79-11ac-4bfd-aaf6-54e237ccc596" />

**Step 5: Append Queries**

An append operation was demonstrated by combining selected queries vertically. This transformation illustrates the ability to stack datasets with similar structures and satisfies the Power Query transformation requirements specified in the project instructions.

<img width="975" height="439" alt="image" src="https://github.com/user-attachments/assets/d21613bf-8f04-44e0-b293-5d75e80d1809" />


**Step 5: Conditional Column Creation**

Conditional logic was applied to derive categorical indicators from numerical variables. For example, recession indicators were converted into descriptive categories to improve interpretability in dashboards and reports.

<img width="975" height="425" alt="image" src="https://github.com/user-attachments/assets/4d99a724-d012-4436-9975-5c22b59366c8" />

<img width="202" height="450" alt="image" src="https://github.com/user-attachments/assets/54fec8bf-0e1a-4f4f-9ffa-ad5538e52a70" />


**Step 6: Custom Column Creation**


<img width="975" height="618" alt="image" src="https://github.com/user-attachments/assets/1754d862-e460-4c74-8a71-ad20d4979801" />

<img width="231" height="619" alt="image" src="https://github.com/user-attachments/assets/c88bd299-9692-48d6-a132-c555efb0b5f1" />


**Step 7: Creation of Dimension Tables**

Separate dimension tables such as Dim_Country and Dim_CrisisStatus were created using query referencing. This supports normalization and improves model performance.

<img width="220" height="224" alt="image" src="https://github.com/user-attachments/assets/bb2854f2-8c42-42f4-b9e0-fa26f629bdd5" />

**Step 8: Applied Steps**

<img width="484" height="797" alt="image" src="https://github.com/user-attachments/assets/f12c74c4-2d2b-457b-9a00-f79865e6462e" />




