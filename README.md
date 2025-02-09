# Data Transformation Project with Snowflake, dbt, and Airflow

This project demonstrates an end-to-end data pipeline that integrates **Snowflake**, **dbt**, and **Airflow** for data transformation and orchestration. The project follows **DRY (Don’t Repeat Yourself)** principles and adheres to best practices in **data modeling** (Star Schema) and software design.

---

## **Key Features**
- **Data Extraction**: Loaded data from a Snowflake sample dataset.
- **Transformations with dbt**:
  - Implemented modular, reusable SQL models.
  - Built **staging models** to clean and standardize raw data.
  - Designed **business-focused marts** (fact and dimension tables) based on Star Schema principles.
- **Orchestration with Airflow**:
  - Automated the pipeline to schedule, execute, and monitor transformations.
- **Storage**: Persisted the final transformed data back into Snowflake for reporting and analytics.

---

## **Project Structure**
```plaintext
.
├── models/
│   ├── marts/          # Business-ready models (fact and dimension tables)
│   │   ├── fact_sales.sql
│   │   ├── dim_customers.sql
│   │   └── dim_products.sql
│   ├── staging/        # Source-aligned staging models
│   │   ├── stg_orders.sql
│   │   ├── stg_customers.sql
│   │   └── stg_products.sql
│   └── macros/         # Reusable functions and utilities
│       └── clean_email.sql
└── airflow/
    └── dags/
