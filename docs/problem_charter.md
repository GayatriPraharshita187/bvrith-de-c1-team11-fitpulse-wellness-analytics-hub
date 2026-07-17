# Problem Charter

**Week:** 1  
**Owner(s):** Charka Cherishma,Kanumuri Gayatri Praharshita,Dharavath Sandhya 
**Project:** FitPulse-Wellness Analytics Hub

---

## 1. Problem Context

FitPulse is a fictional wellness application used by students and young professionals to track workouts, goals, activity streaks, and device usage. The application generates different types of synthetic data such as workout sessions, user profiles, device information, goals, and live activity events.

Raw data alone is not sufficient because it contains errors such as duplicate workout records, negative workout durations, impossible calorie values, and missing device references. These issues make reports unreliable and can lead to incorrect business decisions.

The final dashboard will be used by product managers, wellness program managers, device operations teams, and data analysts to monitor user engagement, goal progress, device reliability, and live workout activities.

---

## 2. Engineering Problem

The project must transform multiple synthetic raw data sources into trusted Bronze, Silver, Data Quality, Gold, and dashboard-ready outputs using Databricks, Spark SQL, and Power BI.

The system should identify and handle data quality issues, create reliable metrics, and support both batch and streaming analytics.

---

## 3. Users / Stakeholders

| User / Stakeholder       | What they need from the data                                    |
| ------------------------ | --------------------------------------------------------------- |
| Product Engagement Lead  | Monitor user activity, workout consistency, and streak accuracy |
| Wellness Program Manager | Track goal completion and identify users falling behind         |
| Device Operations Lead   | Analyze device reliability and missing activity signals         |
| Data & Analytics Manager | Access trusted and explainable metrics for decision making      |
| Business Analysts        | Study trends and generate insights from wellness data           |

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
