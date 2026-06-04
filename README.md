# 🚗 Car Sales Data Warehouse & Business Intelligence

A complete end-to-end Data Warehousing and Business Intelligence project built as part of the **IT3021 - Data Warehousing and Business Intelligence** module at SLIIT.

This project covers the full BI pipeline — from raw OLTP data through ETL processing, data warehousing, OLAP cube development, and interactive Power BI dashboards.

---

## 📁 Project Structure


CarSales_DWBI/
├── CarSales_ETL/               # SSIS ETL project (Visual Studio)
│   ├── CarSales_Load_Staging.dtsx     # Extracts raw data into staging layer
│   ├── CarSales_Load_DW.dtsx          # Transforms and loads the data warehouse
│   ├── CarSales_Accumulating_Fact.dtsx # Updates accumulating fact columns
│   └── CarSales_Data_Profiling.dtsx   # Data profiling package
├── SSAS/                       # SQL Server Analysis Services cube project
│   └── CarSales_SSAS/          # Multidimensional SSAS cube
├── PowerBI/                    # Power BI report file
│   └── CarSales_BI_Report_v2.pbix
├── Book2.xlsx                  # Excel OLAP analysis workbook
└── README.md


---

## 🏗️ Solution Architecture


Source Layer (CarSales OLTP Database)
        ↓
  Staging Layer
        ↓
  ETL Layer — SSIS (Extract → Transform → Load)
        ↓
  Storage Layer — CarSales_DW (Star Schema)
        ↓
  OLAP Layer — SSAS Multidimensional Cube
        ↓
  Reporting Layer — Power BI Dashboards + Excel PivotTables


---

## 🗄️ Data Warehouse Design — Star Schema

**Fact Table:** `Fact_Sales`
- Stores measurable transaction data related to vehicle sales
- Measures: Base Price, Discount Rate, Discount Amount, Final Sale Price, Mileage, Vehicle Year

**Dimension Tables:**

| Table | Description |
|-------|-------------|
| `Dim_Customer` | Customer details — ID, name, gender, city, province, age group |
| `Dim_Vehicle` | Vehicle details — brand, model, engine capacity, fuel type, transmission, body type |
| `Dim_Branch` | Branch details — branch ID, name, city, province |
| `Dim_Salesperson` | Salesperson details for sales analysis |
| `Dim_Date` | Date attributes — full date, month, quarter, year for time-based analysis |

---

## ⚙️ ETL Pipeline (SSIS)

The ETL process is implemented using **SQL Server Integration Services (SSIS)** in Visual Studio with four packages:

### Package 1 — `CarSales_Load_Staging.dtsx`
Extracts raw data from the source OLTP database into the staging layer without transformations.

### Package 2 — `CarSales_Load_DW.dtsx`
Transforms and loads all dimension tables and the central fact table into the data warehouse using surrogate keys.

### Package 3 — `CarSales_Accumulating_Fact.dtsx`
Updates accumulating fact columns to track transaction processing times.

### Package 4 — `CarSales_Data_Profiling.dtsx`
Profiles the source data to understand quality and structure before loading.

---

## 🧊 SSAS Cube — `CarSalesCube`

Built on `CarSales_DW` as the data source. The cube links `Fact_Sales` to all dimension tables via surrogate keys.

**Dimensions implemented:**
- Dim Branch
- Dim Customer
- Dim Date
- Dim Salesperson
- Dim Vehicle

**OLAP Operations Demonstrated (via Excel PivotTables):**

| Operation | Description |
|-----------|-------------|
| **Roll-Up** | Aggregated sales data up to higher levels (e.g., month → year) |
| **Drill-Down** | Expanded from year level down to month-level detail |
| **Slice** | Filtered to a single dimension value |
| **Dice** | Applied multiple simultaneous dimension filters |
| **Pivot** | Rotated dimension axes for different analytical perspectives |
| **Time-Based Analysis** | Trend analysis across date hierarchy |

---

## 📈 Power BI Reports

Connected to `CarSales_DW` via SQL Server. Published to Power BI Service.

| Report | Type | Description |
|--------|------|-------------|
| Report 1 — Matrix | Matrix Visual | Sales breakdown across dimensions |
| Report 2 — Slicers | Interactive Dashboard | Cascading slicers for dynamic filtering |
| Report 3 — Drill-Down | Bar Chart | Hierarchical drill-down analysis |
| Report 4 — Drill-Through | Multi-page Report | Drill-through from summary to customer-level detail |
| KPI Dashboard | KPI Visuals | Key performance indicators for sales performance |

---

## 🛠️ Tech Stack

- **Database:** SQL Server
- **ETL:** SQL Server Integration Services (SSIS) via Visual Studio / SSDT
- **OLAP:** SQL Server Analysis Services (SSAS) — Multidimensional
- **Visualization:** Power BI Desktop & Service, Microsoft Excel (PivotTables)
- **Version Control:** Git / GitHub

---

## 🚀 Getting Started

### Prerequisites
- SQL Server (with SSAS component)
- Visual Studio with SSDT (SQL Server Data Tools)
- Power BI Desktop

### Setup Steps

1. **Restore the data warehouse** using SQL Server Management Studio (SSMS):
sql
RESTORE DATABASE CarSales_DW FROM DISK = 'CarSales_DW.bak'


2. **Configure SSIS connection strings** — update the connection managers in each `.dtsx` package to point to your local SQL Server instance

3. **Run SSIS packages in order:**

1. CarSales_Load_Staging.dtsx
2. CarSales_Load_DW.dtsx
3. CarSales_Accumulating_Fact.dtsx
4. CarSales_Data_Profiling.dtsx (optional)


4. **Deploy SSAS project** — open `CarSales_SSAS` in Visual Studio, right-click → Deploy, then Process Full

5. **Open Power BI file** — update the SQL Server connection string if needed and refresh data

> ⚠️ **Note:** Connection strings in SSIS packages are configured for a local SQL Server instance and will need to be updated to match your environment before running.

---

## 📋 Key Findings

- **Western Province leads in total sales** with LKR 169.35bn, consistently outperforming Central (LKR 85.44bn), Northern (LKR 84.29bn), and Southern (LKR 83.38bn) provinces across all drill levels
- **55-64 age group has the highest purchasing power** across all provinces compared to other age groups
- **All five branches perform comparably** in total sales, each generating approximately LKR 83–85bn, indicating a well-balanced branch network
- **Kandy Motors leads slightly** among branches in overall sales performance
- **Black and Silver colored vehicles generate the highest sales** compared to other vehicle colors
- **Sales in 2022 are significantly higher than 2023**, suggesting either a performance decline or that 2023 data covers only a partial year
- **Sales show a notable decline from mid-year onwards**, visible in the monthly trend line chart
- **Cash and Leasing are the dominant payment methods**, with Bank Loan being the least used
- **Total portfolio metrics:** LKR 422.45bn in Total Sales, 50,000 Total Transactions, LKR 8.45M Average Sale Price, and LKR 404.83bn in Net Sales (after discounts)
- **Salesperson performance is relatively balanced** across the team, with no single salesperson significantly outperforming others

---
## 👤 Author

**Wethmi Kanishka Wijethilaka**
SLIIT — BSc (Hons) in Information Technology, Data Science
Module: IT3021 Data Warehousing and Business Intelligence
