# Lab 3 Patch Prioritization Analysis

## Objective
The objective of this lab is to analyze multiple vulnerabilities across different systems and determine which systems should be patched first based on risk exposure, business impact, and severity.


## Scenario
A vulnerability scanner detected several outdated services across the organization’s infrastructure. Security analysts must determine which systems should be prioritized for patching.


## Vulnerability Scan Results

| System | Service | Exposure | CVSS Score | Severity |
|------|------|------|------|------|
Public Web Server | Apache | Internet Facing | 7.5 | High |
Internal File Server | SMB | Internal Network | 8.8 | High |
Employee Workstation | FTP | Internal Network | 6.2 | Medium |
Network Printer | SMB | Internal Network | 3.4 | Low |


## Patch Prioritization Analysis

### Highest Priority
**Public Web Server – Apache**

Although the CVSS score is slightly lower than the internal file server, this system is exposed to the internet. Internet-facing systems are more likely to be targeted by attackers, making this vulnerability the highest priority.


### Second Priority
**Internal File Server – SMB**

This system has a higher CVSS score and stores organizational data. While it is not internet-facing, an attacker who gains internal access could exploit this vulnerability to access sensitive files.


### Third Priority
**Employee Workstation – FTP**

This vulnerability presents moderate risk but has limited impact compared to server infrastructure.


### Lowest Priority
**Network Printer – SMB**

The printer vulnerability presents minimal impact and does not store sensitive information. This issue can be addressed during routine maintenance.


## Patch Strategy

Recommended remediation order:

1. Patch the public web server immediately
2. Patch the internal file server during the next maintenance window
3. Update employee workstation services
4. Address printer vulnerabilities during routine maintenance


## Security Considerations

When prioritizing patches, security teams consider:

- CVSS severity score
- system exposure (internet vs internal)
- business impact
- sensitivity of stored data

These factors help determine which vulnerabilities pose the greatest risk.


## Analyst Summary
Effective vulnerability management requires prioritizing vulnerabilities based on risk, not just severity scores. Internet-facing systems and critical infrastructure should always receive the highest remediation priority.


## Skills Demonstrated
- vulnerability risk analysis
- patch prioritization strategy
- CVSS interpretation
- defensive security planning
