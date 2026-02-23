# Lab 17 — Command Execution Monitoring

## Objective:
Detect commands executed with elevated privileges and identify potentially suspicious administrative activity.

## Log Source:
/var/log/auth.log

## Step 1 — Extract executed commands from auth logs
grep -a -i "command" /var/log/auth.log

## Step 2 — Extract relevant fields (timestamp + privilege + command path)
grep -a -i "command" /var/log/auth.log | awk '{print $1,$10,$12}'

## Step 3 — Sort chronologically for timeline visibility
grep -a -i "command" /var/log/auth.log | awk '{print $1,$10,$12}' | sort

## Step 4 — Identify most frequently executed commands
grep -a -i "command" /var/log/auth.log | awk '{print $12}' | sort | uniq -c | sort -nr

## Findings:
- Commands executed as root include:
- systemctl, journalctl, grep, jq, apt, ls, tail, tcpdump

## Analysis:
- systemctl / journalctl → system inspection
- grep / tail → log monitoring
- jq → parsing output
- apt → package management
- tcpdump → network capture (notable but legitimate in lab)

## Conclusion:
- No malicious execution detected.
- Activity pattern consistent with system administration and analysis tasks.

## Detection Insight:
## SOC analysts should investigate when:
- rare binaries run as root
- execution frequency spikes
- commands run outside expected hours
- reconnaissance or persistence tools appear

## Skill Demonstrated:
- Log parsing
- Privilege activity monitoring
- Command tracing
- Behavioral analysis
- Incident triage logic
