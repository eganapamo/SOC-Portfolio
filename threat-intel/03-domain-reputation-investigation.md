# Lab 3 – Domain Reputation Investigation

## Objective

Investigate suspicious domains observed in system logs by checking their reputation using threat intelligence platforms.

The goal is to determine whether a domain is associated with malicious activity such as phishing, malware distribution, or command-and-control infrastructure.


## Scenario

During log analysis, a domain was observed in system activity. Suspicious domains may indicate phishing campaigns, malware downloads, or communication with attacker infrastructure.

Threat intelligence analysis is performed to determine whether the domain is associated with known malicious activity.

Example domain indicator:

suspicious-domain.example


## Data Source

Indicators extracted from system logs and network activity.

Example log source:

index=main 
sourcetype=syslog

Domains may appear in logs through:

- web requests
- command execution
- malware communication
- phishing activity


## Step 1 – Search Logs for Domain Activity

### Splunk Query

index=main sourcetype=syslog
| rex "(?<domain>[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})"
| stats count by domain
| sort -count

### Purpose

Extract domains appearing in system logs and identify potentially suspicious domains.


## Step 2 – Identify Suspicious Domain

Select domains that appear unusual or are associated with suspicious system behavior.

Example indicator:

suspicious-domain.example


## Step 3 – Perform Domain Reputation Lookup

Investigate the domain using threat intelligence platforms.

Example sources:

- VirusTotal 
- AlienVault OTX 

These platforms provide information such as:

- malware associations
- phishing campaigns
- command-and-control activity
- domain reputation scores


## Step 4 – Analyze Threat Intelligence Results

Threat intelligence sources may reveal:

- previous abuse reports
- malware infrastructure connections
- known phishing campaigns
- suspicious hosting providers

Analysts review these indicators to determine whether the domain is malicious.


## Findings

Threat intelligence investigation determined whether the domain had been previously associated with malicious infrastructure or suspicious activity.

Domains with negative reputation scores or abuse reports may indicate malicious infrastructure.


## Conclusion

Domain reputation investigation helps analysts determine whether system activity is associated with known malicious domains.

Threat intelligence enrichment provides context that assists security teams in identifying phishing campaigns, malware infrastructure, and attacker communication channels.
