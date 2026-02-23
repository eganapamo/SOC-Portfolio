# Lab 15 — Privilege Escalation Alert Rule Creation

## Objective
Detect privilege escalation activity by identifying commands executed as root using sudo.

## Log Source
/var/log/auth.log


## Step 1 — Identify sudo Command Execution Events
Command:
- grep -a "sudo" /var/log/auth.log | grep -a -i "command="

Purpose:
- Filters authentication logs to show only sudo events where an actual command was executed.


## Step 2 — Extract Timestamp + Target User + Command
Command:
grep -a "sudo" /var/log/auth.log | grep -a -i "command=" | awk '{print $1,$10,$12}'

Purpose:
Displays:
• timestamp
• target account (USER=root)
• executed command path


## Step 3 — Identify Most Frequently Executed Privileged Commands
Command:
- grep -a "sudo" /var/log/auth.log | grep -a -i "command=" | awk '{print $12}' | sort | uniq -c | sort -nr

Purpose:
- Ranks privileged commands by frequency to identify abnormal activity.


## Findings
Observed root command executions:

- 2025-10-06T14:03:40 USER=root COMMAND=/usr/bin/systemctl 
- 2025-10-06T14:04:44 USER=root COMMAND=/usr/bin/grep 
- 2025-10-06T14:06:11 USER=root COMMAND=/usr/bin/journalctl 
- 2025-10-06T14:07:07 USER=root COMMAND=/usr/bin/jq 
- 2025-10-06T14:10:51 USER=root COMMAND=/usr/bin/apt 
- 2025-10-06T14:11:09 USER=root COMMAND=/usr/bin/ls 
- 2025-10-06T14:11:20 USER=root COMMAND=/usr/bin/tail 
- 2025-10-06T19:40:33 USER=root COMMAND=/usr/bin/tcpdump 


## Analysis
- Multiple privileged commands were executed as root via sudo. 
- This indicates successful privilege escalation activity.

Command usage shows:
- system management activity
- log inspection
- package access
- packet capture execution

The presence of tcpdump executed as root is especially notable because it can be used for network sniffing or credential interception.


## Detection Logic
Trigger alert when:
- USER=root appears with COMMAND=
AND
- executed by a non-root account
AND/OR
- multiple root commands occur in short time window


## Analyst Conclusion
- Sudo execution confirms root-level command access. 
- While not automatically malicious, repeated or unexpected privileged commands should be investigated for post-compromise behavior.


## Skill Demonstrated
- Privilege Escalation Detection 
- Linux Log Analysis 
- Command Attribution 
- Threat Hunting Mindset
