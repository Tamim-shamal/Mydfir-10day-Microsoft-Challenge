### Line Chart – Failed Logons Per Hour

This Microsoft Sentinel workbook visualization shows **failed logon attempts (Event ID 4625)** collected from the `SecurityEvent_CL` table.  
It helps identify **brute-force attacks, account lockouts, or authentication spikes** across systems.

---

#### Purpose
Display the hourly trend of failed authentication attempts to quickly spot abnormal increases in network login failures.

---

#### KQL Query
```kql
SecurityEvent_CL
| where EventID_s == "4625"                       // Failed logon events
| summarize Failed = count() by Computer, bin(TimeGenerated, 1h)
| where Failed > 20                               // Highlight abnormal spikes
| order by Failed desc
```

<img width="979" height="401" alt="image" src="https://github.com/user-attachments/assets/9040f2e1-0ae7-4818-981a-d0fc6041b37e" />

#### Visualization Setup
- Setting	Description
- Chart Type	Line Chart
- X-Axis	TimeGenerated (1-hour bins)
- Y-Axis	Failed (Number of failed logons)
- Legend	Computer (Each device displayed as a separate line)
  
#### Interpretation

- X-Axis (Time) → Hourly timeline of log events

- Y-Axis (Count) → Number of failed logons per hour

- Colored Lines → Each represents a different endpoint or server

- Spikes → Potential brute-force activity or misconfigured accounts

## Example Output

#### Why It Matters

Monitoring failed logons per hour provides time-based visibility into authentication anomalies.
A sudden surge in failed attempts on a single host may indicate:

- Password-spray or brute-force attack

- Repeated service account misconfigurations

- Possible compromise attempts via remote logons
