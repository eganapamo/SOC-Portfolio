# Lab 3 – Login Failure Dashboard

## Objective
The objective of this lab was to analyze SSH authentication failures and build a basic login failure dashboard in Splunk. The goal was to identify attacker IP addresses, targeted usernames, and failure trends over time to better understand brute force login attempts.

## Environment
SIEM Platform: Splunk
Log Source: Linux Authentication Logs (auth.log)
Target System: Metasploitable
Attacker Machine: Kali Linux

## Step 1 – Identify SSH Login Failures
The first step was to locate authentication failures in the logs.

Search:
index=main sourcetype=syslog "Failed password"

Result:
Multiple failed login attempts were identified, indicating potential brute force activity targeting the system.

## Step 2 – Extract Attacker Source IP
The attacker IP address was extracted from the log using the rex command.

Search:
index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count as failures by src_ip
| sort - failures
| head 10

Result:
This search identified the top attacker IP addresses responsible for the most failed login attempts.

## Step 3 – Identify Targeted Usernames
The usernames targeted during the brute force attempts were extracted and analyzed.

Search:
index=main sourcetype=syslog "Failed password"
| rex "Failed password for (invalid user\s+)?(?< user >\S+)"
| stats count as failures by user
| sort - failures
| head 10

Result:
This search revealed which user accounts were being targeted the most by the attacker.

## Step 4 – Visualize Login Failures Over Time
Login failures were analyzed over time to detect spikes in activity.

Search:
index=main sourcetype=syslog "Failed password"
| timechart span=1m count as failures

Result:
The time-based visualization showed patterns of login failure activity, helping identify periods of increased attack attempts.

## Step 5 – Identify Targeted Hosts
Failed login attempts were grouped by destination host.

Search:
index=main sourcetype=syslog "Failed password"
| stats count as failures by host
| sort - failures

Result:
This analysis showed which systems were receiving the most authentication failure attempts.

## Conclusion
This lab demonstrated how Splunk can be used to detect and analyze SSH brute force login attempts. By extracting attacker IP addresses, targeted usernames, and analyzing login failure trends over time, analysts can quickly identify suspicious authentication activity and investigate potential intrusion attempts.
