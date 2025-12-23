# gambill-project-retail-globalmart-databricks
This is my first databricks project shared by Gambill 



## Entity Relationship Diagram (ERD)

```mermaid
erDiagram
    DIM_CUSTOMER {
        bigint customer_key PK
        string customer_id
        string customer_name
        string customer_segment
        string country
        string city
        string state
        string postal_code
        string region
        string customer_hash
        timestamp effective_start_timestamp
        timestamp effective_end_timestamp
        boolean is_current_record
        timestamp load_timestamp
        string batch_id
    }

    DIM_PRODUCT {
        bigint product_key PK
        string product_id
        string product_name
        string category
        string sub_category
        string product_hash
        timestamp effective_start_timestamp
        timestamp effective_end_timestamp
        boolean is_current_record
    }

    FACT_SALES {
        bigint sales_key PK
        string order_id
        bigint customer_key FK
        bigint product_key FK
        date order_date
        date ship_date
        int quantity
        decimal sales
        decimal discount
        decimal profit
        timestamp load_timestamp
    }

    DIM_CUSTOMER ||--o{ FACT_SALES : "customer_key"
    DIM_PRODUCT ||--o{ FACT_SALES : "product_key"
