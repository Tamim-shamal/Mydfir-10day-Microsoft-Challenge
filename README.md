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
