# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a dbt dimensional modeling tutorial project using the AdventureWorks dataset. The project demonstrates how to build Kimball-style dimensional models with fact and dimension tables. It includes both Japanese and English documentation covering the complete tutorial from setup to consumption.

## Architecture

- **Data Warehouse**: DuckDB (primary), PostgreSQL (optional)
- **Modeling Framework**: dbt with dimensional modeling patterns
- **Schema Structure**: 
  - Seeds: Raw source data (person, production, sales, date)
  - Marts: Dimensional model output (dim_* tables, fct_sales, obt_sales)
- **Key Pattern**: Uses `dbt_utils.generate_surrogate_key()` for all dimension keys
- **Materialization**: All marts materialized as tables in `marts` schema

## Development Setup

```bash
# Environment setup
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
brew install duckdb

# Navigate to dbt project
cd adventureworks/
```

## Core dbt Commands

```bash
# Install dependencies
dbt deps

# Load seed data
dbt seed --target duckdb

# Build models
dbt run

# Run tests
dbt test

# Generate and serve documentation
dbt docs generate
dbt docs serve
```

## ER Diagram Generation

```bash
# Install dbterd and generate ER diagrams
pip install dbterd --upgrade
pip install dbt-artifacts-parser --upgrade
dbterd run

# Publish to dbdocs
npm install -g dbdocs
dbdocs login
dbdocs build "./target/output.dbml" --project "dbt-dimensional-modelling"
```

## Update Workflow

```bash
# After model changes
dbt docs generate
dbterd run
dbdocs build "./target/output.dbml" --project "dbt-dimensional-modelling"
```

## Model Structure

- **Fact Table**: `fct_sales` - Sales transactions with foreign keys to all dimensions
- **Dimensions**: `dim_customer`, `dim_product`, `dim_address`, `dim_credit_card`, `dim_date`, `dim_order_status`
- **OBT**: `obt_sales` - One Big Table for simplified analytics consumption
- **Seeds**: Source data files in CSV format under `seeds/` directory

## Key Files

- `adventureworks/dbt_project.yml`: Project configuration
- `adventureworks/profiles.yml`: Database connection profiles
- `adventureworks/models/marts/`: Dimensional model definitions
- `docs/`: Tutorial documentation in markdown format