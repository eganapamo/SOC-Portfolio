## Lab 7 - SSH Brute-Force Analysis

## Objective
Identify brute-force SSH login attempts using Linux authentication logs.


## Scenario
While reviewing authentication logs on a Linux system, multiple failed login attempts were observed from a single IP address. The goal was to determine whether this activity indicated a brute-force attack.


## Log Source
/var/log/auth.log


## Commands Used

### Count Failed Login Attempts
grep -a "Failed password" /var/log/auth.log | wc -l


### Identify Source IPs With Most Failures
grep -a "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr


## Findings
3 ::1

- 3 failed login attempts detected
- Source IP = ::1 (localhost)


## Analysis
Log review identified repeated failed SSH login attempts from a single source.

Multiple authentication failures originating from one IP address is a common indicator of brute-force attack activity. In this lab environment, the source IP was localhost (::1), confirming the activity was simulated.

This pattern reflects real-world attack behavior where attackers repeatedly attempt password guesses against exposed SSH services.


## Mitigation Recommendations
- Disable root SSH login
- Implement account lockout policies
- Deploy Fail2Ban or equivalent protection
- Enforce strong password policies
- Monitor authentication logs continuously


## Skills Demonstrated
- Log analysis
- Threat detection
- Command-line investigation
- Incident documentation
- Security reasoning
