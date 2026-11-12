# Airbnb End-to-End Data Engineering Pipeline

<p align="center">
  <b>Snowflake | dbt | AWS S3 | SQL | Python</b>
</p>

## Project Overview

This project focuses on building an end-to-end data engineering pipeline using **Snowflake, dbt (Data Build Tool), AWS S3, and Python** to transform raw Airbnb datasets into structured, analytics-ready data.

The pipeline processes Airbnb listings, bookings, and host information through a Medallion Architecture (Bronze → Silver → Gold), incorporating incremental data loading, data transformations, historical tracking, and data quality validation.

The objective is to demonstrate practical data engineering concepts involved in building scalable and maintainable ELT pipelines on a modern cloud data platform.

## Architecture

The pipeline follows a layered data processing approach:

```text
         Airbnb Source Data (CSV)
                   |
                   v
              AWS S3
                   |
                   v
           Snowflake Staging
                   |
                   v
          +----------------+
          |  Bronze Layer  |
          |  Raw Data      |
          +----------------+
                   |
                   v
          +----------------+
          |  Silver Layer  |
          | Cleaned Data   |
          +----------------+
                   |
                   v
          +----------------+
          |   Gold Layer   |
          | Analytics Data|
          +----------------+
                   |
                   v
          Reporting & Analytics
```

**Technology Stack**

| Component | Technology |
|---|---|
| Cloud Storage | AWS S3 |
| Data Warehouse | Snowflake |
| Data Transformation | dbt |
| Programming | Python |
| Query Language | SQL |
| Templating | Jinja |
| Version Control | Git & GitHub |

## Data Engineering Workflow

### 1. Data Ingestion and Staging

- Airbnb datasets containing bookings, listings, and host information are used as source data.
- CSV files are stored in AWS S3.
- Data is loaded into Snowflake staging tables.
- Staging tables provide the initial layer for downstream transformations.

### 2. Bronze Layer – Raw Data

- Maintains raw ingested data with minimal transformations.
- Organizes source data into separate booking, listing, and host models.
- Provides a foundation for downstream processing.

### 3. Silver Layer – Data Transformation

- Performs data cleaning and standardization.
- Applies SQL-based transformations to improve data consistency.
- Handles business-level transformations and data preparation.
- Uses incremental processing where configured.

### 4. Gold Layer – Analytics Ready

- Combines transformed datasets for analytical consumption.
- Implements fact-oriented and denormalized data models.
- Creates an OBT (One Big Table) model for consolidated Airbnb analytics.
- Organizes final datasets for reporting and business insights.

## Key Data Engineering Concepts

**Incremental Data Loading**

Uses dbt incremental materialization to process new or changed records rather than rebuilding entire datasets on every execution.

**Slowly Changing Dimension (SCD Type 2)**

Uses dbt snapshots to maintain historical changes in bookings, hosts, and listings while preserving previous versions of records.

**Reusable dbt Macros**

Uses Jinja-based macros to centralize reusable SQL logic and reduce repetitive transformations.

**Data Quality and Testing**

Includes dbt testing capabilities for validating source data and transformed models.

**Data Modeling**

Organizes transformations into Bronze, Silver, and Gold layers to improve maintainability and downstream usability.

## Project Structure

```text
Airbnb-Snowflake-DBT-Data-Engineering/
│
├── aws_dbt_snowflake_project/
│   ├── analyses/
│   ├── macros/
│   ├── models/
│   │   ├── sources/
│   │   ├── bronze/
│   │   ├── silver/
│   │   └── gold/
│   ├── seeds/
│   ├── snapshots/
│   ├── tests/
│   └── dbt_project.yml
│
├── DDL/
│   ├── ddl.sql
│   └── resources.sql
│
├── SourceData/
│   ├── bookings.csv
│   ├── hosts.csv
│   └── listings.csv
│
├── Notes/
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

## Prerequisites

- Python 3.12+
- Snowflake account
- AWS account with S3 access
- dbt Core
- dbt Snowflake adapter
- Git

## Setup and Configuration

**1. Clone the repository**

```bash
git clone <your-repository-url>

cd Airbnb-Snowflake-DBT-Data-Engineering
```

**2. Install project dependencies**

```bash
pip install -e .
```

**3. Configure Snowflake**

Create a `profiles.yml` file inside your local `~/.dbt/` directory.

Example configuration:

```yaml
aws_dbt_snowflake_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: <your-account>
      user: <your-username>
      password: <your-password>
      role: <your-role>
      warehouse: COMPUTE_WH
      database: AIRBNB
      schema: DBT_SCHEMA
      threads: 4
```

Use your own Snowflake credentials. Never commit passwords or credentials to GitHub.

**4. Initialize the Snowflake environment**

Execute the SQL scripts available in the `DDL` directory to create the required database objects and staging tables.

**5. Load source datasets**

Load the Airbnb CSV datasets into the corresponding Snowflake staging tables.

## Running dbt Commands

Navigate to the dbt project directory:

```bash
cd aws_dbt_snowflake_project
```

Check Snowflake connectivity:

```bash
dbt debug
```

Install dbt dependencies:

```bash
dbt deps
```

Execute all models:

```bash
dbt run
```

Execute individual layers:

```bash
dbt run --select bronze.*
dbt run --select silver.*
dbt run --select gold.*
```

Execute data quality tests:

```bash
dbt test
```

Execute snapshots:

```bash
dbt snapshot
```

Generate dbt documentation:

```bash
dbt docs generate
dbt docs serve
```

Build models and execute associated tests:

```bash
dbt build
```

## Learning Outcomes

This project provides practical exposure to:

- Modern ELT pipeline architecture.
- Snowflake data warehousing.
- dbt model development and dependency management.
- Medallion Architecture.
- Incremental data processing.
- SCD Type 2 historical data management.
- Jinja templating and reusable SQL macros.
- Data quality testing and validation.
- Cloud-based data ingestion using AWS S3.

**Maintainer:** Preet Gugale

**Technologies:** Snowflake | dbt | AWS | Python | SQL
