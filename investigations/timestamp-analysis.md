## Objective
Determine whether failed SSH login attempts were automated or manual by analyzing timestamp patterns in authentication logs.


## Log Source
/var/log/auth.log


## Step 1 — Identify Failed Login Attempts
Filter authentication log entries containing failed password attempts.

grep "Failed password" /var/log/auth.log

Purpose:
Isolate only failed authentication events for analysis.


## Step 2 — Extract Timestamps
Extract timestamp field from each failed login entry.

grep "Failed password" /var/log/auth.log | awk '{print $1}'

Purpose:
Identify when each failed login occurred.


## Step 3 — Count Attempts Per Timestamp
Group login attempts by timestamp and count occurrences.

grep "Failed password" /var/log/auth.log
| awk '{print $1}'
| sort | uniq -c | sort -nr


## Findings
Example Output:
1 2025-10-06T14:04:44
1 2025-10-06T14:04:33
1 2025-10-06T14:04:29
1 2025-10-06T14:04:24


## Analysis
- Each failed login occurred at a unique timestamp.
- No repeated timestamps detected.
- No evidence of multiple attempts within the same second.


## Interpretation
This pattern does **not indicate rapid automated brute-force activity.**

However:

- Attackers may intentionally delay attempts
- Low-and-slow attacks can evade detection thresholds
- Additional indicators must be analyzed before ruling out automation


## Security Insight
Timing patterns provide valuable attacker intelligence.


## Conclusion
Login attempts occurred sequentially rather than simultaneously. 
No high-speed brute-force behavior detected.

Further investigation is required to determine attacker intent or sophistication level.


## Skills Demonstrated
- Log parsing
- Timestamp analysis
- Pattern recognition
- Threat interpretation
- Command-line investigation
