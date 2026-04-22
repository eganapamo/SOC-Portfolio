# Lab 3 – Alert Investigation (Azure Sentinel)

## Objective:
Investigate suspicious SSH login activity triggered by repeated failed authentication attempts.

## Log Source:
Syslog (Linux auth logs via Azure Sentinel)

## Detection Rule Query:
Syslog
| where Facility in ("auth", "authpriv")
| where ProcessName has "sshd"
| where SyslogMessage has "Failed password"
| extend Ip = extract(@"([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)", 1, SyslogMessage)
| summarize FailedAttempts = count() by Ip
| where FailedAttempts > 5

## Investigation Query:
Syslog
| where Facility in ("auth", "authpriv")
| where ProcessName has "sshd"
| extend Ip = extract(@"([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)", 1, SyslogMessage)
| extend User = extract(@"user (\w+)", 1, SyslogMessage)
| extend Status = case(
    SyslogMessage has "Accepted password", "SUCCESS",
    SyslogMessage has "Failed password", "FAILURE",
    "OTHER"
)
| where Ip == "196.117.187.168"
| project TimeGenerated, Ip, User, Status
| sort by TimeGenerated desc

## Correlation Analysis:
- Observed sequence of events:
  - Multiple FAILED login attempts
  - Connection closures during authentication
  - Eventual SUCCESSFUL login

## Findings:
- IP 196.117.187.168 performed repeated authentication attempts
- Successful login occurred after multiple failures
- Activity focused on "azureuser" account

## Response:
- Flagged IP as suspicious
- Recommended blocking IP address
- Suggested enabling SSH key authentication and disabling password login

## Conclusion:
The alert accurately detected malicious behavior and enabled full investigation of a brute-force attack scenario.
