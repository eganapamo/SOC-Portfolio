## Lab 08 — Targeted Username Detection

## Objective
Identify which usernames attackers attempted to access during failed SSH login attempts.


## Log Source
/var/log/auth.log


## Step 1 — Extract Failed Login Attempts
Command:
grep -a "Failed password" /var/log/auth.log

Purpose:
Filters authentication logs to show only failed login attempts.


## Step 2 — Extract Attempted Usernames
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $9}'

Purpose:
Displays usernames attackers attempted to authenticate as.


## Step 3 — Count Attempts Per Username
Command:
grep -a "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -nr

Purpose:
Counts how many times each username was targeted.


## Findings
3 fakeuser
1 ;


## Analysis
- Username **fakeuser** was targeted most frequently.
- Multiple attempts indicate credential guessing or brute-force activity.
- Entry `;` is a parsing artifact from a differently formatted log line.

---

## Conclusion
Attack activity focused primarily on a single username, indicating targeted brute-force behavior rather than random scanning.
