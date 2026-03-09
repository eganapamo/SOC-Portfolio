# 5. Log Tampering Attempt

## Incident Summary

Commands associated with log tampering and evidence removal were detected.

Attackers frequently attempt to delete or modify logs after gaining system access to hide their activity.

## Detection Query (Splunk)

index=main sourcetype=syslog ("truncate" OR "history -c" OR "unset HISTFILE" OR "rm" OR "echo")
| rex "COMMAND=(?<command>.*)"
| stats count by user host command

## Investigation Findings

Commands associated with log manipulation were observed, indicating possible attempts to erase system activity records.

These commands are commonly used by attackers to remove traces of compromise.

## MITRE ATT&CK Mapping

T1070 – Indicator Removal on Host

## Impact

Log tampering can prevent defenders from identifying attacker activity and reconstructing attack timelines.

## Recommended Response

- Preserve forensic copies of system logs 
- Investigate timeline of attacker activity 
- Restrict permissions to log files 
- Enable centralized log monitoring
