# Week 03 Log — Profiling, Relationships and Architecture

**Week:** 3

**Date Range:** 25 July 2026 – 31 July 2026

**Team:** 11-DataStreamers

**Students:** Charka Cherishma, Kanumuri Gayatri Praharshita, Dharavath Sandhya

**Project:** FitPulse Wellness Analytics Hub

---

# 1. Sprint Goal

Explore and understand all FitPulse source datasets using Databricks. Perform data profiling, validate schema, identify business keys, analyze relationships between datasets, and document observations without modifying the source data. Prepare the project for Bronze layer implementation in Week 4.

---

# 2. Work Completed

| Task | Owner | Status | Evidence |
|------|-------|--------|----------|
| Set up Databricks workspace and mounted project data | Charka Cherishma | Done | Databricks Workspace |
| Loaded users.csv into Spark DataFrame | Charka Cherishma | Done | notebooks/01_data_exploration.ipynb |
| Loaded workouts.parquet into Spark DataFrame | Kanumuri Gayatri Praharshita | Done | Notebook Output |
| Loaded activity_types.csv into Spark DataFrame | Dharavath Sandhya | Done | Notebook Output |
| Explored schema and data types for all datasets | Team | Done | Notebook Screenshots |
| Verified primary keys and business keys | Team | Done | SQL Queries |
| Calculated row counts and distinct record counts | Team | Done | SQL Results |
| Performed relationship validation between datasets | Team | Done | Notebook Results |
| Updated data_dictionary.md with Week 3 observations | Team | Done | docs/data_dictionary.md |
| Updated synthetic_data_assumptions.md | Team | Done | docs/synthetic_data_assumptions.md |
| Uploaded notebook, documentation and screenshots to GitHub | Team | Done | GitHub Repository |

---

# 3. Key Decisions

- Used Spark SQL for all profiling and exploration activities.
- Preserved all raw source data without making modifications.
- Used the project source contracts as the reference for schema validation.
- Documented only observed data quality issues instead of correcting them.
- Deferred all cleaning and transformation activities until Week 4 Bronze implementation.

---

# 4. Blockers / Risks

| Blocker | Impact | Help Needed |
|----------|--------|-------------|
| Large datasets required longer execution time in Databricks | Notebook execution took additional time | No |
| Understanding relationships between multiple datasets | Required careful validation using project documentation | No |
| Initial Spark schema interpretation | Verified manually before documentation | No |

---

# 5. Evidence Added to GitHub

- notebooks/01_data_exploration.ipynb
- docs/data_dictionary.md
- docs/synthetic_data_assumptions.md
- weekly_logs/week03_log.md
- Week 3 notebook screenshots
- SQL query outputs
- Databricks execution screenshots

---

# 6. AI Transparency Note

| Question | Response |
|----------|----------|
| Where AI helped | AI helped explain Spark SQL syntax, notebook structure, documentation formatting, Markdown formatting, and GitHub documentation practices. |
| What we changed after AI suggestion | Verified every SQL query manually, updated documentation based on actual project observations, and removed any assumptions not supported by the FitPulse dataset. |
| What we verified manually | Source files, schema, data types, business keys, row counts, relationships, SQL query outputs, notebook execution, and GitHub repository updates were verified manually. |
| What we can explain without AI | Complete Week 3 workflow including loading datasets, Spark DataFrames, SQL profiling, schema validation, relationship analysis, documentation updates, and preparation for Bronze implementation. |

---

# 7. Next Week Preparation

- Create Bronze Delta tables for all FitPulse datasets.
- Ingest source files into the Bronze layer.
- Add ingestion metadata columns.
- Perform source-to-Bronze reconciliation checks.
- Validate record counts after ingestion.
- Preserve raw data during Bronze implementation.
- Prepare the project for Silver layer development in future weeks.

---
