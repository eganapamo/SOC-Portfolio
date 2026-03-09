# 2. Privilege Escalation Detection

## Incident Summary

Suspicious privilege escalation activity was detected through abnormal sudo and permission modification commands executed on the system.

## Detection Query (Splunk)

index=main sourcetype=syslog ("sudo" OR "chmod")
| rex "COMMAND=(?<command>.*)"
| rex "sudo:\s+(?<user>\w+)"
| stats count by user host command

## Investigation Findings

User executed elevated privilege commands including permission modification using chmod and administrative commands through sudo.

These activities indicate a possible attempt to escalate privileges or modify system permissions.

## MITRE ATT&CK Mapping

T1068 – Privilege Escalation 
T1548 – Abuse Elevation Control Mechanism

## Impact

Unauthorized privilege escalation could allow attackers to modify critical system files and gain administrative control.

## Recommended Response

Review all sudo activity for the affected user 
Validate legitimacy of privilege usage 
Restrict sudo privileges where necessary 
Monitor permission modification commands
