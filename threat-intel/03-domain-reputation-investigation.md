# Lab 3 – Domain Reputation Investigation

## Objective
The objective of this lab is to identify suspicious domains within log data and determine whether the domain may be malicious by performing threat intelligence reputation checks using external intelligence sources.

## Environment
- Splunk Enterprise
- Kali Linux
- Metasploitable
- VirusTotal
- WHOIS lookup tools

## Step 1 – Extract Domains From Logs
Search the logs for potential domains that appear in network activity.

Splunk Search:
index=*
| rex "(?<domain>[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})"
| stats count by domain
| sort count

Explanation:
The regex extracts domain names from logs and counts how often each domain appears.

## Step 2 – Identify Rare or Unusual Domains
Domains with very low frequency may indicate suspicious activity such as phishing or command-and-control communications.

Splunk Search:
index=*
| rex "(?<domain>[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})"
| stats count by domain
| sort count

Explanation:
Sorting by the lowest count allows analysts to quickly identify unusual or rare domains.

## Step 3 – Filter Valid Domains
Remove null values to ensure only real domains are analyzed.

Splunk Search:
index=*
| rex "(?<domain>[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})"
| where isnotnull(domain)
| stats count by domain
| sort count

Explanation:
The "where isnotnull(domain)" command ensures that only extracted domains are displayed.

## Step 4 – Investigate Suspicious Domains
Choose a domain from the results and investigate it using threat intelligence platforms.

Investigation steps:
1. Search the domain on VirusTotal
2. Check domain age with WHOIS
3. Review detection engines
4. Look for known malware or phishing associations

Example suspicious domain indicators:
- Recently registered domains
- Rare domains appearing only once
- Domains flagged by multiple security engines

## Step 5 – Determine Risk Level
Evaluate whether the domain is malicious or benign based on threat intelligence results.

Indicators of malicious domains:
- Multiple malware detections
- Newly registered domains
- Known phishing infrastructure
- Command and control indicators

## Analyst Assessment
If the domain is flagged as malicious, further investigation should determine which hosts contacted the domain and what activity followed.

Recommended response:
- Block the domain at the firewall or DNS level
- Investigate affected hosts
- Review user activity related to the domain
- Monitor for additional suspicious network activity

## Conclusion
This lab demonstrated how to extract domain names from logs, identify suspicious domains based on frequency, and validate domain reputation using threat intelligence sources. Domain reputation analysis is an important technique used by SOC analysts to detect phishing infrastructure and command-and-control activity.
