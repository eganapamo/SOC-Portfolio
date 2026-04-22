# Lab 1 – Brute Force Detection (Azure Sentinel)

## Objective:
Identify IP addresses responsible for repeated failed SSH login attempts followed by a successful login.

## Log Source:
Syslog (Linux auth logs via Azure Sentinel)

## Detection Query:
Syslog
| where Facility in ("auth", "authpriv")
| where ProcessName has "sshd"
| extend Ip = extract(@"([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)", 1, SyslogMessage)
| summarize FailedAttempts = count() by Ip
| where FailedAttempts > 5

## Correlation Query:
Syslog
| where Facility in ("auth", "authpriv")
| where ProcessName has "sshd"
| extend Ip = extract(@"([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)", 1, SyslogMessage)
| extend Status = case(
    SyslogMessage has "Accepted password", "SUCCESS",
    SyslogMessage has "Failed password", "FAILURE",
    "OTHER"
)
| where Ip == "196.117.187.168"
| project TimeGenerated, Ip, Status
| sort by TimeGenerated desc

## Findings:
- Multiple failed login attempts detected from IP 196.117.187.168
- Repeated authentication failures against user "azureuser"
- Successful login observed after several failed attempts

## Conclusion:
This activity indicates a brute-force attack where the attacker successfully gained access after multiple failed login attempts.
