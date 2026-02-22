# Lab 09 — Multi-Event Login Attempt Correlation


## Objective
Identify all login attempts (failed + successful), determine targeted usernames, source IPs, timing behavior, and assess whether activity appears automated or manual.


## Log Source
`/var/log/auth.log`


## Step 1 — Extract All Login Attempts
Command:
grep -a -e "Failed password" -e "Accepted password" /var/log/auth.log

Purpose: 
Displays every authentication attempt regardless of success or failure.


## Step 2 — Extract Targeted Usernames
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $9}'

Purpose: 
Identifies which usernames attackers attempted to authenticate as.


## Step 3 — Identify Source IP Addresses
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

Purpose: 
Determines which IP addresses generated login attempts and ranks them by frequency.


## Step 4 — Correlate IP + Username
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $11,$9}' | sort | uniq -c | sort -nr

Purpose: 
Links attacking IP addresses to targeted usernames.


## Step 5 — Timing Pattern Analysis
Command:
grep -a "Failed password" /var/log/auth.log

Observed timestamps:
- 14:04:29
- 14:04:33
- 14:04:44

Purpose: 
Determines time gaps between attempts to evaluate attack type.


## Findings

| Metric | Result |
|------|-------|
Targeted Username | `fakeuser`
Source IP | `::1`
Failed Attempts | 3
Successful Logins | 0
Time Gaps | 4–11 seconds
Behavior Type | Manual login attempts |


## Analysis

The source IP `::1` generated multiple failed login attempts against the username **fakeuser**. 
No successful authentication events were observed.

Timing intervals between attempts varied slightly (4–11 seconds), indicating **manual interaction** rather than automated brute-force tooling, which typically shows:

- identical intervals
- extremely rapid attempts
- hundreds of attempts per minute


## Conclusion

The activity originated from the local host (`::1`) and appears to be manually generated login attempts targeting a non-existent user. While not a confirmed compromise, this pattern is consistent with early reconnaissance or testing behavior and should be flagged for monitoring.


## Analyst Notes
- IPv6 localhost (`::1`) indicates activity originated from the same system.
- Lack of successful login attempts suggests no account compromise occurred.
- Low attempt volume + irregular timing strongly indicates manual execution.

---
