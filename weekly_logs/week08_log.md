# Week 08 Log — Gold to Power BI Dashboard

**Week:** 8

**Date range:** 24 Aug 2026 – 30 Aug 2026

**Team:** Team 11 – DataStreamers

**Project:** FitPulse – Wellness Analytics Hub


**Students:**

* Charka Cherishma
* Kanumuri Gayatri Praharshita
* Dharavath Sandhya

---

## 1. Sprint Goal

The goal for Week 08 was to connect the approved Gold outputs to Power BI and create the first working dashboard for the FitPulse Wellness Analytics Hub.

The dashboard was designed using Gold outputs only, with suitable KPI cards, charts, and slicers. We also checked that the dashboard values matched the approved Gold data.

---

## 2. Work Completed

| Task                                                         | Owner                        | Status | Evidence                      |
| ------------------------------------------------------------ | ---------------------------- | ------ | ----------------------------- |
| Reviewed the approved Gold tables and their business purpose | Charka Cherishma             | Done   | Gold tables / Week 7 notebook |
| Checked Gold table grain, columns, and required measures     | Charka Cherishma             | Done   | Gold validation               |
| Prepared the approved Gold outputs for Power BI              | Charka Cherishma             | Done   | `06_powerbi_export.ipynb`     |
| Reviewed Power BI data model and field types                 | Kanumuri Gayatri Praharshita | Done   | Power BI model screenshot     |
| Connected approved Gold outputs to Power BI                  | Kanumuri Gayatri Praharshita | Done   | Power BI screenshot           |
| Created the first Power BI dashboard page                    | Kanumuri Gayatri Praharshita | Done   | `powerbi_dashboard.pbix`      |
| Created KPI cards, charts, and slicers                       | Dharavath Sandhya            | Done   | Power BI dashboard            |
| Checked dashboard values against Gold outputs                | Dharavath Sandhya            | Done   | Reconciliation screenshot     |
| Prepared GitHub evidence and Week 08 documentation           | All team members             | Done   | GitHub repository             |

The official Week 08 plan focuses on Gold export, Power BI connection, a first dashboard page, and reconciliation of dashboard values with Gold.

---

## 3. Key Decisions

* Power BI will use **approved Gold outputs only**.
* We selected the FitPulse Gold tables required for the dashboard:

  * `activity_summary`
  * `goal_progress_summary`
  * `device_usage_summary`
* Dashboard visuals were selected based on FitPulse business questions rather than simply creating one visual for every Gold table.
* KPI definitions from the Gold layer were kept unchanged in Power BI.
* Different Gold tables were kept separate where a safe relationship was not required.
* The first dashboard was kept simple and focused on activity, goals, and device health.

The Week 08 project deck specifically identifies these FitPulse Gold tables for the Activity, Goals, and Device Health dashboard story.

---

## 4. Blockers / Risks

| Blocker                                             | Impact                                              | Help Needed                                                     |
| --------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------------------- |
| Power BI refresh or file-path issues                | Dashboard may not refresh correctly                 | Check the Gold export path and refresh the data                 |
| Different Gold tables may have different grains     | Incorrect relationships could produce wrong results | Review grain, keys, and relationship/cardinality before joining |
| Dashboard numbers must match Gold                   | Incorrect KPI values may affect validation          | Recheck important dashboard values against Gold                 |
| Power BI should not use raw, Bronze, or Silver data | Could violate the Week 08 Gold-only requirement     | Verify all Power BI sources before final submission             |

The Week 08 guidance warns against unsafe relationships between different Gold grains and requires dashboard values to be reconciled with the owning Gold table.

---

## 5. Evidence Added to GitHub

* `notebooks/06_powerbi_export.ipynb`
* `dashboard/powerbi_dashboard.pbix`
* `dashboard/README.md`
* `screenshots/week08_*.png`
* `weekly_logs/week08_log.md`
* Small Gold export files used by Power BI, where permitted

The Week 08 repository guide lists the export notebook, PBIX file, README, screenshots, and weekly log as the main evidence locations.

---

## 6. AI Transparency Note

| Question                            | Response                                                                                                                                                                      |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Where AI helped                     | AI helped us understand the Power BI workflow, organize the Week 08 tasks, and understand how to connect approved Gold outputs to the dashboard.                              |
| What we changed after AI suggestion | We adapted the suggestions to our FitPulse project, Gold tables, business questions, and team requirements instead of directly copying the sample project.                    |
| What we verified manually           | We manually checked the Gold tables, column names, data types, dashboard source, visuals, filters, and important dashboard values against the Gold outputs.                   |
| What we can explain without AI      | All team members can explain the Gold-to-Power-BI flow, why Power BI uses Gold only, the purpose of the dashboard visuals, and how dashboard values are reconciled with Gold. |

The project requires every weekly log to clearly state where AI helped, what the team changed, what was verified manually, and what the team can explain without AI.

---

## 7. Next Week Preparation

* Continue using the same Power BI dashboard created in Week 08.
* Improve the dashboard layout and visual presentation.
* Add useful filters and improve dashboard usability.
* Prepare insight notes based on the approved Gold metrics.
* Check that all dashboard visuals continue to use the correct Gold sources.
* Prepare the refined dashboard for Week 09.

Week 09 is intended for dashboard refinement, filters, visual cleanup, and insight storytelling while continuing with the same PBIX created in Week 08.
