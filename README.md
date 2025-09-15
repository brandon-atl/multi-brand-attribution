# Multi-Brand Attribution Analysis 

This repo deploys a small Snowflake model and seeds demo data:
- **% of customers who shopped Brand X first**
- Cross-brand **flows** (A → B) and **cannibalization**
- A Power BI 1-page dashboard demonstrating skillset

## What this repo does
- Runs Snowflake DDL/DML from GitHub Actions using **SnowSQL** in batch mode
- Generates demo data in one of two ways:
  - **Option A (default)**: Python runs in the GitHub runner → CSVs → `PUT` to Snowflake stage → `COPY INTO` tables
  - **Option B**: Python runs **inside Snowflake** (Snowpark stored procedure)
- Publishes a PBI template and DAX in text so changes are reviewable

## Prereqs
- Snowflake account (role with CREATE on target DB/SCHEMA)
- Warehouse (e.g., `COMPUTE_WH`)
- Power BI Desktop 
- GitHub 

## Secrets (GitHub → Settings → Secrets and variables → Actions)
- `SNOW_ACCOUNT`    (e.g., `yqc69876.us-east-1` — your account locator)
- `SNOW_USER`
- `SNOW_PASSWORD`
- `SNOW_ROLE`       (e.g., `SYSADMIN`)
- `SNOW_WAREHOUSE`  (e.g., `COMPUTE_WH`)
- `SNOW_DATABASE`   (e.g., `DEMO_DB`)
- (optional) `SNOW_SCHEMA` defaults to `ANALYTICS`
- `SEED_MODE`       (`LOCAL_CSV` or `SNOWPARK`; default `LOCAL_CSV`)

> GitHub Actions reads these secrets at runtime; never commit creds.  
> See GitHub docs on Actions secrets.  
> **Security tip:** Keep this repo **private** and rotate creds periodically.

## One-click deploy
Push to `main` and wait for GitHub Actions “Deploy to Snowflake”.
- It creates schema, stage, tables, views.
- Seeds data via **SEED_MODE**.
- You can manually re-run the workflow any time.

## Local manual run (optional)
1) Generate CSVs locally:
```bash
python ./python/generate_data.py
