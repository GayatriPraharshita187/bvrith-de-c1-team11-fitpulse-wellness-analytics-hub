# Week 05 Log — Silver Standardization

**Week:** 5

**Date range:** 4 August 2026 – 10 August 2026

**Students:**
- Charka Cherishma
- Kanumuri Gayatri Praharshita
- Dharavath Sandhya

**Team:** 11-DataStreamers

**Project:** FitPulse Wellness Analytics

---

## 1. Sprint Goal

Transform Bronze data into clean Silver tables with standard names, appropriate data types, required joins, and derived fields. The team will also document a raw-to-Silver mapping and verify the Silver table output.

---

## 2. Work Completed

| Task                                                                                                                | Owner                        | Status | Evidence                                       |
| ------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ------ | ---------------------------------------------- |
| Transform Bronze data into clean Silver tables with standardized field names, data types, joins, and derived fields | Charka Cherishma             | Done   | `notebooks/03_silver_transformations.ipynb`    |
| Cast data types and verify the Silver table schema                                                                  | Kanumuri Gayatri Praharshita | Done   | `screenshots/week05_silver_schema.png`         |
| Perform reference joins and document one raw-to-Silver mapping                                                      | Dharavath Sandhya            | Done   | `screenshots/week05_raw_to_silver_mapping.png` |

---

## 3. Key Decisions

* Standardized the field names in the Silver tables instead of keeping the raw Bronze field names unchanged.
* Cast columns to appropriate data types and used reference joins where required.
* Documented one raw-to-Silver mapping to make the transformation explainable.

---

## 4. Blockers / Risks

| Blocker                                             | Impact                                                           | Help Needed                                             |
| --------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------- |
| Different field names and data types in Bronze data | Can cause difficulties during Silver transformation and analysis | Standardize field names and cast appropriate data types |
| Reference data may contain unmatched IDs            | Can affect reference joins                                       | Verify unmatched records during Data Quality checks     |

---

## 5. Evidence Added to GitHub

* `notebooks/03_silver_transformations.ipynb` — Silver transformation notebook updated.
* `screenshots/week05_silver_schema.png` — Silver schema evidence added.
* `screenshots/week05_raw_to_silver_mapping.png` — Raw-to-Silver mapping evidence added.
* `weekly_logs/week05_log.md` — Week 5 log and AI Transparency Note updated.

---

## 6. AI Transparency Note

| Question                            | Response                                                                                                                                   |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Where AI helped                     | AI helped us understand Silver-layer transformations, standard field names, data type casting, reference joins, and raw-to-Silver mapping. |
| What we changed after AI suggestion | We reviewed the AI suggestions and adjusted the transformation logic according to the FitPulse data and project requirements.              |
| What we verified manually           | We manually verified the Silver table schema, field names, data types, joins, mapping, and sample output.                                  |
| What we can explain without AI      | We can explain the Silver layer, field standardization, data type casting, reference joins, and the raw-to-Silver transformation.          |

---

## 7. Next Week Preparation

* Prepare Data Quality checks for the Silver tables.
* Check for invalid and duplicate records and prepare the required pass/fail evidence.
