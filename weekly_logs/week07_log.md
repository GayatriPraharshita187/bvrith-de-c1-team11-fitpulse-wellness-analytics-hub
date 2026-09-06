# Week 07 Log — Gold Metrics

**Week:** 7  
**Date range:** 1 September 2026 – 7 September 2026  
**Team:** Team 11 — DataStreamers  
**Project:** FitPulse Wellness Analytics Hub  

---

## 1. Sprint Goal

Create dashboard-ready Gold layer tables from the Silver data and document the KPI formulas and metric grain for each Gold table. Prepare the Gold layer outputs for future Power BI dashboard integration.

---

## 2. Work Completed

| **Task** | **Owner** | **Status** | **Evidence** |
|---|---|---|---|
| Created `gold_activity_summary` | Charka Cherishma | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_user_activity_summary` | Charka Cherishma | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_goal_progress_summary` | Kanumuri Gayatri Praharshita | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_goal_segment_summary` | Kanumuri Gayatri Praharshita | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_device_usage_summary` | Dharavath Sandhya | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_device_health_summary` | Dharavath Sandhya | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_device_platform_summary` | Charka Cherishma | Done | `notebooks/05_gold_aggregations.ipynb` |
| Created `gold_daily_wellness_summary` | Charka Cherishma | Done | `notebooks/05_gold_aggregations.ipynb` |
| Documented Gold KPI formulas and metric grains | Kanumuri Gayatri Praharshita | Done | `docs/gold_metrics_definition.md` |
| Validated Gold table outputs and row counts | Dharavath Sandhya | Done | `week07_gold_validation.png` |

---

## 3. Key Decisions

- Created eight dashboard-ready Gold tables using the available Silver-layer data.
- Used the `fitpulse-wellness.default` schema with the `gold_` naming convention because a separate Gold schema could not initially be created due to permission restrictions.
- Defined a clear metric grain for every Gold table to avoid incorrect aggregations in dashboards.
- Used documented KPI formulas for activity, goals, devices and daily wellness metrics.
- Since an app-version field was not available in the current Silver device schema, platform and device-type metrics were used for device distribution analysis.

---

## 4. Blockers / Risks

| **Blocker** | **Impact** | **Help Needed** |
|---|---|---|
| Permission to create a separate Gold schema was initially unavailable | Delayed creation of the dedicated Gold schema | Required table-creation permission was obtained |
| App-version field was not available in the current Silver device schema | App-version mix could not be included as a Gold KPI | Documented the limitation and used available platform/device-type fields |
| Gold tables depend on the quality of Silver data | Incorrect Silver records could affect Gold KPIs | Silver outputs and Gold aggregations were validated |

---

## 5. Evidence Added to GitHub

- `notebooks/05_gold_aggregations.ipynb` updated with Gold aggregation SQL.
- `docs/gold_metrics_definition.md` added with KPI formulas and metric grains.
- `weekly_logs/week07_log.md` updated.
- `week07_gold_metrics.png` added as Gold table evidence.
- `week07_gold_validation.png` added as Gold validation evidence.
- Small Gold sample outputs can be added under `data_sample/gold_exports/` if required.

---

## 6. AI Transparency Note

| **Question** | **Response** |
|---|---|
| Where AI helped | AI assisted with SQL structure, Gold table design, KPI formula documentation and troubleshooting Databricks errors. |
| What we changed after AI suggestion | We adapted the SQL to match the actual Silver table schemas, available columns and project requirements. |
| What we verified manually | We manually checked Gold table creation, schemas, row counts, sample records and aggregation outputs in Databricks. |
| What we can explain without AI | We can explain the metric grains, KPI formulas, aggregation logic, Gold-layer design and validation process independently. |

---

## 7. Next Week Preparation

- Review the completed Gold tables and confirm that all dashboard KPIs are available.
- Prepare the Gold-layer outputs for Power BI dashboard development.
- Ensure Power BI uses Gold tables rather than directly querying Bronze or Silver data.
