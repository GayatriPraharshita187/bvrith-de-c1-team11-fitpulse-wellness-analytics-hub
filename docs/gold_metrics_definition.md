# Gold Metrics Definition

**Week:** 7  
**Purpose:** Define dashboard-ready Gold tables and KPI formulas.

---

## 1. Gold Table Catalog

| Gold Table Name | Grain | Source Table(s) | Purpose |
|---|---|---|---|
| `gold_activity_summary` | One row per workout date and activity type | `silver_workouts` | Daily activity metrics by activity type |
| `gold_user_activity_summary` | One row per user | `silver_workouts` | User-level activity performance |
| `gold_goal_progress_summary` | One row per goal | `silver_goals`, `silver_workouts` | Track goal progress against target values |
| `gold_goal_segment_summary` | One row per goal type, target unit and progress status | `gold_goal_progress_summary` | Aggregated goal performance |
| `gold_device_usage_summary` | One row per device type, platform and device status | `silver_devices` | Device usage and status distribution |
| `gold_device_health_summary` | One row per device | `silver_devices`, `silver_workouts` | Device usage and lifetime metrics |
| `gold_device_platform_summary` | One row per platform and device type | `silver_devices` | Device distribution and user coverage |
| `gold_daily_wellness_summary` | One row per workout date | `silver_workouts` | Daily wellness trend metrics |

---

## 2. KPI Definitions

| KPI Name | Formula | Grain | Dashboard Page | Notes |
|---|---|---|---|---|
| Total Workouts | COUNT(DISTINCT workout_id) | Date / Activity | Activity | Counts unique workout sessions |
| Active Users | COUNT(DISTINCT user_id) | Date / Activity | Activity | Counts unique users with workouts |
| Total Duration | SUM(duration_minutes) | Date / Activity | Activity | Total workout duration |
| Total Calories Burned | SUM(calories_burned) | Date / Activity | Activity | Total calories burned |
| Total Distance | SUM(distance_km) | Date / Activity | Activity | Total distance covered |
| Total Repetitions | SUM(repetitions) | Date / Activity | Activity | Total repetitions |
| Total Active Days | COUNT(DISTINCT workout date) | User | User Activity | Number of days with workouts |
| Progress Percentage | (Actual Progress / Target Value) × 100 | Goal | Goals | Measures goal completion |
| Remaining Value | MAX(Target Value - Actual Progress, 0) | Goal | Goals | Remaining amount required |
| Completed Goals | COUNT of goals where Actual Progress >= Target Value | Goal Type / Status | Goals | Goal has reached target |
| At-Risk Goals | COUNT of goals where Progress Percentage < 50% | Goal Type / Status | Goals | Goal progress is below 50% |
| Average Progress % | AVG(progress_percentage) | Goal Type / Status | Goals | Average goal progress |
| Total Devices | COUNT(DISTINCT device_id) | Device Type / Platform | Device Health | Counts unique devices |
| Total Device Users | COUNT(DISTINCT user_id) | Device Type / Platform | Device Health | Counts users associated with devices |
| Active Devices | COUNT of devices with ACTIVE status | Device Type / Platform | Device Health | Devices currently marked active |
| Inactive Devices | COUNT of devices with non-ACTIVE or NULL status | Device Type / Platform | Device Health | Devices not marked active |
| Device Lifetime Days | DATEDIFF(deactivated_date, activated_date) | Device | Device Health | Uses current date when device is still active |
| Average Workout Duration | AVG(duration_minutes) | Date | Wellness | Average duration per workout |
| Average Calories per Workout | AVG(calories_burned) | Date | Wellness | Average calories per workout |

---

## 3. Goal KPI Mapping

| Goal Type | Actual Progress Source |
|---|---|
| `CALORIES` | SUM(calories_burned) |
| `DISTANCE_KM` | SUM(distance_km) |
| `DURATION_MINUTES` | SUM(duration_minutes) |
| `WORKOUT_COUNT` | COUNT(DISTINCT workout_id) |

---

## 4. Validation Checks

Before using Gold tables in Power BI, verify:

- Gold row counts are reasonable.
- No unexpected nulls exist in key dashboard fields.
- KPI totals match manual spot checks.
- Each Gold table follows its documented metric grain.
- Gold tables are created successfully in Databricks.
- Power BI connects to Gold outputs only.
- Metric definitions are documented clearly.
- Gold outputs contain dashboard-ready aggregated metrics.

---

## 5. Data Availability Note

The current Silver device schema does not contain an app-version field.

Therefore, the device Gold metrics use the available platform, device type, device status and device usage fields instead of app-version mix.

---

## 6. Gold Layer Tables Created

The following eight Gold tables were created for Week 7:

1. `gold_activity_summary`
2. `gold_user_activity_summary`
3. `gold_goal_progress_summary`
4. `gold_goal_segment_summary`
5. `gold_device_usage_summary`
6. `gold_device_health_summary`
7. `gold_device_platform_summary`
8. `gold_daily_wellness_summary`
