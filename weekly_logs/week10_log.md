# Week 10 Log — Streaming Simulation and Event Ingestion

**Week:** 10
**Date:** 03 October 2026
**Team:** Team 11
**Project:** FitPulse Wellness Analytics Hub

## 1. Sprint Goal

The goal for Week 10 was to implement and validate streaming data ingestion using Databricks Auto Loader and Structured Streaming. The team also worked on defining the FitPulse workout-event schema, documenting the streaming architecture, validating event counts, and recording implementation limitations.

## 2. Work Completed

| Task                                                                                                         | Owner                        | Status                  | Evidence                                                                                               |
| ------------------------------------------------------------------------------------------------------------ | ---------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------ |
| Set up Auto Loader JSON file-drop simulation and ingest events into the Bronze table                         | Kanumuri Gayatri Praharshita | Completed               | `notebooks/07_streaming_simulation.ipynb`; `bronze_workout_event_files`                                |
| Validate Auto Loader ingestion using row counts, distinct event IDs, source files, and rescued rows          | Kanumuri Gayatri Praharshita | Completed               | 7 rows, 7 distinct event IDs, 4 source files, 0 rescued rows                                           |
| Create and populate the live workout-event source table with simulated events                                | Kanumuri Gayatri Praharshita | Completed               | `fitpulse_live_events`; 7 events                                                                       |
| Process live events into the Streaming Bronze table using Structured Streaming with the AvailableNow trigger | Kanumuri Gayatri Praharshita | Completed               | `bronze_fitpulse_live_events`; source and target counts both 7                                         |
| Define and update the FitPulse workout-event schema                                                          | Kanumuri Gayatri Praharshita | Updated; review pending | `streaming/kafka_event_schema.json`                                                                    |
| Review the event schema and verify that its fields match the project event data                              | Cherka Cherishma             | Pending confirmation    | `streaming/kafka_event_schema.json`                                                                    |
| Review the streaming design and verify the documented input paths, output tables, and checkpoint paths       | Dharavath Sandhya            | Pending confirmation    | `streaming/structured_streaming_design.md`                                                             |
| Document the streaming workflow, validation results, and implementation limitations                          | Kanumuri Gayatri Praharshita | In progress             | `streaming/structured_streaming_design.md`                                                             |
| Prepare the Week 10 sprint log, including decisions, blockers, and AI transparency                           | Cherka Cherishma             | In progress             | `weekly_logs/week10_log.md`                                                                            |
| Review the sprint log and confirm team details and task statuses                                             | Dharavath Sandhya            | Pending confirmation    | `weekly_logs/week10_log.md`                                                                            |
| Configure and verify the continuous SQL streaming pipeline using `FROM STREAM(...)`                          | Kanumuri Gayatri Praharshita | Blocked                 | Databricks catalog/schema permission issue                                                             |
| Capture screenshots of Auto Loader and live-event reconciliation                                             | Dharavath Sandhya            | Pending                 | `screenshots/week10_autoloader_reconciliation.png`; `screenshots/week10_live_event_reconciliation.png` |
| Review the evidence and confirm that the required Week 10 files are committed to GitHub                      | Cherka Cherishma             | Pending                 | `notebooks/07_streaming_simulation.ipynb`; `streaming/`; `screenshots/`; `weekly_logs/`                |

## 3. Key Decisions

* Used Databricks Auto Loader to ingest controlled JSON file drops incrementally into the Bronze table.
* Used synthetic workout events to simulate live event ingestion without requiring Kafka infrastructure.
* Used the `AvailableNow` trigger for Structured Streaming because the current compute environment did not support the `processingTime` trigger.
* Used persistent checkpoint locations to support incremental processing and track processed data.
* Defined a FitPulse-specific workout-event schema in `streaming/kafka_event_schema.json`.
* Kept Kafka as a documented architectural option rather than implementing Kafka infrastructure.
* Recorded the continuous SQL streaming pipeline as pending because the pipeline setup encountered a catalog/schema permission issue.

## 4. Blockers / Risks

| Blocker                                                                                         | Impact                                                                                                 | Help Needed                                                                    |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| The current Databricks compute does not support the `processingTime` trigger                    | Continuous processing at a fixed interval could not be demonstrated using the original trigger         | Confirm a supported compute configuration or use an approved alternative       |
| Pipeline creation encountered a permission error when selecting the `fitpulse-wellness` catalog | The continuous SQL pipeline using `FROM STREAM(...)` could not be successfully configured and verified | Obtain the required permissions or assistance from the workspace administrator |
| Required screenshots and GitHub evidence still need to be checked                               | The repository may not yet contain all required Week 10 evidence                                       | Capture, review, and commit the actual screenshots and notebook                |
| Team review of documentation and individual task assignments is pending                         | The sprint log may not yet reflect the final agreed division of work                                   | Confirm ownership and completion status with all team members                  |

## 5. Evidence Added to GitHub

* `streaming/kafka_event_schema.json` — updated FitPulse workout-event schema.
* `streaming/structured_streaming_design.md` — streaming design documentation, including ingestion paths, output tables, checkpoints, metrics, and limitations.
* `notebooks/07_streaming_simulation.ipynb` — required notebook for the streaming simulation.
* `screenshots/week10_autoloader_reconciliation.png` — planned Auto Loader validation evidence; add after capturing the screenshot.
* `screenshots/week10_live_event_reconciliation.png` — planned live-event validation evidence; add after capturing the screenshot.
* `weekly_logs/week10_log.md` — Week 10 sprint log.

**Evidence note:** The Auto Loader and live-event count validations were verified in Databricks. Confirm that the notebook, design document, log, and screenshots are actually committed to GitHub before marking this entire section complete.

## 6. AI Transparency Note

| Question                             | Response                                                                                                                                                                                                                                                                                                                |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Where AI helped                      | AI assisted with structuring the streaming documentation, defining the event schema, explaining Auto Loader and Structured Streaming, preparing validation queries, and drafting the sprint log.                                                                                                                        |
| What we changed after AI suggestions | The implementation used the actual FitPulse table names, volume paths, event fields, checkpoint locations, and observed validation results rather than generic sample values. The streaming approach was adjusted to use the `AvailableNow` trigger after the original trigger was rejected by the compute environment. |
| What we verified manually            | The Auto Loader output contained 7 rows, 7 distinct event IDs, 4 source files, and 0 rescued rows. The live-event source contained 7 events, and the Streaming Bronze target contained 7 events with 7 distinct event IDs. A source-versus-target count comparison also returned 7 for each table.                      |
| What we can explain without AI       | The team can explain the purpose of Auto Loader, the role of checkpoints in tracking processed data, the source-to-Bronze ingestion flow, the event schema, the count reconciliation checks, and why the continuous SQL pipeline remains pending.                                                                       |
