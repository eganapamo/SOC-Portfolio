# Failed Login Log Field Analysis

## Objective
Analyze authentication logs to identify which fields contain attacker usernames and IP addresses during failed login attempts.


## Log Source
/var/log/auth.log


## Step 1 — Identify Failed Log Events
grep -a "Failed password" /var/log/auth.log

Confirmed presence of failed login attempts.


## Step 2 — Count Total Failed Attempts
grep -a "Failed password" /var/log/auth.log | wc -l

Result:
4


## Step 3 — Determine Field Structure
To understand log structure, print number of fields and full line:

grep -a "Failed password" /var/log/auth.log | awk '{print NF, $0}'

Result showed:

14 fields per log entry


## Step 4 — Identify Username Field
Testing field positions:

grep -a "Failed password" /var/log/auth.log | awk '{print $3}'

Output showed process name, not username.

Further analysis revealed username appears in **field 9**.

Confirmed:

grep -a "Failed password" /var/log/auth.log | awk '{print $9}'

Output:
fakeuser
fakeuser
fakeuser


## Step 5 — Identify Source IP Field
Inspection showed IP address located in **field 11**.

Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $11}'

Output:
::1
::1
::1


## Analyst Conclusion
The authentication log uses a consistent field structure:

| Field | Meaning |
|------|--------|
$9 | Username |
$11 | Source IP |

Repeated failed attempts from same username and IP indicate brute-force activity.


## Skills Demonstrated
- Log parsing
- Field analysis
- Pattern validation
- Command-line investigation
- Security reasoning

---

## Analyst Notes
This lab strengthened understanding of log field structures and how attackers appear within authentication logs. Correct field identification is critical for building accurate detection logic and SIEM queries.
