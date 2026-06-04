# 🚗 Car Sales Data Warehouse & Business Intelligence

A complete end-to-end Data Warehousing and Business Intelligence project built as part of the **IT3021 - Data Warehousing and Business Intelligence** module at SLIIT.

This project covers the full BI pipeline — from raw OLTP data through ETL processing, data warehousing, OLAP cube development, and interactive Power BI dashboards.

---

## 📁 Project Structure

```
CarSales_DWBI/
├── CarSales_ETL/
│   ├── CarSales_Load_Staging.dtsx
│   ├── CarSales_Load_DW.dtsx
│   ├── CarSales_Accumulating_Fact.dtsx
│   └── CarSales_Data_Profiling.dtsx
├── SSAS/
│   └── CarSales_SSAS/
├── PowerBI/
│   └── CarSales_BI_Report_v2.pbix
├── Book2.xlsx
└── README.md
```

---

## 🏗️ Solution Architecture

```
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
```

---

## 🗄️ Data Warehouse Design — Star Schema

**Fact Table:** `Fact_Sales`
- Measures: Base Price, Discount Rate, Discount Amount, Final Sale Price, Mileage, Vehicle Year

**Dimension Tables:**

| Table | Description |
|-------|-------------|
| `Dim_Customer` | Customer ID, name, gender, city, province, age group |
| `Dim_Vehicle` | Brand, model, engine capacity, fuel type, transmission, body type |
| `Dim_Branch` | Branch ID, name, city, province |
| `Dim_Salesperson` | Salesperson details for sales analysis |
| `Dim_Date` | Full date, month, quarter, year for time-based analysis |

---

## ⚙️ ETL Pipeline (SSIS)

| Package | Description |
|---------|-------------|
| `CarSales_Load_Staging.dtsx` | Extracts raw data from source into staging layer |
| `CarSales_Load_DW.dtsx` | Transforms and loads dimension and fact tables |
| `CarSales_Accumulating_Fact.dtsx` | Updates accumulating fact columns |
| `CarSales_Data_Profiling.dtsx` | Profiles source data quality |

---

## 🧊 SSAS Cube

Built on `CarSales_DW`. Includes two hierarchies:

- **Date Hierarchy:** Year → Quarter → Month → Day
- **Branch Hierarchy:** Province → City → Branch Name

**OLAP Operations demonstrated via Excel PivotTables:**

| Operation | Description |
|-----------|-------------|
| Roll-Up | Aggregated sales by province |
| Drill-Down | Province → City → Branch Name |
| Slice | Filtered to a single province (Western) |
| Dice | Multiple filters: Province + Age Group |
| Pivot | Sales by Age Group and Branch Name |
| Time-Based | Yearly sales comparison (2022 vs 2023) |

---

## 📈 Power BI Reports

| Report | Description |
|--------|-------------|
| Report 1 — Matrix | Sales by Province, Branch, and Customer Age Group |
| Report 2 — Slicers | Interactive dashboard with Year, Quarter, Province, Payment Method slicers |
| Report 3 — Drill-Down | Hierarchical drill-down by Year → Quarter → Month and Province → Branch |
| Report 4 — Drill-Through | Branch-level detail view triggered from any province data point |
| KPI Dashboard | Total Sales, Transactions, Avg Sale Price, Net Sales with monthly trends |

**DAX Measures:**
```
Total Sales        = SUM(Fact_Sales[Final_Sale_Price_LKR])
Total Transactions = COUNTROWS(Fact_Sales)
Avg Sale Price     = AVERAGE(Fact_Sales[Final_Sale_Price_LKR])
Net Sales          = SUM(Fact_Sales[Final_Sale_Price_LKR]) - SUM(Fact_Sales[Discount_Amount_LKR])
Total Discount     = SUM(Fact_Sales[Discount_Amount_LKR])
```

---

## 📋 Key Findings

- **Western Province leads** with LKR 169.35bn in total sales, consistently outperforming Central (85.44bn), Northern (84.29bn), and Southern (83.38bn)
- **55–64 age group has the highest purchasing power** across all provinces
- **All five branches perform comparably**, each generating approximately LKR 83–85bn
- **Kandy Motors leads slightly** among branches in overall sales performance
- **Black and Silver vehicles generate the highest sales** compared to other colors
- **Sales in 2022 are significantly higher than 2023**, suggesting either a decline or partial-year data
- **Sales decline notably from mid-year onwards**, visible in the monthly trend line chart
- **Cash and Leasing are the dominant payment methods**, with Bank Loan being least used
- **Overall KPIs:** LKR 422.45bn Total Sales · 50,000 Transactions · LKR 8.45M Avg Sale Price · LKR 404.83bn Net Sales

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| SQL Server | Data Warehouse storage |
| SSIS (Visual Studio) | ETL pipeline |
| SSAS | Multidimensional OLAP cube |
| Power BI Desktop & Service | Interactive dashboards |
| Microsoft Excel | PivotTable OLAP analysis |

---

## 🚀 Getting Started

### Prerequisites
- SQL Server with SSAS component
- Visual Studio with SSDT
- Power BI Desktop

### Setup Steps

1. Restore the data warehouse:
```sql
RESTORE DATABASE CarSales_DW FROM DISK = 'CarSales_DW.bak'
```

2. Update SSIS connection strings to your local SQL Server instance

3. Run SSIS packages in order:
```
1. CarSales_Load_Staging.dtsx
2. CarSales_Load_DW.dtsx
3. CarSales_Accumulating_Fact.dtsx
4. CarSales_Data_Profiling.dtsx  (optional)
```

4. Deploy SSAS project — right-click `CarSales_SSAS` → Deploy → Process Full

5. Open Power BI file and update the SQL Server connection string if needed

> ⚠️ **Note:** Connection strings are configured for a local SQL Server instance and will need updating for your environment.

---

## 👤 Author

**Wethmi Kanishka Wijethilaka**
SLIIT — BSc (Hons) in Information Technology, Data Science
Module: IT3021 Data Warehousing and Business Intelligence
