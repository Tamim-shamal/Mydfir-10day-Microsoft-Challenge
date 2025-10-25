# KQL Queries

What the task was:
Investigate authentication anomalies such as failed logon spikes (4625) and analyze process creation logs (4688) for correlation with suspicious activity.

# Steps I Took

## 1) Detected failed logon spikes (Event 4625)
```kusto
SecurityEvent_CL
| where EventID_s == "4625"
| summarize Failed = count() by Computer, bin(TimeGenerated, 1h)
| where Failed > 20
| order by Failed desc
```
This helped me identify systems experiencing unusually high login failures — potential brute-force or misconfigured service accounts.

## 2) Found top accounts with failed logons (Event 4625)
```kusto
SecurityEvent_CL
| where EventID_s == "4625"
| summarize Failed = count() by Account_s
| top 10 by Failed desc
```
This allowed me to quickly identify which accounts were most often targeted or misused.

## 3) Identified process-related fields in logs (Event 4688)

``` kusto
SecurityEvent_CL
| where EventID_s == "4688"                   // Focus only on process creation events
| extend d = pack_all()                       // Pack all columns into one dynamic object (key/value pairs)
| mv-expand k = bag_keys(d)                   // Expand all column names into separate rows
| summarize rows = count() by tostring(k)     // Count how often each column name appears (cast to string)
| where k matches regex                       // Filter only for relevant columns
    @"(?i)(newprocess|image|process|cmd|command|path|exe|parent|eventdata)"
| order by rows desc                          // Sort descending to show the most common fields first
```
This helped me explore available fields in SecurityEvent_CL to better understand process activity for future detections.
