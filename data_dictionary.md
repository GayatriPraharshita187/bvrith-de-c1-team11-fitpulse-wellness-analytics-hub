# Data Dictionary

**Week:** 2  
**Purpose:** Define raw, reference, Silver, and streaming fields.

---

## 1. Source File Catalog

| File Name | Grain | Purpose | Approx. Rows | Notes |
|-----------|-------|---------|--------------|-------|
| users.json | One row per user | Stores user profile information | Sample dataset | Raw user data |
| workouts.parquet | One row per workout | Stores workout records | Sample dataset | Raw workout data |
| goals.csv | One row per goal | Stores user fitness goals | Sample dataset | Raw goal data |
| devices.csv | One row per device | Stores wearable device information | Sample dataset | Reference data |
| activity_types.csv | One row per activity | Stores activity type lookup | Sample dataset | Reference data |
| workout_event.json | One row per event | Streaming workout events | Sample dataset | JSON event stream |

---

## 2. Raw File Schema: `users.json`

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| user_id | Integer | Yes | 1001 | Unique user ID |
| name | String | Yes | Rahul | User name |
| age | Integer | Yes | 25 | User age |
| gender | String | Yes | Male | User gender |
| height | Float | Yes | 172.5 | Height in cm |
| weight | Float | Yes | 68.5 | Weight in kg |

---

## 3. Raw File Schema: `workouts.parquet`

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| workout_id | Integer | Yes | 5001 | Unique workout ID |
| user_id | Integer | Yes | 1001 | User ID |
| activity_type | String | Yes | Running | Workout activity |
| duration_minutes | Integer | Yes | 45 | Workout duration |
| calories_burned | Integer | Yes | 420 | Calories burned |
| distance_km | Float | No | 6.5 | Distance covered |
| workout_date | Date | Yes | 2026-07-10 | Workout date |

---

## 4. Reference File Schema

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| activity_id | Integer | Yes | 1 | Unique activity ID |
| activity_name | String | Yes | Running | Name of activity |
| device_id | Integer | Yes | 101 | Unique device ID |
| device_name | String | Yes | Smart Watch | Wearable device |
| goal_id | Integer | Yes | 201 | Unique goal ID |
| goal_type | String | Yes | Weight Loss | Fitness goal |

---

## 5. Canonical Silver Table Design

Final Silver table name:

```text
silver_workout_summary
```

| Silver Field | Data Type | Source Mapping | Business Meaning |
|--------------|-----------|----------------|------------------|
| workout_id | Integer | workouts.workout_id | Unique workout record |
| user_id | Integer | users.user_id | User identifier |
| activity_name | String | activity_types.activity_name | Workout activity |
| duration_minutes | Integer | workouts.duration_minutes | Duration of workout |
| calories_burned | Integer | workouts.calories_burned | Calories burned |
| workout_date | Date | workouts.workout_date | Date of workout |

---

## 6. Streaming Event Schema

| Field Name | Data Type | Required? | Example | Description |
|------------|-----------|-----------|---------|-------------|
| event_id | String | Yes | EVT-1001 | Unique event ID |
| user_id | Integer | Yes | 1001 | User identifier |
| workout_id | Integer | Yes | 5001 | Workout identifier |
| event_timestamp | Timestamp | Yes | 2026-07-03T10:15:00+05:30 | Event time |
| event_type | String | Yes | Workout Started | Event category |
