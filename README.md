# gambill-project-retail-globalmart-databricks
This is my first databricks project shared by Gambill 



## Entity Relationship Diagram (ERD) of Silver Layer

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
