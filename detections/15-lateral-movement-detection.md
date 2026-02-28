# Lab 15 – Lateral Movement Detection

## Objective
Detect potential lateral movement by identifying successful remote logins and session creation originating from non-local sources.

## Detection Logic
Lateral movement occurs after initial access when an attacker pivots from one system to another using remote authentication protocols such as SSH. Indicators include accepted logins and session creation from remote IP addresses.

## Log Source
/var/log/auth.log

## Detection Query Used
grep -aiE "\baccepted\b|\bsession open\b|\bssh\b" /var/log/auth.log | grep -vi "failure" | grep -iv "command" | grep -Ev "127.0.0.1|::1" | grep -v "cron"

## Investigation Method
The query searched for successful authentication and session establishment while removing noise such as failed logins, command executions, localhost activity, and automated system processes.

Noise removed:
- Failed login attempts
- Command execution activity
- Localhost activity (127.0.0.1 / ::1)
- Cron jobs

This ensured focus remained on real remote authentication events rather than internal system behavior.

## Findings
Successful SSH logins were identified originating from:
192.168.56.1

## Evidence included:
- Accepted password authentication
- SSH session establishment

## Interpretation
This activity indicates potential lateral movement, as access was established from a remote non-local IP using SSH authentication.

Command execution was intentionally excluded to isolate movement behavior rather than post-access activity.

## MITRE ATT&CK Mapping
T1021 – Remote Services

## Conclusion
Lateral movement was successfully detected through identification of remote SSH authentication and session creation from a non-local host.
