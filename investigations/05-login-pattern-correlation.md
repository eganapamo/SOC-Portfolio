## Lab 5 - Login Pattern Correlation

## Objective
Identify relationships between failed login attempts by correlating timestamp, username, and source IP address.


## Log Source
/var/log/auth.log


## Step 1 — Filter Failed Login Attempts
Command:
grep "Failed password" /var/log/auth.log

Purpose:
This filters authentication logs to display only failed login attempts.


## Step 2 — Focus on Investigated Source IP
Command:
grep "Failed password" /var/log/auth.log | grep "::1"

Purpose:
Filters results to show only attempts from the investigated IP address (::1).


## Step 3 — Extract Timestamp, Username, and IP
Command:
grep "Failed password" /var/log/auth.log | grep "::1" | awk '{print $1,$2,$3,$9,$11}'

Purpose:
Extracts key investigation fields:
- Timestamp
- Username
- Source IP


## Step 4 — Count Attempts Per User/IP Combination
Command:
grep "Failed password" /var/log/auth.log | grep "::1" | awk '{print $9,$11}' | sort | uniq -c | sort -nr

Purpose:
Counts how many failed login attempts occurred for each username/IP pair.


## Findings
Output shows repeated login attempts from the same IP address targeting the same user.

Example pattern:
3 invalid ::1


## Analysis
Observations:
- All attempts originated from ::1
- Same username targeted repeatedly
- Attempts occurred seconds apart

Interpretation:
This pattern indicates repeated login attempts rather than normal user error.


## Conclusion
The login attempts demonstrate characteristics consistent with brute-force behavior:

- repeated attempts
- short time interval
- single source
- same target account


## Security Insight
Correlation of multiple log fields is necessary to identify attack behavior. 
Single-field searches may miss attack patterns.


## Skills Demonstrated
Log parsing 
Field extraction 
Event correlation 
Pattern analysis 
Linux command-line investigation


## MITRE ATT&CK Mapping
T1110 — Brute Force


## Analyst Takeaway
Security investigations rely on correlating multiple data points. 
This lab demonstrates how combining timestamp, username, and source IP reveals attack behavior patterns.
