# Week 04 Log — Source to Bronze Data Pipeline

Week: 4  
Date Range: 31 July 2026 – 7 August 2026  
Team: 11-DataStreamers  
Students: Charka Cherishma, Kanumuri Gayatri Praharshita, Dharavath Sandhya
Project: FitPulse Wellness Analytics

---

## 1. Sprint Goal

The goal of this sprint was to ingest the FitPulse source datasets into the Bronze layer using Databricks. We created Bronze Delta tables, loaded the raw CSV files into the Bronze layer, validated the data loading process, and ensured the raw data was stored without any transformations for future processing.

---

## 2. Work Completed

| Task | Owner | Status | Evidence |
|---|---|---|---|
| Created Week 04 Databricks notebook | Charka Cherishma | Done | notebooks/week04_source_to_bronze |
| Uploaded raw CSV files to Databricks | Kanumuri Gayatri Praharshita | Done | DBFS Upload |
| Created Bronze database and Delta tables | Dharavath Sandhya | Done | SQL Notebook |
| Loaded source data into Bronze tables | Charka Cherishma | Done | SQL Execution |
| Validated record counts and data loading | Kanumuri Gayatri Praharshita | Done | Query Results |
| Reviewed notebook, captured screenshots, and updated GitHub | Dharavath Sandhya | Done | evidence/week04 |

---

## 3. Key Decisions

- Used Delta tables to store raw data in the Bronze layer for reliability and scalability.
- Loaded source data without applying transformations to preserve the original dataset.

---

## 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|---|---|---|
| Minor SQL syntax errors during execution | Delayed notebook completion | Corrected SQL syntax and reran the queries |
| Incorrect DBFS file path initially | Data loading failed | Verified the correct upload path and updated the notebook |

---

## 5. Evidence Added to GitHub

- Updated `notebooks/week04_source_to_bronze`
- Added Databricks notebook execution screenshots
- Added Bronze table screenshots
- Added SQL query execution screenshots
- Updated `weekly/week04_log.md`

---

## 6. AI Transparency Note

| Question | Response |
|---|---|
| Where AI helped | AI assisted in understanding the Week 04 requirements, preparing SQL queries, troubleshooting Databricks errors, and improving project documentation. |
| What we changed after AI suggestion | We corrected SQL syntax, verified DBFS paths, organized the notebook, and improved the weekly documentation before submission. |
| What we verified manually | We manually executed every SQL command, verified Bronze table creation, checked record counts, and confirmed successful data ingestion. |
| What we can explain without AI | We can explain the Databricks workflow, Bronze layer architecture, Delta table creation, source-to-Bronze ingestion process, and validation of loaded data. |

---

## 7. Next Week Preparation

- Start implementing the Silver layer transformations.
- Perform data cleaning, validation, and quality checks.
- Create Silver Delta tables from the Bronze layer.
- Prepare documentation and evidence for Week 05 submission.
