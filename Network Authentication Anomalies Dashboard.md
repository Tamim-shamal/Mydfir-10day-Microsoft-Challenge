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

# Failed Logon

This document provides explanations for three key **Microsoft Sentinel KQL queries** used to investigate authentication and process-related anomalies in `SecurityEvent_CL`.

## Query 1 — Failed Logon Spikes (Event 4625)

```kusto
SecurityEvent_CL
| where EventID_s == "4625"
| summarize Failed = count() by Computer, bin(TimeGenerated, 1h)
| where Failed > 20
| order by Failed desc
```
<img width="973" height="313" alt="image" src="https://github.com/user-attachments/assets/dfd31f22-02de-404e-9bb6-84150b9e2bc6" />

### Top Accounts with Failed Logons

The chart below shows the top accounts that experienced failed logon attempts, visualized using Microsoft Sentinel’s workbook.  
Each bar (or table row) represents an account with the total number of failed logon events.

#### Sample Output
| Account_s | Failed |
|------------|---------|
| \ADMINISTRATOR | 10,255 |
| \admin | 1,989 |
| \administrator | 1,864 |
| \ADMIN | 598 |
| Tamarindo@tamacc\Administrator | 373 |

### Interpretation
- **\ADMINISTRATOR** shows a disproportionately high number of failed logon attempts, suggesting potential brute-force or password-spray activity.  
- Lower-volume failures (e.g., `\admin`, `\administrator`) might still indicate repeated login attempts or system misconfigurations.  
- This data helps the SOC team prioritize **which accounts** to investigate first, focusing on those most frequently targeted.

### Recommended Action
- **Investigate** accounts with excessive failures for potential attack activity.  
- **Correlate** these accounts with IP sources, geolocations, or time patterns.  
- **Review** password policies and lockout thresholds to reduce brute-force risks.


