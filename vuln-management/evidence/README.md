# Vulnerabilty management evidence

This folder contains supporting evidence for the cybersecurity labs documented in this repository.

## Nmap Service Scan

The screenshot shows the results of a service version scan performed against the Metasploitable target host.

Command used:

nmap -sV 192.168.56.102

The scan identifies multiple exposed services including:

- OpenSSH
- Apache
- Samba
- FTP
- PostgreSQL
- MySQL

These services are intentionally outdated in the Metasploitable environment and represent common vulnerabilities that security teams must identify and remediate.

This scan demonstrates how exposed services and software versions can be discovered before performing vulnerability risk analysis.

