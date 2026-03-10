# Lab 2 Malicious IP Enrichment Analysis

## Objective

Enrich extracted Indicators of Compromise (IOCs) by checking attacker IP addresses against external threat intelligence sources.

The goal is to determine whether an IP address observed in system logs is associated with known malicious activity.


## Scenario

During log analysis, a suspicious IP address was identified performing multiple authentication attempts against a Linux system.

The objective of this lab is to determine whether the IP address is associated with known malicious activity by consulting threat intelligence platforms.

Example IOC extracted from logs:

Attacker IP: 192.168.56.104


## Data Source

Indicator extracted from:

Linux authentication logs analyzed in Splunk.

Source:

index=main 
sourcetype=syslog

The IP address was identified during brute-force authentication activity.


## Step 1 – Identify Suspicious IP From Logs

### Splunk Query

index=main sourcetype=syslog ("Failed password" OR "Accepted password")
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by attacker_ip
| sort -count

### Purpose

Extract source IP addresses responsible for authentication attempts and identify suspicious activity patterns.


## Step 2 – Select Suspicious IP Indicator

From the extracted indicators, select the IP address responsible for multiple authentication attempts.

Example indicator:

192.168.56.104

This IP will be used for threat intelligence enrichment.


## Step 3 – Check IP Reputation Using Threat Intelligence Platforms

Investigate the extracted IP address using threat intelligence platforms.

Example sources:

- AbuseIPDB 
- VirusTotal 

These platforms provide information about known malicious infrastructure.


## Step 4 – Analyze Reputation Results

Threat intelligence platforms provide information such as:

- Abuse reports associated with the IP 
- Malware infrastructure connections 
- Command and control activity 
- Historical attack campaigns 
- Geographic location and ASN ownership 

Analysts use this information to determine whether the IP address has been associated with malicious activity.


## Step 5 – Correlate Threat Intelligence With Log Activity

Combine log data with reputation intelligence.

Observed behavior from logs:

- Multiple failed SSH authentication attempts 
- Repeated login attempts targeting system accounts 

Threat intelligence enrichment determines whether the IP has been associated with:

- brute-force attacks 
- botnet infrastructure 
- scanning activity 
- known attacker infrastructure


## Findings

Analysis revealed the IP address performing authentication attempts was associated with suspicious behavior patterns consistent with brute-force login attempts.

Threat intelligence enrichment helps analysts determine whether the activity originates from known malicious infrastructure.


## Conclusion

Threat intelligence enrichment allows analysts to add context to indicators extracted from logs.

By correlating authentication activity with external intelligence sources, analysts can determine whether suspicious activity originates from known malicious infrastructure.

This process improves incident response decision-making and helps security teams prioritize threats.
