## 11 — Privilege Escalation Trace

## Objective
Identify privilege escalation activity by detecting sudo usage and commands executed as root from authentication logs.

## Log Source
/var/log/auth.log

## Step 1 — Detect Privilege Escalation Events
Command:
grep -a "sudo" /var/log/auth.log

Purpose:
Filters authentication logs to display only entries where sudo was used.


## Step 2 — Extract Timestamp + Elevated Command
Command:
grep -a "sudo" /var/log/auth.log | grep "COMMAND" | awk '{print $1,$10,$12}'

Purpose:
Displays:
• timestamp 
• elevated user 
• command executed as root 


## Step 3 — Sort Activity Chronologically
Command:
grep -a "sudo" /var/log/auth.log | grep "COMMAND" | awk '{print $1,$10,$12}' | sort

Purpose:
Orders all privilege escalation activity by time.


## Findings
Example Output:

2025-10-06T14:03:40 USER=root COMMAND=/usr/bin/systemctl 
2025-10-06T14:04:44 USER=root COMMAND=/usr/bin/grep 
2025-10-06T14:06:11 USER=root COMMAND=/usr/bin/journalctl 


## Analysis
Multiple sudo commands were executed by user *kali* to run commands as root.

Observed elevated commands included:
• systemctl 
• grep 
• journalctl 

All privilege escalation events were timestamped sequentially and occurred within a short time window.


## Conclusion
Root privileges were successfully obtained and used to execute administrative commands.

This activity indicates intentional privilege escalation behavior and should be investigated further if observed in a production environment.
