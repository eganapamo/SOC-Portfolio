# 4. Sensitive File Access Detection

## Incident Summary

Access attempts to sensitive files were detected using file reading utilities such as cat, less, more, vim, and nano.

These commands may indicate reconnaissance or attempts to access sensitive configuration or credential files.

## Detection Query (Splunk)

index=main sourcetype=syslog ("cat" OR "less" OR "more" OR "vim" OR "nano")
| rex "COMMAND=(?<command>.*)"
| stats count by user host command

## Investigation Findings

User executed commands capable of reading sensitive system files.

Attackers frequently use these commands to access files such as:

/etc/passwd 
/etc/shadow 
/etc/sudoers

These files contain authentication and privilege configuration data.

## MITRE ATT&CK Mapping

T1005 – Data from Local System

## Impact

Unauthorized access to credential files could allow attackers to escalate privileges or extract password hashes.

## Recommended Response

- Audit file access logs 
- Restrict access permissions for sensitive files 
- Monitor repeated file access attempts 
- Implement file integrity monitoring
