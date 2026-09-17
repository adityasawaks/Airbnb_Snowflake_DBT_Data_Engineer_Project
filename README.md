# 🏡 Airbnb Data Engineering Platform

> **End-to-end cloud data pipeline built with Snowflake, dbt, AWS, and Python.**

A production-style data engineering portfolio project that transforms Airbnb booking, host, and listing data into reliable, analytics-ready datasets. The project demonstrates modern warehouse architecture, incremental processing, historical tracking, reusable SQL, and data quality practices.

---

## ✨ What This Project Demonstrates

- Cloud-based data ingestion using **AWS S3**
- Data warehousing with **Snowflake**
- SQL transformation and orchestration using **dbt**
- Medallion architecture: **Bronze → Silver → Gold**
- Incremental models for efficient processing
- Slowly Changing Dimensions — **SCD Type 2**
- Reusable dbt macros and Jinja templates
- Data quality testing and lineage tracking
- Analytics-ready fact tables and One Big Table design

---

## 🧭 Architecture

```text
                 ┌────────────────────┐
                 │   Source CSV Files │
                 │ bookings / hosts / │
                 │      listings      │
                 └──────────┬─────────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │      AWS S3        │
                 │   Cloud Storage    │
                 └──────────┬─────────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │ Snowflake Staging  │
                 │   Raw Source Data  │
                 └──────────┬─────────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │  Bronze  │  │  Silver  │  │   Gold   │
        │ Raw Data │  │ Cleaned  │  │Analytics │
        └──────────┘  └──────────┘  └──────────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │ BI / Analytics /   │
                 │ Business Reporting │
                 └────────────────────┘
```

---

## 🛠️ Technology Stack

| Area | Technology |
|---|---|
| Cloud warehouse | Snowflake |
| Cloud storage | AWS S3 |
| Transformation | dbt Core |
| Programming | Python 3.12+ |
| SQL templating | Jinja |
| Version control | Git / GitHub |
| Testing | dbt tests |
| Documentation | dbt Docs |

### Core dbt capabilities

- Incremental materializations
- Snapshots for SCD Type 2
- Custom macros
- Jinja loops and templating
- Data tests and documentation
- Model dependency and lineage tracking

---

## 🗂️ Data Layers

### 🥉 Bronze — Raw Layer

Stores source data with minimal transformation.

| Model | Description |
|---|---|
| `bronze_bookings` | Raw booking transactions |
| `bronze_hosts` | Raw host information |
| `bronze_listings` | Raw property listings |

### 🥈 Silver — Transformation Layer

Cleans, validates, and standardizes source data.

| Model | Description |
|---|---|
| `silver_bookings` | Validated booking records |
| `silver_hosts` | Enhanced host profiles and quality metrics |
| `silver_listings` | Standardized listing data and price categories |

### 🥇 Gold — Analytics Layer

Provides business-ready datasets for reporting and analysis.

| Model | Description |
|---|---|
| `obt` | One Big Table joining bookings, listings, and hosts |
| `fact` | Fact table for dimensional analysis |
| Ephemeral models | Intermediate transformation logic |

### 🕒 Historical Snapshots — SCD Type 2

Snapshots preserve changes over time for point-in-time analysis.

- `dim_bookings`
- `dim_hosts`
- `dim_listings`

---

## 📁 Repository Structure

```text
AWS_DBT_Snowflake/
│
├── README.md
├── pyproject.toml
├── main.py
│
├── SourceData/
│   ├── bookings.csv
│   ├── hosts.csv
│   └── listings.csv
│
├── DDL/
│   ├── ddl.sql
│   └── resources.sql
│
└── aws_dbt_snowflake_project/
    ├── dbt_project.yml
    ├── ExampleProfiles.yml
    │
    ├── models/
    │   ├── sources/
    │   │   └── sources.yml
    │   ├── bronze/
    │   │   ├── bronze_bookings.sql
    │   │   ├── bronze_hosts.sql
    │   │   └── bronze_listings.sql
    │   ├── silver/
    │   │   ├── silver_bookings.sql
    │   │   ├── silver_hosts.sql
    │   │   └── silver_listings.sql
    │   └── gold/
    │       ├── fact.sql
    │       ├── obt.sql
    │       └── ephemeral/
    │           ├── bookings.sql
    │           ├── hosts.sql
    │           └── listings.sql
    │
    ├── macros/
    │   ├── generate_schema_name.sql
    │   ├── multiply.sql
    │   ├── tag.sql
    │   └── trimmer.sql
    │
    ├── analyses/
    │   ├── explore.sql
    │   ├── if_else.sql
    │   └── loop.sql
    │
    ├── snapshots/
    │   ├── dim_bookings.yml
    │   ├── dim_hosts.yml
    │   └── dim_listings.yml
    │
    ├── tests/
    │   └── source_tests.sql
    │
    └── seeds/
```

---

## 🚀 Getting Started

### 1. Prerequisites

Before starting, make sure you have:

- A Snowflake account
- An AWS account for S3 storage
- Python **3.12 or newer**
- `pip` or `uv`
- Git

### 2. Clone the repository

```bash
git clone <repository-url>
cd AWS_DBT_Snowflake
```

### 3. Create and activate a virtual environment

**Windows PowerShell**

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Linux / macOS**

```bash
python -m venv .venv
source .venv/bin/activate
```

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

Or install from `pyproject.toml`:

```bash
pip install -e .
```

### Main dependencies

```text
dbt-core>=1.11.2
dbt-snowflake>=1.11.0
sqlfmt>=0.0.3
```

---

## ❄️ Snowflake Configuration

Create the dbt profile file:

```text
~/.dbt/profiles.yml
```

Example configuration:

```yaml
aws_dbt_snowflake_project:
  target: dev

  outputs:
    dev:
      type: snowflake
      account: <your-account-identifier>
      user: <your-username>
      password: <your-password>
      role: ACCOUNTADMIN
      database: AIRBNB
      schema: dbt_schema
      warehouse: COMPUTE_WH
      threads: 4
```

> **Security note:** Never commit credentials to Git. Prefer environment variables or a secure secrets manager.

---

## 🧱 Initialize Snowflake

### Create staging tables

Run the SQL scripts from the `DDL/` directory in Snowflake:

```sql
-- Execute DDL/ddl.sql
```

### Load source files

| File | Target table |
|---|---|
| `bookings.csv` | `AIRBNB.STAGING.BOOKINGS` |
| `hosts.csv` | `AIRBNB.STAGING.HOSTS` |
| `listings.csv` | `AIRBNB.STAGING.LISTINGS` |

---

## ▶️ Running the Pipeline

Move into the dbt project:

```bash
cd aws_dbt_snowflake_project
```

### Validate the connection

```bash
dbt debug
```

### Install dbt packages

```bash
dbt deps
```

### Run every model

```bash
dbt run
```

### Run individual layers

```bash
dbt run --select bronze.*
dbt run --select silver.*
dbt run --select gold.*
```

### Run data quality tests

```bash
dbt test
```

### Execute snapshots

```bash
dbt snapshot
```

### Generate and serve documentation

```bash
dbt docs generate
dbt docs serve
```

### Build the complete project

```bash
dbt build
```

---

## ⚙️ Engineering Highlights

### 1. Incremental Processing

Incremental models process only newly arrived or changed records instead of rebuilding the entire table.

```sql
{{ config(materialized='incremental') }}

{% if is_incremental() %}
WHERE CREATED_AT > (
    SELECT COALESCE(MAX(CREATED_AT), '1900-01-01')
    FROM {{ this }}
)
{% endif %}
```

### 2. Reusable Macros

The `tag()` macro categorizes nightly prices into business-friendly groups.

```sql
{{ tag('CAST(PRICE_PER_NIGHT AS INT)') }}
    AS PRICE_PER_NIGHT_TAG
```

Example categories:

- `low`
- `medium`
- `high`

### 3. Dynamic SQL with Jinja

The OBT model uses Jinja loops to make joins and column selection easier to maintain.

```sql
{% set configs = [...] %}

SELECT
    {% for config in configs %}
        ...
    {% endfor %}
```

### 4. Slowly Changing Dimensions

Snapshot models preserve historical versions of records and maintain validity periods for point-in-time analysis.

### 5. Layer-Based Schemas

```text
AIRBNB.BRONZE.*
AIRBNB.SILVER.*
AIRBNB.GOLD.*
```

---

## ✅ Data Quality and Lineage

### Testing coverage

- Source data validation
- Unique key checks
- Not-null checks
- Referential integrity
- Custom business rules

### Lineage visibility

dbt documentation can show:

- Upstream dependencies
- Downstream impacts
- Model relationships
- Source-to-consumption flow

---

## 🔐 Security and Performance

### Security practices

- Keep `profiles.yml` outside the repository
- Use environment variables for secrets
- Apply Snowflake role-based access control
- Protect personally identifiable information
- Avoid exposing credentials in logs

### Performance practices

- Use incremental models for large datasets
- Use ephemeral models for intermediate logic
- Apply suitable clustering keys in Snowflake
- Keep SQL formatted and version-controlled
- Review model changes through pull requests

---

## 🐞 Troubleshooting

### Connection errors

Check:

- Snowflake account identifier
- Username and password
- Warehouse availability
- Network connectivity
- Role permissions

### Compilation errors

Try:

```bash
dbt debug
```

Then verify:

- Model references
- Jinja syntax
- File names
- Project configuration
- Dependencies

### Incremental load problems

Rebuild the affected model:

```bash
dbt run --full-refresh
```

Also verify source timestamps and incremental filters.

---

## 🔭 Future Improvements

- [ ] Build data quality dashboards
- [ ] Add CI/CD automation
- [ ] Introduce more advanced business metrics
- [ ] Connect Power BI or Tableau
- [ ] Add pipeline monitoring and alerting
- [ ] Implement PII masking
- [ ] Expand the automated testing suite
- [ ] Add orchestration with Airflow or another scheduler

---

## 📚 Useful Resources

- [dbt Documentation](https://docs.getdbt.com/)
- [Snowflake Documentation](https://docs.snowflake.com/)
- [dbt Best Practices](https://docs.getdbt.com/guides/best-practices)

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch

   ```bash
   git checkout -b feature/AmazingFeature
   ```

3. Commit your changes

   ```bash
   git commit -m "Add some AmazingFeature"
   ```

4. Push the branch

   ```bash
   git push origin feature/AmazingFeature
   ```

5. Open a pull request

---

## 👤 Project Information

**Project:** Airbnb Data Engineering Pipeline  
**Focus:** Cloud Data Engineering and Analytics  
**Technologies:** Snowflake · dbt · AWS · Python  
**License:** Portfolio demonstration
