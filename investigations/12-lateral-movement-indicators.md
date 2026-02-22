## Lab 12 — Lateral Movement Indicators


# Objective:
Detect possible lateral movement by identifying source IP addresses involved in authentication attempts.

# Log Source:
/var/log/auth.log


# Step 1 — Extract source addresses from auth log
grep -a -i "from" /var/log/auth.log | awk '{print $11}'


# Step 2 — Count occurrences of each source
grep -a -i "from" /var/log/auth.log | awk '{print $11}' | sort | uniq -c


# Step 3 — Rank sources highest → lowest
grep -a -i "from" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr


# Findings:
69 entries contained no IP value in field 11
3 entries originated from ::1


# Analysis:
::1 is the IPv6 loopback address (local system).
All authentication attempts originated locally.
No external source IPs detected.


# Conclusion:
No lateral movement observed.
Activity was generated from the same host only.
