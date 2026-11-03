# NYC Taxi Data Engineering Pipeline on Databricks

An end-to-end data engineering project that ingests NYC TLC yellow taxi trip data, cleans it with PySpark, models it in a Bronze/Silver/Gold medallion architecture on Delta Lake, and benchmarks SQL query performance across different table layouts.

## Architecture

```
NYC TLC (Parquet / CSV over HTTPS)
        |
        v
Unity Catalog Volume (raw files)
        |
        v
Bronze  - raw Delta table (typed, with source_file + ingested_at)
        |
        v
Silver  - cleaned and enriched trips (+ zone lookup)
        |
        v
Gold    - daily_summary, hourly_demand, top_pickup_zones
        |
        v
Dashboard + SQL benchmark
```

Add your diagram image at `docs/architecture.png`.

## Tech stack
Databricks, PySpark, Spark SQL, Delta Lake, Unity Catalog, Databricks Jobs, Databricks SQL Dashboards, Git.

## Repository structure

| Path | Purpose |
|---|---|
| `notebooks/` | Pipeline notebooks (00 to 05), run in order |
| `sql/` | Profiling, table variants, benchmark queries |
| `jobs/` | Exported Databricks Job definition |
| `dashboard/` | Exported dashboard |
| `results/` | Benchmark results, cleaning summary, sample data |
| `docs/` | Screenshots and diagrams |
| `data/` | Data source links (no raw data stored) |

## How to run

1. Create a Databricks workspace (Free Edition works).
2. Import this repo through **Workspace > Create > Git folder**.
3. Run notebooks in order: `00_setup`, `01_ingest_download`, `02_bronze`, `03_silver_cleaning`, `04_gold_aggregates`, `05_benchmark`.
4. Or create a Job from `jobs/nyc_taxi_pipeline_job.json` (replace `<YOUR_EMAIL>` in the paths).
5. If `CREATE CATALOG` is not permitted, use the default `workspace` catalog and replace `nyc_taxi`.

## Data source
See [`data/README.md`](data/README.md). Jan to Jun 2024 yellow taxi trips by default; extend the `months` list for more.

## Data cleaning rules

Row counts are in `results/cleaning_summary.csv`. Fill the table below after running.

| Rule | Rows removed |
|---|---|
| Duplicates | _fill_ |
| Pickup outside Jan-Jun 2024 | _fill_ |
| Dropoff not after pickup | _fill_ |
| Distance outside 0.1-100 miles | _fill_ |
| Fare <= 0 | _fill_ |
| Total <= 0 | _fill_ |
| Passengers outside 1-6 | _fill_ |
| Duration outside 1-300 min | _fill_ |
| Speed >= 80 mph | _fill_ |

## SQL benchmark

Four layouts of the same Silver table were compared on five queries (median of 3 runs): baseline, partitioned by month, liquid clustering on (pickup_date, PULocationID), and ZORDER on the same columns.

| Query | baseline | partitioned | clustered | zorder |
|---|---|---|---|---|
| Q1 count by month | _fill_ | _fill_ | _fill_ | _fill_ |
| Q2 filter one day | _fill_ | _fill_ | _fill_ | _fill_ |
| Q3 top zones | _fill_ | _fill_ | _fill_ | _fill_ |
| Q4 hourly tip | _fill_ | _fill_ | _fill_ | _fill_ |
| Q5 long trips | _fill_ | _fill_ | _fill_ | _fill_ |

**Conclusions:** _write your findings here (which layout won for date and zone filters, and whether partitioning helped at this data size)._

## Screenshots
- Job run: `docs/job_run_screenshot.png`
- Dashboard: `docs/dashboard_screenshot.png`
- Query profile: `docs/query_profile_screenshot.png`

## Challenges and learnings
_Write 3 to 5 points (schema differences between months, data quality issues, Free Edition limits, benchmark caching effects)._

## Possible improvements
Auto Loader for incremental ingestion, data quality expectations, green taxi and FHV data, MERGE-based incremental loads, Delta time travel.
