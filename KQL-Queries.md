# KQL Queries

## 1) Failed Logon Spikes (Event 4625)
```kusto
SecurityEvent_CL
| where EventID_s == "4625"
| summarize Failed = count() by Computer, bin(TimeGenerated, 1h)
| where Failed > 20
| order by Failed desc
```

## 2) Top Accounts with Failed Logons
```kusto
SecurityEvent_CL
| where EventID_s == "4625"
| summarize Failed = count() by Account_s
| top 10 by Failed desc
```
## 3) Identify Process-Related Fields in SecurityEvent_CL

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
