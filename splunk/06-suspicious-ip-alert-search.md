# Lab 6 – Suspicious IP Alert Search

## Objective
Identify suspicious source IP addresses performing repeated authentication attempts and determine whether those IPs successfully authenticated. If successful authentication occurs, pivot the investigation to the compromised user and host to analyze post-authentication activity.

## Environment
- Splunk Enterprise
- Linux syslog authentication logs
- Metasploitable VM generating SSH activity
- Kali Linux used to simulate attacker login attempts


## Step 1 – Detect Failed Login Attempts

The investigation begins by identifying failed SSH login attempts which may indicate a brute-force attack.

Search:

index=main sourcetype=syslog "Failed password"

Example Log:

Failed password for msfadmin from 192.168.56.104 port 22 ssh2


## Step 2 – Extract Attacker IP Address

If Splunk has not automatically extracted the IP address, use the rex command to extract it from the raw event.

Search:

index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"

This creates a new field:

attacker_ip


## Step 3 – Identify Top Attacker IPs

Aggregate failed login attempts to identify the most active attacker IP addresses.

Search:

index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| stats count as failures by attacker_ip
| sort - failures

Example Result:

attacker_ip        failures
192.168.56.104     23

This indicates repeated brute-force attempts from that IP.


## Step 4 – Check if the Attacker Successfully Logged In

After identifying a suspicious IP, determine whether the attacker eventually succeeded.

Search:

index=main sourcetype=syslog "Accepted password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| search attacker_ip=192.168.56.104
| table _time host user attacker_ip

Example Event:

Accepted password for msfadmin from 192.168.56.104 port 22 ssh2

If results appear, the attacker successfully authenticated.

## Step 5 – Pivot Investigation to Host and User Activity

Once authentication succeeds, the investigation shifts from the attacker IP to the compromised host and user account.

Search:

index=main sourcetype=syslog
("sudo" OR "su" OR "adduser" OR "useradd" OR "usermod" OR "chmod")
| table _time host user COMMAND

This reveals post-login activity such as:

/bin/su
/bin/chmod
/usr/sbin/adduser
/usr/sbin/usermod

These commands may indicate privilege escalation or persistence attempts.


## Analysis

Multiple failed SSH login attempts were detected from a suspicious source IP. 
Further investigation confirmed whether the same IP successfully authenticated. 

After authentication succeeded, the investigation pivoted from the attacker IP to the compromised host and user account to analyze commands executed after login.


## Conclusion

This lab demonstrated how SOC analysts identify suspicious IP addresses performing brute-force login attempts, determine whether the attacker gained access, and pivot the investigation to host and user activity to detect post-compromise behavior.
