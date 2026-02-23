# Lab 14 — Failed Login Threshold Detection

## Objective:
Identify IP addresses responsible for 3 or more failed login attempts.

## Log Source:
/var/log/auth.log

## Step 1 — Extract failed login events
grep -a "Failed password" /var/log/auth.log

## Step 2 — Extract source IP addresses
grep -a "Failed password" /var/log/auth.log | awk '{print $11}'

## Step 3 — Count attempts per IP
grep -a "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c

## Step 4 — Filter IPs with ≥3 failures
grep -a "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | awk '$1 >= 3'

## Step 5 — Output only suspicious IP addresses
grep -a "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | awk '$1 >= 3 {print $2}'

## Findings:
- Suspicious IP detected: ::1

## Analysis:
- The IP ::1 generated 3 failed authentication attempts.
- Repeated failures from the same source may indicate brute-force activity.

## Conclusion:
- Detection rule validated — flag IP addresses with ≥3 failed login attempts.
