# Week 09 Log — Dashboard Refinement and Insight Communication

**Week:** 9

**Date range:** 31 Aug 2026 – 6 Sep 2026

**Team:** Team 11 – DataStreamers

**Project:** FitPulse – Wellness Analytics Hub

**Students:**

* Charka Cherishma
* Kanumuri Gayatri Praharshita
* Dharavath Sandhya

---

## 1. Sprint Goal

The goal for Week 09 was to refine the existing Week 08 Power BI dashboard and make it clearer, easier to use, and ready for presentation.

We tested slicers and visual interactions, reconciled important dashboard values with the approved Gold tables, and prepared evidence-based insight notes.

---

## 2. Work Completed

| Task                                                        | Owner                        | Status | Evidence                                             |
| ----------------------------------------------------------- | ---------------------------- | ------ | ---------------------------------------------------- |
| Reopened and reviewed the Week 08 Power BI dashboard        | Charka Cherishma             | Done   | `dashboard/powerbi_dashboard.pbix`                   |
| Verified that Power BI still uses approved Gold sources     | Charka Cherishma             | Done   | Final model screenshot                               |
| Checked KPI values against the owning Gold tables           | Charka Cherishma             | Done   | Reconciliation screenshot                            |
| Tested slicers and filter interactions                      | Kanumuri Gayatri Praharshita | Done   | `screenshots/week09_04_filter_interaction.png`       |
| Checked relationships, cardinality, and filter behaviour    | Kanumuri Gayatri Praharshita | Done   | Power BI model screenshot                            |
| Refined dashboard layout and visual hierarchy               | Dharavath Sandhya            | Done   | Refined dashboard screenshots                        |
| Improved titles, labels, number formats, and readability    | Dharavath Sandhya            | Done   | Refined Power BI pages                               |
| Prepared evidence-backed dashboard insights                 | Dharavath Sandhya            | Done   | `docs/dashboard_insights.md`                         |
| Updated dashboard README with source and validation details | All team members             | Done   | `dashboard/README.md`                                |
| Added Week 09 evidence and documentation to GitHub          | All team members             | Done   | `screenshots/week09_*` / `weekly_logs/week09_log.md` |

Week 09 requires the same Week 08 PBIX to be refined and validated, with visual cleanup, interaction testing, reconciliation, insights, README updates, and screenshots.

---

## 3. Key Decisions

* Continued using the **same Week 08 Power BI dashboard** instead of creating a new dashboard.
* Kept the approved **Gold-only source model** unchanged.
* Refined dashboard pages based on business questions rather than creating unnecessary visuals.
* Tested slicers and filters to make sure they affect only the intended visuals.
* Did not create new unsupported KPIs using DAX.
* Kept independent Gold tables independent when no safe relationship was required.
* Dashboard insights were written only from values and patterns that could be traced back to Gold data.

The Week 09 guide specifically states that the Week 8 Gold sources and safe relationships should remain stable, while pages and insights are refined around business questions.

---

## 4. Blockers / Risks

| Blocker                                         | Impact                                               | Help Needed                                                       |
| ----------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------------- |
| Incorrect filter interaction between visuals    | Could show misleading dashboard values               | Review Power BI interactions and relationship settings            |
| Different Gold tables may have different grains | Unsafe relationships could produce incorrect results | Verify grain, keys, and cardinality before changing relationships |
| Dashboard value differs from Gold value         | KPI cannot be trusted                                | Reconcile using the same filter state and Gold formula            |
| Limited Gold data for a new business question   | May not support a new KPI or visual                  | Record the gap instead of creating an unsupported DAX measure     |
| Dashboard contains too many visuals             | Makes the report difficult to understand             | Remove unnecessary visuals and improve visual hierarchy           |

The Week 09 guide identifies unsafe filter behaviour, missing Gold support, unclear metric ownership, and refresh mismatches as key risks.

---

## 5. Evidence Added to GitHub

* `dashboard/powerbi_dashboard.pbix`
* `dashboard/README.md`
* `docs/dashboard_insights.md`
* `screenshots/week09_01_final_model.png`
* `screenshots/week09_02_refined_page_01.png`
* `screenshots/week09_04_filter_interaction.png`
* `screenshots/week09_05_filtered_reconciliation.png`
* `screenshots/week09_06_insights_evidence.png`
* `weekly_logs/week09_log.md`

The Week 09 guide identifies the refined PBIX, dashboard insights, README, genuine Week 09 screenshots, and weekly log as the required evidence.

---

## 6. AI Transparency Note

| Question                            | Response                                                                                                                                                                                                                                  |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Where AI helped                     | AI helped us understand dashboard refinement, Power BI filter interactions, visual organization, and how to structure evidence-backed insight notes.                                                                                      |
| What we changed after AI suggestion | We adapted the suggestions to our FitPulse project, approved Gold tables, existing Week 08 dashboard, and actual business questions. We did not directly copy the demonstration project.                                                  |
| What we verified manually           | We manually checked the Power BI Gold sources, relationships, slicer behaviour, visual values, dashboard filters, and reconciliation with the owning Gold tables.                                                                         |
| What we can explain without AI      | All three team members can explain the Week 08 to Week 09 dashboard flow, why Gold is the only dashboard source, how slicers and filters work, how values are reconciled with Gold, and how dashboard insights are supported by evidence. |

The project requires four honest AI transparency points in every weekly log: where AI helped, what the team changed, what was manually verified, and what the team can explain without AI.

---

## 7. Next Week Preparation

* Start the **Week 10 streaming simulation** using the approved workout event JSON design.
* Prepare the streaming branch using Structured Streaming / Auto Loader as required.
* Test the streaming pipeline with events such as duplicates, late events, malformed JSON, and device ID issues.
* Create and validate the streaming Gold metric for the live workout feed.
* Keep the Week 09 batch Power BI dashboard unchanged and Gold-only while the streaming work begins.

Week 10 is specifically the streaming phase: JSON workout events move through Structured Streaming into streaming Bronze and a live Gold metric. The Week 09 batch dashboard should remain intact.
