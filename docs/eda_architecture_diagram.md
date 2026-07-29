# EDA Architecture Block Diagram

The diagram below shows how the EDA assets are organized on top of the Gold analytical layer.

```mermaid
flowchart TD
    A[Gold Layer Views\n- gold.dim_customers\n- gold.dim_products\n- gold.fact_sales] --> B[EDA Foundation\n01 Database Exploration\n02 Dimension Exploration\n03 Date Range Exploration]
    B --> C[Core Metric Analysis\n04 Measures Exploration\n05 Magnitude Analysis]
    C --> D[Advanced Analysis\n06 Ranking Analysis\n07 Change Over Time\n08 Cumulative Analysis\n09 Performance Analysis]
    D --> E[Segmentation & Contribution\n10 Data Segmentation\n11 Part-to-Whole Analysis]
    E --> F[Business Reporting Outputs\n12 gold.report_customers\n13 gold.report_products]
```

## Reading the Diagram

- **Gold Layer Views** are the analytical base for all EDA queries.
- **Foundation scripts** profile structure and temporal coverage.
- **Core and advanced analysis scripts** generate KPI, trend, and comparative insights.
- **Final reporting scripts** materialize reusable views for business consumption.
