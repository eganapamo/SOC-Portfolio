# LAB 14 — Log Tampering Detection

## OBJECTIVE:
Detect evidence of log tampering attempts by identifying commands commonly used by attackers to erase, modify, or disable logging activity after gaining access to a system.

## ENVIRONMENT:
Platform: Kali Linux
Log Source: /var/log/auth.log

## BACKGROUND:
Attackers often attempt to remove traces of their activity after gaining access. This typically involves deleting logs, clearing command history, disabling history logging, or overwriting files.

Common log tampering techniques include:

- Removing logs → rm
- Shrinking logs → truncate
- Clearing history → history -c
- Disabling history logging → unset HISTFILE
- Overwriting logs → echo

These commands represent defense evasion activity.

## STEP 1 — Search for Log Tampering Indicators

Command used:

grep -aiE '\btruncate\b|\brm\b|\bhistory -c\b|\bunset HISTFILE\b|\becho.*\b' /var/log/auth.log

Explanation:

- -a → treat file as text
- -i → ignore case sensitivity
- -E → allow extended pattern matching
- \b → ensures full word match (not partial matches)

The pipe symbol ( | ) acts as OR logic to detect multiple tampering techniques in a single query.

## STEP 2 — Analyze Results

Output displayed system-related entries such as:

xfce4-screensaver-dialog: pam_unix
setuid failed: Operation not permitted

These entries were identified as normal system noise and not attacker-driven activity.

No evidence of:

- rm usage
- truncate usage
- history clearing
- HISTFILE disabling
- log overwriting

was found in the log file.

## STEP 3 — Differentiate Noise vs Signal

Noise:
Routine system operations such as authentication checks and GUI services.

Signal:
Commands showing intent to erase or modify logs.

No malicious tampering indicators were observed.

## CONCLUSION:

The analysis did not reveal any log tampering activity.

This confirms:

- No attempts were made to delete logs
- No history clearing was detected
- No history logging was disabled
- No overwrite activity was observed

Understanding these tampering indicators is critical because attackers typically attempt to remove traces after executing commands, escalating privileges, or establishing persistence.

This lab reinforced the ability to detect defense evasion behavior through log analysis.
