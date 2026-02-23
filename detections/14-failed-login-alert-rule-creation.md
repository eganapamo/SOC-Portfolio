# Lab 14 — Failed Login Alert Rule Creation

## Objective
Create a detection rule that identifies potential brute-force attacks by triggering when multiple failed login attempts occur from a single IP address.

## Log Source
/var/log/auth.log

## Step 1 — Extract Failed Login Entries
Command:
grep -a "Failed password" /var/log/auth.log

Purpose:
Filters authentication logs to show only failed login attempts.


## Step 2 — Extract Source IP Addresses
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $11}'

Purpose:
Displays only the IP addresses responsible for failed login attempts.


## Step 3 — Count Attempts Per IP
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

Purpose:
Counts how many failed login attempts each IP performed and sorts from highest to lowest.


## Step 4 — Detection Logic
Alert Condition:
Trigger alert if any IP generates **3 or more failed login attempts**

Detection Rule Logic:
Failed attempts from same IP ≥ 3 = Suspicious activity


## Findings
Top IP:
::1 — 3 failed attempts


## Analyst Conclusion
The source IP ::1 generated multiple failed login attempts, meeting the threshold for suspicious authentication activity. This pattern may indicate brute-force behavior or unauthorized access attempts.


## Detection Value
This rule helps identify:
• brute force attacks 
• password spraying attempts 
• unauthorized access attempts 

---

## Skill Demonstrated
Security Detection Engineering 
Log Analysis 
Threat Pattern Recognition 
SOC Alert Logic Creation
