# Week 06 Log — FitPulse Data Quality, Trusted & Quarantine

**Week:** 6
**Date range:** 14 August 2026-20August 2026
**Team:** Team-11 DataStreamers
**Project:** FitPulse – Wellness Analytics Hub

---

## 1. Sprint Goal

The goal of Week 6 was to apply the approved FitPulse Data Quality rules to the Silver Candidate tables. Failed records were identified and routed to the appropriate Quarantine tables, while valid records were routed to Trusted Silver. The workflow also retained DQ status, failure information and traceability metadata for validation and downstream processing.

---

## 2. Work Completed

| **Task**                                                                                          | **Owner**                    | **Status** | **Evidence**                                               |
| ------------------------------------------------------------------------------------------------- | ---------------------------- | ---------- | ---------------------------------------------------------- |
| Read and verified the Silver Candidate tables produced in Week 5                                  | Charka Cherishma             | Done       | `screenshots/week06_dq_rules.png`                          |
| Implemented the approved FitPulse DQ rules for Activity Types, Users, Devices, Goals and Workouts | Kanumuri Gayatri Praharshita | Done       | Notebook                                                   |
| Evaluated the approved rules and generated the DQ failure scorecard                               | Dharavath Sandhya            | Done       | `screenshots/week06_rule_scorecard.png`                    |
| Routed valid records to Trusted Silver and failed records to Quarantine                           | Kanumuri Gayatri Praharshita | Done       | `screenshots/week06_trusted_quarantine_reconciliation.png` |
| Validated Trusted and Quarantine reconciliation and zero intersection                             | Dharavath Sandhya            | Done       | `screenshots/week06_trusted_quarantine_reconciliation.png` |
| Documented multi-rule failures and the correction/replay approach                                 | Charka Cherishma             | Done       | `screenshots/week06_failed_record.png`                     |
| Captured Delta table history for Trusted Workouts and Quarantine Workouts                         | Kanumuri Gayatri Praharshita | Done       | `screenshots/week06_rerun_replay.png`                      |

---

## 3. Key Decisions

* Used the Week 5 Silver Candidate tables as the inputs for Week 6 Data Quality processing.
* Applied the approved FitPulse DQ rule IDs and their defined severities.
* Evaluated the five Week 6 batch entities: **Activity Types, Users, Devices, Goals and Workouts**.
* Routed each physical Candidate record to either Trusted Silver or Quarantine.
* Preserved failed records in Quarantine rather than deleting or silently correcting them.
* Retained all applicable DQ failures for a record instead of stopping after the first failure.
* Applied the approved dependency order across the batch entities.
* Verified that Candidate records are fully accounted for by the Trusted and Quarantine outputs.
* Verified that no record appears in both Trusted and Quarantine.
* Used `CREATE OR REPLACE TABLE` logic to support controlled reruns without accumulating duplicate records.
* Treated corrections and replay as a controlled upstream process; existing Quarantine records were not directly edited.
* Deferred `DQ-EVT-001` to Week 10 because it applies to streaming workout events.

---

## 4. Blockers / Risks

| **Blocker / Risk**                                                                                         | **Impact**                                                                                | **Resolution / Help Needed**                                                         |
| ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Some Workout records fail multiple DQ rules                                                                | Failure reasons can be difficult to interpret when several rules apply to the same record | Retained all applicable failed rule IDs and assigned the highest applicable severity |
| Week 6 rulebook uses `silver_candidate_*` naming while the implemented Week 5 tables use `silver_*` naming | Can create ambiguity when referring to the governed input tables                          | Documented the naming mismatch for review                                            |
| Duration tolerance requires confirmation                                                                   | May affect the final interpretation of duration-related failures                          | Requires mentor confirmation before being treated as finalized                       |
| Domain value lists require confirmation                                                                    | May affect validation of categorical fields                                               | Requires mentor confirmation                                                         |
| `goal_type` / `target_unit` pairing requires confirmation                                                  | May affect goal validity checks                                                           | Requires mentor confirmation                                                         |
| Streaming event validation was not part of the Week 6 batch scope                                          | `DQ-EVT-001` could not be evaluated during Week 6                                         | Deferred `DQ-EVT-001` to Week 10 streaming validation                                |

---

## 5. Evidence Added to GitHub

### Notebook

* `notebooks/04_data_quality_checks.ipynb`

### Documentation

* `docs/data_quality_summary.md`

### Screenshots

* `screenshots/week06_dq_rules.png`
* `screenshots/week06_rule_scorecard.png`
* `screenshots/week06_trusted_quarantine_reconciliation.png`
* `screenshots/week06_failed_record.png`
* `screenshots/week06_rerun_replay.png`

### Weekly Log

* `weekly_logs/week06_log.md`

---

## 6. AI Transparency Note

| **Question**                        | **Response**                                                                                                                                                                                                                                                                 |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Where AI helped                     | AI was used to explain DQ rule implementation, PASS/FAIL evaluation, Trusted and Quarantine routing, DQ metadata, reconciliation and correction/replay concepts.                                                                                                             |
| What we changed after AI suggestion | The team reviewed and aligned the rule conditions, rule IDs, entity names, table names, dependency order and routing logic with the approved FitPulse DQ rulebook and actual Databricks implementation.                                                                      |
| What we verified manually           | Reviewed the Silver Candidate inputs, DQ rule results, Trusted and Quarantine routing, failure metadata, reconciliation results, multi-rule failures and Delta table history in Databricks.                                                                                  |
| What we can explain without AI      | We can explain how DQ rules are evaluated, why records are routed to Trusted or Quarantine, how multiple failures are retained, how Trusted and Quarantine reconciliation works, and why quarantined records should be corrected upstream and replayed through the pipeline. |

---

## 7. Next Week Preparation

* Use only the Trusted Silver outputs as inputs for Week 7 Gold processing.
* Review the approved Gold tables, grains, business keys and KPI definitions.
* Validate the Gold aggregation logic against the Trusted Silver data.
* Ensure that fact-to-fact joins and aggregate calculations do not introduce double counting.
* Prepare the Gold outputs required for downstream Power BI analytics.
