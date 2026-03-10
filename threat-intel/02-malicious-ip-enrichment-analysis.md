# Lab 2 – Malicious IP Enrichment Analysis

## Objective
The objective of this lab is to identify suspicious IP addresses from log data and determine whether the IP is known to be malicious by enriching the indicator using external threat intelligence sources.

## Environment
- Splunk Enterprise
- Kali Linux
- Metasploitable
- VirusTotal
- AbuseIPDB

## Step 1 – Identify Suspicious IP Addresses
Search authentication logs for repeated failed login attempts which may indicate brute force activity.

Splunk Search:
index=main sourcetype=syslog "Failed password"
| rex "from\s+(?<attacker_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by attacker_ip
| sort -count

Explanation:
This query extracts source IP addresses from failed SSH login attempts and counts how many times each IP appears.

## Step 2 – Identify Potential Brute Force Sources
Investigate IP addresses that generate a high number of failed login attempts.

Indicators of suspicious activity:
- Large number of failed authentication attempts
- Attempts targeting multiple usernames
- Repeated attempts within a short time period

## Step 3 – Enrich the IP With Threat Intelligence
Select a suspicious IP address from the results and investigate its reputation using external threat intelligence platforms.

Investigation Process:
1. Copy the suspicious IP address from Splunk results
2. Search the IP on AbuseIPDB
3. Search the IP on VirusTotal
4. Review threat intelligence results

Information gathered:
- Abuse confidence score
- Number of abuse reports
- Malware or botnet associations
- Geographic origin of the IP

## Step 4 – Determine Risk Level
Evaluate whether the IP address is likely malicious.

Indicators of malicious IP activity include:
- High abuse confidence score
- Multiple reports of brute force activity
- Known botnet or scanning infrastructure

## Analyst Assessment
If the IP address is confirmed malicious, it likely represents an attacker attempting unauthorized access through brute force authentication attempts.

Recommended Response:
- Block the IP address at the firewall
- Implement rate limiting or account lockout policies
- Monitor for additional authentication attempts
- Review successful login attempts from the same IP

## Conclusion
This lab demonstrated how SOC analysts extract suspicious IP addresses from logs and enrich them using external threat intelligence sources. IP enrichment allows analysts to quickly determine whether an IP address is associated with known malicious activity and helps prioritize security investigations.
