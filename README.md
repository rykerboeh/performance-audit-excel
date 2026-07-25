# Retail Operations and Inventory Health Audit

![Microsoft Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

An interactive, executive-ready retail analytics command center built in Microsoft Excel. This workbook ingests multi-year transaction logs, engineers dynamic inventory safety thresholds, corrects unweighted profit margin distortions, and isolates regional stockout exposure in real time.

---

## Business Case & Objectives
Retail operations managers frequently face visibility gaps where high-level revenue numbers mask underlying operational friction—such as unprofitable product lines, distorted profit margin metrics, and regional inventory stockouts.

This project bridges that gap by transforming raw store sales records (~10,000 transactions) into a modular, interactive operational audit. The objective is to provide executive leadership with a single-screen decision hub that tracks:

* Financial Performance: Trailing monthly sales, gross profit trajectories, and weighted product margins.

* Margin Hygiene: Eliminating mathematical skew caused by unweighted margin averages on discounted SKUs.

* Inventory Health: Identifying localized stockout risks and reorder priorities across geographic regions.

  

<img width="800" height="503" alt="ezgif com-optimize" src="https://github.com/user-attachments/assets/f0dec171-7205-4843-b879-f29700a49bd3" />

---

## Tech Stack & Tools
* Core Analytics Engine: Microsoft Excel

* Data Modeling & Calculation: Pivot Tables, Pivot Calculated Fields (= Profit / Sales), Structured Table Formulas ([@Sales] - [@Profit], IF(), RANDBETWEEN()), GETPIVOTDATA(), Defensive Error Handling (IFERROR())

* Interactive UI & Visualization: Connected Slicers, Custom PivotCharts (Line & Clustered Bar), Dynamic KPI Summary Cards

---

## Repository Structure
```text
├── data/
│   └── raw/
│       └── Sample - Superstore.csv         # Source Kaggle retail transaction dataset
├── excel/
│   └── performance_audit.xlsx      # Master interactive Excel workbook
└── README.md                              # Master project documentation
```
---

## Data Pipeline Architecture

### Phase 1: Ingestion & Feature Engineering (01_raw_data)
* Data Cleaning: Converted standard CSV records into an official Excel Table (SalesData), removing redundant non-analytical dimensions (Row ID, Country, Postal Code).

* Operational Helper Columns:

  * Total Cost = =[@Sales] - [@Profit]

  * Profit Margin % = =[@Profit] / [@Sales]

  * Current Stock = Generates mock operational inventory counts via =RANDBETWEEN(5, 100) (frozen as static values).

  * Reorder Threshold = Fixed baseline stock limit (20 units).

  * Reorder Flag = =IF([@[Current Stock]] <= [@[Reorder Threshold]], "REORDER", "OK")

### Phase 2: Modular Analytics Layer (02_ to 04_)
* Category Financials (02_category_financials): Audits revenue and profit across product hierarchies. Implements a custom Pivot Calculated Field (= Profit / Sales) to compute true weighted margins, fixing the unweighted average margin skew on discounted items (e.g., correcting Binders from an unweighted -20% mean to a true +15% weighted margin).

* Sales Trends (03_sales_trends): Maps monthly sales and profit trajectories grouped by Year and Month.

* Stockout Audit (04_regional_stockout): Filters active REORDER flags grouped by Region and State to highlight supply chain vulnerabilities.

### Phase 3: Logic Abstraction & Executive Dashboard (05_ & 06_)
* Reference Scratchpad (05_reference): Utilizes GETPIVOTDATA() wrapped inside IFERROR() to pull dynamic totals directly from the calculation layer, shielding the presentation layer from layout shifts or #REF! errors.

* Executive Dashboard (06_dashboard): A visual grid featuring 4 dynamic KPI cards, 3 styled PivotCharts, and 4 global Slicers (Year, Region, Segment, Category) cross-connected to filter the entire dashboard synchronously.

## Key Business Insights
* Margin Distortion Uncovered: Relying on simple row-level margin averages heavily misrepresents sub-category health. While unweighted calculations flagged Binders as heavily loss-making (-20%), true volume-weighted analysis revealed a healthy +15% profit margin contribution.

* Category Profit Loss: Furniture generated $742k in gross revenue but retained only a 2% profit margin, driven down by heavy losses in Tables (-9% margin) and Bookcases (-3% margin).

* Geographic Inventory Risk: Out of 9,994 operational lines, 1,634 items (~16%) are currently at or below safety stock limits, with high concentration risks identified in high-volume hubs like Texas and New York.
---

Developed by Ryker Boeh — Connect with me on https://www.linkedin.com/in/rboeh
