# Local Data Lake — CSV, Parquet and DuckDB

A Python ETL project that turns order records into a local, queryable data lake.
It demonstrates date transformation, monthly partitioning and SQL analytics without
requiring a database server or cloud account.

## What the pipeline does

1. Reads `data/raw/orders.csv` with Pandas.
2. Converts `order_date` to a datetime and derives `year_month`.
3. Writes a Parquet dataset partitioned by month to `data/processed/orders.parquet/`.
4. Uses DuckDB to report order counts and total amounts grouped by `status`.

**Stack:** Python, Pandas, PyArrow, DuckDB.

## Run locally

Run these commands from the repository root. Python and pip are required.
The repository includes a small input CSV and a sample processed partition.

```shell
git clone https://github.com/ilyass74/data-lake-local.git
cd data-lake-local
python -m pip install pandas pyarrow duckdb
python etl_local.py
```

For an isolated environment, create and activate a Python virtual environment first.
Dependencies are not version-pinned in this repository.

## Input and output

The script requires at least these CSV columns:

| Column | Use |
| --- | --- |
| `order_date` | Date conversion and monthly partitioning |
| `status` | SQL grouping |
| `total_amount` | SQL sum |

Other input columns are preserved. Output directories have the form
`data/processed/orders.parquet/year_month=YYYY-MM/` and contain Parquet files.
The terminal prints a summary with `status`, `nb_orders` and `total_amount`.

## Project files

- [etl_local.py](etl_local.py): extraction, transformation, Parquet writing and SQL query.
- [data/raw/orders.csv](data/raw/orders.csv): included sample input.
- `data/processed/`: generated dataset and included sample output.

## Current scope

This is a batch learning project. It does not implement schema validation,
incremental ingestion, deduplication, orchestration or automated tests.
The writer does not explicitly clear existing partitions: repeated runs may retain
older Parquet files and inflate query results. Use a fresh output location or
intentionally clear only generated output before a full refresh.
No throughput or scalability benchmark is claimed.

## Related project

[Airflow + MinIO Data Lake](https://github.com/ilyass74/airflow-minio-data-lake)
extends the same order-processing idea with object storage and scheduling.

## Implementation reference

All behavior described here is based on [etl_local.py](etl_local.py).
