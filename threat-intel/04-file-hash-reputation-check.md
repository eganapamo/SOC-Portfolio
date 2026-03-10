# Lab 4 – File Hash Reputation Check

## Objective

Analyze suspicious file hashes observed in system activity by checking their reputation using malware intelligence platforms.

The goal is to determine whether the file hash corresponds to known malware or malicious software.


## Scenario

During system monitoring or malware investigation, a suspicious file was identified.

File hashes are commonly used to uniquely identify files and determine whether they match known malicious samples.

Example hash indicator:

SHA256: example-file-hash


## Data Source

File hashes may be extracted from:

- malware alerts
- antivirus detections
- file monitoring systems
- endpoint detection tools

Threat intelligence platforms are used to determine whether the hash corresponds to known malware.


## Step 1 – Identify Suspicious File Hash

File hashes can be obtained from system logs, malware alerts, or endpoint monitoring tools.

Example hash:

example-file-hash


## Step 2 – Perform Hash Reputation Lookup

Check the file hash using malware intelligence platforms.

Example sources:

- VirusTotal 
- Hybrid Analysis 
- MalwareBazaar 
- AlienVault OTX 

These platforms provide information such as:

- malware classification
- detection rates
- sandbox analysis results
- associated malware families


## Step 3 – Analyze Reputation Results

Threat intelligence platforms may reveal:

- antivirus detection results
- malware family classification
- behavior observed in sandbox environments
- related malicious infrastructure

This information helps analysts determine whether the file is malicious.


## Findings

Threat intelligence analysis determines whether the file hash corresponds to known malware samples.

Malicious file hashes often appear in multiple malware databases and may have high detection rates across antivirus engines.


## Conclusion

File hash reputation checks allow analysts to quickly determine whether a file is associated with known malware.

This process enables security teams to identify malicious files and respond appropriately by isolating infected systems and removing malicious software.
