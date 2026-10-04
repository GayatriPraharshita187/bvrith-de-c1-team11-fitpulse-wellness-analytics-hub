# Week 11 – FitPulse Wellness Analytics Hub: Pipeline Walkthrough

## Purpose

This document explains the end-to-end flow of the FitPulse Wellness Analytics Hub, from synthetic source-data generation and ingestion through Databricks transformations, data-quality checks, Gold-layer analytics, Power BI reporting, and the separate live-event simulation.

The project demonstrates data engineering concepts using Databricks, Delta tables, Unity Catalog, a Medallion Architecture, a Databricks SQL Warehouse, and Power BI.

---

## 1. Pipeline Run Order

The main batch analytics pipeline follows the sequence below.

| Step | Notebook / File | Purpose / Expected Output |
|---|---|---|
| 1 | `src/generate_synthetic_data.py` | Generates synthetic FitPulse source datasets |
| 2 | `notebooks/01_data_exploration.ipynb` | Explores source structures, columns, data types, volumes, and potential quality issues |
| 3 | `notebooks/02_bronze_ingestion.ipynb` | Loads source data into persistent Bronze Delta tables |
| 4 | `notebooks/03_silver_transformations.ipynb` | Creates standardized Silver Candidate tables |
| 5 | `notebooks/04_data_quality_checks.ipynb` | Runs data-quality validation checks |
| 6 | `notebooks/05_gold_aggregations.ipynb` | Produces business-oriented Gold analytics tables |
| 7 | `notebooks/06_powerbi_export.ipynb` | Prepares or exports Gold data for Power BI, according to the implemented notebook logic |
| 8 | `notebooks/07_streaming_simulation.ipynb` | Supports the separate streaming or simulated live-analytics workflow |

**Execution note:** The table represents the documented project sequence. Confirm the actual notebook dependencies and whether every notebook must be run for a normal refresh. The live-event workflow should be documented separately if it is not a dependency of the batch Gold pipeline.

## 2. Architecture Explanation

The FitPulse Wellness Analytics Hub follows the Medallion Architecture pattern in Databricks.

The main analytics pipeline transforms synthetic fitness and wellness data into structured datasets and business-ready metrics.

The architecture has three primary data layers:

- **Bronze:** Raw ingested source data persisted as Delta tables.
- **Silver:** Structured and standardized Candidate datasets prepared for downstream processing.
- **Gold:** Aggregated and business-oriented datasets designed for analytics and reporting.

Data exploration occurs before ingestion to help understand the source data. Data-quality checks are performed after Silver transformations, before the results are relied on for Gold analytics.

The resulting Gold tables are accessible through Databricks and can be queried by Power BI through a Databricks SQL Warehouse.

The main batch flow is:

`Synthetic Data → Source Files → Exploration → Bronze → Silver Candidate → Data Quality Checks → Gold → Databricks SQL Warehouse → Power BI`

The live-event simulation is a related but separate flow:

`Event Generator → Live-Event Source Table → Pipeline-Managed Streaming Table`

The live-event flow should not be presented as feeding the batch Gold tables unless a downstream transformation explicitly connects the two.

## 3. Source Data

The project uses five synthetic source datasets stored in a Databricks Unity Catalog Volume.

**Volume root:**

`/Volumes/fitpulse-wellness/default/fitpulsehub/`

| Source file | Description |
|---|---|
| `activity_types.csv` | Activity definitions and associated activity information |
| `devices.csv` | Device information |
| `goals.csv` | User fitness and wellness goals |
| `users.json` | User profile information |
| `workouts.parquet` | Workout and activity records |

These datasets represent the primary entities used by the analytics pipeline: users, workouts, devices, activity types, and goals.

The source data is synthetic and is intended for demonstrating data engineering and analytics workflows.

## 4. Data Exploration

**Notebook:** `notebooks/01_data_exploration.ipynb`

Data exploration is performed before building the transformation pipeline. Its purpose is to understand the structure and characteristics of the incoming datasets.

The documented exploration scope includes:

- Identifying the available source files.
- Inspecting dataset structures and column names.
- Understanding data types.
- Examining record volumes.
- Investigating missing or unusual values.
- Understanding relationships between datasets.
- Identifying potential data-quality concerns.

This stage provides context for ingestion, transformation, validation, and downstream metric design.

Exploration results should be recorded as evidence rather than treated as proof that all identified issues have already been corrected.

## 5. Bronze Layer – Raw Ingestion

**Notebook:** `notebooks/02_bronze_ingestion.ipynb`

The Bronze layer provides the raw persistent landing layer for the five source datasets.

The recorded Bronze table counts are:

| Bronze table | Recorded rows |
|---|---:|
| `bronze_activity_types` | 8 |
| `bronze_devices` | 5,001 |
| `bronze_goals` | 18,001 |
| `bronze_users` | 8,000 |
| `bronze_workouts` | 60,000 |

The Bronze layer is intended to retain source records and provide a stable foundation for subsequent processing.

The documented technical metadata fields include:

- `source_file`
- `ingestion_timestamp`
- `ingestion_run_id`

These fields support source traceability and ingestion auditing where they are populated by the implementation.

Bronze ingestion establishes persistent Delta tables that downstream transformations can read without repeatedly parsing the original source files.

**Validation evidence:** The row counts above are recorded observations. For reproducibility, rerun the relevant SQL count checks and retain the execution results alongside the documentation.

## 6. Silver Layer – Standardization and Transformation

**Notebook:** `notebooks/03_silver_transformations.ipynb`

The Silver transformation stage converts the Bronze representation into structured datasets for downstream validation and analytics.

The Silver Candidate tables are:

- `silver_activity_types_candidate`
- `silver_devices_candidate`
- `silver_goals_candidate`
- `silver_users_candidate`
- `silver_workouts_candidate`

The documented transformation pattern is:

`Bronze → Standardization → Safe Type Conversion → Transformation Logic → Silver Candidate`

The intended responsibilities of this stage include:

- Standardizing source values.
- Safely converting data types.
- Applying the implemented transformation rules.
- Preserving source-to-output traceability.
- Checking whether expected source records remain represented after transformation.

The recorded Bronze-to-Silver Candidate row-count reconciliation is:

| Dataset | Bronze rows | Silver Candidate rows |
|---|---:|---:|
| Activity types | 8 | 8 |
| Devices | 5,001 | 5,001 |
| Goals | 18,001 | 18,001 |
| Users | 8,000 | 8,000 |
| Workouts | 60,000 | 60,000 |

These counts show that the recorded physical row counts match between the two layers.

**Important limitation:** Matching row counts do not, by themselves, prove that values were transformed correctly, that duplicates are absent, that every record is valid, or that referential integrity is satisfied. Those claims require additional checks.

The `Candidate` suffix is retained in the documented table names. Do not rename these as trusted or production Silver tables unless that is how the project actually implements them.

## 7. Data Quality Checks

**Notebook:** `notebooks/04_data_quality_checks.ipynb`

Data-quality validation is performed after the Silver transformation stage and before the resulting data is relied on for business analytics.

The documented validation scope includes:

- Missing-value checks.
- Data-type validation.
- Duplicate detection.
- Relationship and referential-integrity checks.
- Date validation.
- Business-rule checks.

The intended flow is:

`Silver Candidate → Data Quality Checks → Validated Results → Gold Processing`

The actual outcome of each check should be recorded in the notebook or its execution output. A check being present in a notebook does not automatically mean the data passed it.

Where the implementation uses quarantine tables or locations, document the relevant object names, rejection criteria, and record counts from the actual execution results. Do not describe every Silver record as trusted unless the project has an explicit validation and acceptance process that establishes this.

## 8. Gold Layer – Business-Ready Analytics

**Notebook:** `notebooks/05_gold_aggregations.ipynb`

The Gold layer contains business-oriented summaries used for analytics and reporting.

The project has eight recorded Gold tables:

| Gold table | Recorded rows | Analytical purpose |
|---|---:|---|
| `gold_activity_summary` | 1,267 | Activity-level workout metrics |
| `gold_daily_wellness_summary` | 181 | Daily wellness and workout metrics |
| `gold_device_health_summary` | 5,000 | Device health and device-level information |
| `gold_device_platform_summary` | 16 | Device and platform breakdowns |
| `gold_device_usage_summary` | 56 | Device usage summaries |
| `gold_goal_progress_summary` | 17,925 | Goal targets, progress, and status |
| `gold_goal_segment_summary` | 12 | Goal segmentation by relevant categories |
| `gold_user_activity_summary` | 7,396 | User-level activity summaries |

These are the recorded counts from the project work. They should be revalidated against the current tables before being presented as the latest counts.

### 8.1 `gold_daily_wellness_summary`

This table supports daily wellness and workout analysis.

Documented fields include:

- `wellness_date`
- `total_workouts`
- `active_users`
- `total_duration_minutes`
- `total_calories_burned`
- `total_distance_km`
- `total_repetitions`
- `average_workout_duration_minutes`
- `average_calories_per_workout`

It supports questions about workout volume, daily participation, workout duration, calories burned, and changes over time.

### 8.2 `gold_activity_summary`

This table supports analysis by workout date and activity type.

Documented fields include:

- `workout_date`
- `activity_type`
- `total_workouts`
- `active_users`
- `total_duration_minutes`
- `total_calories_burned`
- `total_distance_km`
- `total_repetitions`

It can be used to compare activity types and examine how workout metrics vary over time.

### 8.3 `gold_user_activity_summary`

This table provides user-level activity summaries.

Documented fields include:

- `user_id`
- `total_workouts`
- `total_active_days`
- `total_duration_minutes`
- `total_calories_burned`
- `total_distance_km`
- `total_repetitions`

It supports analysis of accumulated activity and participation at the user level.

### 8.4 `gold_goal_progress_summary`

This table supports goal tracking and progress analysis.

Documented fields include:

- `goal_id`
- `user_id`
- `goal_type`
- `target_value`
- `target_unit`
- `goal_start_date`
- `goal_end_date`
- `goal_status`
- `activity_type_scope`
- `actual_progress`
- `progress_percentage`
- `remaining_value`
- `progress_status`

It supports analysis of targets, recorded progress, remaining values, and goal status.

Progress percentages and actual progress must be interpreted according to the implemented business logic, including the goal type, unit, scope, and relevant dates.

### 8.5 Device and goal segmentation tables

The remaining Gold tables extend the analytics model:

- `gold_device_health_summary` supports device-level health and status analysis.
- `gold_device_platform_summary` supports comparisons across device platforms and types.
- `gold_device_usage_summary` supports summarized device usage analysis.
- `gold_goal_segment_summary` supports grouped analysis of goal categories and statuses.

For the repository documentation, the exact grouping keys, aggregation rules, and grain of these four tables should be taken from their schemas and SQL transformations.

### 8.6 Gold-layer modeling considerations

Gold tables can have different grains. For example, a daily summary and a user-level summary do not represent the same unit of analysis.

When using them together:

- Do not add aggregate measures from different grains without checking their meaning.
- Do not assume that an average of averages produces a valid overall average.
- Do not sum distinct-user counts across dates and treat the result as unique users over the entire period.
- Validate goal progress according to the goal's unit and scope.
- Confirm relationship cardinality before connecting tables in Power BI.

The purpose of Gold is not merely to reduce row counts. It is to provide clearly defined metrics that can be interpreted correctly by downstream reporting.

## 9. Gold to Power BI

The reporting path is:

`Gold Tables → Databricks SQL Warehouse → Power BI → Data Model → Measures → Visuals → Dashboard`

The Databricks SQL Warehouse connection used during the project is:

- **Server hostname:** `dbc-710f445c-1913.cloud.databricks.com`
- **HTTP path:** `/sql/1.0/warehouses/34afe8ca986cfc0b`

These connection details identify the Databricks endpoint. Authentication must be configured separately, and credentials or tokens should never be committed to the GitHub repository.

Power BI uses the Databricks connection to access data for the report. The model then combines selected tables, relationships, measures, and visuals to present the analytics.

The main reporting questions include:

- How many workouts were recorded?
- How does workout activity change over time?
- Which activity types account for the most workouts or calories?
- How much workout time has been recorded?
- How active are users?
- What progress has been recorded toward fitness goals?
- How are device health and usage distributed?

The dashboard should be validated against Databricks query results so that displayed totals match the intended definitions and table grains.

## 10. Power BI KPIs

The documented KPI scope includes:

- Total Workouts
- Total Users
- Total Workout Time
- Average Workout Time
- Total Active Days
- Average Workouts per Year
- Total Goals
- Completed Goals
- Average Goal Completion

Each KPI should have a defined calculation and a clear source.

For example, total workout time should be based on the appropriate duration field and should use a table or measure that does not duplicate workouts. Total users must distinguish between a unique-user count and a sum of daily active-user counts. Average goal completion must follow the project's chosen definition of completion and must handle applicable goal types consistently.

The exact DAX expressions, filter behavior, and KPI definitions should be documented from the Power BI model rather than inferred from the KPI labels alone.

## 11. Power BI Visualizations

The dashboard organizes the Gold metrics into business-oriented views.

### Workout analysis

Examples include:

- Total workout KPI.
- Daily workout trends.
- Total workout time.
- Average workout duration.

### Activity analysis

Examples include:

- Calories burned by activity type.
- Workout counts by activity type.
- Activity-level comparisons.

### Goal analysis

Examples include:

- Total goals.
- Completed goals.
- Goal progress.
- Goal completion metrics.

### Device analysis

Where the corresponding Gold tables are used in the report, device-health and device-usage summaries can support device-status, platform, and usage comparisons.

Visuals should be selected to answer specific analytical
