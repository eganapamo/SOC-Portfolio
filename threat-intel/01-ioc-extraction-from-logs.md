# lab 1– IOC Extraction From Logs

## Objective

Identify potential **Indicators of Compromise (IOCs)** from system authentication logs by extracting suspicious IP addresses, usernames, and activity patterns.

The goal is to demonstrate how analysts extract threat indicators from logs and prepare them for **threat intelligence enrichment and correlation**.


## Scenario

Authentication logs contain multiple login attempts to a Linux system.

The objective is to:

- Extract attacker IP addresses
- Identify targeted usernames
- Analyze login patterns
- Determine potential brute-force activity

These extracted indicators can later be enriched using **threat intelligence feeds**.


## Data Source

Index: main 
Sourcetype: syslog 
Source: Linux authentication logs

These logs contain:

- Failed SSH login attempts
- Successful SSH logins
- Source IP addresses
- Targeted usernames


## Step 1 – Identify Authentication Activity

### Splunk Query

index=main sourcetype=syslog ("Failed password" OR "Accepted password")

### Purpose

This search identifies authentication attempts within the system logs.


## Step 2 – Extract Attacker IP Addresses

### Splunk Query

index=main sourcetype=syslog ("Failed password" OR "Accepted password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by attacker_ip
| sort -count

### Purpose

This query extracts attacker IP addresses and counts the number of login attempts associated with each.


## Step 3 – Extract Targeted Usernames

### Splunk Query

index=main sourcetype=syslog "Failed password"
| rex "for\s+(invalid user\s+)?(?<username>\S+)"
| stats count by username
| sort -count

### Purpose

This search extracts usernames from failed login attempts to identify targeted accounts.


## Step 4 – Correlate IP Addresses With Usernames

### Splunk Query

index=main sourcetype=syslog ("Failed password" OR "Accepted password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<username>\S+)"
| stats count by attacker_ip username
| sort -count

### Purpose

This correlation reveals attacker behavior patterns and targeted accounts.


## Step 5 – Identify Potential Brute Force Activity

### Splunk Query

index=main sourcetype=syslog ("Failed password" OR "Accepted password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| eval status=if(searchmatch("Failed password"),"failed","success")
| stats count(eval(status="failed")) as failed_attempts
        count(eval(status="success")) as successful_attempts
        by attacker_ip
| where failed_attempts > 5

### Purpose

This identifies IP addresses that show signs of brute-force login activity.


## Findings

Analysis of authentication logs revealed:

- Multiple failed login attempts targeting system accounts
- Repeated login attempts from the same source IP address
- Login patterns consistent with brute-force authentication attempts

These indicators represent potential **Indicators of Compromise (IOCs)**.


## Conclusion

Threat intelligence analysis often begins with extracting IOCs directly from system logs.

By identifying suspicious IP addresses and targeted usernames, analysts can generate indicators that can be enriched with external threat intelligence sources such as:

- IP reputation databases
- Threat intelligence feeds
- Malware command and control infrastructure

This process enables security teams to identify potential attackers and improve detection capabilities.
