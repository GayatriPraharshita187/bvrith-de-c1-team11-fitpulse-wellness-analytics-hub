# Data Dictionary

**Week:** 2  
**Purpose:** Define raw, reference, Silver, and streaming fields.

---

## 1. Source File Catalog

| File Name | Grain | Purpose | Approx. Rows | Notes |
|---|---|---:|---:|---|
| `users_sample.csv` | One row per user | Stores user profile information | 100 | Synthetic user data |
| `workout_sample.csv` | One row per workout | Stores workout activity details | 500 | Synthetic workout records |
| `activity_reference.csv` | One row per activity type | Reference data for workout categories | 10 | Lookup table |
| `streaming_events.json` | One row per event | Simulates real-time fitness events | 1000 | JSON event records |

---

## 2. Raw File Schema: `users_sample.csv`

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| `user_id` | string | Yes | `USR-0001` | Unique user ID |
| `name` | string | Yes | `Rahul Kumar` | User name |
| `age` | integer | Yes | `24` | User age |
| `gender` | string | Yes | `Female` | User gender |
| `height_cm` | decimal | Yes | `165` | Height in centimeters |
| `weight_kg` | decimal | Yes | `60.5` | Weight in kilograms |
| `city` | string | No | `Hyderabad` | User city |

---

## 3. Raw File Schema: `workout_sample.csv`

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| `workout_id` | string | Yes | `WRK-0001` | Unique workout ID |
| `user_id` | string | Yes | `USR-0001` | User reference |
| `workout_date` | date | Yes | `2026-07-20` | Workout date |
| `activity_type` | string | Yes | `Running` | Workout type |
| `duration_minutes` | integer | Yes | `45` | Workout duration |
| `calories_burned` | integer | Yes | `350` | Calories burned |
| `steps` | integer | No | `7200` | Total steps |

---

## 4. Reference File Schema: `activity_reference.csv`

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| `activity_id` | string | Yes | `ACT-001` | Activity ID |
| `activity_name` | string | Yes | `Walking` | Activity name |
| `category` | string | Yes | `Cardio` | Activity category |
| `intensity_level` | string | Yes | `Moderate` | Exercise intensity |

---

## 5. Canonical Silver Table Design

Final Silver table name:

```text
silver_user_workouts
```

| Silver Field | Data Type | Source Mapping | Business Meaning |
|---|---|---|---|
| `record_id` | string | `workout_id` | Unique analytics record |
| `user_id` | string | `user_id` | User identifier |
| `event_date` | date | `workout_date` | Date used for analytics |
| `activity_type` | string | `activity_type` | Workout performed |
| `duration_minutes` | integer | `duration_minutes` | Workout duration |
| `calories_burned` | integer | `calories_burned` | Calories burned |
| `steps` | integer | `steps` | Steps completed |
| `city` | string | `users.city` | User location |

---

## 6. Streaming Event Schema

| Field Name | Data Type | Required? | Example | Description |
|---|---|---|---|---|
| `event_id` | string | Yes | `EVT-0001` | Unique event ID |
| `event_timestamp` | timestamp | Yes | `2026-07-20T10:15:00+05:30` | Event timestamp |
| `user_id` | string | Yes | `USR-0001` | User identifier |
| `event_type` | string | Yes | `Workout Started` | Event category |
| `device_id` | string | Yes | `FIT-001` | Fitness device ID |
| `heart_rate` | integer | No | `92` | Heart rate (BPM) |
| `steps` | integer | No | `120` | Steps recorded |
| `calories` | decimal | No | `8.5` | Calories burned during the event |
