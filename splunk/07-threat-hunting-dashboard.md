# Lab 7 – Threat Hunting Dashboard

## Objective
Create a threat hunting dashboard in Splunk that helps identify suspicious authentication activity such as brute-force attacks, password spraying, top attacker IPs, targeted usernames, and successful logins.

## Environment
- Splunk Enterprise
- Linux syslog authentication logs
- Metasploitable VM generating SSH activity
- Kali Linux used to simulate attacker login attempts


## Step 1 – Detect Failed Login Attempts

Start by identifying failed SSH authentication attempts.

Search:

index=main sourcetype=syslog "Failed password"

These logs usually appear as:

Failed password for msfadmin from 192.168.56.104 port 22 ssh2


## Step 2 – Extract Attacker IP Address

If Splunk has not automatically extracted the source IP, use rex.

Search:

index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"

This creates the field:

attacker_ip


## Step 3 – Top Attacker IPs Panel

Aggregate failed login attempts by attacker IP to identify the most active sources.

Search:

index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| stats count as failures by attacker_ip
| sort - failures
| head 10

This panel highlights the most aggressive attacker IP addresses.


## Step 4 – Most Targeted Usernames Panel

Identify which usernames are being targeted most often.

Search:

index=main sourcetype=syslog "Failed password"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| stats count as failures by user
| sort - failures
| head 10

This panel shows the accounts most frequently targeted by attackers.


## Step 5 – Password Spraying Panel

Password spraying occurs when one attacker IP tries many different usernames. 
For this detection, both the number of unique usernames and the actual usernames attempted are useful.

Search:

index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| rex "for\s+(invalid user\s+)?(?<user>\S+)"
| stats dc(user) as targeted_users values(user) as attempted_users count as failures by attacker_ip
| where targeted_users > 5
| sort - targeted_users

This panel detects one IP attempting many usernames and shows which usernames were targeted.


## Step 6 – Successful Login Panel

After detecting suspicious failed login activity, determine whether any attacker eventually succeeded.

Search:

index=main sourcetype=syslog "Accepted password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| table _time host user attacker_ip

This panel shows successful authentication events and helps analysts confirm possible compromise.


## Step 7 – Dashboard Purpose

The dashboard combines these panels:

1. Top Attacker IPs
2. Most Targeted Usernames
3. Password Spraying Detection
4. Successful Login Events

This provides a quick SOC view of authentication-related threats.


## Analysis

The dashboard aggregates suspicious authentication activity and highlights common attacker patterns such as repeated login failures, brute-force attempts, password spraying, and successful logins. By showing both attacker IPs and targeted usernames, analysts can quickly determine whether authentication activity is malicious and whether compromise may have occurred.


## Conclusion

This lab demonstrated how a threat hunting dashboard can improve visibility into authentication attacks. By combining multiple Splunk searches into dashboard panels, analysts can monitor attacker behavior more efficiently, identify high-risk login patterns, and investigate potential compromises faster.
