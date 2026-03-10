# Lab 1 – IOC Extraction From Logs

## Objective
The objective of this lab is to identify Indicators of Compromise (IOCs) such as IP addresses, domains, and file hashes from system logs. Extracting IOCs allows security analysts to quickly identify potentially malicious activity and investigate threats within an environment.

## Environment
- Splunk Enterprise
- Kali Linux
- Metasploitable
- Linux syslog data

## Step 1 – Identify Indicators of Compromise in Logs
Security analysts first search logs for potential indicators such as IP addresses, domains, or file hashes.

Splunk Search:
index=* sourcetype=syslog

Explanation:
This search retrieves system log data where potential indicators may appear.

## Step 2 – Extract IP Addresses
Use regex extraction to identify source IP addresses within the logs.

Splunk Search:
index=* sourcetype=syslog
| rex "(?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| sort -count

Explanation:
The rex command extracts IP addresses and the stats command counts how frequently each IP appears in the logs.

## Step 3 – Extract Domain Indicators
Domains may appear in logs when systems connect to external services.

Splunk Search:
index=* sourcetype=syslog
| rex "(?<domain>[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})"
| where isnotnull(domain)
| stats count by domain
| sort count

Explanation:
The regex extracts domain names from logs. Rare domains can sometimes indicate suspicious or malicious activity.

## Step 4 – Identify Rare or Suspicious Indicators
Security analysts often focus on rare indicators because malicious infrastructure typically appears infrequently in logs.

Indicators of interest include:
- Rare IP addresses
- Rare domains
- Unusual connection patterns
- Indicators appearing during abnormal hours

## Step 5 – Investigate the Indicator
Once an indicator is identified, the analyst investigates it using external threat intelligence sources.

Investigation tools include:
- VirusTotal
- AbuseIPDB
- WHOIS lookup
- Threat intelligence feeds

## Analyst Assessment
Extracting indicators of compromise allows analysts to identify suspicious activity quickly. Indicators such as unknown IP addresses or rare domains should be investigated to determine whether they are associated with malicious infrastructure.

## Conclusion
This lab demonstrated how to extract indicators of compromise from logs using Splunk. IOC extraction is a critical step in threat hunting and incident response because it allows analysts to identify and investigate suspicious activity within a network environment.
