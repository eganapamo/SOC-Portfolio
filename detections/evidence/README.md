# Detection Evidence

## Objective
Identify attacker behavior across the intrusion lifecycle using log-based detection techniques.


## Steps Performed
- Searched authentication logs for suspicious access attempts
- Identified shell execution activity
- Detected file access behavior
- Monitored privilege escalation attempts
- Identified persistence creation activity
- Investigated lateral movement indicators
- Detected log tampering attempts


## Findings
Analysis of /var/log/auth.log revealed indicators consistent with an active compromise:

- Repeated suspicious authentication attempts originating from a single internal IP
- Interactive shell execution activity detected (bash, sh)
- Post-compromise file interaction observed (cat, vim, nano)
- Privileged command execution via sudo indicating escalation attempts
- Unauthorized account creation (useradd, groupadd)
- SSH session activity associated with newly created service account
- Command history deletion (truncate, rm) suggesting defense evasion


## Conclusion

Log evidence supports the presence of a multi-stage intrusion involving:

Initial Access → Execution → Privilege Escalation → Persistence → Discovery → Defense Evasion → Lateral Movement
Attacker actions were identified through authentication logs and correlated command execution patterns.

Detection logic was developed to identify:

- Brute force login activity
- Privilege escalation behavior
- Unauthorized account creation
- Command execution anomalies
- Log tampering attempts
- Internal lateral movement via SSH

This demonstrates the ability to map attacker behavior to observable log artifacts and engineer detection strategies accordingly.


## Evidence Mapping

- 01-Initial Access (SSH login attempts)
- 02-Execution (Shell invocation)
- 03-Privilege Escalation (sudo activity)
- 04-Persistence (Account creation)
- 05-Discovery (System interaction)
- 06-Defense Evasion (History deletion)
- 07-Lateral Movement (SSH reuse via svcbackup)

