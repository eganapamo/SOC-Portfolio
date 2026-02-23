# Lab 16 — Suspicious IP Detection

## Objective:
Identify the most suspicious source IPs based on failed SSH authentication attempts, then optionally enrich by showing which usernames each IP targeted.

## Log Source:
/var/log/auth.log

## Step 1 — Filter failed login events
grep -a "Failed password" /var/log/auth.log

## Step 2 — Extract source IP addresses (field may vary by environment; in my lab it's $11)
grep -a "Failed password" /var/log/auth.log | awk '{print $11}'

## Step 3 — Count failed attempts by IP (rank highest first)
grep -a "Failed password" /var/log/auth.log \
| awk '{print $11}' \
| sort \
| uniq -c \
| sort -nr

## Step 4 — Flag suspicious IPs (threshold >= 3) and output the IP only
grep -a "Failed password" /var/log/auth.log \
| awk '{print $11}' \
| sort \
| uniq -c \
| sort -nr \
| awk '$1 >= 3 {print $2}'

### — Exclude localhost / lab noise (IPv6 ::1 and IPv4 127.0.0.1)
grep -a "Failed password" /var/log/auth.log \
| awk '{print $11}' \
| sort \
| uniq -c \
| sort -nr \
| awk '$1 >= 3 {print $2}' \
| grep -vE '^(::1|127\.0\.0\.1)$'



## Findings (example from my lab context):
Suspicious IP: ::1 (3 failed attempts)

## Analysis:
- Any IP with repeated failed authentication attempts (>=3) is suspicious.
- Enrichment with targeted usernames strengthens the investigation narrative.

## Conclusion:
Suspicious IPs were successfully identified and ranked using auth.log evidence.
