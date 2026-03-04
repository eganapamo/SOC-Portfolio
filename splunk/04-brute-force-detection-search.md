# Lab 4 – Brute Force Detection Search

## Objective
Detect potential SSH brute force attacks by identifying repeated failed login attempts originating from the same source IP within a short time window. The goal is to identify suspicious authentication activity that may indicate password guessing or credential attacks.

## Environment
SIEM Platform: Splunk 
Data Source: Linux authentication logs 
Index: main 
Sourcetype: syslog 
Target System: Metasploitable VM 
Attacker System: Kali Linux

## Step 1 – Identify Failed SSH Login Attempts
First, search for failed SSH authentication attempts in the syslog data.

index=main sourcetype=syslog "Failed password"

This query returns all failed SSH login attempts recorded in the authentication logs.

## Step 2 – Extract the Source IP Address
The source IP address is not automatically parsed from the raw log event, so it must be extracted using the rex command.

| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"

This regular expression extracts the attacker’s IP address from the log line and stores it in the new field attacker_ip.

## Step 3 – Extract the Username Being Targeted
Next, extract the username involved in the failed authentication attempt.

| rex "for\s+(invalid user\s+)?(?<user>\S+)"

This captures the username following the word "for", handling both valid and invalid user login attempts.

## Step 4 – Group Events into Time Windows
To detect rapid login attempts, group authentication events into 5-minute time windows.

| bin _time span=5m

This command creates time buckets that help identify bursts of login attempts within a short timeframe.

## Step 5 – Count Login Failures Per Source IP
Aggregate the results to determine how many failed login attempts came from each attacker IP within each time window.

| stats count as failures values(user) as usernames by attacker_ip _time

count calculates the total number of failed attempts. 
values(user) lists the unique usernames attempted by the attacker. 
Grouping by attacker_ip and _time allows us to see attack behavior within specific time intervals.

## Step 6 – Filter Suspicious Activity
To reduce noise, filter results to show only events where multiple failures occurred.

| where failures >= 2

This removes normal login mistakes and highlights suspicious activity more likely to indicate brute force attempts.

### Step 7 – Sort Results
Sort the results to prioritize the most aggressive attack activity.

| sort - failures

The dash (-) sorts the results in descending order, showing the highest number of failures first.

## Step 8 – Format the Output
Display only the most relevant fields for easier investigation.

| table attacker_ip failures usernames

This produces a cleaner output focused on the attacker IP, the number of failures, and the usernames targeted.

## Final Detection Query
index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| bin _time span=5m
| stats count as failures values(user) as usernames by attacker_ip _time
| where failures >= 2
| sort - failures
| table attacker_ip failures usernames

## Analysis
The search identifies repeated failed login attempts originating from the same source IP within a short time window. When an IP address attempts authentication with multiple usernames or produces numerous failures, it may indicate brute force or password spraying activity.

Using values(user) allows analysts to quickly identify which accounts were targeted by the attacker during the authentication attempts.

## Conclusion
This lab demonstrated how Splunk can be used to detect SSH brute force attacks by analyzing authentication logs. By extracting attacker IP addresses, grouping events into time windows, and counting repeated failures, analysts can quickly identify suspicious login activity and begin further investigation to determine whether a system compromise occurred.
