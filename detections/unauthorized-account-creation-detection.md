# Lab 09 – Unauthorized Account Creation Detection

## Objective
Detect unauthorized account creation events in Linux by analyzing authentication logs and extracting relevant indicators.


## Scenario

- An alert indicates that a new user account may have been created on the system. 
- The goal is to confirm whether a new user was added, identify the username, and extract relevant metadata from `/var/log/auth.log`.


## Step 1 – Search for Account Creation Events

- We searched for `adduser` and `useradd` events inside the authentication log.

sudo grep -aiE "adduser|useradd" /var/log/auth.log | grep -iv "command"

## Output

2026-02-25T08:03:04.084154-05:00 kali useradd[146599]: new user: name=labuser, UID=1001, GID=1001, home=/home/labuser, shell=/bin/bash, from=/dev/pts/1


## Step 2 – Extract Username

To isolate the username from the log line:

sudo grep -aiE "adduser|useradd" /var/log/auth.log \
| grep -iv "command" \
| awk -F"name=" '{print $2}' \
| awk -F"," '{print $1}'

## Output
labuser


## Step 3 – Key Log Indicators Identified

From the log entry:

| Field | Value |
|-------|-------|
| Timestamp | 2026-02-25 08:03:04 |
| Host | kali |
| Process | useradd |
| Username Created | labuser |
| UID | 1001 |
| GID | 1001 |
| Home Directory | /home/labuser |
| Shell | /bin/bash |
| Source | /dev/pts/1 (local terminal session) |


## Analysis

- Account was created using `useradd`
- Executed from local terminal (`pts/1`)
- No remote IP observed
- No bulk account creation
- No immediate privilege escalation detected in this event

On its own, this event is not automatically malicious. 
However, in a real-world SOC environment, the following pivots would be required:

- 1. Identify who executed `useradd`
- 2. Determine whether `sudo` was used
- 3. Check if the new user was added to privileged groups (sudo, admin, wheel)
- 4. Review subsequent login activity
- 5. Investigate any post-creation privilege escalation


## Detection Logic Summary

This lab demonstrates how to:

- Detect account creation events in auth logs
- Remove command noise from log searches
- Extract specific log fields using `awk`
- Perform initial triage on account creation alerts


## Conclusion

A new user account `labuser` was successfully detected through log analysis. 
The extraction and filtering process reduced noise and isolated relevant indicators, simulating real SOC workflow for unauthorized account creation detection.
