# Lab 2 CVSS Risk Prioritization

## Objective
The objective of this lab is to analyze multiple vulnerabilities using CVSS scores and determine which vulnerabilities should be prioritized for remediation.


## Scenario
A vulnerability scan detected several vulnerabilities across different systems in the network. The task is to prioritize which vulnerabilities should be remediated first based on severity and potential risk.


## Vulnerability Results

| System | Service | CVSS Score | Severity |
|------|------|------|------|
Web Server | Apache | 9.8 | Critical |
Internal Server | OpenSSH | 7.5 | High |
Employee Workstation | FTP | 5.3 | Medium |
Printer | SMB | 3.1 | Low |


## Risk Prioritization

### Critical Vulnerability
**Web Server — Apache (CVSS 9.8)**

This vulnerability is the highest priority because the system is a public-facing web server. Exploitation could allow attackers to gain remote access or execute code on the server.


### High Vulnerability
**Internal Server — OpenSSH (CVSS 7.5)**

This vulnerability affects an internal system but still poses significant risk if exploited by an attacker who gains internal network access.


### Medium Vulnerability
**Employee Workstation — FTP (CVSS 5.3)**

This vulnerability presents moderate risk and should be addressed during the next patch cycle.


### Low Vulnerability
**Printer — SMB (CVSS 3.1)**

Low-risk vulnerability with minimal impact. This issue can be remediated during routine maintenance.


## Recommended Remediation Strategy

1. Patch critical vulnerabilities immediately.
2. Address high severity vulnerabilities during the next security update cycle.
3. Schedule medium vulnerabilities during maintenance windows.
4. Monitor and document low-risk vulnerabilities.


## Analyst Summary
CVSS scores help security teams prioritize remediation efforts by focusing on vulnerabilities that pose the greatest risk to the organization. Critical and high severity vulnerabilities should always be addressed first to reduce potential attack surface.


## Skills Demonstrated
- CVSS interpretation
- vulnerability prioritization
- risk assessment
- remediation planning
