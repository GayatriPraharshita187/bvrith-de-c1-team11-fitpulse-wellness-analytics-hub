# Data Quality Summary

**Week:** 6
**Purpose:** Summarize data quality rules, failures, routing decisions, and business impact.

---

## 1. Quality Rule Results

The Week 6 batch DQ process evaluates five Silver Candidate entities in dependency order: **Activity Types → Users → Devices → Goals → Workouts**.

Eight approved rule IDs exist in the Week 6 rulebook. `DQ-EVT-001` is deferred to Week 10 because it applies to streaming workout events.

| **Rule ID** | **Rule Name**                             |   **Severity** | **Failed Records** | **Business Impact**                                                                        |
| ----------- | ----------------------------------------- | -------------: | -----------------: | ------------------------------------------------------------------------------------------ |
| DQ-WRK-001  | Workout identity and duplicate validation |       Critical |              1,200 | Invalid or duplicated workout records can distort workout-level metrics                    |
| DQ-REF-001  | Reference and relationship validation     |       Critical |              1,455 | Invalid user, device, activity, or goal references can affect joins and downstream metrics |
| DQ-TIM-001  | Timestamp and active-period validation    |       Critical |                538 | Invalid chronology or dates outside valid periods can make time-based analysis unreliable  |
| DQ-DUR-001  | Workout duration validation               |          Major |              5,686 | Invalid duration values can distort workout-time and duration-based metrics                |
| DQ-CAL-001  | Measure and value validation              |          Major |              5,655 | Invalid calories, distance, or repetitions can distort activity and performance metrics    |
| DQ-STS-001  | Workout status consistency                |       Critical |              6,936 | Inconsistent completion/status information can make workout metrics unreliable             |
| DQ-GOL-001  | Goal validity and workout contribution    |          Major |                 0* | Invalid goal configuration or contribution rules can affect goal-progress metrics          |
| DQ-EVT-001  | Workout event validation                  | Critical/Major |      Not evaluated | Streaming event validation is outside the Week 6 batch scope and is deferred to Week 10    |

* `DQ-GOL-001` is part of the approved rulebook and workout contribution logic, but the Week 6 rule scorecard output shown in the notebook reports failures for the six rules above. The notebook also contains mentor-confirmation items for the goal type/unit compatibility and workout goal-contribution scope.

### Candidate → Trusted → Quarantine

| **Entity**     | **Candidate** | **Trusted** | **Quarantine** |
| -------------- | ------------: | ----------: | -------------: |
| Users          |         8,000 |       7,920 |             80 |
| Devices        |         5,000 |       4,924 |             76 |
| Goals          |        18,000 |      17,649 |            351 |
| Workouts       |        60,000 |      42,854 |         17,146 |
| Activity Types |             7 |           7 |            N/A |

For Users, Devices, Goals, and Workouts, reconciliation produced **variance = 0**, confirming:

`Candidate = Trusted + Quarantine`

Activity Types are reference data and are handled separately without a standalone quarantine table.

---

## 2. Failed Record Examples

The notebook confirms that a single physical record can contain multiple failed rule IDs. Failed records are retained in Quarantine rather than being silently discarded.

| **Rule ID** | **Sample Record ID** | **Failure Reason**                                                                 | **Action / Handling**           |
| ----------- | -------------------- | ---------------------------------------------------------------------------------- | ------------------------------- |
| DQ-REF-001  | `WRK0053686`         | Referenced `device_id` is null and the workout also has reference-related failures | Routed to `quarantine_workouts` |
| DQ-DUR-001  | `WRK0053686`         | Workout duration failed the duration validation                                    | Routed to `quarantine_workouts` |
| DQ-CAL-001  | `WRK0053686`         | Workout measure validation failed                                                  | Routed to `quarantine_workouts` |
| DQ-STS-001  | `WRK0053686`         | Workout status consistency validation failed                                       | Routed to `quarantine_workouts` |

The notebook shows that `WRK0053686` carries multiple failed rule IDs:

`DQ-REF-001, DQ-DUR-001, DQ-CAL-001, DQ-STS-001`

with `highest_severity = CRITICAL`.

This demonstrates that the pipeline **does not short-circuit after the first failure**. All applicable failures are retained on the same physical row.

---

## 3. What Should Block Gold Metrics?

The following failures should block affected records from being promoted into Trusted Silver and therefore prevent them from contributing to Gold metrics:

* **DQ-WRK-001:** Invalid or duplicate workout identity can create duplicate or unreliable workout metrics.
* **DQ-REF-001:** Broken references can produce invalid joins and incorrect user, device, activity, or goal analysis.
* **DQ-TIM-001:** Invalid timestamps or active-period violations can corrupt time-based metrics.
* **DQ-DUR-001:** Invalid workout duration can distort total and average workout-time metrics.
* **DQ-CAL-001:** Invalid calories, distance, or repetitions can distort activity-performance metrics.
* **DQ-STS-001:** Inconsistent workout status can cause invalid completed-workout metrics.
* **DQ-GOL-001:** Invalid goal configuration or contribution logic can affect goal-progress metrics.

`DQ-EVT-001` is not part of the Week 6 batch Gold blocking scope because workout-event validation is deferred to Week 10.

---

## 4. Quality Summary

The Week 6 DQ process successfully evaluated the five batch Candidate entities and routed failed physical rows to Quarantine rather than silently dropping them. Workouts had the largest number of quarantined records, with **17,146 of 60,000** records failing at least one applicable rule. The rule scorecard shows that **DQ-STS-001** had the highest failure count at **6,936**, followed by **DQ-DUR-001** with **5,686** and **DQ-CAL-001** with **5,655**. These failures are important for dashboards because status, duration, and measure errors can directly distort workout and activity metrics. Candidate-to-Trusted/Quarantine reconciliation produced **zero variance** for Users, Devices, Goals, and Workouts, and Trusted ∩ Quarantine was verified as empty. Multi-rule inspection also confirmed that one physical workout can retain several failed rule IDs. The main items requiring mentor review are the explicitly marked threshold/domain decisions, including duration tolerance and goal type/unit compatibility.
