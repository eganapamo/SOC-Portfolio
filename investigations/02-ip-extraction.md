## Lab 2 - IP Extraction

## Objective
Identify attacker IP addresses responsible for failed login attempts using regex extraction and log parsing.


## Log Source
/var/log/auth.log


## Step 1 — Find Failed Logins

grep "Failed password" /var/log/auth.log


## Step 2 — Extract Only IP Addresses
Used extended regex to isolate IPv4 addresses:

grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' /var/log/auth.log

### Explanation
- -E → enables extended regex
- -o → prints only matching values
- ([0-9]{1,3}\.){3} → first 3 octets with dots
- [0-9]{1,3} → last octet


## Step 3 — Sort + Count Attempts Per IP
grep "Failed password" /var/log/auth.log \
| grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' \
| sort | uniq -c | sort -nr


## Findings
Example Output:
3 192.168.1.10
1 10.0.0.5

### Interpretation
- IP with highest count = most likely attacker
- Multiple attempts from same IP suggests brute force activity


## Analyst Conclusion
The log analysis revealed repeated failed authentication attempts originating from a single IP address. This behavior is consistent with brute-force login activity.


## MITRE ATT&CK Mapping
- **T1110 — Brute Force**


## Skills Practiced
- Regex extraction
- Log parsing
- Sorting + counting events
- Threat pattern recognition


## Key Takeaway
Counting delimiters (dots) instead of values is essential for building correct regex patterns when parsing structured log data.
