# Lab 2 – Field Extraction Rules

## Objective
The goal of this lab was to extract useful security fields from authentication logs in Splunk. Field extraction allows analysts to convert raw log data into structured information that can be easily searched, filtered, and analyzed during security investigations.

## Environment
SIEM Platform: Splunk
Log Source: Linux authentication logs (auth.log)
Target System: Metasploitable
Attacker Machine: Kali Linux

## Step 1 – Identify Failed Authentication Events
Search for SSH authentication failures to locate brute force attempts.

Search:
index=main "Failed password"

This search revealed repeated login attempts targeting the Metasploitable host.

## Step 2 – Extract Username Field
The username targeted during the failed authentication attempt was extracted using the rex command.

Search:
index=main "Failed password"
| rex "Failed password for (?<user>\w+)"
| table _time host user

Result:
The username field was successfully extracted from the raw log data.

## Step 3 – Extract Attacker IP Address
The source IP address used during the failed login attempt was extracted.

Search:
index=main "Failed password"
| rex "from\s+(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| table _time host user src_ip

Result:
The attacker IP address was successfully identified from the authentication logs.

## Step 4 – Extract Source Port
The SSH port used during the authentication attempt was extracted to provide additional connection context.

Search:
index=main "Failed password"
| rex "port\s+(?<src_port>\d+)"
| table _time host user src_ip src_port

Result:
The network port associated with the login attempt was extracted.

## Step 5 – Identify Successful Authentication
Successful SSH logins were analyzed to determine when access was obtained.

Search:
index=main "Accepted password"
| rex "Accepted password for (?<user>\w+)"
| rex "from\s+(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| table _time host user src_ip

Result:
The successful login event was identified along with the associated user account and source IP.

## Conclusion
This lab demonstrated how field extraction allows analysts to transform raw authentication logs into structured fields such as username, source IP address, and network port. Extracting these fields improves investigative efficiency and allows analysts to correlate login activity, identify brute force attempts, and track attacker behavior across systems.
