# SOC Incident Investigations

This folder contains simulated Security Operations Center (SOC) incident investigations based on log analysis performed in Splunk.

Each investigation demonstrates how a security analyst analyzes suspicious activity, correlates events, reconstructs attacker behavior, and determines the impact of a potential compromise.

The investigations simulate real-world SOC workflows including authentication analysis, command execution tracking, privilege escalation detection, and log tampering investigation.


## Investigation Methodology

Each investigation follows a structured SOC analysis process:

1. Identify suspicious activity within system logs 
2. Extract key fields such as:
   - attacker IP
   - username
   - host
   - executed commands
   - timestamps 
3. Correlate events across multiple log entries 
4. Reconstruct the attacker’s activity timeline 
5. Determine potential impact and recommend response actions 

This methodology reflects common Security Operations Center investigation practices.


## Investigations Included

## 01 – SSH Brute Force Attack
Investigation of repeated SSH authentication failures followed by successful login attempts.

Key indicators analyzed:
- Failed password attempts
- Successful authentication
- Attacker IP identification
  

## 02 – Privilege Escalation
Analysis of commands associated with privilege escalation activity.

Examples analyzed:
- sudo
- su
- permission modification commands

The investigation determines whether an attacker attempted to gain elevated privileges on the system.


## 03 – Suspicious Command Execution
Investigation of suspicious command execution that may indicate attacker activity.

Examples include:
- bash
- python
- shell execution
- netcat usage

This analysis helps determine whether an attacker executed tools after gaining system access.


## 04 – Sensitive File Access
Investigation of commands used to access sensitive files.

Examples analyzed:
- cat
- less
- more
- nano
- vim

This helps determine whether an attacker attempted to read system configuration files or sensitive data.


## 05 – Log Tampering
Investigation of commands associated with log manipulation.

Examples analyzed:
- history clearing
- environment variable manipulation
- log file modification

This type of activity often indicates an attacker attempting to hide evidence of their actions.


## Skills Demonstrated

These investigations demonstrate practical SOC analyst skills including:

- Log analysis
- Event correlation
- Threat hunting
- Incident investigation
- Attacker activity reconstruction
- Security incident reporting


## Tools Used

- Splunk Enterprise 
- Linux syslog authentication logs 
- Splunk Search Processing Language (SPL)


These investigations simulate the types of security incidents that analysts investigate daily in a Security Operations Center environment.
