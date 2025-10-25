# Mydfir-10day-Microsoft-Challenge
**Microsoft Sentinel SOC Challenge — Network (Authentication) Traffic Anomalies**

By Tamim Shamal

Day 9 of the 10-Day Microsoft SOC Challenge

# Overview

**Why am I sharing this?**

To demonstrate my ability to detect, analyze, and document network authentication anomalies within Microsoft Sentinel using real Windows event data (SecurityEvent_CL).

**What I want someone reading my GitHub to understand about me:**

That I can build detection logic in KQL, identify suspicious authentication activity like failed logon spikes or process-related events, and visualize these findings in a structured, SOC-ready format.

# Project Overview

## What this challenge is:

This Day 9 SOC Challenge focused on detecting Network (Authentication) Traffic Anomalies by analyzing Windows Security Events using Microsoft Sentinel. The project emphasized identifying failed logon spikes, compromised accounts, and related process activity.

## What tools I used:

- Microsoft Sentinel

- Azure Log Analytics Workspace

- Kusto Query Language (KQL)

- Microsoft Defender for Endpoint (data source)

- MITRE ATT&CK Framework (T1110 – Brute Force)

## What skills I practiced:

- Querying and filtering event logs using KQL

- Detecting failed logon spikes (Event ID 4625)

- Analyzing process creation events (Event ID 4688)

- Using mv-expand and regex to explore column structures

- Visualizing anomalies in Sentinel Dashboards

## What I built or investigated:

- Queries to detect failed authentication spikes and top failing accounts

- Identification of process-related fields within SecurityEvent_CL

- A Sentinel Workbook dashboard to visualize authentication anomalies

# [What You Did and Learned](./KQL-Queries.md)

**1) Failed Logon Spikes (Event 4625)**

- What it does: Counts failed logons per computer over 1-hour buckets and surfaces systems with unusually high failure volume.

- Why it matters: Spikes often indicate password-spray, brute-force attempts, or misconfigured services.

- Output: Computer, hour bucket, and Failed count (only where Failed > 20, sorted highest first).

- How to act: Investigate top hosts, correlate with source IPs, and check for account lockouts or geo/time anomalies.

**2) Top Accounts with Failed Logons**

- What it does: Ranks the accounts generating the most 4625 events.

- Why it matters: Identifies users being targeted or misconfigured service accounts.

- Output: Account_s with total failed attempts (Top 10).

- How to act: Review sign-in sources, enforce MFA/lockout, rotate credentials for service accounts, and add conditional access where applicable.

**3) Identify Process-Related Fields in SecurityEvent_CL (for 4688)**

- What it does: Explores the schema of your custom table to discover which columns actually contain process/command-line data for process creation events (4688).

- Why it matters: Custom _CL tables vary; this reveals the real column names (e.g., Image, EventData_s, etc.) so you can write reliable detections (e.g., PowerShell/Certutil/Rundll32).

- Output: A frequency list of field names matching process/image/cmd/…, sorted by how often they appear.

- How to act: Take the top relevant column (e.g., Image or EventData_s) and use it in follow-up detections (PowerShell encoded commands, downloads, Defender tampering, suspicious parents).

# Screenshots or Visuals

**Microsoft Sentinel Workbook Screenshot:**

[*Network Authentication Anomalies Dashboard*](./Network%20Authentication%20Anomalies%20Dashboard.md)

## Includes:

- Line Chart: Failed Logons Per Hour

- Bar Chart: Top 10 Accounts by Failed Logons

- Table: Process Field Analysis from Event 4688

# Summary of Results  
### Microsoft Sentinel 10-Day SOC Challenge  
**By Tamim Shamal**

---

## What I Can Now Do That I Couldn’t Before

Through this challenge, I developed hands-on expertise with **Microsoft Sentinel** and **KQL (Kusto Query Language)** for threat detection and analysis. I can now confidently:

- **Detect authentication anomalies** using Event IDs **4625** (failed logons) and **4688** (process creation).  
- **Use KQL** to explore, filter, and summarize complex log data in `SecurityEvent_CL`.  
- **Create visual dashboards and automated detections** in Microsoft Sentinel to monitor authentication spikes, brute-force attempts, and suspicious process executions.  
- **Correlate event data** across hosts and accounts for better incident context.  

---

## Most Impactful Part of the Challenge

The most valuable skill I learned was using the KQL functions **`mv-expand`** and **regular expressions (regex)** to explore dynamic fields and discover hidden telemetry within **Event 4688 (process creation)** logs.  

This method allowed me to:
- Identify **relevant process-related fields** (`NewProcessName`, `CommandLine`, etc.).  
- Understand **how process execution telemetry is structured** within Windows Security Events.  
- Improve **data normalization and detection coverage** by knowing exactly which fields contain valuable indicators.  

> *“Learning to extract insights directly from raw event data was a real game changer for process-level threat detection.”*

---

## What I Want to Keep Improving

Moving forward, I plan to continue enhancing my Sentinel and KQL skills by focusing on:

- **Combining authentication and process data** to build correlated detections (e.g., failed logon followed by PowerShell execution).  
- **Integrating threat intelligence (TI) enrichment** such as IP reputation and external mapping.  
- **Expanding dashboards** to cover both endpoint and network telemetry for a complete security overview.  
- **Automating alert triage** by integrating KQL detections with playbooks (Logic Apps).  

---

## Resource List

Here are the key references and learning materials that supported this project:

- [Microsoft Sentinel Documentation](https://learn.microsoft.com/en-us/azure/sentinel/)  
- [Windows Security Audit Events](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625)  
- [MITRE ATT&CK – Brute Force (T1110)](https://attack.mitre.org/techniques/T1110/)  
- [KQL Query Language Reference](https://learn.microsoft.com/en-us/azure/data-explorer/kql-quick-reference)  

---

## Reflection Summary

This challenge solidified my ability to think like a **SOC Analyst** — turning raw telemetry into actionable intelligence.  
From failed logons to process analysis, I learned how to **detect early signs of compromise** using Sentinel’s power.  

> _“Microsoft Sentinel is not just a SIEM — it’s an investigation platform that empowers analysts to see the full picture.”_


End of the page


