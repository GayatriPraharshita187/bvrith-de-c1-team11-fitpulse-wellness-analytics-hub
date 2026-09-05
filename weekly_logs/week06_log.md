#Week: 6
Date range: 4 August 2026 – 10 August 2026

Students:

Charka Cherishma
Kanumuri Gayatri Praharshita
Dharavath Sandhya

Team: 11-DataStreamers
Project: FitPulse Wellness Analytics


---

##1. Sprint Goal

Evaluate the Week 5 Silver Candidate data using the approved Week 6 Data Quality rules. Identify data quality issues, retain all rule failures, and route each record correctly to Trusted Silver or Quarantine.

Also, verify that no records are lost or duplicated by performing reconciliation and membership checks.

##2. Work Completed
Task	Owner	Status	Evidence
Verified the five Week 5 Candidate tables and prepared baseline count checks	Team 11	Done	04_data_quality_checks.ipynb
Implemented DQ checks for Activity Types	Team 11	Done	activity_types_checked and activity_types_routed
Implemented DQ checks and routing for Users	Team 11	Done	trusted_silver_users, quarantine_users
Implemented DQ checks and routing for Devices	Team 11	Done	trusted_silver_devices, quarantine_devices
Implemented DQ checks and routing for Goals	Team 11	Done	trusted_silver_goals, quarantine_goals
Implemented detailed DQ checks and routing for Workouts	Team 11	Done	trusted_silver_workouts, quarantine_workouts
Added rule failure IDs, failure count and severity for workout records	Team 11	Done	workouts_routed
Added reconciliation checks to prove Candidate = Trusted + Quarantine	Team 11	In progress	Reconciliation SQL queries
Added Trusted ∩ Quarantine membership checks	Team 11	In progress	Membership SQL query
Prepared controlled rerun and Delta history validation	Team 11	In progress	DESCRIBE HISTORY queries

The notebook follows the dependency order Activity Types → Users → Devices → Goals → Workouts, using Trusted upstream tables for dependent validations.

## 3. Key Decisions

Followed the approved Team 11 Week 6 DQ rulebook and implemented the eight defined rule IDs within the Week 6 batch scope. DQ-EVT-001 was kept out of this week's implementation because it is specified for Week 10 streaming.
Used the actual Week 5 table names (silver_users, silver_devices, silver_goals, silver_activity_types, silver_workouts) instead of silently renaming them to the _candidate names mentioned in the rulebook. The naming mismatch was identified for discussion with the mentor.
Preserved multiple failed rule IDs for a single physical workout row instead of stopping after the first failure.
Used CREATE OR REPLACE TABLE for Trusted and Quarantine outputs so controlled reruns do not accumulate duplicate records.

## 4. Blockers / Risks

Blocker	Impact	Help Needed
Duration tolerance for DQ-DUR-001 is marked as CONFIRM WITH MENTOR	Final workout duration validation cannot be considered fully approved until the tolerance is confirmed	Mentor confirmation
Approved user_status domain needs confirmation	The domain check currently uses ACTIVE, INACTIVE and SUSPENDED and is marked for mentor confirmation	Mentor confirmation
goal_type ↔ target_unit approved mapping is not finalized	Complete DQ-GOL-001 goal compatibility validation cannot be finalized	Mentor confirmation
Goal-contribution activity-scope rule needs confirmation	Workout-to-goal contribution validation has a pending approved scope requirement	Mentor confirmation
Rulebook table names differ from actual Week 5 table names	Could cause confusion between documentation and implementation	Discuss naming with mentor

---

## 5. Evidence Added to GitHub

-04_data_quality_checks.ipynb — Week 6 Data Quality Checks notebook.
SQL checks for baseline Candidate counts.
SQL logic for Trusted Silver and Quarantine routing.
Rule scorecard query.
Candidate/Trusted/Quarantine reconciliation query.
Trusted ∩ Quarantine membership validation.
Multi-rule workout failure inspection.
DESCRIBE HISTORY queries for controlled rerun verification.

The notebook contains the complete SQL implementation and exit checklist for these validations.
---

## 6. AI Transparency Note

Question	Response
Where AI helped	AI was used to assist with structuring the Week 6 data-quality workflow, SQL checks, routing logic and reconciliation approach based on the approved DQ rulebook.
What we changed after AI suggestion	The implementation was aligned with Team 11's actual Week 5 table names and dependency order. The notebook also explicitly preserved the naming mismatch instead of silently changing table names.
What we verified manually	The DQ rule IDs, entity scope, routing destinations, dependency order, reconciliation logic and mentor-confirmation placeholders were reviewed against the approved Week 6 requirements.
What we can explain without AI	We can explain how each entity is checked, how records are classified as PASS/FAIL, how Trusted and Quarantine tables are created, how failed rule IDs are retained, and how reconciliation proves that records are not lost or duplicated.

---

## 7. Next Week Preparation

-Confirm the pending DQ thresholds and domain mappings with the mentor.
Remove CONFIRM WITH MENTOR placeholders after approval.
Run the notebook against the actual Week 5 Candidate snapshot.
Capture actual Candidate, Trusted and Quarantine counts.
Verify variance = 0 for Users, Devices, Goals and Workouts.
Verify Trusted ∩ Quarantine = 0.
Capture rule scorecard and multi-rule failure evidence.
Capture DESCRIBE HISTORY evidence after a controlled rerun.
Report and resolve the silver_candidate_* vs silver_* naming mismatch.
