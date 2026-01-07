<h1 align="center">ETL Documentation</h1>

This document describes the ETL process of the **100cep Gateway** project, structured in the **Bronze, Silver and Gold** layered architecture on Databricks Unity Catalog, using the public Brazilian e-commerce dataset from Olist and a complementary chargebacks dataset.

***

## Environment Overview

The pipeline follows the *medallion* architecture pattern, with data organized in progressively refined layers (**Bronze → Silver → Gold**) to improve quality, governance and analytical usability.
Data management, permissions and lineage are handled via Unity Catalog, using the `catalog.schema.table` convention and separating schemas by layer (`staging`, `bronze`, `silver` and `gold`).

### Catalog and Schemas

- Catalog: `100cep_gateway`.
- Schemas:
    - `staging`: initial reception of files and volumes.
    - `bronze`: raw tables in Delta format.
    - `silver`: cleaned, standardized and integrated data.
    - `gold`: analytical model (fact and dimensions) ready for consumption.

***

## Environment Preparation: [01_preparation](.databricks/pipeline/notebooks/01_preparation.ipynb)

The preparation script creates and/or recreates the catalog and schemas, ensuring a clean environment for pipeline execution.
Before creating them, the process drops (when existing) the catalog and its schemas, avoiding leftovers from previous runs that could impact data consistency.

**Main Responsibilities**

- Create the `100cep_gateway` catalog.
- Create the `staging`, `bronze`, `silver` and `gold` schemas.
- Ensure that all subsequent objects (volumes, tables, views) are created in this catalog.

***

## Data Ingestion and Staging: [02_download](.databricks/pipeline/notebooks/02_download.ipynb)

In this step, the physical ingestion of data files into the Databricks environment is carried out, using Unity Catalog volumes for centralized storage.

### Volume and Datasets

- Creation of a volume called `imdb` in the `staging` schema to store the project's data files.
- Installation and use of the `kagglehub` library for direct download of datasets hosted on Kaggle.


### Data Sources

- Main dataset: **Brazilian E‑Commerce Public Dataset by Olist** (orders, payments, products, customers, sellers, deliveries and reviews).
- Complementary dataset: `datasets/ai_dataset/chargebacks_dataset.csv`, simulating chargebacks associated with e-commerce transactions.


### Staging Flow

- Download of the Olist dataset via `kagglehub`.
- Copy of the downloaded CSV files to the `imdb` volume in the `staging` schema, ensuring governance and availability of the data for the next layers.

***

## Bronze Layer — Structured Raw Data: [03_bronze](.databricks/pipeline/notebooks/03_bronze.ipynb)

The **Bronze** layer performs the initial ingestion of CSV files into Delta tables, preserving the granularity and content of the original data, but already in a standardized tabular structure.

### Bronze Layer Objective

- Load all CSV files from the staging directory into Delta tables.
- Keep the format as close as possible to the original, ensuring full traceability of data origin.
- Adopt consistent naming with `_raw` suffix to indicate raw data.


### Ingestion Rules

- Automated reading of all files in the staging directory.
- Use of header and separator appropriate to the CSV standard.
- Derivation of the table name from the file name, with a rule to ensure the `100cep_` prefix when necessary.


### Bronze Tables Created

The following tables were created in the `bronze` schema, each related to a source CSV file:

- `100cep_customers_raw`
- `100cep_geolocation_raw`
- `100cep_order_items_raw`
- `100cep_order_payments_raw`
- `100cep_order_reviews_raw`
- `100cep_orders_raw`
- `100cep_products_raw`
- `100cep_sellers_raw`
- `100cep_product_category_name_translation_raw`
- `100cep_chargebacks_raw`

These tables form the historical and auditable base for transformations in the Silver and Gold layers.

***

## Silver Layer — Clean and Integrated Data: [04_silver](.databricks/pipeline/notebooks/04_silver.ipynb)

The **Silver** layer applies cleaning, standardization and enrichment rules over the Bronze tables, preparing data ready for analysis and dimensional modeling.

### Main Transformations

- **Standardization of identifiers**
    - Normalization of order, customer, product and seller keys (removal of unwanted spaces, case standardization) to ensure consistency across tables.
- **Handling of dates and times**
    - Conversion of relevant dates and timestamps to the `America/Sao_Paulo` time zone, with a standardized format for time-based analyses.
- **Normalization of statuses and types**
    - Translation and standardization of order statuses and payment types to Portuguese, with removal of accents and use of uppercase letters.
- **Data typing and conversion**
    - Conversion of numeric columns (installments, prices, freight, total values) to appropriate types (`INT`, `FLOAT`, `DECIMAL`).
- **Enrichments**
    - Extraction of customers' ZIP code prefixes for geographic analyses and segmentations.


### Thematic Silver Tables

From the Bronze tables, consolidated tables were created in the Silver layer:

- `100cep_pedidos`: view of orders with status, dates and main metrics.
- `100cep_pagamentos`: payment details, types, installments and values.
- `100cep_itens_pedidos`: items of each order, with products, quantities and prices.
- `100cep_clientes`: customer registry with location information and ZIP code prefix.

These tables serve as the base for the dimensions and fact in the Gold layer.

***

## Gold Layer — Analytical Model (Star Schema): [05_gold](.databricks/pipeline/notebooks/05_gold.ipynb)

The **Gold** layer materializes the analytical model in a **star schema**, with dimensional tables and a central fact table for analysis of transactions, risk and commercial performance.

### Dimensions

- **`dim_clientes`**
    - Single record per customer, with identifier and location attributes (including ZIP code prefix).
- **`dim_vendedores`**
    - Consolidated seller data, enabling performance and geographic coverage analyses.
- **`dim_pagamentos`**
    - Payment methods and related attributes, including risk classification by transaction type.
- **`dim_data`**
    - Calendar table with daily granularity, containing date, day of the week, month, year and other time attributes.
- **`dim_geolocalizacao`**
    - Information on cities and geolocation that are valid and relevant to the business.
- **`dim_chargebacks`**
    - Detailed chargebacks, with standardized reasons and responses, aimed at risk and fraud analyses.


### Fact Table

- **`fato_transacoes`**
    - Consolidates the transactions carried out, integrating orders, customers, sellers, payments, total values and order statuses.
    - It is the central table for analyses of sales, revenue, customer behavior, operational efficiency and chargeback impact.

The relationships between fact and dimensions follow the previously discussed star schema modeling (`fato_transacoes` linked to `dim_clientes`, `dim_vendedores`, `dim_data`, `dim_geolocalizacao` and `dim_chargebacks`).

***

## Data Lineage

The lineage of the pipeline can be summarized as:

- **Source**
    - Kaggle CSV (Olist dataset) + `datasets/ai_dataset/chargebacks_dataset.csv`.
- **Bronze (raw)**
    - CSV files ingested into `*_raw` tables in the `bronze` schema.
- **Silver (cleaned)**
    - Cleaned, standardized and enriched data in thematic tables (`100cep_pedidos`, `100cep_pagamentos`, `100cep_itens_pedidos`, `100cep_clientes`).
- **Gold (analytics)**
    - Dimensional model with `dim_clientes`, `dim_vendedores`, `dim_pagamentos`, `dim_data`, `dim_geolocalizacao`, `dim_chargebacks` and `fato_transacoes`.

***

## Pipeline Scripts

The project scripts are organized sequentially, allowing manual execution or orchestration as an ETL job:


| Order | Script | Description |
| :-- | :-- | :-- |
| 1 | `./.databricks/pipeline/notebooks/01_preparation.ipynb` | Creation of the `100cep_gateway` catalog and the `staging`, `bronze`, `silver` and `gold` schemas. |
| 2 | `./.databricks/pipeline/notebooks/02_download.ipynb` | Volume creation, download via `kagglehub` and copy of CSVs to the staging volume. |
| 3 | `./.databricks/pipeline/notebooks/03_bronze.ipynb` | Ingestion of CSVs into `*_raw` Delta tables in the Bronze layer. |
| 4 | `./.databricks/pipeline/notebooks/04_silver.ipynb` | Cleaning, standardization, enrichment and creation of thematic Silver tables. |
| 5 | `./.databricks/pipeline/notebooks/05_gold.ipynb` | Creation of dimensions and `fato_transacoes` in the Gold layer. |