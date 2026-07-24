# Retail Operations and Inventory Health Audit

![POSTGRESQL](https://img.shields.io/badge/PostgreSQL-4169E1.svg?style=for-the-badge&logo=PostgreSQL&logoColor=white)
![Microsoft Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

An end-to-end relational data pipeline that ingests raw supplier audit data, maps point-in-time compliance windows, and quantifies organizational financial exposure to underperforming or un-audited suppliers.

---

## Business Case & Objectives
This business case features an organization that is shifting to sustainable procurement, but is struggling to isolate where vendor compliance failures affect operations. Fragmented data masks when an order is placed with an underperforming supplier, leaving the organization exposed to financial and operational risks and penalties.

This project unifies the relevant data, crafting informed metrics, uncovering actionable insights into the organization's potential liabilities, and exploring how ESG compliance correlates with downstream company logistics like shipping delays, increased organizational carbon footprint, and freight cost penalties.

<img width="800" height="503" alt="ezgif com-optimize" src="https://github.com/user-attachments/assets/f0dec171-7205-4843-b879-f29700a49bd3" />

---

## Tech Stack & Tools
* Database Engine & Core Language: SQL (PostgreSQL)
* Advanced Relational Modeling: Window Functions (LEAD(), ROW_NUMBER()), Conditional Aggregation (CASE WHEN), Left Joins, Defensive Math Architecture (NULLIF())

---

## Repository Structure
```text
├── data/
│   ├── raw/                        
│       └── products.csv            # Source Kaggle product data
│       └── purchase_orders.csv     # Source Kaggle purchase order data
│       └── suppliers.csv           # Source Kaggle supplier data
│       └── sustainability_audits   # Synthetic data for audit touchpoint
├── SQL/                            # Data modeling & analytics
│   └── 01_data_creation_queries.sql            # Defines table schemas for raw data import
│   └── 02_baseline_compliance_queries          # Basic supplier metrics and current compliance standing
│   └── 03_financial_liability_queries.sql      # Explores concentration points of organizational financial liability
│   └── 04_freight_impact_queries.sql           # Explore freight impact of poor performing suppliers
└── README.md                       # Master project documentation
```
---

## Data Pipeline Architecture

### Phase 1: Database Creation & Baseline Compliance (01_ & 02_)

* Schema Enforcement (01_data_creation_queries.sql): Builds optimized tables for suppliers, products, purchase_orders, and sustainability_audits using strict primary and foreign key constraints.

* Descriptive Baselines (02_baseline_compliance_queries.sql): Tracks geographic vendor densities, contract lengths, and identifies current failing suppliers.

* Trajectory Tracking: Implements dual windowed CTEs (FIRST_VALUE / LAST_VALUE equivalents with ROW_NUMBER()) to classify metrics like waste diversion, water intensity, and labor safety as Improving, Worsening, or Stagnant.

### Phase 2: Financial Liability & Concentration Modeling (03_)

* Temporal Effective-Dating: Utilizes the LEAD() window function to construct rolling audit validity windows (status_start_date to status_end_date). This  pairs each historical purchase order with the exact compliance status of the vendor at the exact time that the transaction occurred.

* Risk Concentration Layers: Aggregates total spend proportions across distinct dimensions (product categories, country of origin, and specific vendors) to identify what percentage of organizational capital is actively bound to high-risk suppliers.

### Phase 3: Operational Freight & Vulnerability Audits (04_)

* Reliability Benchmarking: Correlates audit ratings with shipping fulfillment speeds (delivery_date - promised_delivery_date) to see if failing suppliers hold higher supply chain delay risks.

* Carbon Footprint Analysis: Uses conditional aggregations (COUNT(CASE WHEN...) * 100.0 / NULLIF(...)) to evaluate whether failing vendors rely disproportionately on high-emissions transport (Air) vs. lower-emissions alternatives (Rail/Sea).

* Financial Penalty Tracking: Measures average freight and duty costs incurred across compliance tiers to uncover hidden operational overhead.

* System Leak Isolation: Employs relational Left Anti-Joins (WHERE right_table.id IS NULL) to map active purchase volume flowing to legacy, un-audited suppliers.

## How to Reproduce & Run Locally

### 1. Clone the Repository
```bash
git clone https://github.com/rykerboeh/supply-chain-esg-audit.git
cd supply-chain-esg-audit
```

### 2. Initialize the Relational Schema
* Execute sql/01_data_creation_queries.sql in your PostgreSQL instance to establish core table definitions.

* Import your local operational CSV files into the corresponding database tables.

### 3. Run the Analytics Pipeline
* Run files 02_ through 04_ sequentially to explore baseline behaviors, extract point-in-time risk concentrations, and isolate logistical efficiency gaps.

---

Developed by Ryker Boeh — Connect with me on https://www.linkedin.com/in/rboeh
