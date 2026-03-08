# Lab 9 – Notable Event Investigation

## Objective
Investigate a notable authentication alert to determine whether the activity represents a potential brute-force attack followed by successful access. The goal is to analyze authentication logs, identify the attacker source IP, determine the affected account and host, and evaluate whether suspicious activity occurred after login.

## Environment
Platform: Splunk Enterprise 
Data Source: Linux authentication logs (auth.log) 
Log Type: SSH authentication events 
Test Environment: Kali Linux attacking Metasploitable system

## Step 1 – Identify Authentication Failures
The investigation begins by identifying failed login attempts that may indicate brute-force activity.

Search Query:
index=main sourcetype=syslog "Failed password"

Purpose:
This search identifies repeated failed SSH authentication attempts that could indicate an attacker attempting to guess credentials.

## Step 2 – Extract Source IP Address
To identify the attacker, the source IP address is extracted from the log events.

Search Query:
index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<source_ip>\d+\.\d+\.\d+\.\d+)"

Purpose:
The rex command extracts the attacker IP address from the log line so it can be analyzed as a field.

## Step 3 – Identify Target Username
The username targeted during authentication attempts is extracted from the logs.

Search Query:
index=main sourcetype=syslog "Failed password"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"

Purpose:
This identifies which user account was targeted during the attack attempts.

## Step 4 – Aggregate Failed Login Attempts
The failed authentication attempts are summarized to determine how many attempts were made per attacker IP and user.

Search Query:
index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<source_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| stats count as failed_attempts by source_ip user host
| sort -failed_attempts

Purpose:
This provides visibility into the attacker IP, the targeted account, the affected host, and the number of login failures.

## Step 5 – Identify Successful Login
After detecting repeated failures, the next step is to determine if the attacker successfully authenticated.

Search Query:
index=main sourcetype=syslog "Accepted password"

Purpose:
This identifies successful SSH authentication events.

## Step 6 – Investigate Post-Login Activity
If a successful login occurs, further investigation is required to determine if the attacker executed commands or escalated privileges.

Example Searches:
index=main sourcetype=syslog "sudo"
index=main sourcetype=syslog "chmod"
index=main sourcetype=syslog "useradd"

Purpose:
These searches help identify potential privilege escalation or persistence activity after login.

## Findings
The investigation revealed multiple failed SSH authentication attempts originating from a single IP address. The attacker repeatedly attempted to authenticate against the same account before eventually achieving successful login access.

Indicators observed:
- Multiple failed SSH login attempts
- Successful login from the same attacker IP
- Authentication activity targeting a specific user account

These behaviors strongly indicate a brute-force attack that resulted in successful system access.

## Recommended Response
1. Block the attacker IP address at the firewall or network perimeter.
2. Disable and reset credentials for the compromised account.
3. Investigate all activity performed by the account after the successful login.
4. Review logs for privilege escalation or suspicious command execution.
5. Implement account lockout policies or MFA to prevent future brute-force attempts.

## MITRE ATT&CK Mapping
Technique: T1110 – Brute Force 
Tactic: Credential Access

## Conclusion
This investigation demonstrates how Splunk can be used to detect and analyze brute-force authentication attacks. By extracting key fields such as source IP and username and correlating failed and successful login attempts, analysts can identify compromised accounts and take immediate response actions to secure the system.
