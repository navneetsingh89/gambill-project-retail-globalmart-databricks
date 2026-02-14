# GlobalMart Retail Analytics on Databricks

End-to-end retail analytics pipeline built on Databricks using a Medallion Architecture (`Bronze -> Silver -> Gold`) and consumed in Power BI.

## Project Overview

This project processes GlobalMart retail data into analytics-ready models for reporting and business insights:

- Bronze: raw ingestion and landing
- Silver: cleaned and conformed dimensional model
- Gold: business views for KPI and dashboard use cases
- Power BI: semantic/reporting layer connected to Gold outputs

## Repository Structure

```text
Bronze/
  bronze_ingestion.ipynb

Silver/
  dim_customer.ipynb
  dim_product.ipynb
  dim_region.ipynb
  dim_date.ipynb
  fact_sales.ipynb

Gold/
  1_sales_base.ipynb
  2_vw_profitability.ipynb
  3_vw_logistics_efficiency.ipynb
  4_vw_customer_value.ipynb
  5_vw_sales_base_masked.ipynb
  old_vw_script.ipynb

PowerBIReports/
  Gimball Retail GlobalMart Project.pbix
  databricks-Serverless Starter Warehouse.pbids
```

## Data Flow

1. `Bronze/bronze_ingestion.ipynb`
   Loads raw source data to Bronze tables with ingestion metadata.
2. Silver dimension notebooks
   Builds `dim_customer`, `dim_product`, `dim_region`, and `dim_date`.
3. `Silver/fact_sales.ipynb`
   Creates the central fact table and links to conformed dimensions.
4. Gold notebooks
   Creates analytics views for profitability, logistics, and customer value.
5. Power BI
   Connects to Gold outputs for dashboarding and stakeholder reporting.

## Suggested Execution Order

Run notebooks in this order to avoid dependency issues:

1. `Bronze/bronze_ingestion.ipynb`
2. `Silver/dim_customer.ipynb`
3. `Silver/dim_product.ipynb`
4. `Silver/dim_region.ipynb`
5. `Silver/dim_date.ipynb`
6. `Silver/fact_sales.ipynb`
7. `Gold/1_sales_base.ipynb`
8. `Gold/2_vw_profitability.ipynb`
9. `Gold/3_vw_logistics_efficiency.ipynb`
10. `Gold/4_vw_customer_value.ipynb`
11. `Gold/5_vw_sales_base_masked.ipynb`

## Tech Stack

- Databricks Notebooks
- Apache Spark / PySpark
- Delta Lake (typical for Databricks Medallion projects)
- Power BI

## Entity Relationship Diagram (Silver Layer)

```mermaid
erDiagram
    FACT_SALES {
        BIGINT fact_sales_key PK
        BIGINT customer_key FK
        BIGINT product_key FK
        BIGINT region_key FK
        INT order_date_key FK
        INT ship_date_key FK
        STRING order_id
        STRING ship_mode
        INT order_quantity
        DECIMAL sales_amount
        DECIMAL discount_amount
        DECIMAL profit_amount
        BOOLEAN is_return
        INT abs_order_quantity
        DECIMAL abs_sales_amount
        TIMESTAMP ingestion_ts
        STRING source_file_path
        TIMESTAMP load_timestamp
    }

    DIM_CUSTOMER {
        BIGINT customer_key PK
        STRING customer_id
        STRING customer_name
        STRING customer_segment
        STRING country
        STRING city
        STRING state
        STRING postal_code
        STRING region
        STRING customer_hash
        TIMESTAMP effective_start_timestamp
        TIMESTAMP effective_end_timestamp
        BOOLEAN is_current_record
        TIMESTAMP load_timestamp
        STRING batch_id
    }

    DIM_PRODUCT {
        BIGINT product_key PK
        STRING product_id
        STRING product_name
        STRING category
        STRING sub_category
        STRING product_hash
        TIMESTAMP effective_start_timestamp
        TIMESTAMP effective_end_timestamp
        BOOLEAN is_current_record
        TIMESTAMP load_timestamp
        STRING batch_id
    }

    DIM_REGION {
        BIGINT region_key PK
        STRING country
        STRING state
        STRING city
        STRING postal_code
        STRING region_name
        TIMESTAMP load_timestamp
        STRING batch_id
    }

    DIM_DATE {
        INT date_key PK
        DATE date
        INT day
        STRING day_name
        INT day_of_week
        INT day_of_year
        INT week_of_year
        STRING year_week
        INT month
        STRING month_name
        STRING year_month
        INT quarter
        STRING quarter_name
        INT year
        BOOLEAN is_weekend
        BOOLEAN is_weekday
        BOOLEAN is_month_start
        BOOLEAN is_month_end
        BOOLEAN is_quarter_start
        BOOLEAN is_quarter_end
        BOOLEAN is_year_start
        BOOLEAN is_year_end
        TIMESTAMP load_timestamp
    }

    DIM_CUSTOMER ||--o{ FACT_SALES : customer_key
    DIM_PRODUCT  ||--o{ FACT_SALES : product_key
    DIM_REGION   ||--o{ FACT_SALES : region_key
    DIM_DATE     ||--o{ FACT_SALES : order_date_key
    DIM_DATE     ||--o{ FACT_SALES : ship_date_key
```

## How to Use This Repository

- Import notebooks into your Databricks workspace.
- Configure storage paths, catalog/schema names, and secrets as needed.
- Execute notebooks in the suggested order.
- Validate Gold layer outputs.
- Open `PowerBIReports/Gimball Retail GlobalMart Project.pbix` and point it to your Databricks warehouse.

## Notes

- `Gold/old_vw_script.ipynb` appears to be legacy and may not be required in the main run.
- Update this README with environment-specific details (catalog, schema, cluster/warehouse config) for production use.
