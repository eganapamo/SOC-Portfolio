# Lab 5 — Persistence Detection

## Objective:
Identify signs of attacker persistence by detecting account modifications, password changes, or privilege manipulation events in authentication logs.

## Command Used:
grep -a -i -E "useradd|adduser|usermod|userdel|groupadd|groupdel|passwd|chage|chsh|new user|changed password" /var/log/auth.log

## Explanation:
- -a  → treats log as text
- -i  → case insensitive search
- -E  → extended regex for multiple keyword patterns
- Keywords represent persistence techniques such as:
- account creation, deletion, modification, or credential changes.

## Findings:
- No matching entries returned.

## Interpretation:
- No evidence of persistence mechanisms detected.
- No user accounts were created, modified, or removed.
- No password or privilege changes observed.

## Analyst Conclusion:
- System shows no indicators of persistence activity.
- Authentication logs appear clean for the queried indicators.
