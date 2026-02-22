# Investigations Lab Series

## Overview
This folder contains hands-on security investigation labs focused on analyzing Linux authentication logs and identifying suspicious activity patterns. Each lab demonstrates practical SOC analyst skills including log parsing, event correlation, attacker behavior analysis, and incident reasoning.

These exercises simulate real-world investigation workflows performed during security monitoring and incident response.


## Objectives
The purpose of this investigation series is to demonstrate the ability to:

- Parse raw authentication logs
- Extract relevant forensic fields
- Identify attack indicators
- Correlate multi-event activity
- Detect brute force attempts
- Trace privilege escalation
- Reconstruct attacker timelines
- Identify lateral movement indicators
- Produce structured incident reports


## Tools & Techniques Used
- grep (pattern filtering)
- awk (field extraction)
- sort / uniq (aggregation)
- Linux log analysis
- Event correlation
- Timeline reconstruction
- Behavioral analysis


## Lab Index

| Lab | Title | Focus |
|-----|------|------|
| 01 | Failed Login Field Analysis | Log parsing |
| 02 | IP Extraction | Source tracing |
| 03 | Username Extraction | Account targeting |
| 04 | Timestamp Analysis | Time correlation |
| 05 | Login Pattern Correlation | Behavioral patterns |
| 06 | Suspicious Login Analysis | Threat detection |
| 07 | SSH Brute Force Analysis | Attack detection |
| 08 | Targeted Username Detection | Recon behavior |
| 09 | Multi-Event Login Attempt Correlation | Event chaining |
| 10 | Session Timeline Reconstruction | Timeline analysis |
| 11 | Privilege Escalation Trace | Root activity tracking |
| 12 | Lateral Movement Indicators | Host movement detection |
| 13 | Incident Summary Reporting | Analyst reporting |


## Methodology
Each investigation follows a structured SOC workflow:

1. Identify suspicious activity
2. Extract relevant fields
3. Correlate related events
4. Determine attacker behavior
5. Validate findings
6. Document results


## Skills Demonstrated
This investigation set validates practical competency in:

- Log analysis
- Threat detection
- Incident investigation
- Security reasoning
- Command-line forensics
- Analytical thinking


## Analyst Note
All analysis performed manually using command-line tools to demonstrate foundational investigative capability without reliance on automated detection platforms.
