# 3. Suspicious Command Execution

## Incident Summary

Evidence of suspicious shell command execution was detected within authentication logs indicating potential attacker activity after system access.

## Detection Query (Splunk)

index=main sourcetype=syslog ("bash" OR "sh" OR "python" OR "nc")
| rex "COMMAND=(?<command>.*)"
| stats count by host user command

## Investigation Findings

Suspicious command execution was observed involving shell interpreters and potential remote execution tools.

These commands are commonly used by attackers to execute scripts, establish reverse shells, or perform further system enumeration.

## MITRE ATT&CK Mapping

T1059 – Command and Scripting Interpreter

## Impact

Execution of arbitrary commands allows attackers to control system operations and potentially deploy malware or persistence mechanisms.

## Recommended Response

Review executed commands for malicious activity 
Identify scripts or binaries launched by attacker 
Monitor outbound network connections 
Isolate affected systems if necessary
