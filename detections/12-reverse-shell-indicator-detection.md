# LAB 12 – Reverse Shell Indicator Detection

## Objective:
Detect potential reverse shell activity by identifying execution tools commonly used by attackers after gaining access to a system. The goal is to uncover signs of command execution that indicate interactive control of the host.

## Environment:
Platform: Kali Linux
Log Source: /var/log/auth.log
Tools Used: grep

## Step 1 – Identify Execution Indicators
Search for shell and execution-related tools that attackers commonly use after gaining access.

Command:
grep -aEi '\bbash\b|\bsh\b|\bpython\b|\bnc\b' /var/log/auth.log

Explanation:
This command filters authentication logs for activity involving execution tools such as bash, sh, python, or netcat (nc). These are commonly used to establish interactive access or reverse shells.

## Step 2 – Analyze Output
Review returned entries for evidence of command execution activity.

## Observed Indicators:
- shell=/bin/bash
- Commands containing bash execution
- Execution chains involving tools such as chmod, wget, curl, bash, or sh

These entries indicate that commands were actively run on the system rather than simple authentication attempts.

## Step 3 – Correlate with Post-Access Behavior
Reverse shell activity often follows successful access. Execution tools appearing in logs suggest the attacker may be interacting directly with the system.

## Execution tools detected:
- bash
- sh
- python
- nc

These tools are commonly used to:
- Run remote commands
- Establish command-and-control sessions
- Maintain interactive shell access

## Step 4 – Identify Attack Phase
Based on observed behavior, activity aligns with the execution phase of an attack lifecycle.

Typical attack sequence:
1. Initial access (login attempts)
2. Command execution (bash, sh, python, nc)
3. Tool download (wget, curl)
4. Privilege escalation (sudo, chmod)
5. Persistence (useradd, usermod, groupadd)

Presence of execution tools suggests attacker activity beyond authentication.

## Step 5 – Security Significance
Shell execution within authentication logs is a high-risk indicator because it implies:
- Interactive command usage
- Potential reverse shell establishment
- Active system manipulation

This is not normal authentication behavior and should be investigated.

## Conclusion:
Reverse shell indicators were successfully identified by detecting execution tools within system authentication logs. The presence of bash, sh, python, or nc activity suggests interactive system control and potential post-compromise behavior. Monitoring for these indicators enables early detection of malicious command execution and strengthens defensive response capabilities.
