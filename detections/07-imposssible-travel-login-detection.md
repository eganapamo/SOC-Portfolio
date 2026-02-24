# Lab 07 – Impossible Travel Login Detection (SSH)

## Objective
Detect “impossible travel” behavior by identifying the same user account logging in from different source IP addresses within an unrealistically short time window.

## Detection Strategy
1. Extract successful SSH logins (Accepted password) from /var/log/auth.log.
2. Pull out:
   - Timestamp
   - Username
   - Source IP
3. Convert the timestamp to **seconds since midnight** so logins can be compared numerically.
4. Group by user and look for **multiple source IPs** occurring close together (example threshold: ≤ 1 hour).  
   > In your data, the same user (kali) logs in twice from the same IP (192.168.56.1). This is a *baseline* run that proves the logic works. When you have multiple IPs, this method will flag the “impossible travel” pattern.

## Detection Command (full pipeline)
grep -a "Accepted password" /var/log/auth.log \
| awk '{
  - Split ISO timestamp field ($1) into date and time around "T"
  split($1,a,"T");

  - Split time part (a[2]) into HH:MM:SS.mmm (or HH:MM:SS) around ":"
  split(a[2],b,":");

  - Split seconds field to drop milliseconds (SS.mmm -> SS)
  split(b[3],c,".");

  - Convert HH:MM:SS into seconds since midnight
  sec = (b[1]*3600) + (b[2]*60) + c[1];

  - Output: seconds username source_ip
  print sec, $7, $9}'

## Example output format:
- 2976030 kali 192.168.56.1
- 3018057 kali 192.168.56.1

## Meaning:
- Column 1 = timestamp converted to seconds since midnight
- Column 2 = username
- Column 3 = source IP


## Threshold Logic (examples)
Flag if:
Same user appears with 2+ different IPs
And the time difference between logins is ≤ 3600 seconds (1 hour)

Example analytic idea (manual / next step):
- For each user: compare the earliest vs latest sec across different IPs
- If latest - earliest <= 3600 → suspicious
- Once your dataset contains at least two different source IPs for the same user, this will demonstrate a true “impossible travel” case.

## Findings (from my current data)
- User: kali
- Source IP observed: 192.168.56.1
- Successful logins recorded: 2
- Result: No impossible travel detected (only one source IP)

## Risk Assessment
If the same user account authenticates from geographically distant IPs within minutes/hours, it may indicate:
- Credential compromise
- Session hijacking
- Shared credentials / unauthorized remote access

## MITRE ATT&CK Mapping
- T1078 – Valid Accounts (credentialed logins)
- T1110 – Brute Force (often precedes successful login, depending on context)

## Skills Demonstrated
- Linux authentication log analysis (/var/log/auth.log)
- Filtering successful SSH events (grep)
- Field extraction and parsing with awk
- Timestamp parsing + normalization to seconds
- Building detection logic for behavioral anomalies (impossible travel)

## Conclusion
This lab builds an “impossible travel” detector by extracting successful SSH logins, converting timestamps into comparable numeric values, and preparing the data needed to detect rapid logins from different IPs. My current dataset shows repeated logins from a single IP (normal behavior), but the same pipeline will surface impossible travel once multiple source IPs appear for the same user within a short time window.






