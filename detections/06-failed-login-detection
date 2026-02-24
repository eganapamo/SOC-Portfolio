# Lab 06 – Failed Login Spike Detection

## Objective
Detect abnormal spikes in failed SSH login attempts within a defined time window.

## Detection Strategy
Count failed login events per minute and trigger an alert if the number of failures exceeds a defined threshold.

## Detection Command

grep "Failed password" /var/log/auth.log \
| awk '{print $1}' \
| cut -d: -f1-2 \
| sort \
| uniq -c \
| sort -nr

## Threshold Logic
Alert if:
- Failed attempts ≥ 10 within 1 minute

Example threshold filter:

grep "Failed password" /var/log/auth.log \
| awk '{print $1}' \
| cut -d: -f1-2 \
| sort \
| uniq -c \
| awk '$1 >= 10'

## Findings
- Spike observed at 2025-10-06T14:04
- 4 failed login attempts detected within one minute
- Activity consistent with brute-force authentication behavior

## Risk Assessment
Repeated authentication failures in short intervals indicate possible brute-force attack attempts.

## MITRE ATT&CK Mapping
T1110 – Brute Force

## Skills Demonstrated
- Log parsing
- Time-based aggregation
- Threshold detection logic
- Bash pipeline construction
- Basic anomaly detection
