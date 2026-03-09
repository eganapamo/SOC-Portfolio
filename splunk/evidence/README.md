# Splunk Evidence

This folder contains screenshots of Splunk search results used as evidence for the detections and investigations documented in this repository.

The screenshots demonstrate real log analysis performed in Splunk Enterprise, showing how security events were identified, correlated, and investigated.

## Purpose

The evidence included in this folder demonstrates the ability to:

- Analyze authentication logs
- Detect brute force attacks
- Investigate privilege escalation attempts
- Identify suspicious command execution
- Perform threat hunting using SIEM queries
- Correlate events across multiple log entries

Each screenshot represents actual search results from the Splunk queries written in the corresponding lab files.

## Evidence Included

### Brute Force Detection Evidence
Shows multiple failed SSH login attempts followed by successful authentication from the same attacker IP.

Evidence demonstrates:

- attacker IP extraction
- failed vs successful authentication counts
- correlation of login events
- compromised account identification

### Privilege Escalation Detection Evidence
Shows commands associated with privilege escalation such as:

- sudo
- su
- chmod
- user account modifications

Evidence demonstrates:

- command execution visibility
- user attribution
- host identification
- command frequency analysis

### Suspicious Command Execution Evidence
Shows detection of potentially malicious command activity such as:

- bash
- python
- sh
- netcat (nc)

Evidence demonstrates:

- process execution visibility
- attacker activity after authentication
- command usage patterns

### Correlation Search Evidence
Demonstrates correlation logic used to detect attack patterns such as:

- multiple failed logins
- successful login from same IP
- suspicious authentication behavior

This type of detection is commonly used in Security Operations Centers to identify successful brute force compromises.

### Notable Event Investigation Evidence
Shows the investigation of a correlated security event including:

- attacker IP
- targeted user account
- affected host
- authentication statistics

This demonstrates the ability to perform SOC-style event triage and investigation.

## Skills Demonstrated

This evidence demonstrates practical skills in:

- SIEM log analysis
- Security event correlation
- Incident investigation
- Detection engineering
- Threat hunting

## Tools Used

- Splunk Enterprise
- Linux syslog authentication logs
- Custom Splunk Search Processing Language (SPL) queries


This evidence supports the detection rules, investigations, and threat analysis documented throughout this SOC Analyst portfolio.

