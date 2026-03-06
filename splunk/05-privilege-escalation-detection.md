# Lab 5 – Privilege Escalation Detection (Splunk)

## Objective
Detect potential privilege escalation and persistence activity in Linux systems using Splunk.

Attackers who gain access to a Linux system often escalate privileges and create persistence using commands such as:

- sudo
- su
- chmod
- adduser
- usermod
- groupadd

This lab demonstrates how to detect these behaviors using Splunk log analysis.


## Dataset

Linux system logs ingested into Splunk.

Index:
index=main

Source Type:
sourcetype=syslog


## Detection Query

index=main sourcetype=syslog ("sudo" OR "chmod" OR "su" OR "adduser" OR "groupadd" OR "usermod")
| rex "sudo:\s+(?<user>\S+)\s+:"
| stats count as attempts values(COMMAND) as commands by user host
| sort -attempts
| table user host commands attempts


## Detection Logic

The search detects suspicious command execution related to privilege escalation or persistence.

Key detection steps:

1. Filter logs containing privileged commands 
2. Extract the username executing the command 
3. Aggregate commands by user and host 
4. Count how many suspicious commands were executed 


## Results

The detection identified suspicious activity from user **msfadmin**:

User: msfadmin 
Host: 192.168.56.102 
Commands executed:
- /bin/chmod
- /bin/su
- /usr/sbin/adduser
- /usr/sbin/usermod

Total attempts: 12


## Attack Behavior Observed

The command sequence indicates a typical post-exploitation pattern:

Privilege Escalation
↓
sudo / su
↓
Permission modification
↓
chmod
↓
Account creation
↓
adduser / usermod

This behavior is commonly associated with attackers establishing persistent access on compromised systems.


## MITRE ATT&CK Mapping

Tactics involved:

Privilege Escalation 
Persistence 
Defense Evasion


## Key Takeaway

By aggregating privileged command activity per user and host, analysts can quickly identify abnormal administrative behavior and investigate potential compromise.
