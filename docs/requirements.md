# Project Requirements

**Week:** 1  
**Project:** FitPulse Wellness Analytics Hub  
**Purpose:** Define the functional, data, dashboard, and evidence requirements for the project.

---

## 1. Functional Requirements

| ID | Requirement | Priority |
|---|---|---|
| FR-01 | Ingest synthetic source files into persistent Bronze Delta tables using Databricks and PySpark. | Must have |
| FR-02 | Create standardized Silver Candidate tables through the implemented cleaning, type-conversion, and transformation logic. | Must have |
| FR-03 | Implement data-quality validation checks and record their execution results. | Must have |
| FR-04 | Create Gold tables containing business-oriented workout, wellness, device, user-activity, and goal-progress summaries. | Must have |
| FR-05 | Build a Power BI dashboard using selected Gold tables for the main batch analytics report. | Must have |
| FR-06 | Simulate JSON workout events and process them through the separate live-event workflow into a pipeline-managed table. | Must have |

---

## 2. Data Requirements

| ID | Requirement |
|---|---|
| DR-01 | Use synthetic fitness and wellness source data representing users, workouts, devices, activity types, and goals. |
| DR-02 | Document the assumptions and generation logic used to create the synthetic FitPulse datasets. |
| DR-03 | Include or identify data-quality scenarios such as missing values, duplicates, invalid references, and invalid measurements where supported by the generated data and validation implementation. |
| DR-04 | Keep any sample data committed to GitHub small enough for demonstration and practical repository use. Prefer documenting how to regenerate larger datasets when appropriate. |
| DR-05 | Store and access source datasets through the documented Databricks Unity Catalog Volume: `/Volumes/fitpulse-wellness/default/fitpulsehub/`. |
| DR-06 | Preserve source-to-output traceability where supported by the ingestion and transformation implementation. |
| DR-07 | Document the schemas, expected table grains, and recorded row counts for Bronze, Silver Candidate, and Gold datasets. |

---

## 3. Dashboard Requirements

| ID | Requirement |
|---|---|
| BI-01 | Use Gold-layer tables as the data sources for the main batch analytics dashboard. |
| BI-02 | Include KPI cards, time-based trends, activity-type comparisons, goal-progress analysis, and relevant interactive filters. |
| BI-03 | Document the dashboard's purpose, KPI definitions, visual choices, and validated insights in `docs/dashboard_insights.md`, if this file is part of the repository structure. |
| BI-04 | Include the documented KPI scope: Total Workouts, Total Users, Total Workout Time, Average Workout Time, Total Active Days, Average Workouts per Year, Total Goals, Completed Goals, and Average Goal Completion. |
| BI-05 | Validate dashboard totals and measures against Databricks query results and the actual definitions implemented in Power BI. |
| BI-06 | Configure relationships and filter behavior according to the grain and keys of the selected Gold tables. |
| BI-07 | Document that Power BI Import mode displays data from its last successful refresh and does not automatically refresh simply because upstream data changes. |
| BI-08 | Treat live-event reporting separately from the batch Gold dashboard unless a verified integration connects the two workflows. |

---

## 4. Evidence Requirements

| ID | Requirement |
|---|---|
| EV-01 | Commit weekly progress logs to GitHub with work completed, challenges, outcomes, and next steps. |
| EV-02 | Store relevant screenshots of notebook execution, table outputs, data-quality results, pipeline status, and Power BI dashboards in the `screenshots/` folder, where applicable. |
| EV-03 | List external learning and implementation references in `docs/references.md`. |
| EV-04 | Disclose AI assistance in weekly logs and project documentation where applicable, and review generated suggestions against the actual implementation. |
| EV-05 | Commit notebooks, code, and documentation using meaningful Git commit messages. |
| EV-06 | Record validation evidence for important row counts, data-quality checks, Gold aggregations, and Power BI KPI calculations. |
| EV-07 | Document known limitations, execution dependencies, and any incomplete or unverified functionality. |

---

## 5. Implementation Notes

- The primary batch workflow is documented as synthetic data generation, exploration, Bronze ingestion, Silver Candidate transformations, data-quality checks, Gold aggregations, and Power BI reporting.
- The live-event simulation is a separate workflow and should not be described as feeding the batch Gold tables without evidence of an implemented connection.
- Recorded table counts are reference observations and should be rechecked against the current Databricks tables.
- The presence of a requirement does not establish that it has been implemented or passed validation. Completion should be supported by code, notebook outputs, test results, or screenshots.
