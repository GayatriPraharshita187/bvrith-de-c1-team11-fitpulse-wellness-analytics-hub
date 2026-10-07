# Power BI Dashboard Folder

This folder contains the final Power BI dashboard deliverable for the **FitPulse Wellness Analytics Hub**.

The dashboard is built from approved **Gold-layer outputs** and is intended to provide the controlled reporting layer for the project.

## Dashboard Deliverable

The final Power BI file should be saved here:

```text
dashboard/finaldashboard
```

The Power BI dashboard uses the approved Gold outputs as its analytical source.

### Gold Tables Used

The Week-8 Power BI implementation used the following Gold tables:

* `gold_daily_wellness_summary`
* `gold_activity_summary`
* `gold_user_activity_summary`
* `gold_goal_progress_summary`

These tables support:

* Daily wellness and workout analysis
* Activity-type analysis
* User activity analysis
* Goal progress and status analysis

## Power BI Source Rule

Power BI must connect to **Gold outputs only**.

```text
Approved Gold Layer
        ↓
   Power BI Model
        ↓
   Dashboard Visuals
```

Dashboard visuals must not connect directly to:

* Raw source files
* Bronze tables
* Silver tables
* Streaming/raw event files

The documented Week-8 hand-off used the Databricks environment and Power BI connection to the approved Gold layer.

## Power BI Model

The recorded Week-8 model includes the following relationship:

```text
gold_user_activity_summary[user_id]
        ↓
gold_goal_progress_summary[user_id]
```

Relationship:

* Cardinality: Many-to-one (`*:1`)
* Cross-filter direction: Single
* Status: Active

Additional Week-8 relationships are **not confirmed / not implemented** in the available project record.

## Dashboard Scope

The initial Power BI dashboard covers the following analytical areas:

* Daily workout activity
* Activity-type analysis
* User activity
* Goal progress and status

The dashboard was established as the first working reporting layer from the approved Gold data.

Later dashboard refinement and visual redesign are treated as subsequent work and are not documented as part of the initial Week-8 implementation.

## Screenshots

Dashboard screenshots should be stored in:

```text
screenshots/
```

The recommended minimal evidence set is:

```text
screenshots/week08_powerbi_model.png
screenshots/week08_first_dashboard.png
screenshots/week08_reconciliation.png
```

The reconciliation screenshot should only be added after an actual Gold-to-Power-BI reconciliation has been performed.

## Dashboard Insights

Business insights derived from the dashboard should be documented separately in:

```text
docs/dashboard_insights.md
```

This keeps dashboard implementation documentation separate from analytical findings.

## Validation

A formal Power BI-to-Gold reconciliation is **not confirmed / not implemented** in the available Week-8 record.

Therefore, this folder does not claim specific reconciliation PASS results or matching values unless they are supported by captured project evidence.

## Power BI File-Size Rule

The preferred submission is:

```text
dashboard/powerbi_dashboard.pbix
```

along with the relevant screenshots and dashboard insight notes.

If the `.pbix` file becomes too large to manage cleanly in GitHub:

1. Keep the final dashboard screenshots in `screenshots/`.
2. Keep the dashboard insights in `docs/dashboard_insights.md`.
3. Add a short note to this README explaining where the PBIX is stored for mentor review.
4. Do not upload multiple heavy PBIX versions to GitHub.

Only the final dashboard version should be treated as the submission artifact.

## Repository Structure

```text
dashboard/
├── README.md
└── powerbi_dashboard.pbix

screenshots/
├── week08_powerbi_model.png
├── week08_first_dashboard.png
└── week08_reconciliation.png

docs/
└── dashboard_insights.md
```

> **Note:** Screenshot filenames above represent the recommended evidence structure. They should only be committed once the corresponding screenshots have actually been captured.
