# Lab 4 – File Hash Reputation Investigation

## Objective
The objective of this lab is to identify suspicious file hashes in system logs and determine whether the file is malicious by performing threat intelligence reputation checks using external intelligence sources such as VirusTotal.

## Environment
- Splunk Enterprise
- Kali Linux
- Metasploitable
- Public threat intelligence platforms (VirusTotal)

## Step 1 – Identify Suspicious File Hashes in Logs
Search the logs for potential file hashes that may appear in endpoint activity.

Splunk Search:
index=*
| rex "(?<file_hash>[A-Fa-f0-9]{32,64})"
| stats count by file_hash
| sort -count

Explanation:
This search extracts possible file hashes using regex and counts how many times each hash appears in the logs.

## Step 2 – Identify Unique File Hashes
Identify hashes that appear rarely in the environment since uncommon hashes may indicate suspicious or newly introduced files.

Splunk Search:
index=*
| rex "(?<file_hash>[A-Fa-f0-9]{32,64})"
| stats count by file_hash
| sort count

Explanation:
Sorting by lowest count allows analysts to identify rare files that could be malicious.

## Step 3 – Select a Suspicious Hash
Choose a hash value from the search results for further investigation.

Example:
44d88612fea8a8f36de82e1278abb02f

## Step 4 – Perform Threat Intelligence Lookup
Investigate the hash using a threat intelligence platform.

Process:
1. Open VirusTotal
2. Paste the file hash into the search bar
3. Review detection results and reputation

Information gathered:
- Malware classification
- Number of security engines detecting the file
- Behavior indicators
- Malware family identification

## Step 5 – Determine Risk Level
Based on the threat intelligence results determine whether the file is malicious or benign.

Indicators of malicious activity include:
- Multiple antivirus detections
- Known malware family
- Associated malicious behavior

## Analyst Assessment
If the hash is flagged by multiple detection engines, the file should be considered malicious and investigated further.

Recommended actions:
- Identify the host where the file executed
- Isolate the affected system
- Remove the malicious file
- Investigate persistence or lateral movement activity

## Conclusion
This lab demonstrated how to extract file hashes from logs, identify suspicious or rare files, and validate their reputation using threat intelligence platforms. File hash analysis is a critical step in malware detection and incident investigation within SOC environments.
