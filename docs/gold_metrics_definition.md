# FitPulse Gold Metrics Definition

**Week:** 7
**Trusted inputs:** Validated Silver tables
**Built output:** FitPulse Gold summary tables
**Boundary:** Gold design and build; no Power BI implementation

---

## 1. Header and Scope

The Gold layer converts validated Silver data into analytics-ready summary tables for the FitPulse Wellness Analytics Hub.

Each Gold table is defined by an explicit grain and contains measures appropriate to that grain. The Gold layer is designed to support downstream Power BI reporting without requiring unsafe joins between unrelated aggregate tables.

The Gold layer implemented in Week 7 contains eight Gold tables:

1. `gold_activity_summary`
2. `gold_user_activity_summary`
3. `gold_goal_progress_summary`
4. `gold_goal_segment_summary`
5. `gold_device_usage_summary`
6. `gold_device_health_summary`
7. `gold_device_platform_summary`
8. `gold_daily_wellness_summary`

---

## 2. Gold Table Catalog

| **Table**                      | **Grain**                                 | **Source / Inputs**          | **Purpose**                           | **Status** |
| ------------------------------ | ----------------------------------------- | ---------------------------- | ------------------------------------- | ---------- |
| `gold_activity_summary`        | Workout date + activity type              | Silver workout data          | Daily activity performance            | Built      |
| `gold_user_activity_summary`   | User                                      | Silver workout data          | User-level activity and engagement    | Built      |
| `gold_goal_progress_summary`   | Goal                                      | Silver goals + workout data  | Goal progress and completion analysis | Built      |
| `gold_goal_segment_summary`    | Goal type + target unit + progress status | `gold_goal_progress_summary` | Aggregated goal performance           | Built      |
| `gold_device_usage_summary`    | Device type + platform + device status    | Silver device/workout data   | Device usage and status analysis      | Built      |
| `gold_device_health_summary`   | Device                                    | Silver device/workout data   | Device health and lifetime metrics    | Built      |
| `gold_device_platform_summary` | Platform + device type                    | Silver device/workout data   | Device ecosystem distribution         | Built      |
| `gold_daily_wellness_summary`  | Workout date                              | Silver workout data          | Daily wellness trends                 | Built      |

---

## 3. KPI Definition Register

### 3.1 Activity Summary

**Output table:** `gold_activity_summary`

**Grain:** One row per `workout_date` and `activity_type`.

**Business purpose:** Provide daily activity-level metrics for analytics and Power BI reporting.

**Measures:**

* **Total Workouts** = `COUNT(DISTINCT workout_id)`
* **Active Users** = `COUNT(DISTINCT user_id)`
* **Total Duration** = `SUM(duration_minutes)`
* **Total Calories Burned** = `SUM(calories_burned)`
* **Total Distance** = `SUM(distance_km)`
* **Total Repetitions** = `SUM(repetitions)`

**Validation:** Duplicate-key check on `workout_date + activity_type`.

---

### 3.2 User Activity Summary

**Output table:** `gold_user_activity_summary`

**Grain:** One row per `user_id`.

**Business purpose:** Provide user-level activity and engagement measures.

**Measures:**

* **Total Workouts** = `COUNT(DISTINCT workout_id)`
* **Total Active Days** = `COUNT(DISTINCT workout date)`
* **Total Duration** = `SUM(duration_minutes)`
* **Total Calories Burned** = `SUM(calories_burned)`
* **Total Distance** = `SUM(distance_km)`
* **Total Repetitions** = `SUM(repetitions)`

**Validation:** Duplicate-key check on `user_id`.

---

### 3.3 Goal Progress Summary

**Output table:** `gold_goal_progress_summary`

**Grain:** Intended as one row per `goal_id`.

**Business purpose:** Measure progress toward individual user goals.

**Measures:**

* **Actual Progress** = workout metric accumulated during the goal period
* **Progress Percentage** = `(Actual Progress / Target Value) × 100`
* **Remaining Value** = `MAX(Target Value - Actual Progress, 0)`

**Goal metric mapping:**

| **Goal Type / Unit** | **Actual Progress Calculation** |
| -------------------- | ------------------------------- |
| `CALORIES`           | `SUM(calories_burned)`          |
| `DISTANCE_KM`        | `SUM(distance_km)`              |
| `DURATION_MINUTES`   | `SUM(duration_minutes)`         |
| `WORKOUT_COUNT`      | `COUNT(DISTINCT workout_id)`    |

**Status classification:**

* **COMPLETED:** Progress Percentage >= 100%
* **AT_RISK:** Goal is not completed and progress is below 50%
* **IN_PROGRESS:** Remaining applicable goal is neither completed nor at risk

Invalid or unsupported goal units are excluded from KPI calculations.

**Validation:** Duplicate `goal_id` check and comparison of duplicated records across target value, goal type, dates, status and activity scope.

**Validation finding:** The built table contains 17,925 rows and 17,825 unique goals, with 100 duplicated goal IDs. Therefore, `goal_id` should not yet be assumed to be a unique downstream business key.

---

### 3.4 Goal Segment Summary

**Output table:** `gold_goal_segment_summary`

**Grain:** Goal type + target unit + progress status.

**Business purpose:** Provide aggregated goal performance for segmentation and reporting.

**Measures:**

* **Total Goals** = `COUNT(DISTINCT goal_id)`
* **Average Progress Percentage** = `AVG(progress_percentage)`
* **Completed Goals** = count of `COMPLETED` goals
* **At-Risk Goals** = count of `AT_RISK` goals
* **In-Progress Goals** = count of `IN_PROGRESS` goals
* **Average Remaining Value** = `AVG(remaining_value)`

**Validation:** Duplicate-key check on `goal_type + target_unit + progress_status`.

---

### 3.5 Device Usage Summary

**Output table:** `gold_device_usage_summary`

**Grain:** Device type + platform + device status.

**Business purpose:** Analyze device usage and status distribution.

**Measures:**

* **Total Devices**
* **Total Users**
* **Active Devices**
* **Inactive Devices**

**Validation:** Duplicate-key check on `device_type + platform + device_status`.

---

### 3.6 Device Health Summary

**Output table:** `gold_device_health_summary`

**Grain:** One row per `device_id`.

**Business purpose:** Provide device-level health, usage and lifetime information.

**Measures:**

* **Total Workouts**
* **Total Users**
* **Device Lifetime in Days**

**Validation:** Duplicate-key check on `device_id`.

---

### 3.7 Device Platform Summary

**Output table:** `gold_device_platform_summary`

**Grain:** Platform + device type.

**Business purpose:** Provide device ecosystem distribution and user coverage.

**Measures:**

* **Total Devices**
* **Total Users**
* **Active Devices**
* **Inactive Devices**

**Validation:** Duplicate-key check on `platform + device_type`.

**Scope note:** App-version analysis was not implemented because app-version data is not available in the current Silver/Device schema.

---

### 3.8 Daily Wellness Summary

**Output table:** `gold_daily_wellness_summary`

**Grain:** One row per `wellness_date`.

**Business purpose:** Provide daily wellness and workout trend metrics.

**Measures:**

* **Total Workouts** = `COUNT(DISTINCT workout_id)`
* **Active Users** = `COUNT(DISTINCT user_id)`
* **Total Duration** = `SUM(duration_minutes)`
* **Total Calories Burned** = `SUM(calories_burned)`
* **Total Distance** = `SUM(distance_km)`
* **Total Repetitions** = `SUM(repetitions)`
* **Average Workout Duration** = `AVG(workout duration)`
* **Average Calories per Workout** = `AVG(calories per workout)`

**Validation:** Duplicate-key check on `wellness_date`.

---

## 4. Cross-Table Validation

The Gold build included validation checks across the Gold outputs.

### Goal-to-User Coverage

A cross-check between `gold_goal_progress_summary` and `gold_user_activity_summary` identified **2 users present in goal progress but absent from the user activity summary**.

These records were retained for investigation.

### Goal Key Validation

The Gold goal progress table contains **100 duplicated `goal_id` values**.

This finding must be considered before using `goal_id` as a guaranteed unique key in downstream Power BI relationships or calculations.

### Grain Principle

Gold tables are maintained at separate business grains. Metrics such as distinct users should not be summed across incompatible aggregate grains because this can introduce double counting.

---

## 5. Validation Method

The Gold implementation was validated through:

* Gold table row-count checks
* Duplicate business-key checks
* Output schema inspection
* Sample-row inspection
* Goal-level duplicate investigation
* Cross-table user coverage checks
* Device-level and platform-level consistency checks

Notebook execution evidence is maintained separately from this design document.

---

## 6. Power BI Boundary

This document defines the Gold-layer metrics and table contracts only.

Power BI implementation, relationships, visual design, slicers and dashboard calculations are outside the scope of this Week 7 Gold Metrics Definition document.
