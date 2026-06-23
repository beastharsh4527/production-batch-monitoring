# Monthly Production Stability Report
### IT Production – Batch Monitoring | May 2025

**Prepared by:** [Your Name], Data Analyst – IT Production
**Reporting period:** 1–31 May 2025
**Audience:** Production Management / Service Review Committee

---

## 1. Executive summary

Overall batch stability for May was **below target**, driven almost entirely by one system. Of **310 scheduled job runs**, **77.4% completed successfully within SLA**. The remaining runs either failed (52) or breached their SLA window (18), generating **70 incidents**.

The single largest driver was the **Reconciliation** system, and within it one job, **RECON_CASH_DAILY**, which accounted for the majority of failures. The root cause has been identified as a recurring database timeout that worsens under month-end load. A preventive change is recommended (Section 5).

---

## 2. Headline numbers

| Metric | Result |
|---|---|
| Total job runs | 310 |
| Success rate (within SLA) | 77.4% |
| Failed runs | 52 |
| Late runs (SLA breach) | 18 |
| Total incidents raised | 70 |
| Critical incidents (P1 + P2) | 34 |
| Open incidents at period end | 4 |

---

## 3. Where the instability is

Failure rates were not spread evenly. They concentrated in two systems:

- **Reconciliation** was the least stable system, with a **41.9% failure rate** and the lowest SLA compliance at **53.2%**.
- **Market Data** was second, at a 19.4% failure rate.
- The remaining systems (Payments, Regulatory Reporting, Settlements, Reference Data) all performed within acceptable ranges (3–13% failure).

At the job level, **RECON_CASH_DAILY** was the clear outlier, responsible for **18 failures** in the month, more than double the next worst job.

---

## 4. What is causing the failures

Grouping all 70 incidents by root cause:

- **Capacity** – the largest category (around 49% of incidents), concentrated at month-end.
- **Database** – the second largest (around 27%), almost all of them the RECON_CASH_DAILY timeout.
- **Upstream/Feed, Application, Infrastructure** – the remainder, low volume.

The failure trend across the month confirms the capacity link: failures were stable for most of May, then **spiked in the final three days** as month-end processing volumes peaked.

---

## 5. Root cause and recommended action

**Problem identified (PRB0002001):** 14 separate incidents on RECON_CASH_DAILY trace to a single underlying cause, a **database timeout** on the cash reconciliation job. The job runs within SLA on normal days but times out when month-end volumes increase.

**Recommended preventive change:**
1. Add or review database indexing on the reconciliation tables to reduce query time.
2. Increase the job's database timeout threshold as an interim safeguard.
3. Re-schedule or split the month-end run to spread peak load.

Treating these as one problem with one permanent fix, rather than re-reacting to the same incident each night, should remove the largest single source of instability in the estate.

---

## 6. Mean time to resolve (MTTR)

Incident resolution time scaled correctly with priority, confirming triage is working as intended:

| Priority | Avg. resolution time |
|---|---|
| P1 (critical) | ~51 minutes |
| P2 / P3 | ~2 hours |
| P4 (low) | ~4.5 hours |

---

## 7. Next period focus

- Implement and verify the RECON_CASH_DAILY change before next month-end.
- Monitor Market Data as the second-highest contributor.
- Continue tracking SLA compliance with a target of returning the estate to 90%+.

---

*Data sourced from production batch run logs and the incident management record for May 2025. Figures generated from the monitoring pipeline (Python, SQL, Power BI).*
