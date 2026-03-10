# Threat Intelligence Evidence

## Objective

This folder contains supporting screenshots and artifacts for the Threat Intelligence labs. 
The evidence demonstrates how indicators of compromise (IOCs) were extracted from logs and investigated using threat intelligence sources.

The purpose of this evidence is to show the analytical workflow used by SOC analysts to identify, enrich, and validate suspicious indicators.

## Evidence Overview

Each file corresponds to a specific threat intelligence investigation performed in the lab environment.

Evidence includes:

• Splunk queries used to extract indicators 
• Investigation results within Splunk 
• Threat intelligence reputation checks 
• External verification using intelligence platforms

## Evidence Files

- 01 IOC extraction from logs 
- 02 AbuseIPDB example
- 03 Domain reputation investigation 
- 04 File hash reputation check
- 05 Virustotal example 

## Investigation Workflow

The typical threat intelligence workflow followed in these labs:

1. Identify potential indicators in logs 
2. Extract indicators using Splunk searches and regex 
3. Analyze frequency and context of indicators 
4. Enrich indicators using external intelligence sources 
5. Determine whether the indicator is benign or malicious

## Intelligence Sources Used

The following threat intelligence platforms were used during investigations:

• VirusTotal 
• AbuseIPDB 

These platforms provide reputation information that helps analysts determine whether an indicator is associated with malicious activity.

## Conclusion

The evidence contained in this folder demonstrates the process of extracting and validating indicators of compromise. Threat intelligence enrichment is an essential skill for SOC analysts because it allows security teams to quickly determine whether suspicious activity is linked to known malicious infrastructure.

