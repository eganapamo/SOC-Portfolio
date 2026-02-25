# Lab 08 – Suspicious Privilege Assignment Detection

## Objective
Detect and investigate unauthorized privilege escalation by identifying users added to high-privilege Linux groups within /var/log/auth.log.


## Environment
- System: Kali Linux
- Log Source: /var/log/auth.log
- Privileged Groups Monitored:
  - sudo
  - wheel
  - admin


## Investigation Process

## Step 1 – Identify Privileged Group Modifications

Executed the following detection query:

sudo grep -aiE ".* to group.*(sudo|wheel|admin)" /var/log/auth.log

Purpose:
- Detect any user added to privileged groups
- Use wildcard matching to capture all usernames
- Limit results to high-risk groups only


## Step 2 – Remove Investigation Command Noise

To eliminate grep commands logged during investigation:

- sudo grep -aiE ".* to group.*(sudo|wheel|admin)" /var/log/auth.log | grep -iv "command="

This ensured only true privilege modification events were displayed.


## Relevant Log Output

- 2026-02-25T08:04:23.807147-05:00 kali usermod[147274]: add 'labuser' to group 'sudo'


## Field Analysis

- Actor Host: kali
- Process: usermod
- Target User: labuser
- Privileged Group: sudo
- Timestamp: 2026-02-25T08:04:23
- Action Type: Group Addition

No additional users were:
- Added to sudo
- Added to wheel
- Added to admin
- Removed from privileged groups
- Modified within monitored privileged groups


## Validation

Confirmed:
- Only one privileged modification occurred
- No mass privilege escalation
- No multiple-user additions
- No additional suspicious group activity

Detection logic successfully filtered all unrelated noise and isolated the true event.


## Security Assessment

This event represents a privilege escalation via group membership modification.

In a real-world environment, the following would be verified:
- Whether the change was approved
- Whether the actor had administrative authorization
- Whether the activity occurred during expected business hours
- Whether the source host/IP was recognized

If unapproved, this would be escalated as potential unauthorized privilege escalation.


## Detection Logic Summary

Regex Pattern Used:
".* to group.*(sudo|wheel|admin)"

Key Concepts Demonstrated:
- Wildcard matching for username flexibility
- Privileged group filtering
- Log noise reduction
- Investigation-focused querying
- Precision detection tuning


## Real-World Adaptation

In production environments:
- Privileged group names vary by organization
- Detection logic must align with business-defined administrative groups
- Monitoring should include file integrity checks for:
  - /etc/group
  - /etc/passwd
  - /etc/shadow
  - /etc/sudoers

This lab demonstrates how to detect privilege assignment events without relying on specific usernames.


## Skills Demonstrated

- Privilege Escalation Detection
- Linux Log Analysis
- Regex Filtering
- Noise Reduction Techniques
- Investigation Workflow Development
- Detection Tuning


## Analyst Conclusion

Only one privileged group modification occurred:
- labuser → added to sudo

No additional unauthorized privilege activity was observed within the monitored groups.

Lab successfully demonstrates detection and investigation of suspicious privilege assignment events.
