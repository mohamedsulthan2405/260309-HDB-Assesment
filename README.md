# HDB Resale Flat Prices — ETL / ELT Pipeline

ETL pipeline for Singapore HDB resale transaction data (2012–2016), built on Databricks using Python and PySpark. Implements Medallion Architecture (Bronze / Silver / Gold).

---

## Requirements

- Databricks workspace (Sandbox or Cloud)
- Cluster: Databricks Runtime 11.x or later
- Python 3.x / PySpark

---

## Setup

**1. Upload source CSV files to DBFS**

Download data from https://data.gov.sg/collections/189/view and upload to:
```
dbfs:/FileStore/hdb_resale_datasets/
```

**2. Import notebooks**

Upload all notebooks from the `notebooks/` folder into the same Databricks workspace folder:
```
/Workspace/Users/<your-email>/hdb/
```

**3. Check settings in `00_nb_configs`**

| Setting | Default |
|---------|---------|
| `SOURCE_PATH` | `dbfs:/FileStore/hdb_resale_datasets/` |
| `DATE_START` | `2012-01-01` |
| `DATE_END` | `2016-12-31` |

---

## How to Run

Run `99_nb_master_orchestration` to execute the full pipeline in one step.

Or run manually in this order:

```
00_nb_configs        → settings
01_nb_raw            → ingest CSVs
02_nb_cleaned        → clean & validate
03_nb_transformed    → transform & hash
04_nb_reporting      → dimensional model
```

---

## Outputs

| Table | Layer | Description |
|-------|-------|-------------|
| `bronze.raw_hdb` | Bronze | Raw ingested records |
| `bronze.cleaned_hdb` | Bronze | Records passing all QC checks |
| `bronze.failed_hdb` | Bronze | Rejected records with reason |
| `silver.transformed_hdb` | Silver | Cleaned + resale_identifier |
| `silver.hashed_hdb` | Silver | Transformed + SHA-256 hash |
| `gold.dim_*` / `gold.fact_resale` | Gold | Snowflake schema |
| `gold.rpt_*` | Gold | Analytical report tables |

---

## Docs

See `docs/HDB_Pipeline_Technical_Guide.docx` for full pipeline documentation and `docs/architecture/` for AWS architecture diagrams.
