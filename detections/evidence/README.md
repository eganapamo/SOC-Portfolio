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
- Suspicious authentication activity observed
- Shell execution detected (bash / python / sh)
- File access activity observed (cat / less / vim / nano)
- Privilege escalation attempts detected (sudo / chmod)
- Persistence activity observed (useradd / usermod / groupadd)
- Lateral movement detected via SSH sessions
- Log tampering indicators identified (history clearing attempts)


## Conclusion
Evidence suggests a structured attack chain involving:

- Initial Access → Execution → Privilege Escalation → Persistence → Lateral Movement → Defense Evasion
- Detection methodology successfully identified attacker behaviors at each stage.


## Evidence Files
- 01 → Initial access detection
- 02 → Shell activity detection
- 03 → File access detection
- 04 → Privilege escalation detection
- 05 → Persistence detection
- 06 → Lateral movement detection
- 07 → Log tampering detection



