# Production Batch Monitoring & Incident Analytics

A monitoring and analytics project built around a bank's overnight batch job estate. It tracks job runs, failures, SLA breaches, and incidents, finds the root cause of recurring problems, and presents it all on an interactive dashboard, the way an IT Production support team actually works.

**Built with Python, SQL, and Power BI.**

<br>

---

## 🎯 The Problem

Banks run hundreds of batch jobs overnight: payments settlement, cash reconciliation, market data loads, regulatory reports. Some fail, some run late and miss their SLA deadline, and every failure raises an incident.

A production support team needs to see the health of the whole estate at a glance, catch failures fast, and work out *why* the same things keep breaking so they can fix them permanently. This project builds that monitoring and analytics layer end to end.

<br>

---

## 📂 A Note on the Data

This kind of data, real batch logs and incident tickets, is internal and sensitive, so it is not publicly available.

The dataset here is **synthetic, generated with Python to model a realistic production environment**: most jobs succeed, one system is less stable than the rest, one job repeatedly times out against the database, and failures rise at month-end under heavier load. The data was deliberately built with realistic messiness (duplicates, missing values, inconsistent labels) so it could be cleaned as part of the project.

<br>

---

## 🗃️ What's in This Repo

- **`production_monitoring.ipynb`** — the full Python notebook: data generation, data-quality cleaning, and all the SQL analysis.
- **`job_runs_clean.csv`** — the cleaned job-run data (310 runs).
- **`incidents.csv`** — the incident records (70 tickets), structured like a ServiceNow export.
- **`dashboard.png`** — screenshot of the Power BI monitoring dashboard.
- **`monthly-production-stability-report.md`** — a one-page management report summarising the findings.

<br>

---

## ⚙️ What It Does

**1. Generates a realistic dataset (Python / pandas)**
Ten batch jobs across six systems, run daily for a month, with built-in failure patterns and SLA targets. Incidents are raised for every failed or late run, with priority, assignment group, root cause, and a link to a problem record.

**2. Cleans the data (Python)**
Removes duplicate run records, flags missing end timestamps, standardises inconsistent status labels, and catches invalid records where an end time falls before a start time. Each fix is documented.

**3. Analyses it in SQL (SQLite)**
KPIs for production stability: success rate, failure rate by system, SLA breach percentage, worst-offending jobs (using `HAVING`), MTTR by priority, and incident volume by root cause. Runs and incidents are combined with a `JOIN`.

**4. Models the ITIL Incident → Problem → Change flow**
Recurring incidents on the same root cause are linked to a single problem record, separating the symptom (a job failing every night) from the underlying cause (one database timeout) and the fix (a change).

**5. Visualises it in Power BI**
An interactive dashboard with KPI cards (success rate, SLA %, open incidents), failure trends, worst-offender charts, and a root-cause breakdown. Includes a custom **DAX measure** for SLA compliance and a slicer for cross-filtering.

<br>

---

## 📊 Key Findings

- Overall SLA compliance for the month was **77.4%**, below a 90% target.
- **Reconciliation** was the least stable system: a **41.9% failure rate** and only **53.2%** SLA compliance.
- One job, **RECON_CASH_DAILY**, drove most of the instability with **18 failures**, more than double any other job.
- Root cause: **14 of those failures were the same database timeout**, worsening at month-end under peak load. They were linked into a single problem, with a preventive change recommended (database indexing, timeout increase, load spreading).
- **Capacity (≈49%)** and **Database (≈27%)** were the two largest incident categories, and failures spiked in the final three days of the month, confirming the month-end load link.

<br>

---

## 🛠️ Skills Demonstrated

- Data generation and simulation
- Data cleaning and quality remediation (duplicates, missing values, inconsistent and invalid records)
- SQL: aggregation, `GROUP BY` / `HAVING`, `JOIN`, computed metrics
- Production KPIs: SLA compliance, MTTR, failure rates, root cause analysis
- ITIL concepts: Incident, Problem, Change
- Power BI: dashboards, DAX measures, slicers, cross-filtering
- Reporting: turning analysis into a one-page summary for management

<br>

---

## ▶️ How to Run It

1. Open `production_monitoring.ipynb` in Google Colab or Jupyter.
2. Run the cells top to bottom. It generates the data, cleans it, and runs the analysis (no external dataset needed).
3. The cleaned CSVs are written out at the end, and these feed the Power BI dashboard.

<br>

---

**Author:** [Your Name]
**GitHub:** [your GitHub URL]
