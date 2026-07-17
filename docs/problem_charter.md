# Problem Charter

**Week:** 1  
**Owner(s):** [Student names]  
**Project:** [Project title]

---

## 1. Problem Context

Explain the domain in simple language.

Prompts:

- What real-world process or operation does this project represent?
- What kinds of data are generated?
- Why is raw data not enough?
- Who would use the final dashboard or metrics?

---

## 2. Engineering Problem

Write the data engineering problem clearly.

Example format:

> The project must convert multiple raw source files into trusted Bronze, Silver, Data Quality, Gold, and dashboard-ready outputs using Databricks and Power BI.

---

## 3. Users / Stakeholders

| User / Stakeholder | What they need from the data |
|---|---|
| [Example: Operations Head] | [Example: View daily demand and service issues] |
| [Example: Analyst] | [Example: Compare trends and investigate failures] |

---

## 4. Scope Inclusions

The team will build:

* Synthetic data generation scripts
* Raw source files (Workouts, Users, Devices, Goals)
* Bronze layer ingestion in Databricks
* Silver layer standardization and transformation
* Data Quality validation and quarantine process
* Gold layer KPI and metric tables
* Power BI dashboard with four views
* Streaming simulation using workout event JSON files
* GitHub documentation and weekly evidence submission

---

## 5. Scope Exclusions

The team will not build:

* A real fitness or healthcare application
* Production deployment environment
* Usage of real personal or health data
* Payment gateway or authentication modules
* Mobile application interfaces
* Downloaded or copied internet projects
* Fake screenshots or unexplained AI-generated work

---

## 6. Success Criteria

By the end of 12 weeks, the project will be successful if:

* The complete pipeline can be explained from source to dashboard.
* Bronze, Silver, Data Quality, Gold, and Streaming layers are implemented successfully.
* Gold metrics and Power BI dashboards provide reliable insights.
* The streaming pipeline demonstrates live workout monitoring.
* All three team members can explain the architecture and workflow.
* GitHub contains weekly evidence, documentation, notebooks, and final submission files.
* Dashboard metrics can be traced back to the original source data.
