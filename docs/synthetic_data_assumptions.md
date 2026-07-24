

**Week:** 2  
**Purpose:** Document how the synthetic data used in the FitPulse Wellness Analytics project is created.

---

## 1. Synthetic Data Boundary

This project uses **synthetic wellness and fitness data only**. It must not be presented as real user, patient, customer, company, or healthcare data. All records are generated solely for educational and demonstration purposes.

---

## 2. Domain Assumptions

| Area | Assumption |
|---|---|
| Geography / Scope | Hyderabad, Telangana (synthetic wellness data) |
| Time Period | July 2026 – September 2026 |
| Source Systems | Fitness tracker data, workout logs, nutrition records |
| Event Types | Workout, calories burned, heart rate, sleep tracking |
| Reference Data | Exercise types, workout categories, user profiles |

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
| Duplicate IDs | 0.2%–0.5% | Tests uniqueness constraints |
| Missing values | 1%–3% | Tests data completeness |
| Invalid reference keys | 0.5%–1% | Tests referential integrity |
| Negative / impossible values | 0.1%–0.5% | Tests validation and range checks |
| Timestamp inconsistencies | 0.1%–0.3% | Tests chronological ordering |

---

## 5. Manual Verification

Before using the generated data, the team must verify:

- Row counts are reasonable.
- Required key fields are present.
- Dates and numeric values fall within realistic ranges.
- Controlled data quality issues exist but do not dominate the dataset.
- Source files contain enough variation to demonstrate data cleaning and standardization.
- File names and formats match the project repository structure.

---

## 6. AI Transparency Note

- AI was used to help structure the documentation and suggest realistic synthetic data assumptions.
- The team reviewed and modified the content before uploading it to GitHub.
- All assumptions and file details were manually verified by the team.
```
