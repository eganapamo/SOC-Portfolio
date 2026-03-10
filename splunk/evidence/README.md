# Splunk Evidence

## Objective

Demonstrate practical SIEM log analysis and detection development using Splunk.

The evidence in this folder supports the Splunk labs and shows the actual queries and results used to detect suspicious security activity.

The goal of these searches is to simulate how a Security Operations Center (SOC) analyst uses a SIEM to monitor logs, detect threats, and investigate potential incidents.


## Steps Performed

• Searched authentication and system logs using Splunk queries  

• Extracted important fields from raw log data such as:
- source IP
- username
- host
- executed commands

• Identified abnormal authentication patterns  

• Correlated events across multiple log entries  

• Detected security threats including:
- brute force login attempts
- privilege escalation activity
- suspicious command execution

• Investigated correlated events to determine potential compromise


## Findings

- Authentication logs contained multiple failed login attempts  

- Successful logins occurred following repeated authentication failures  

- Privilege escalation commands were observed within system logs  

- Suspicious command execution activity was identified  

- Correlation searches revealed attacker behavior patterns across events  

- Evidence demonstrates how SIEM queries can detect malicious activity in system logs


## Conclusion

The Splunk queries and evidence demonstrate how security analysts detect and investigate threats using SIEM platforms.

These searches show how log data can be used to identify authentication attacks, privilege escalation attempts, and suspicious command execution.

This workflow reflects common monitoring and investigation practices performed in a Security Operations Center environment.


## Evidence Files

- 01 → Basic log search query  
- 02 → Field extraction from log events  
- 03 → Login failure dashboard analysis  
- 04 → Brute force detection search results  
- 05 → Privilege escalation detection evidence  
- 06 → Suspicious IP alert query results  
- 07 → Threat hunting search query  
- 08 → Correlation search detection results  
- 09 → Notable event investigation evidence