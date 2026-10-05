# Week 07 Log - Gold Table Design and Build

**Week:** 7
**Date range:** 21 August 2026 – 27 August 2026
**Team:** Team FitPulse
**Project:** FitPulse - Wellness Analytics Hub

---

## 1. Sprint Goal

The goal of Week 7 was to transform the validated Silver data into dashboard-ready Gold tables for the FitPulse Wellness Analytics Hub.

The work focused on defining Gold table grains and KPI logic, creating the required Gold aggregations, validating keys and measures, checking potential duplication and cross-table consistency issues, and preparing the Gold layer for downstream Power BI analytics.

---

## 2. Work Completed

| **Task**                                                           | **Owner**                    | **Status** | **Evidence**                                                    |
| ------------------------------------------------------------------ | ---------------------------- | ---------- | --------------------------------------------------------------- |
| Reviewed the Silver inputs and prepared the Gold-layer schema      | Charka Cherishma             | Done       | `week07_01_trusted_handoff.png`                                 |
| Defined Gold table grains and KPI calculations                     | Kanumuri Gayatri Praharshita | Done       | `week07_02_kpi_contract.png`                                    |
| Built `gold_activity_summary` and `gold_user_activity_summary`     | Charka Cherishma             | Done       | `week07_06_gold_table_created.png`                              |
| Built `gold_goal_progress_summary` and `gold_goal_segment_summary` | Kanumuri Gayatri Praharshita | Done       | `week07_06_gold_table_created.png`, `week07_07_gold_sample.png` |
| Built device-related Gold summaries                                | Dharavath Sandhya            | Done       | `week07_06_gold_table_created.png`, `week07_07_gold_sample.png` |
| Built `gold_daily_wellness_summary`                                | Charka Cherishma             | Done       | `week07_06_gold_table_created.png`, `week07_07_gold_sample.png` |
| Validated Gold table grains and duplicate-key conditions           | Dharavath Sandhya            | Done       | `week07_08_grain_measure_checks.png`                            |
| Investigated goal-level duplication and cross-table user coverage  | Kanumuri Gayatri Praharshita | Done       | `week07_08_grain_measure_checks.png`                            |
| Reviewed Gold outputs and row counts across all eight tables       | All                          | Done       | `week07_06_gold_table_created.png`                              |

---

## 3. Gold Table Catalog

| **Gold Table**                 | **Grain**                                 | **Purpose**                               | **Observed Rows** |
| ------------------------------ | ----------------------------------------- | ----------------------------------------- | ----------------: |
| `gold_activity_summary`        | Workout date + activity type              | Daily activity-level metrics              |             1,267 |
| `gold_user_activity_summary`   | User                                      | Individual user activity metrics          |             7,396 |
| `gold_goal_progress_summary`   | Goal                                      | Goal progress and completion metrics      |            17,925 |
| `gold_goal_segment_summary`    | Goal type + target unit + progress status | Aggregated goal performance               |                12 |
| `gold_device_usage_summary`    | Device type + platform + device status    | Device usage and status metrics           |                56 |
| `gold_device_health_summary`   | Device                                    | Device health, usage and lifetime metrics |             5,000 |
| `gold_device_platform_summary` | Platform + device type                    | Device distribution and user coverage     |                16 |
| `gold_daily_wellness_summary`  | Workout date                              | Daily wellness trend metrics              |               181 |

---

## 4. Key Decisions

* Used the validated Silver data as the input for Gold processing.
* Defined each Gold table around an explicit business grain.
* Created separate Gold summaries for activity, users, goals, devices and daily wellness.
* Used distinct counts where required for workout and user metrics.
* Avoided combining unrelated aggregate tables at incompatible grains.
* Preserved goal progress calculations at the goal level.
* Used separate device Gold tables for device-level, platform-level and usage/status analysis.
* Treated duplicate-key findings as validation issues requiring investigation rather than silently removing records.
* Prepared the Gold layer as the governed analytics source for downstream Power BI work.
* App-version analysis was not implemented because app-version data was not available in the current Silver/Device schema.

---

## 5. Validation Findings

### Grain Validation

Duplicate-key checks were performed for the defined Gold grains:

* `gold_daily_wellness_summary` by `wellness_date`
* `gold_activity_summary` by `workout_date + activity_type`
* `gold_user_activity_summary` by `user_id`
* `gold_goal_progress_summary` by `goal_id`
* `gold_device_health_summary` by `device_id`
* `gold_device_platform_summary` by `platform + device_type`
* `gold_device_usage_summary` by `device_type + platform + device_status`
* `gold_goal_segment_summary` by `goal_type + target_unit + progress_status`

### Goal Progress Finding

The validation identified **100 duplicated `goal_id` values** in `gold_goal_progress_summary`.

The table contained:

* **17,925 total rows**
* **17,825 unique goals**
* **100 goal IDs with duplicate rows**

Additional checks were performed on duplicated goal records to compare target value, goal type, dates, status and activity scope.

This finding should be reviewed before treating `goal_id` as a guaranteed unique business key in downstream reporting.

### Cross-Table User Coverage

A cross-check between `gold_goal_progress_summary` and `gold_user_activity_summary` identified **2 users present in goal progress but not in the user activity summary**.

These records were retained for investigation rather than silently removed.

---

## 6. Blockers / Risks

| **Blocker / Risk**                                                    | **Impact**                                                                         | **Resolution / Help Needed**                                                      |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `gold_goal_progress_summary` contains 100 duplicated goal IDs         | A direct assumption of one row per `goal_id` may be unsafe for downstream analysis | Investigate the duplicated goal records and determine the correct business key    |
| Two goal users are not present in `gold_user_activity_summary`        | Cross-table user analysis may exclude these users                                  | Review the underlying Silver records and determine why activity coverage differs  |
| Gold tables have different grains                                     | Joining unrelated Gold summaries can multiply records and inflate measures         | Keep Gold tables at their defined grain and use validated relationships           |
| Distinct-user metrics cannot always be summed across aggregate tables | Can lead to double counting                                                        | Calculate distinct users at the required reporting grain                          |
| Average metrics are already aggregated                                | Re-averaging them without appropriate weighting can produce misleading results     | Use the approved KPI definition and validate against the underlying eligible data |
| Device-level and platform-level metrics have different grains         | Direct aggregation across these tables can duplicate device/user counts            | Use the appropriate Gold table for each analytical question                       |
| App-version data is unavailable                                       | An app-version Gold table cannot be implemented from the current schema            | Use platform and device-type metrics instead                                      |

---

## 7. Evidence Added to GitHub

### Notebook

* `notebooks/05_gold_aggregations.ipynb`

### Documentation

* `docs/gold_metrics_definition.md`

### Screenshots

* `screenshots/week07_01_trusted_handoff.png`
* `screenshots/week07_02_kpi_contract.png`
* `screenshots/week07_03_scope_profile.png`
* `screenshots/week07_04_lookup_uniqueness.png`
* `screenshots/week07_05_join_validation.png`
* `screenshots/week07_06_gold_table_created.png`
* `screenshots/week07_07_gold_sample.png`
* `screenshots/week07_08_grain_measure_checks.png`
* `screenshots/week07_09_measure_reconciliation.png`
* `screenshots/week07_10_controlled_rerun.png`

### Weekly Log

* `weekly_logs/week07_log.md`

---

## 8. AI Transparency Note

| **Question**                        | **Response**                                                                                                                                                                                                                                           |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Where AI helped                     | AI was used to support Gold-layer design, explain table-grain concepts, review KPI definitions, identify potential double-counting risks and organize validation steps.                                                                                |
| What we changed after AI suggestion | The team reviewed table grains, KPI definitions, aggregation logic and validation checks against the actual FitPulse notebook and Gold-layer implementation.                                                                                           |
| What we verified manually           | Reviewed the Silver inputs, Gold table creation queries, KPI calculations, row counts, duplicate-key checks, cross-table user coverage and Gold output samples in Databricks.                                                                          |
| What we can explain without AI      | We can explain how Silver data is transformed into Gold summaries, why each table has its defined grain, how KPI calculations work, why duplicate and cross-table checks are necessary, and how the Gold layer supports downstream Power BI analytics. |

---

## 9. Next Week Preparation

* Review the validated Gold tables and their business grains before Power BI integration.
* Investigate the duplicated `goal_id` records before using goal progress as a one-row-per-goal dataset.
* Review the two users missing from `gold_user_activity_summary`.
* Define Power BI relationships using the appropriate Gold dimensions and grains.
* Map dashboard KPIs to the corresponding Gold fields.
* Validate Power BI KPI results against the Databricks Gold outputs.
* Begin dashboard implementation using the approved Gold layer.
