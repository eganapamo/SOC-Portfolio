# Lab 10 — Brute Force Pattern Detection 

## Objective
- Detect repeated failed SSH login attempts and identify the **source IP addresses** responsible.
- This simulates early-stage brute-force behavior detection in a SOC environment.


## Environment
- Host: kali
- Log Source: /var/log/auth.log
- Event Focus: SSH authentication failures


## Step 1 — Identify Failed Login Attempts
grep -ai "failed password" /var/log/auth.log

This command searches authentication logs for SSH login failures.

Observed log pattern:

- Failed password for invalid user fakeuser from 192.168.56.1 port 37395 ssh2
- Failed password for invalid user fakeuser from ::1 port 60870 ssh2

Key observation:

The IP address is NOT always in the same position because the log structure changes when phrases like invalid user appear.
So we cannot safely use fixed fields like $11.


## Step 2 — Extract Attacker IP Address (Field-Safe Method)

Instead of assuming field positions, we dynamically locate the IP by finding the word:
- from
and printing the value immediately after it.
grep -ai "failed password" /var/log/auth.log | awk '{for(i=1;i<=NF;i++) if($i=="from") print $(i+1)}'

Explanation:

| Component | Meaning |
|-----------|---------|
| for(i=1;i<=NF;i++) | Loop through every word in the log line |
| $i=="from" | Detect where the source indicator appears |
| $(i+1) | Print the word after it (the IP address) |

Lab Output:

::1
192.168.56.1


## Step 3 — Count Failed Attempts Per IP

Now we rank attackers based on frequency.

grep -ai "failed password" /var/log/auth.log \
| awk '{for(i=1;i<=NF;i++) if($i=="from") print $(i+1)}' \
| sort | uniq -c | sort -nr

## Lab Output:

- 3 192.168.56.1
- 3 ::1


## Findings

| IP Address | Attempts | Risk Level |
|------------|----------|------------|
| 192.168.56.1 | 3 |  Higher Risk |
| ::1 | 3 |  Lower Risk |

## Risk Analysis

**192.168.56.1**
- Network-based source
- Represents a host attempting repeated authentication
- Possible brute-force behavior

**::1**
- Localhost activity
- Likely internal testing or system process


## SOC Thinking Applied

Indicators of suspicious activity:

- Multiple failed logins 
- Attempts against invalid users 
- Repeated attempts from same IP 

This pattern aligns with:
Early-stage brute-force reconnaissance


## Technical Takeaway

Because auth logs are **not structurally consistent**, using:

for(i=1;i<=NF;i++)

ensures reliable extraction of attacker IPs regardless of shifting field positions.

This mirrors real-world SIEM parsing logic.


## Conclusion

Successfully:

- Detected failed SSH login attempts
- Extracted attacker IP addresses
- Ranked most frequent offenders
- Identified potential brute-force activity
