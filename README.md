# Mydfir-10day-Microsoft-Challenge
**Microsoft Sentinel SOC Challenge — Network (Authentication) Traffic Anomalies**

By Tamim Shamal

Day 9 of the 10-Day Microsoft SOC Challenge

# Overview

In this challenge, I investigated **Network Authentication Traffic Anomalies** using Microsoft Sentinel and KQL to detect suspicious patterns in authentication logs (SecurityEvent_CL). The main goal was to identify failed logon spikes, brute-force attempts, and abnormal process activity indicating possible network intrusion.

# [KQL Queries](./KQL-Queries.md)

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
