# Security Risk Register

> A working, board-ready cyber risk operations workbook. Issue intake, ROSI prioritization, risk-domain heat map, and a monthly executive dashboard — all in one Excel file, with calibration tips drawn from CVSS, EPSS, CISA KEV, and NIST SP 800-53.

[![Format](https://img.shields.io/badge/Format-Excel%20.xlsx-217346)]()
[![Methodology](https://img.shields.io/badge/Methodology-FAIR%20%2B%20ROSI-1F3A5F)]()
[![Frameworks](https://img.shields.io/badge/Calibration-CVSS%20%7C%20EPSS%20%7C%20CISA%20KEV-555)]()

---

## Why this exists

Most security teams live in two parallel worlds. The first is the operational world — vulnerability scanner output, audit findings, pentest reports — which is messy, inconsistent, and labelled with severity schemes that disagree with each other. The second is the board world — heat maps, risk appetite statements, ALE figures, and a monthly read-out.

There is no commercial tool that lets a single security manager move smoothly between those two worlds in Excel, the format most GRC practitioners actually live in. This workbook is that tool.

---

## What's inside (seven tabs)

| # | Tab | Cadence | What it does |
|---|---|---|---|
| 1 | **Assumptions** | Set once | Configure financial context — revenue, insurance coverage, risk appetite threshold, severity-to-dollar mapping |
| 2 | **Issue Intake** | When new findings arrive | Paste raw scanner / audit / pentest output. Normalization reference maps raw language to structured FAIR inputs |
| 3 | **Issue Register** | Ongoing — source of truth | Master log of all issues with full financial context. Status, dates, owners |
| 4 | **Priority Ranking** | Weekly — operational | All open issues ranked by ROSI. Sprint planning view of the top 5 highest-return remediations |
| 5 | **Risk Domain Heat Map** | Monthly — analytical | Domain-level ALE rollup and 5×5 likelihood/impact grid |
| 6 | **Monthly Dashboard** | Monthly — board / exec | Pipeline table, KPI scorecard with RAG status, pre-drafted Risk Committee narrative |
| 7 | **ROSI & Prioritization** | Calculation engine | Raw ranking data feeding Priority Ranking. Do not delete |

Workflow: **Assumptions → Issue Intake → Issue Register → Priority Ranking → Heat Map → Monthly Dashboard**

---

## The ROSI formula (the heart of the workbook)

```
ALE          = Asset Value × Threat Frequency × Vulnerability Probability × (1 − Control Effectiveness)
Residual ALE = ALE × (1 − Risk Reduction %)
ROSI         = (ALE − Residual ALE − Remediation Cost) / Remediation Cost
```

Findings are then bucketed into five priority tiers by ROSI multiple:

| Tier | ROSI / ALE Threshold | Recommended Action |
|---|---|---|
| **TIER 1 — IMMEDIATE** | ROSI > 20× OR ALE > $2M | Fix within 7 days. No budget conversation needed. |
| **TIER 2 — HIGH PRIORITY** | ROSI 10–20× OR ALE $500K–$2M | Fix within 30 days. Include in next sprint. |
| **TIER 3 — STANDARD** | ROSI 3–10× OR ALE $100K–$500K | Fix within 90 days. Schedule in roadmap. |
| **TIER 4 — MONITOR** | ROSI 1–3× OR ALE < $100K | Fix when capacity allows. Consider risk acceptance. |
| **TIER 5 — DEPRIORITIZE** | ROSI < 1× | Cost exceeds benefit. Formally accept or transfer risk. |

---

## Calibration reference (built into the Issue Intake tab)

The workbook does not assume you can guess Threat Frequency or Vulnerability Probability from a vague finding. The Issue Intake tab includes a normalization reference mapping raw finding language to structured inputs, by source tool:

- **Vulnerability scanner** (Nessus / Qualys / Tenable) → CVSS exploitability score as a proxy for Vuln Prob. EPSS score (annualized) if available.
- **Cloud posture** (CSPM / Security Hub) → Frequency = likelihood of opportunistic scan. Loss magnitude = data classification × records exposed.
- **Identity audit** → Scale ALE by account count × avg privilege level. Admin accounts 3–5× standard ALE.
- **Penetration test** → Pentest finding = confirmed vulnerability. Set Vuln Prob at the high end (0.8+).
- **Compliance / audit gap** → Process risk, not technical exploit. Conservative frequency. Loss = regulatory exposure + audit finding cost.
- **SIEM / detection gap** → Model as: detection failure increases dwell time, which increases loss magnitude. Add dwell-time multiplier.
- **Patch gap** → Check CISA KEV. If on KEV, set frequency at 0.85+. Days-exposed increases Vuln Prob over time.

The Calibration Tips section also addresses:

- **Threat Frequency** — SIEM data (count credential-based alert events over 90 days → annualize), Verizon DBIR industry rates, CISA KEV floor.
- **Vulnerability Probability** — CVSS exploitability sub-score ÷ 10 as a starting point. EPSS × 12 for annual. Pentest finding = confirmed.
- **Asset Value at Risk** — Annual Revenue ÷ 8,760 hrs × Max Downtime hrs (revenue-at-risk). Records × $150–$250 (Ponemon). GDPR up to 4% global revenue. HIPAA up to $1.9M per violation category.
- **Control Effectiveness** — Anchored to NIST SP 800-53 ratings and CIS Control implementation groups.

---

## The Monthly Dashboard (the differentiator)

Most ROSI tools stop at the prioritized backlog. This workbook continues into a Risk-Committee-ready monthly dashboard:

- **6-month issue pipeline table** with ingested / closed / open and new vs. closed financial exposure
- **KPI scorecard** with target / actual / variance / RAG status / financial context / trend / board-alert threshold / action required, across MTTR, ingestion rate, critical issues open > SLA, portfolio ALE vs. risk appetite, and average ROSI
- **Pre-drafted Risk Committee narrative** covering executive summary, critical items, financial impact, trend & outlook, and recommended actions — adapt the language, do not rewrite the structure

The narrative block is the part that takes a finance-aware practitioner ten minutes to fill in, vs. ninety minutes to compose from scratch. That is the time savings the workbook is designed around.

---

## How to use

1. **Open `Security_Risk_Quantification_Model_v3.xlsx`** — no add-ins, no macros.
2. **Set your assumptions once** on the Assumptions tab. Annual revenue, insurance coverage, risk appetite threshold, severity-to-dollar mapping.
3. **Paste raw findings** into the Issue Intake tab. Use the normalization reference to set Threat Frequency, Vulnerability Probability, and Control Effectiveness. ALE, Residual ALE, and ROSI auto-calculate. Mark "Yes" to transfer to the Issue Register.
4. **Run the Priority Ranking tab weekly** for operational sprint planning.
5. **Run the Monthly Dashboard tab monthly** for Risk Committee or board read-out. Copy the narrative block and adapt to the current period.

---

## Compatible AI-powered intake

If you want to skip the manual normalization step, two of my other repos produce output that is schema-compatible with the Issue Intake tab here:

- [`security-issues-prioritization`](https://github.com/rafatyazdani/security-issues-prioritization) — AI normalizes raw scanner / pentest findings into structured FAIR inputs. Free, local Ollama backend.
- [`compliance-copilot`](https://github.com/rafatyazdani/compliance-copilot) — AI converts raw audit / compliance findings (SOC 2, ISO 27001, GDPR, DORA) into structured remediation JSON.

Both tools' output can be pasted directly into the Issue Register tab.

---

## Related artifacts in the portfolio

- For the **portfolio-level financial exposure modelling** (Monte Carlo on the aggregate ALE this register sums into), see [`cyber-risk-quantification`](https://github.com/rafatyazdani/cyber-risk-quantification) and [`CRQ-Dashboard`](https://github.com/rafatyazdani/CRQ-Dashboard).
- For **control investment ROI** (which controls to invest in, not which findings to fix first), see [`cyber-roi-calculator`](https://github.com/rafatyazdani/cyber-roi-calculator).
- For the **board view** of what this register's portfolio totals roll up to, see [`cyber-risk-dashboard`](https://github.com/rafatyazdani/cyber-risk-dashboard).
- Methodology source: **CRQ-F Framework** Phase 1 (Input Calibration), Phase 2 (Control ROI), Phase 5 (Board Risk P&L).

---

## License

Apache 2.0 — free to use, adapt, and deploy in commercial contexts with attribution.

---

**Built by Rafat Yazdani** — CPA · CISSP · CISA · CCSK · AWS Certified
Security Strategy & Cyber Risk Quantification — Accenture Security

> Methodology backbone: **CRQ-F Framework** (Cyber Risk Quantification — Financial).
> Portfolio overview: [github.com/rafatyazdani](https://github.com/rafatyazdani) — Methodology · Quantification math · Consulting products · Governance & maturity · Operations · Board output.
