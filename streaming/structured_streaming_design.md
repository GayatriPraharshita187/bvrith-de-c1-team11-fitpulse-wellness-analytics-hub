# Structured Streaming Design

**Week:** 10
**Purpose:** Explain the FitPulse streaming simulation.

---

## 1. Streaming Scenario

The FitPulse streaming simulation demonstrates two ingestion workflows.

**Demo 1: Auto Loader file ingestion**

Synthetic workout events are stored in JSON files in a managed Databricks volume. Auto Loader detects and processes the files incrementally, writing the events to a Bronze table.

**Demo 2: Live workout-event simulation**

A Python simulator appends synthetic workout events to a source Delta table. Structured Streaming reads the source table and appends available events to a Streaming Bronze table. A persistent checkpoint tracks processing progress so newly appended events can be processed incrementally.

The live-event test uses the `AvailableNow` trigger, which processes available data and then terminates. It does not currently demonstrate a continuously running pipeline.

---

## 2. Event Source

### Demo 1: Auto Loader

| Item              | Description                                                                              |
| ----------------- | ---------------------------------------------------------------------------------------- |
| Event file format | JSON                                                                                     |
| Input path        | `/Volumes/fitpulse-wellness/default/fitpulsehub/week10_streaming/drops/`                 |
| Processing method | Databricks Auto Loader                                                                   |
| Output table      | `fitpulse-wellness.default.bronze_workout_event_files`                                   |
| Checkpoint path   | `/Volumes/fitpulse-wellness/default/fitpulsehub/week10_streaming/checkpoints/autoloader` |
| Schema location   | `/Volumes/fitpulse-wellness/default/fitpulsehub/week10_streaming/schemas/autoloader`     |

**Observed validation:** 7 rows, 7 distinct event IDs, 4 source files, and 0 rescued rows.

### Demo 2: Live workout events

| Item              | Description                                                                               |
| ----------------- | ----------------------------------------------------------------------------------------- |
| Event format      | Synthetic workout events in a Delta table                                                 |
| Source table      | `fitpulse-wellness.default.fitpulse_live_events`                                          |
| Processing method | Structured Streaming with `AvailableNow`                                                  |
| Output table      | `fitpulse-wellness.default.bronze_fitpulse_live_events`                                   |
| Checkpoint path   | `/Volumes/fitpulse-wellness/default/fitpulsehub/week10_streaming/checkpoints/live_bronze` |

**Observed validation:** 7 source events, 7 Streaming Bronze events, and 7 distinct target event IDs.

---

## 3. Near-Real-Time Metric

| Metric                       | Formula                                                                  | Use                                                                   |
| ---------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| Ingested workout-event count | Count of rows in `fitpulse-wellness.default.bronze_fitpulse_live_events` | Shows how many simulated workout events have reached Streaming Bronze |

The metric can be checked by querying the target table:

```sql
SELECT COUNT(*) AS total_events
FROM `fitpulse-wellness`.default.bronze_fitpulse_live_events;
```

During the test, the target count reached 7 and matched the source count.

**Timing limitation:** Because the tested query uses `AvailableNow`, the metric was validated after finite processing runs. Continuous live updates have not been demonstrated.

---

## 4. Limitations

* This is an educational streaming simulation, not a production event platform.
* Kafka is documented for production architecture awareness only; Kafka infrastructure has not been implemented.
* Streaming events are synthetic and intended for learning and validation.
* The current compute rejected the processing-time trigger with `INFINITE_STREAMING_TRIGGER_NOT_SUPPORTED`.
* The live-event demonstration therefore uses `AvailableNow`, which processes available data and terminates.
* A continuously running SQL pipeline using `FROM STREAM(...)` remains pending and has not been successfully verified.
* An attempted ETL pipeline configuration reported insufficient permission to create schemas in the `fitpulse-wellness` catalog.
