# Lab 13 — Incident Summary Reporting

## Objective:
## Produce a clean, recruiter-ready incident summary from Linux auth logs:
- What happened (failed SSH attempts)
- Who was targeted (username)
- Where it came from (source IP)
- When it happened (timeline + gaps)
- Any success? (accepted password check)
- Analyst conclusion (manual vs automated, impact)

## Log Source:
/var/log/auth.log

## Notes:
- My lab data shows ::1 (IPv6 localhost) and the targeted user "fakeuser".
- In real SOC work, these same steps apply to external IPs too.


## Step 1 — Confirm the incident scope (failed SSH logins)

grep -a "Failed password" /var/log/auth.log


## Step 2 — Count total failed attempts

grep -a "Failed password" /var/log/auth.log | wc -l


## Step 3 — Identify targeted usernames (field may vary; in my dataset $9 matched fakeuser)

grep -a "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -nr


## Step 4 — Identify source IPs (your dataset uses IPv6 localhost ::1 in field $11)

grep -a "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr


## Step 5 — Build a simple timeline (timestamp is field $1 in my dataset)

grep -a "Failed password" /var/log/auth.log | awk '{print $1}' | sort


## Step 6 — Show the combined context (timestamp + source IP + username)

grep -a "Failed password" /var/log/auth.log | awk '{print $1, $11, $9}'


## Step 7 — Check for any successful logins (none expected in my lab)

grep -a "Accepted password" /var/log/auth.log


## Step 8 — Incident Summary 

## 'INCIDENT_SUMMARY'

## Findings
- **Event Type:** SSH authentication failures ("Failed password")
- **Targeted Username (most frequent):** fakeuser
- **Top Source IP:** ::1 (IPv6 localhost)
- **Successful Authentication:** None observed ("Accepted password" not found)
- **Timeline:** Failed attempts occurred across multiple distinct timestamps (not all at the same exact second)

## Analysis
- The repeated failures against a single username suggest **credential guessing** behavior.
- The source IP **::1** indicates the activity originated from the **local host** (lab-generated), not an external attacker.
- The attempts are **not strongly indicative of automation** in my dataset because they are not occurring in a large burst at the exact same timestamp; spacing between attempts suggests **manual or low-rate activity**.

## Conclusion
- **Impact:** No confirmed compromise (no successful logins found).
- **Disposition:** Suspicious failed login activity observed; in a real environment, next steps would include:
  - validating source IP ownership and geo/ASN (if external),
  - checking for correlated "Accepted password" or session activity,
  - reviewing sudo/command execution after login.
