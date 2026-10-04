# Dashboard Insights

**Project:** FitPulse Wellness  
**Week:** 10  
**Purpose:** Document the dashboard structure, KPI definitions, data sources, validation approach, and evidence-backed insights from the FitPulse Wellness Power BI dashboard.

---

## 1. Dashboard Pages

| Dashboard Page | Purpose |
|---|---|
| Wellness Overview | Present a high-level view of wellness activity and daily wellness metrics. |
| Activity Analysis | Analyze workout activity, workout types, duration, and calories burned. |
| User Activity | Examine user-level activity metrics and participation patterns. |
| Goal Progress | Monitor progress toward wellness goals using the available goal-progress data. |
| Device Analytics | Review device usage, device health, and platform-level distributions. |
| Live Events | Monitor generated workout events, event counts, activity types, and recent event details. |

*Note: These are proposed functional page descriptions based on the available FitPulse datasets and dashboard work. Use the names that actually appear in the final PBIX file.*

---

## 2. Key Performance Indicators (KPIs)

| KPI | Description | Owning Gold Table |
|---|---|---|
| Total Workouts | Total workout records represented by the selected dataset and filter context. | `gold_user_activity_summary` or a suitable detailed workout table |
| Total Workout Duration | Aggregate workout duration for the selected scope, where the underlying fields support summation. | `gold_activity_summary` |
| Calories Burned | Aggregate calories burned for the selected activity scope. | `gold_activity_summary` |
| Daily Wellness Metrics | Summary of available daily wellness measures. | `gold_daily_wellness_summary` |
| Goal Progress | Summary of progress toward wellness goals, interpreted according to the available goal-progress fields. | `gold_goal_progress_summary` |
| Device Count | Number of devices represented in the selected device dataset. | `gold_device_health_summary` |
| Device Distribution | Distribution of devices by platform and device type. | `gold_device_platform_summary` |
| Total Live Events | Number of generated live events currently loaded into the Power BI model. | `fitpulse_live_events` |
| Unique Live Events | Number of distinct live event identifiers. | `fitpulse_live_events` |
| Latest Event Timestamp | Latest event timestamp in the loaded live-events dataset. | `fitpulse_live_events` |

**KPI definition note:** Confirm the precise aggregation rules and available fields before final submission. In particular, avoid summing pre-aggregated averages or combining summary tables in ways that double-count records.

---

## 3. Dashboard Visualizations

| Visualization | Purpose |
|---|---|
| KPI Cards | Display key wellness, activity, goal, device, and live-event metrics. |
| Column Chart | Compare workout activity across activity types. |
| Line Chart | Show changes in activity or wellness metrics over time, where the chosen Gold table supports a meaningful date series. |
| Goal Progress Visual | Compare goal progress using the available goal-progress measures. |
| Device Distribution Chart | Compare devices across device types and platforms. |
| Device Health Summary | Review device health and status information. |
| Live Event Activity Chart | Show live-event counts by activity type. |
| Recent Events Table | Display recent event details, including event ID, activity type, user ID, duration, calories burned, and event timestamp. |
| Slicers and Filters | Allow users to explore the supported date, activity, device, platform, and other relevant dimensions. |

Visuals should be included only where their fields and aggregation logic are supported by the actual data model.

---

## 4. Approved Gold Sources

The FitPulse Wellness dashboard uses the following Gold-layer tables created in Databricks.

| Gold Table | Purpose |
|---|---|
| `gold_activity_summary` | Summarize workout activity and associated workout metrics. |
| `gold_daily_wellness_summary` | Provide daily wellness summaries and associated metrics. |
| `gold_device_health_summary` | Summarize device health, device attributes, and associated usage information. |
| `gold_device_platform_summary` | Summarize device distributions by platform and device type. |
| `gold_device_usage_summary` | Summarize device usage across the available dimensions. |
| `gold_goal_progress_summary` | Provide goal-progress information for analysis. |
| `gold_goal_segment_summary` | Summarize goal information across the available goal segments. |
| `gold_user_activity_summary` | Summarize user-level activity metrics. |

### Live Events Source

The live-events page uses the existing Databricks source table:

`fitpulse-wellness.default.fitpulse_live_events`

The Power BI query was changed to read this source directly rather than the stale pipeline-managed target, `bronze_fitpulse_live_events_pipeline`. The source table receives new records from the existing `fitpulse_live_event_generator` job.

This change allows Power BI to load the events generated by the existing job without relying on the paused ETL pipeline to refresh the streaming-table target.

---

## 5. Evidence-Based Insights

### Insight 1 — Live Event Generation

- **Question:** Is the live-event generator producing records that can be consumed by Power BI?
- **Observation:** After the Power BI query was redirected to `fitpulse_live_events`, the displayed event count increased from 28 to 353.
- **Filter/Time Scope:** The dataset loaded during the successful Power BI refresh.
- **Visual/Page:** Live Events page.
- **Owning Source:** `fitpulse-wellness.default.fitpulse_live_events`.
- **Measure/Field:** Count of `event_id`.
- **Evidence:** The Power BI event count changed from 28 to 353 after the source query was updated.
- **Interpretation:** The source table contains a larger set of generated events than the previous pipeline-managed target contained at the time of the comparison.
- **Limitation:** This validates the source change and data load at that point in time. It does not establish continuous Power BI refresh or prove that every generated event is displayed immediately.

### Insight 2 — Live Event Uniqueness

- **Question:** Are live event identifiers unique in the loaded dataset?
- **Observation:** The source-table validation previously returned matching total-event and distinct-event counts of 248.
- **Filter/Time Scope:** Source-table snapshot at the time of SQL validation.
- **Owning Source:** `fitpulse-wellness.default.fitpulse_live_events`.
- **Measure/Field:** Total rows and distinct `event_id` values.
- **Evidence:** Both counts were 248 in the recorded validation.
- **Interpretation:** No duplicate event IDs were detected in that validated snapshot.
- **Limitation:** This result applies only to the validated snapshot. It should be rechecked if later events are generated.

### Insight 3 — Live Event Monitoring

- **Question:** Can the dashboard display event activity and recent event details from the generator?
- **Observation:** The Power BI query successfully loaded the source table, and the event count updated to 353.
- **Visual/Page:** Live Events page, event-count card, activity-type chart, and recent-events table, where present in the final report.
- **Owning Source:** `fitpulse-wellness.default.fitpulse_live_events`.
- **Measure/Fields:** `event_id`, `activity_type`, `user_id`, `duration_minutes`, `calories_burned`, and `event_timestamp`.
- **Interpretation:** The source connection supports reporting on the generated event records and their available attributes.
- **Limitation:** Power BI currently uses Import mode, so newly generated events appear only after the model is refreshed. The generator's one-minute schedule does not itself refresh the imported Power BI data.

### Additional Gold-Layer Insights

Additional business insights about workout activity, daily wellness, goal progress, and device usage should be documented after checking the actual Power BI visual values against their owning Gold tables. No numerical findings or causal explanations are asserted here because the corresponding validated dashboard values have not been provided.

---

## 6. Dashboard Interaction Validation

The following checks should be completed against the final FitPulse Power BI report:

- Selecting an activity type filters only the visuals that are intended to respond to that selection.
- Selecting a date filters the relevant activity and wellness visuals where compatible date relationships exist.
- Selecting a device platform affects the intended device-analysis visuals.
- Selecting a goal segment filters compatible goal-progress visuals without introducing unintended cross-table filtering.
- Live-event activity selections update the related event visualizations where interactions are configured.
- Date, user, device, and goal relationships are validated to prevent accidental duplication or misleading totals.
- Interactions do not change the intended meaning of pre-aggregated Gold measures.

Document any interaction that is intentionally disabled or restricted.

---

## 7. Dashboard Validation

Before final submission, verify that:

- The dashboard uses the intended Gold-layer tables for analytical pages.
- The Live Events page uses `fitpulse-wellness.default.fitpulse_live_events`.
- KPI definitions match the actual aggregation and grain of the underlying tables.
- Counts, totals, and date-based metrics reconcile with the corresponding Databricks outputs.
- Pre-aggregated averages and distinct counts are not incorrectly summed.
- Relationships and filter directions behave as intended.
- Visual titles, axis labels, units, and date scopes are clear.
- Live-event counts and timestamps are checked after refreshing Power BI.
- The report's Import-mode refresh limitation is documented.
- Screenshots and validation evidence are saved for submission.
- The final PBIX file is saved at the project's designated dashboard path.

---

## 8. Limitations

- Dashboard findings are descriptive and do not establish causal relationships.
- KPI values depend on the active filters, date scope, and data loaded into Power BI.
- Gold summary tables may have different grains and should not be joined or aggregated indiscriminately.
- A distinct count of users or events must not be inferred by summing counts from separate aggregate records.
- The live-event generator produces source records independently of Power BI refresh.
- The Power BI report uses Import mode; therefore, newly generated events are not automatically reflected in the report immediately.
- The pipeline-managed streaming target was stale during the earlier validation, so the Live Events page was redirected to the source table.
- Any unresolved difference between Power BI and Databricks outputs must be investigated and documented before final submission.

---

## 9. Week 10 Boundary

Week 10 includes the FitPulse streaming simulation, event generation, source-table validation, and integration of live-event data into Power BI.

The existing `fitpulse_live_event_generator` job writes generated events to `fitpulse-wellness.default.fitpulse_live_events`. The Power BI Live Events page reads this source table directly.

The pipeline-managed target, `bronze_fitpulse_live_events_pipeline`, remains a separate workload and was not used as the active Power BI source after its refresh became stale.

Further work on automatic Power BI refresh or near-real-time visual updates should be documented separately from the successful source-table integration. The generator job and the Power BI refresh mechanism are independent components and must be validated separately.
