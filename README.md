# ELT Data Pipeline with Snowflake, dbt, and Airflow

This project is a data pipeline that extracts sample data from Snowflake, loads it into the data warehouse, transforms it with dbt’s modular SQL models, and automates the process using Airflow.

## Project Screen Shot(s)

---

# Project Setup Instructions  

## 1️⃣ Prerequisites  
Make sure you have the following libraries installed in your virtual environment:  
- **[Astronomer](https://www.astronomer.io/)** (for Airflow orchestration)  
- **[dbt](https://www.getdbt.com/)** (for data transformations)  
- **[Snowflake Connector](https://docs.snowflake.com/en/user-guide/python-connector)** (for database connection)
  
## 2️⃣ Setup Snowflake Online
```sql
  -- create accounts
use role accountadmin;

create warehouse dbt_wh with warehouse_size='x-small';
create database if not exists dbt_db;
create role if not exists dbt_role;

show grants on warehouse dbt_wh;

grant role dbt_role to user <username> ;
grant usage on warehouse dbt_wh to role dbt_role;
grant all on database dbt_db to role dbt_role;

use role dbt_role;

create schema if not exists dbt_db.dbt_schema;

-- clean up
use role accountadmin;

drop warehouse if exists dbt_wh;
drop database if exists dbt_db;
drop role if exists dbt_role;
```

## 3️⃣ Clone the Repository  
```bash
git clone <repository-url>
cd <project-folder>
```
## 4️⃣ Configure Snowflake Connection
Since this project uses Snowflake:

Create a profiles.yml file (if not already present):
Location: ~/.dbt/profiles.yml
```bash
Add your Snowflake credentials:
snowflake_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: <your_account>
      user: <your_username>
      password: <your_password>
      role: <your_role>
      database: <your_database>
      warehouse: <your_warehouse>
      schema: <your_schema>
      threads: 10
      client_session_keep_alive: false
```

## 5️⃣ Set up Astronomer (Airflow) 
### Initialise your airflow project
```bash
astro dev init
```
### Start Airflow locally 
```bash
Astro dev start
```
- Default credentials (Username - admin, Password admin)
- In the airflow UI, find dbt_dag 
- Trigger the DAG manually 
- Monitor the task execution and check logs for debugging if needed

Your data pipeline is now set up, extracting data from snowflake, transforming it with dbt, and orchestrating workflows using airflow via Astronomer

---

## Project Structure
```plaintext
.
├── models/
│   ├── marts/          # Business-ready models (fact and dimension tables)
│   │   ├── fct_orders.sql
│   │   ├── int_order_items.sql
│   │   └── int_order_items_summary.sql
│   │   └── generic_tests.yml
│   ├── staging/        # Source-aligned staging models
│   │   ├── stg_tpch_line_items.sql
│   │   ├── stg_tpch_orders.sql
│   │   └── tpch_sources.yml
│   └── macros/         # Reusable functions and utilities
│       └── pricing.sql
```

## Reflection
This is a code-along tutorial that I recently completed, driven by my interest in working with new technologies and leveraging the massive computational power of cloud data warehouses, where data transformations are handled directly within the data warehouse. This project covers industry-relevant software engineering concepts such as modular, reusable SQL models and data modeling techniques like fact tables based on the star schema. Additionally, Airflow is used to visualize the DAG (Directed Acyclic Graph) workflow, clearly illustrating the sequence and dependencies of tasks to be executed.

I found that dbt (Data Build Tool) truly shines when working with structured data in modern data warehouses, offering simplicity, scalability, and maintainability. Here are some of the features in dbt that I found particularly useful:

Automatic Data Lineage and Documentation: dbt generates data lineage graphs and documentation, showing how datasets are connected.
Modular Approach: dbt breaks down transformations into small, reusable SQL models, promoting code reusability and maintainability.
Built-in Data Quality Testing: dbt supports data quality tests (e.g., uniqueness, non-null constraints, and referential integrity) through simple YAML configuration files.
Dbt built-in data quality testing (Uniqueness, not null, referential integrity) with simple YAML configs file
