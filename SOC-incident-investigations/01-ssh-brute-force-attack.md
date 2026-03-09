# 1. SSH Brute Force → Account Compromise

## Incident Summary
Multiple failed SSH authentication attempts were detected followed by successful login attempts from the same source IP address. This pattern indicates a potential brute force attack resulting in account compromise.

## Detection Query (Splunk)

index=main sourcetype=syslog ("Failed password" OR "Accepted password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| eval login_status=if(searchmatch("Failed password"),"failed","success")
| stats count(eval(login_status="failed")) as failed_attempts count(eval(login_status="success")) as successful_attempts values(user) as usernames values(host) as host by attacker_ip
| where failed_attempts >= 4 AND successful_attempts >= 1
| table attacker_ip usernames host failed_attempts successful_attempts

## Investigation Findings

Attacker IP: 192.168.56.104 
Target Username: msfadmin 
Host: 192.168.56.102 

Observed multiple failed SSH authentication attempts followed by successful logins, confirming that the attacker eventually gained access to the system.

## MITRE ATT&CK Mapping

T1110 – Brute Force 
T1078 – Valid Accounts

## Impact

Attacker successfully authenticated using compromised credentials and gained access to the system.

## Recommended Response

Block attacker IP at firewall or endpoint 
Reset compromised account credentials 
Review command execution logs after login 
Check for persistence mechanisms 
Enable multi-factor authentication
