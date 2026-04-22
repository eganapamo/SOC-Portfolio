# Lab 2 – Suspicious Login Detection (Azure Sentinel)

## Objective:
Identify unusual SSH login behavior based on repeated access attempts from a single external IP.

## Log Source:
Syslog (Linux auth logs via Azure Sentinel)

## Detection Query:
Syslog
| where Facility in ("auth", "authpriv")
| where ProcessName has "sshd"
| where SyslogMessage has "Accepted password"
| extend Ip = extract(@"([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)", 1, SyslogMessage)
| extend User = extract(@"user (\w+)", 1, SyslogMessage)
| summarize LoginCount = count() by Ip, User
| order by LoginCount desc

## Investigation Query:
Syslog
| where Facility in ("auth", "authpriv")
| where ProcessName has "sshd"
| extend Ip = extract(@"([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)", 1, SyslogMessage)
| extend User = extract(@"user (\w+)", 1, SyslogMessage)
| extend Status = case(
    SyslogMessage has "Accepted password", "SUCCESS",
    SyslogMessage has "Failed password", "FAILURE",
    SyslogMessage has "connection closed", "FAILURE",
    "OTHER"
)
| where Ip == "196.117.187.168"
| project TimeGenerated, Ip, User, Status
| sort by TimeGenerated desc

## Findings:
- High number of login attempts from IP 196.117.187.168
- Multiple failed authentication attempts followed by a successful login
- Activity targeted the "azureuser" account

## Conclusion:
Login behavior is abnormal and consistent with an attacker attempting persistence after gaining access.
