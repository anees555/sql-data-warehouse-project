# SQL Data Warehouse Project

This project implements a modern SQL Server data warehouse using a **Medallion architecture** (**Bronze → Silver → Gold**).  
It includes end-to-end ETL logic, data cleaning and standardization, dimensional modeling, and quality checks for analytics-ready reporting.

## Project Goals

- Consolidate CRM and ERP source data into a centralized warehouse.
- Apply structured transformations to improve data quality and consistency.
- Deliver business-ready dimensional and fact models for reporting and analysis.
- Validate data integrity at Silver and Gold layers through SQL-based quality checks.

## Architecture Overview

### Bronze Layer (Raw Ingestion)
- Stores raw data from source CSV files with minimal transformation.
- Uses `BULK INSERT` in a stored procedure for ingestion.
- Designed for traceability and reproducible reloads.

### Silver Layer (Cleaned & Standardized)
- Applies cleansing, normalization, and transformation logic.
- Handles nulls, invalid values, duplicate handling, and field standardization.
- Adds warehouse metadata columns such as `dwh_create_date`.

### Gold Layer (Business Model)
- Exposes analytics-ready star schema objects using SQL views:
  - `gold.dim_customers`
  - `gold.dim_products`
  - `gold.fact_sales`
- Optimized for BI/reporting consumption.

## Data Pipeline Workflow

1. **Initialize database and schemas**
   - Run `scripts/init_database.sql`
2. **Create Bronze objects**
   - Run `scripts/bronze/ddl_bronze.sql`
3. **Load Bronze data**
   - Run `scripts/bronze/load_data_bronze.sql` (`EXEC bronze.load_bronze;`)
4. **Create Silver objects**
   - Run `scripts/silver/ddl_silver.sql`
5. **Load Silver data**
   - Run `scripts/silver/load_data_silver_sql` (`EXEC silver.load_silver;`)
6. **Create Gold views**
   - Run `scripts/gold/ddl_gold.sql`
7. **Run quality checks**
   - `test/quality_checks_silver.sql`
   - `test/quality_checks_gold.sql`

## Tools and Techniques

- **Database**: Microsoft SQL Server
- **Language**: T-SQL
- **Modeling approach**: Star schema (dimensions + fact)
- **Data architecture**: Medallion (Bronze/Silver/Gold)
- **Ingestion**: `BULK INSERT` from CSV
- **Transformation patterns**:
  - Standardization with `CASE`, `TRIM`, `UPPER`, `REPLACE`
  - Deduplication with `ROW_NUMBER()`
  - Null/invalid handling with `ISNULL`, `NULLIF`, conditional logic
  - Temporal handling with `LEAD()` for end-date derivation
- **Quality validation**: SQL assertion-style checks for uniqueness, referential integrity, and data consistency

## Repository Structure

```text
sql-data-warehouse-project/
├── datasets/                    # Source data files (placeholders in repo)
├── docs/
│   ├── data_catalog.md          # Gold-layer data dictionary
│   ├── data_model.png
│   ├── data_flow_diagram1.drawio.png
│   └── data_warehouse_architecture.drawio (3).png
├── scripts/
│   ├── init_database.sql        # Creates database and Bronze/Silver/Gold schemas
│   ├── bronze/
│   │   ├── ddl_bronze.sql       # Bronze table definitions
│   │   └── load_data_bronze.sql # Bronze load stored procedure
│   ├── silver/
│   │   ├── ddl_silver.sql       # Silver table definitions
│   │   └── load_data_silver_sql # Silver load stored procedure
│   └── gold/
│       └── ddl_gold.sql         # Gold analytical views
├── test/
│   ├── quality_checks_silver.sql
│   └── quality_checks_gold.sql
└── README.md
```

## Data Quality and Validation

The project includes SQL-based validation scripts to ensure:

- No null/duplicate keys in core entities.
- Standardized categorical values.
- Valid date ranges and logical date ordering.
- Correct sales calculations (`sales = quantity × price`).
- Fact-to-dimension relationship integrity in Gold layer.

## Usage Notes

- The initialization script drops and recreates the `DataWarehouse` database; use caution in non-development environments.
- Update file paths in `BULK INSERT` statements to match your local environment before execution.
- Execute scripts in the workflow order shown above for a successful full pipeline run.

## Documentation

- See `/docs/data_catalog.md` for detailed Gold-layer column definitions.
