# Synthetic Data Assumptions

**Week:** 2  
**Purpose:** Document how educational data is created.

---

## 1. Synthetic Data Boundary

This project uses synthetic wellness and fitness data only. It must not be presented as real user, patient, customer, company, or healthcare data.

---

## 2. Domain Assumptions

| Area | Assumption |
|---|---|
| Geography / scope | Hyderabad, Telangana (synthetic wellness data) |
| Time period | July 2026 – September 2026 |
| Source systems | Fitness tracker data, workout logs, nutrition records |
| Event types | Workout, calories burned, heart rate, sleep tracking |
| Reference data | Exercise types, workout categories, user profiles |

---

## 3. Data Volume Assumptions

| File | Approximate Rows | Reason |
|---|---:|---|
| `users.json` | 100 | Sample user records |
| `workouts.parquet` | 500 | Workout history |
| `goals.csv` | 100 | User fitness goals |
| `devices.csv` | 100 | User device information |
| `activity_types.csv` | 20 | Activity reference data |
| `workout_event.json` | 300 | Streaming workout events |

---

## 4. Controlled Data Quality Issues

| Issue Type | Approx. Share | Why Include It |
|---|---:|---|
| Duplicate IDs | 0.2%–0.5% | Tests uniqueness |
| Missing values | 1%–3% | Tests completeness |
| Invalid reference keys | 0.5%–1% | Tests referential integrity |
| Negative / impossible values | 0.1%–0.5% | Tests range rules |
| Timestamp inconsistencies | 0.1%–0.3% | Tests chronology |

---

## 5. Manual Verification

Before using generated data, the team must check:

- Row counts are reasonable.
- Key fields exist.
- Dates and numeric values look realistic.
- Controlled defects exist but do not dominate the dataset.
- Source files are different enough to require real standardization.
